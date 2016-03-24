title: 逆向工程实战分享
date: 2016-3-24
toc: false
tags: 逆向工程
categories: 逆向工程
author: 金山
---

逆向工程一般与说来就是在没有源代码的情况下，通过一定手段分析软件结构，挖掘出有用的信息或绕过软件自身的一些限制。目前对逆向的研究主要集中在windows, ardroid,mac和ios这几个平台，各平台的发展也参差不齐。

<!--more-->

* 对windows平台的研究可以追溯到win32时代，因此目前发展比较成熟，而且拥有大量的工具和插件，比较有名的工具如ollydbg和ida pro。魔高一尺道高一丈,在逆向研究发展的火热的同时对软件的保护的研究也迅速发展起来。目前windows下的收费软件大多数都有一定的保护措施如加壳，对逆向的研究增加了一些难度。
* android是一个后兴起的平台，由于拥有大量的用户，所以对android的逆向也很成熟。android app 逆向后是一种高层次的汇编语言，也有一些工具可将之转换为更高级的java语言，虽然有部分代码混淆，但是可以很方便的注入代码，因此逆向相对容易一些。android逆向一般要经过反编译，dex转换，代码修改，重新打包，重新签名，运行调试这几个过程。
* mac平台用的人更少，对之研究相对少些，不过windows平台的技术可以借鉴，只不过逆向的工具或插件比较少，好在这个平台的大多数软件都没有壳，难度系数一般。
* iOS未接触, 但感觉跟Mac平台的逆向差不多

## 开始真正的逆向之前先逆向一个简单的程序
``` c++
#include <iostream>
using namespace std;

int max(int a, int b) {
	// return a > b ? a : b;
	if (a > b)
		return a;
	else
		return b;
}

int main() {
	int result = max(56, 32);
	cout << "Result: " << result;
	return 0;
}

```

未优化编译并逆向后的主要代码如下（在右侧增加了一些注释）:

``` asm
__Z3maxii:        // max(int, int)    ; have 8B on stack or two vars
00000001000010d0         push       rbp         ; XREF=0x1000000d0, _main+25
00000001000010d1         mov        rbp, rsp
00000001000010d4         mov        dword [ss:rbp+var_8], edi ; assign first var
00000001000010d7         mov        dword [ss:rbp+var_C], esi ; assign second var
00000001000010da         mov        esi, dword [ss:rbp+var_8] ; stack: rbp, rsi
00000001000010dd         cmp        esi, dword [ss:rbp+var_C]
00000001000010e0         jle        0x1000010f1 ; >=

00000001000010e6         mov        eax, dword [ss:rbp+var_8]
00000001000010e9         mov        dword [ss:rbp+var_4], eax
00000001000010ec         jmp        0x1000010f7

00000001000010f1         mov        eax, dword [ss:rbp+var_C] ; XREF=__Z3maxii+16
00000001000010f4         mov        dword [ss:rbp+var_4], eax

00000001000010f7         mov        eax, dword [ss:rbp+var_4] ; XREF=__Z3maxii+28
00000001000010fa         pop        rbp
00000001000010fb         ret

_main:
0000000100001100         push       rbp
0000000100001101         mov        rbp, rsp
0000000100001104         sub        rsp, 0x10   ; alloc some memory on stack
0000000100001108         mov        edi, 0x38   ; argument #1 for method __Z3maxii
000000010000110d         mov        esi, 0x20   ; argument #2 for method __Z3maxii
0000000100001112         mov        dword [ss:rbp+var_4], 0x0 ; init a var with 0
0000000100001119         call       __Z3maxii   ; max(int, int) ; result assigned to eax
000000010000111e         mov        rdi, qword [ds:imp___got___ZNSt3__14coutE] ; load object(cout) address
0000000100001125         lea        rsi, qword [ds:0x100001f3c]  ; "Result: "
000000010000112c         mov        dword [ss:rbp+var_8], eax ; max result assign to 2th var
000000010000112f         call       imp___stubs___ZNSt3__1lsINS_11char_traitsIcEEEERNS_13basic_ostreamIcT_EES6_PKc ; std::__1::basic_ostream<char, std::__1::char_traits<char> >& std::__1::operator<< <std::__1::char_traits<char> >(std::__1::basic_ostream<char, std::__1::char_traits<char> >&, char const*)
0000000100001134         mov        esi, dword [ss:rbp+var_8]
0000000100001137         mov        rdi, rax ; eax != rdi, cout object has changed 
000000010000113a         call       imp___stubs___ZNSt3__113basic_ostreamIcNS_11char_traitsIcEEElsEi ; std::__1::basic_ostream<char, std::__1::char_traits<char> >::operator<<(int)
000000010000113f         xor        esi, esi ; reset
0000000100001141         mov        qword [ss:rbp+var_10], rax ; assign result(output stream) to  last var
0000000100001145         mov        eax, esi
0000000100001147         add        rsp, 0x10 ; balance stack
000000010000114b         pop        rbp       ; restore rbp
000000010000114c         ret  
```

理解上面的代码需要注意一些调用规定，上面的代码使用的fastcall调用约定

* 调用约定
	* stdcall 参数从右向左压入堆栈；函数自身平衡堆栈；
	* cdecl 参数首先由有向左压入堆栈；调用者负责平衡堆栈
	* fastcall 使用寄存器传递参数        

## 实战Reveal [Download from offical website](http://revealapp.com/download/)
本次破解使用的reveal版本是1.6.3(5790)，也是目前的最新版。由于编写一个注册机的难度较高，本次破解只是针对打补丁的方式，想挑战的话可以尝试编写一个注册机。

### 工具
工欲善其事，必先利其器，mac下的破解工具主要有俩个，一个是hopper，另外一个是ida pro。本次破解使用hopper，这个软件更容易上手，注意需要装额外的调试工具，也可从官网下载到。

### 破解流程
#### 方法一
1. 直接打开Reveal, 复制提示的字符串`Your free trial of Reveal has expire`，也可能是其它的，取决于你的reveal是否过期
2. 使用Hopper加载Reveal
3. 搜索前面复制的字符串`Your free trial of Reveal has expire`，跳到引用该字符串的地方，这时会跳到`[IBATrialModeReminderWindowController trialExpiresTitle]`·函数中间的某个地方，查找函数调用的对照，只要在根源处注释掉即可达到破解的目的
4. 继续查找引用或调用这个函数·trialExpiresTitle·的地方，没有找到。换个思路，搜索字符串`IBATrialModeReminderWindowController`，这是会调到`[IBATrialModeReminderWindowController controller]`函数里面
5. 继续向上查找调用该函数`[IBATrialModeReminderWindowController controller]`的地方，会跟进这个函数`[IBATrialModeReminderPresenter showTrialModeSheetForWindow:]`
6. 继续向上查找，会进入`[IBATrialModeReminderPresenter presentTrialModeReminderIfNecessaryForWindow:]`·这个函数，观察如下的代码，已经定位到我们想要的地方
    
``` asm
000000010007f306         mov        r14, rax
000000010007f309         mov        rsi, qword [ds:0x1001b3150]                 ; @selector(shouldShowTrialModeSheet), argument "selector" for method imp___got__objc_msgSend
000000010007f310         mov        rdi, rbx
000000010007f313         call       qword [ds:imp___got__objc_msgSend]
000000010007f319         test       al, al
000000010007f31b         je         0x10007f330

000000010007f31d         mov        rsi, qword [ds:0x1001b3158]                 ; @selector(showTrialModeSheetForWindow:), argument "selector" for method imp___got__objc_msgSend
000000010007f324         mov        rdi, rbx                                    ; argument "instance" for method imp___got__objc_msgSend
000000010007f327         mov        rdx, r14
000000010007f32a         call       qword [ds:imp___got__objc_msgSend]

000000010007f330         mov        rdi, r14                                    ; argument "instance" for method imp___got__objc_release, XREF=-[IBATrialModeReminderPresenter presentTrialModeReminderIfNecessaryForWindow:]+40
000000010007f333         pop        rbx
000000010007f334         pop        r14
000000010007f336         pop        rbp
000000010007f337         jmp        qword [ds:imp___got__objc_release]
```
7. 注意地址000000010007f31b处的代码`je 0x10007f330`，如果不跳转回执行下面的代码就会显示提示对话框，因此把条件取反，或者无条件跳转改成`jmp je 0x10007f330`即可
    
#### 方法二
    
如果觉得上面步骤繁琐的话，其实还有更简单的方法，按照一般的编程思维，一般都会写一个shouldShow...的方法来显示提示对话框，所以直接搜索字符串·shouldShow·，调到这个函数`[IBATrialModeReminderPresenter shouldShowTrialModeSheet]`，这个函数的代码如下：

```asm
-[IBATrialModeReminderPresenter shouldShowTrialModeSheet]:
000000010007fde3         push       rbp                                         ; Objective C Implementation defined at 0x10018dc40 (instance)
...
...
000000010007fdf1         mov        r15, rdi
000000010007fdf4         call       sub_10007e91a
000000010007fdf9         test       al, al
000000010007fdfb         jne        0x10007fe06

000000010007fdfd         call       sub_10007e95e
000000010007fe02         test       al, al
000000010007fe04         je         0x10007fe1a

000000010007fe06         xor        ebx, ebx                                    ; XREF=-[IBATrialModeReminderPresenter shouldShowTrialModeSheet]+24

000000010007fe08         movzx      eax, bl 
...
...                                    
000000010007fe19         ret        
```

觉得汇编不好理解不直观的话,可以来点黑科技，看自动生成的高级语言代码：

```C
char -[IBATrialModeReminderPresenter shouldShowTrialModeSheet](void * self, void * _cmd) {
    r15 = self;
    if ((sub_10007e91a() != 0x0) && (sub_10007e95e() == 0x0)) {
            r14 = [[NSDate date] retain];
            r13 = [[[r15 class] lastPresentationDateTime] retain];
            [r13 release];
            rbx = 0x1;
            if (r13 != 0x0) {
                    [r14 timeIntervalSinceReferenceDate];
                    var_30 = intrinsic_movsd(var_30, xmm0);
							...
							...
                    COND = xmm1 > 0x0;
                    rbx = COND ? 0x1 : 0x0;
                    [r13 release];
            }
            [r14 release];
    }
    else {
            rbx = 0x0;
    }
    rax = rbx & 0xff;
    return rax;
}
```

仔细分析代码，发现两处关键代码：

```asm
000000010007fdfb         jne        0x10007fe06
000000010007fe04         je         0x10007fe1a
```
所以只需要把000000010007fdfb地址处的代码改为`jmp 0x10007fe1a`即可

以上的操作都是在内存中修改的，关闭后重新打开还是无法使用，执行File -> Produce New Executable file, 这是会提示你签名无效，是否移除签名，选择否，(如果选择是的话，每次启动都会有个提示很烦人的),替换原来的可执行文件，重新启动Reveal,会提示`This copy of Reveal is damaged`，解决方法同上，不在这里赘述。
	
## 安全防范
* web请求加入Authorization header
	REST服务的web请求中加入Authorization header验证，对资源进行校验。可有效阻止爬虫。
* 代码混淆（Android）
* 花指令
	伪装其他编译器的特征码，干扰反汇编，绕过杀毒软件。
* 加壳
	在二进制的程序中植入一段代码，在运行的时候优先取得程序的控制权，做一些额外的工作。常见的壳分为压缩壳和加密壳，加壳的软件直接破解难度很大需要先进行脱壳处理。
* 加密, 代码签名
	一定程度上阻止逆向。
* 调试检测
	用ring3级下的调试器对可执行程序进行调试时,调试器会把被调试的可执行程序作为一个子线程进行跟踪.这时被调试的可执行程序的PEB结构偏移0x02处的BeingDebugged的值为1,如果可执行程序未被调试,则值为0,所以可以利用这个值来检测程序是否被ring3级下的调试器调试。可以有效阻止动态调试。


