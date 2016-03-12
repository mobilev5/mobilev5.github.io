title: 带你领略 Swift 的风骚
date: 2016-2-24 16:26:13
toc: true
tags: Swift
categories: iOS开发
author: Makee(酒仙)
---

`Swift`，已经出来一年多了，想必不少人都曾对它抱着观望的态度，如果现在的你还是这样的态度，那么是时候去改变了。作为一个年龄比我还大的语言————`Objective-C`，是时候淡出历史的舞台了，有人说`Objective-C`和`Swift`会并存，但我觉得，短期内会这样，从长远的角度来看，并存的可能性并不大。毕竟，它们不是`C`和`C++`这样的关系，它们更像`Delphi`与`C#`这样的存在。从可维护的角度来说，并存的代价比较大，所以，对于`Objective-C`，放手吧。

<!--more-->

作为一个绝对现代化的编程语言，Swift集合了当下众多流行语言的特性，这与保守的`Objective-C`产生了鲜明的对比。如果你还在犹豫踌躇中，那么就让本文带你领略下新欢的风骚，这一切，你值得拥有！


## 不得不说的简洁和优雅

谈到Swift，不得不把它的优雅放在第一位，作为一门现代化的编程语言，优雅是它必备的特质，那么什么是**优雅**？所谓优雅就是在书写或阅读时，有种简单自然、一气呵成的感觉，不罗嗦、不做作，这是语法上的优雅，而设计中的优雅，又何尝不是这样？


### 声明和实现的合并

这可能是Swift向现代化语言看齐的第一步，在很多传统的编程语言里，例如C++、Pascal、Objective-C，类或方法的声明与实现是分离开的。当然C++是可以在头文件里直接实现内联方法，但这很不自然，也不安全，因为它会暴露给最终用户。

以往声明和实现的分离，其实更像是`接口定义`与`接口实现`，但这种强制性的设定给使用者也会带来一些麻烦。就拿Objective-C来说，如果我要定义一个私有类，则会写在“.m”文件中，但这时候又不得不将声明也书写一遍，这是没有太多意义的；又比如，我一个简单到只有属性的`PONSO`，还不得不将一个空的实现书写一遍（_请抛开自动属性这些用户不需要知道的细节_）。

任何一门现代化的编程语言，都摈弃了这种分离的方式，其原因是为了追求更加的简洁和优雅。而在这种合并的方式下，也迫使我们不会将一个类写得特别庞大，以前的团队合作中，大家只会看你的头文件，而现在，更多的是看你具体的代码实现了（_无疑，这也是一种Code Review_）。


### 扩展（Extension）

扩展，这也是Swift中非常有特色的设计，也是苹果编程语言的一贯传承。Swift中的扩展，其实是对Objective-C中`Category`和`Extension`的合并，在Objective-C中，我们会书写类似下面的代码：

```objc
@interface OCObject : NSObject
@end

@interface OCObject (Category) <NSCoding>
@end
```

对比Swift，如下：

```swift
class SwiftObject { }

extension SwiftObject: NSCoding { }
```

扩展主要可应用于以下场景：

1. 对原有类增加新协议适配，新方法
2. 对实现代码按逻辑块进行划分（_比如按实现的协议划分_）

在我所接触的其它语言里，也有扩展一说，比如C#，但C#的扩展其实完全只是语法糖，而Objective-C或Swift中的扩展，并不仅仅是语法上的便利，更有运行时的支持，考虑下面代码：

```swift
protocol SwiftProtocol { }

class SwiftClass { }

extension SwiftClass: SwiftProtocol { }

let swiftObject = SwiftClass()

print(swiftObject is SwiftProtocol)
// print: true
```

不得不说，扩展对代码的优雅性上，也给出了不少的支援，相比于一个实现了N多协议的类，我们用`extension`进行划分会清晰很多。


### 可选类型和可选链

可选类型，也就是在一个类型的定以后，增加一个`?`号，代表这个变量可以为`nil`。这也是Swift相对于Objective-C的一个重大改进，使得代码更加安全，表述性更强。但，需要注意的是，这并不是Swifit特有的，在.Net平台中，可选类型也是非常常见的。对于以下的Objective-C代码：

```objc
- (NSString *)userAddress:(HJUser *)user {
    if (user == nil) return nil;
    if (user.city == nil) return nil;
    if (user.street == nil) return nil;

    return [NSString stringWithFormat:@"%@%@", user.city, user.street];
}
```

从使用者的角度来说，我们必须要判断返回值，因为我们根本不确定是否会返回`nil`；而从设计者的角度来说，我们也很头疼，我们也不知道调皮的用户到底会给我们传入什么。而`__nonnull`这样的标示，根本就无法阻止这样的行为：

```objc
- (NSString * __nonnull)test:(NSString * __nonnull)arg {
    return nil;
}
```

这段代码在Xcode 7 beta2中编译，没有任何错误和警告。想一想上面的那段`userAddress `代码，其实我们很确定，如果传入的参数为空或者其属性为空，则返回值肯定为空；而对方法的设计而言，参数为空是没有任何意义的，我们应该让使用者保证他传入的参数是不能为`nil`的，而不是在两端都对`nil`进行判定，这无疑增加了复杂度（_三个方面：设计、使用、调试_），也不合理。所以，在Swift中对其进行改善：

```swift
func getUserAddress(user: HJUser) -> String {
    return user.city + user.street
}
```

我们可以安全的调用上面的方法，并不需要多余的`nil`判断，那么可选类型应该应用于什么场景呢？**可选类型，应该应用于某个变量、参数或返回值，存在空或非空两种合理的状态下**。也就是说它可以为空，也可以不为空，并且，从逻辑的角度考虑很合理。比如，下面的代码：

```swift
func findUser(username: String) -> HJUser? {
    // 如果找到了则返回
    // 否则返回nil
    return nil
}
```

从逻辑的角度考虑，我们查找数据，有两种结果：找到和没有找到，这样的场景下就特别适合使用可选类型。那么配合可选链，我们的代码会非常简洁和优雅：

```swift
func findUserRealName(username: String) -> String? {
    return findUser(username)?.info?.realName
}
```


### 模式匹配（Patterns）

模式匹配，是函数式编程里非常常见的一个特性，这也和语言的优雅性息息相关，在Swift中，大概有以下几种模式匹配：

1. 通配符（Wildcard Pattern）
2. 标识符（Identifier Pattern）
3. 值绑定（Value-Binding Pattern）
4. 元组（Tuple Pattern）
5. 枚举（Enumeration Case Pattern）
6. 可选（Optional Pattern）
7. 类型转换（Type-Casting Pattern）
8. 表达式（Expression Pattern）

关于模式匹配，下面的代码进行了很好的阐述：

```swift
// Identifier Pattern
let points: [(Int, Int)?] = [(0, 0), (1, 1), nil, (3, 3)]

if case .Some(let p) = points[0] {  // Enumeration Pattern
    print(p)
}

if let p = points[0] { // Identifier Pattern
   print(p)
}

for case let point? in points {  // Optional Pattern

    let (x, _) = point  // Tuple Pattern
    print(x)

    switch point {
    case (0, 0):        // Expression Pattern
        print("0, 0")
    case let (1, y):    // Value-Binding Pattern
        print(y)
    case _:             // Wildcard Pattern
        print("Wildcard Pattern")
    }
}
```

对于模式匹配的深入理解，有助于对Swift的阅读，也有助于编写出更加简洁优雅的代码。anyway，只要记住，**模式匹配是一种类似于正则表达式的捕获规则**，比如通配符`_`可以捕获任何值，`(x, y)`只能捕获二元组，`(0, y)`只能匹配以0为第一元的二元组，可选和枚举也类似。从抽象的角度来说，所有的模式匹配，都有以下特质：

* 需要有一个输入与之进行匹配测试
* 匹配结果有两种：成功或失败
* 匹配成功时，可捕获所匹配到的值

That's cool！感受下，如果没有这些便捷的模式匹配，用传统的条件分支语句，会写成怎样？

### 闭包（Closure）

没有闭包的编程语言，就不能称之为函数式编程语言，我们来看看百度百科对闭包的解释：

> 闭包是可以包含自由（_未绑定到特定对象_）变量的代码块；这些变量不是在这个代码块内或者任何全局上下文中定义的，而是在定义代码块的环境中定义（_局部变量_）。

Objective-C中也有闭包，也就是`block`，但就语法的反人类程度就已经很令人发指了，更别说和函数指针定义在一起时，是多么令人奔溃了。也就是说`block`的设计并不简洁、优雅，这点在Swift中有了很好的改进，Swift中，已直接将它称之为**闭包**：

```swift
func sendRequest(url: String, response:(String) -> Void) {
    // ...
}
```

在Swift中，闭包的定义其实可以抽象成`() -> ()`这样的通用模式，也就是一个输入**推导出**一个输出，这是非常直观的定义方式，也更贴近其它编程语言中闭包的定义方式（_C#，Java8中的Lambda表达式_）。而在Swift中，为了更加优雅，放置在参数最后的闭包使用时可以放置到参数括号外，闭包输入参数可以用`$0、$1...`这样的方式来捕获，如下：

```swift
sendRequest("http://www.baidu.com") {
    print($0)
}
```

这种闭包放置在参数括号外的特性，Swift将它称之为**尾随闭包**。

在Swift中，和其它函数式编程语言一样，闭包更像是嵌套函数，或者称之为内部函数，Swift中所有的函数声明，都可以用闭包表达式来描述，例如：

```swift
func login(username: String, password: String) -> Bool {
    //....
}
```

用闭包表达式来描述：`(String, String) -> Bool`，**所有类型为闭包的参数，都可以用签名相同的函数来替代**，所以，先前的`sendRequest`，可以直接如下使用：

```swift
sendRequest("http://www.baidu.com", response: print)
```

这便是**函数式编程**的一个精髓所在，函数不仅可以被调用，还可以将其作为参数或返回值进行传递，这可能是和**命令式编程**最大的区别了。

另外在使用闭包时，由于它能捕获当前上下文中的变量，特别是对`self`而言，这很容易导致循环引用。以往在Objective-C中的常见做法是定义一个`weak`的`self`，只使用那个`weak`的`self`，但这样依然会有出错的可能，这点在Swift中也进行了改良：

```swift
sendRequest("http://www.baidu.com") { [weak self] resp  -> Void  in
    //...
}
```

这种情况下，闭包内所有的`self`都是`weak`的，强制性的需要你进行`nil`的判断。这样的设计，使得Swift更加安全和优雅。关于安全，这正是我们接下来需要探讨的内容！


## 类型和类型安全

Objective-C是一个弱类型的语言，或者说是一个比较动态的语言，而Swift与之截然不同，Swift是一个名副其实的强类型语言。相比之下，弱类型的语言更加灵活，但更容易出错，而强类型的语言，描述性和约束性更强，也更加安全。

### 类型推断

类型推断是现代化编程语言的趋势，在`C#`中引入了`var`关键字，大大简化了方法和变量的定义。Swift中自然是拥有了类型推断的能力，没有类型推断的Objective-C代码如下：

```objc
- (void)doSomething {
    OCObject *obj = [OCObject new];
    NSString *str = [obj test:@"hello world"];
}
```

由于没有类型推断，变量定义时必须要定义它所属于的类型，方法的返回值也是类似。而从某种角度来说，其实通过等号右侧完全可以推断出左侧的类型，所以在Swift中，编译器会智能的做这样的推断，可以帮我们省略下很多代码的编写：

```swift
func doSomething {
    let obj = SwiftObject()
    let str = obj.test("hello world")
}
```

当然，类型推断的好处远远不止这种形式，在泛型和闭包中，类型推断简直就是其设计的点睛之笔。而类型推断，也进一步的阐述了Swift是强类型语言，否则就不可能推断出所需类型，那么在Objective-C中常见的`unrecognized selector` Crash，在纯粹的Swift中永远不会发生。


### 值类型

值类型也是Swift中伟大的创举之一，值类型的一个显著特征便是在赋值和传递时会进行复制。为什么说Swift中值类型是一个创举呢？因为在我所经历的高级语言里，从未见过将字符串和框架内集合类型定义为值类型的，不得不说，Swift是第一个。

选择值类型，往往是为了对象在多线程环境下更加安全，因为它`复制`的特性，我们需要面对的只是单个实例对象，这使得我们对代码更加可控。另外，值类型中方法如果要修改成员变量，则必须使用`mutating`修饰：

```swift
mutating func withMutableCharacters<R>(body: (inout String.CharacterView) -> R) -> R
```

这会让我们在使用或设计时，更清楚值对象的变化原因，而外界对值对象的修改也是有很大限制，考虑下面的代码：

```swift
struct TestStruct {
    var field1 = 3
}

func modifyStruct(var st: TestStruct) {
    st.field1 = 4
}

var st = TestStruct()

modifyStruct(st)

print(st.field1)
```

执行结果为`3`，为什么会这样？因为在进行参数传递的时候，函数体操作的只是`st`的副本，`st`在传递时进行了复制。所以，即便是将`var st = TestStruct()`改为`let st = TestStruct()`也不会有任何编译问题，这是完全合法的操作。如果的确要在函数内修改传入参数，则使用下面的方式：

```swift
func modifyStruct(inout st: TestStruct) {
    st.field1 = 4
}

var st = TestStruct()

modifyStruct(&st)
```

这与很多编程语言中的参数传递方式是类似的，也就是两种：**传引用**和**传值**，默认情况下，Swift参数的传递都是“传值”的方式，这种情况下，引用类型会多出一个指向实例内存的指针，而值类型会进行复制（_可以说，指针就是值类型_）。

了解了值类型与引用类型的本质区别，那么还有很多值得去尝试的地方，比如在值类型中定义引用类型的成员，那么在该值对象的副本上修改该引用成员，依然会影响到主体。具体实践就留给在座的各位了。


### 枚举类型

在目前主流的编程语言中，枚举是非常常见的一种值类型，而大多数人对它的用法一直还停留在`C或C++`的那种形式上。枚举是什么？顾名思义，**枚举是一系列有限的状态集合**，那么涉及到状态时，我们会很自然的想到用枚举来表示。如果仅仅用来表示状态，那么枚举的使用范围就非常有限，但在实际的开发中，有很多时候，我们有些数据仅在某种状态下才具有意义，或者说，这种数据只能存在于特定状态中。比如我们做网络请求时，会有两种状态：**成功**或**失败**，而仅仅在失败时，**错误消息**这个数据才有意义。那么这时候如果使用面向对象的思维来解决，我们可能需要定义一个通用的**状态**基类，然后有两个子类来实现不同状态。但在Swift中，你有另外的选择，那就是**枚举关联值**。

所谓枚举关联值，便是在枚举中的每一种状态下，都可以关联一些个数据。其实这种做法在Java中早就有了，但Java的枚举比起Swift的，还是有所不足的。我们先来看看Java的枚举：

```java
public enum Color {
    RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);

    private String name;
    private int index;

    private Color(String name, int index) {
        this.name = name;
        this.index = index;
    }
}

```

可以看到，Java的枚举的确也有关联值，但数量和数值都是固定死的，它也无法解决上面我们提到的那个问题。所以，还是看看Swift是怎样解决这样的问题吧：

```swift
public enum ResponseStatus {
    case Success
    case Failure(errorMessage: String)
}

let status = ResponseStatus.Failure(errorMessage: "网络连接中断")

// 模式匹配中的 Enumeration Pattern 哦~
if case .Failure(let msg) = status {
    print(msg)
}

switch status {
case .Success:
    print("success")
case .Failure(let msg):
    print(msg)
}
```

是不是很酷？相当酷！这样给我们减少了一些细小类的编写，并且更加合理和直观。除了关联值之外，枚举还可以有它自己的构造函数和方法，这会给我们在设计状态相关的逻辑时提供不少的便利。除此之外，枚举还是可以定义成泛型的，这样的灵活性给了我们更大的发挥空间，所以，我们再来看看泛型！


### 泛型

泛型，这是多么激动人心的一个特性啊，不知道你是否和我一样对泛型抱有极高的期待。作为一个现代化的编程语言，怎么可以没有泛型呢？转入Objective-C之后，很多时候的设计，都卡在了泛型这块，使得我不得不多写出一些类来完成设计。虽然如今的Objective-C中加入了泛型，但依然没有达到我的预期，而Swift中的泛型，虽然还有些欠缺，却也已经是足够强大了。

什么时候该用泛型呢？我总结如下：**当某种逻辑，可应用于一系列有相似点的对象，为了确保拥有强类型的特性时，则需要使用泛型**。对于这句话的理解，需要进行一些深度分析的，很多时候其实我们并不需要泛型，使用基类即可满足。聚个例子吧，当我们进行一些图形绘制程序的设计时，一个图形元素的绘制可能会抽象出这样的接口：

```swift
protocol GraphicElement {
    func draw(panel: GraphicPanel)
}
```

那么在我们的绘图引擎中，绘制元素的方法应该如何定义？使用泛型么？这里其实并不适合使用泛型，因为我们没有必要保留强类型的特性，我们只关心`draw`方法，所以使用基类即可：

```swift
func drawElement(element: GraphicElement) {
    element.draw(self)
}
```

而在某些情况下，我们需要使用强类型的特性，这会使得我们代码更加简洁和安全，这时候，我们就需要使用泛型。比如，在一个通用的消息过滤模块，我们需要对消息内容进行关键字过滤，那么过滤前和过滤后的消息类型应该是一致的，这时候，我们就需要保留强类型的特性，所以，要使用泛型：

```swift
class Message {
    let content: String

    init(content: String) {
        self.content = content
    }
}

class GroupMessage : Message {
    private(set) var groupId: Int

    init(id: Int, content: String) {
        self.groupId = id
        super.init(content: content)
    }
}

func filterMessage<T: Message>(message: T) -> T {
    // ... filter 
    return message
}

var message = GroupMessage(id: 12, content: "hello world")
message  = filterMessage(message)
```

请时刻记住泛型的使用场景，避免没必要的泛型设计。在Swift中，目前泛型还是有欠缺的，少了`逆变`和`协变`的支持，而在某些场景下，这是必须的。相信在不久的将来，这个特性会被弥补上来的，毕竟，Objective-C中已经有了这样的支持，虽然不尽人意。

逆变和协变，不仅体现在泛型上，在继承链中的方法覆写上，也有应用。而很多人对这样的概念仍是一知半解，甚至陷入了错误的认知，希望在这里可以帮助大家真正理解它的适用场景。最基本的概念如下：

* **逆变**：父类可以替代子类
* **协变**：子类可以替代父类

为什么会有这两种概念存在？其实最根本的原因还是为了类型安全，在`C#`的语法设计中，对于数组默认是允许协变的，这样导致存在安全隐患，这也是`C#`为数不多的设计缺陷之一。考虑下面的`C#`代码：

```c#
class Super { }

class Sub : Super {
	public void test() { }
}

public static void Main(string[] args) {
	var subs = new Sub[3];
	subs[0] = new Sub();
	subs[1] = new Sub();
	
	Super[] supers = subs;   // 协变
	supers[1] = new Super();
	
	// 这里会崩溃掉，抛出 ArrayTypeMismatchException
	subs[1].test(); 
}
```

可以说，这种设计是很糟糕的，因为它没有保证到类型的安全，我们明明声明的是一个`Sub`的数组，里面却可以混入一个不是`Sub`的类型，这便是**协变的陷阱**，相同的问题在Objective-C和Java中同样存在（_Java中会抛出ArrayStoreException异常_）。而在Swift中，如下的代码：

```swift
class Super {}

class Sub : Super {
    func test() {}
}

var subs = [Sub]()
subs.append(Sub())
subs.append(Sub())

var supers: [Super] = subs

supers[1] = Super()
subs[1].test()
```

这样的代码是不会出现任何问题的，因为在Swift中，`Array`是值类型，`supers[1] = Super()`只是对副本的修改，并不会影响到`subs`。这样的协变特性，目前只有系统库能享有，我们自己定义泛型是无法做到的，但，从安全性的角度来说，值类型的泛型应该默认支持协变，这是没有任何副作用的。

协变和逆变的另一个使用场景，便是方法参数和返回值的约束，**参数和返回值应该是可协变的，而闭包中的参数应该是可逆变的**，请好好的理解我说的这句话，加以实践，你会明白这其间的道理。关于泛型，也就说到这里，更多内容还需大家自己去领悟，接下来，我们看看更多有意思的东西！


### 元组类型

似乎所有的函数式编程语言里都有元组类型，包括`C#`这种命令式编程语言里也引入了元组，其实元组是很简单的东西，但在Swift中却尤为重要。**元组类型可以看作是一种有序字典**，在编程语言中，所有的`Plain Object`都是可以用字典来表示，只是使用起来不是很便利。而，元组是一种表现力更强，使用起来更方便的`Plain Object`。

什么时候适合使用元组？大体在下面几种情况下：

1. 多返回值的函数里
2. 用于临时的数据传输对象（_DTO_）

```swift
func mutilReturn() -> (String, String) {
    return ("Hello", "World")
}

func tempDTO() {
    var cellSummary: (id: Int, display: String)
    cellSummary.id = 1
    cellSummary.display = "hello world"

    print(cellSummary)
}
```

元组在面对这种有意义但又很小的简单对象时非常有用，可以帮我们减少很多细小类的编写，**需要注意的是，元组是值对象**，所以它拥有所有值对象的特性。另外，**Swift中，所有的变量定义都是一个一元组**，可以通过下面的代码验证：

```swift
let i = 456
let i2: Int = 566

// 通过下标访问元组项
print(i.0.0.0.0)
print(i2.0.0.0.0)
```

这种设计可以让元组类型和普通的值类型进行平滑过渡，所以`Type`和`(Type)`在Swift中是等同的，考虑下面的代码：

```swift
class Item {
    let value: Int
    init(_ value: Int) {
        self.value = value
    }
}

// 一元组数组
var array: [(Item)] = [(Item(1)), (Item(2)),Item(3), Item(4), Item(5)];

func test(i: Item) {
    print(i.value)
}

test(array[0])
```

上面的代码是没有任何问题的，所以，在Swift中，元组是一等公民，也可以说，只要你使用了Swift，你就已经在使用元组了。


### 嵌套类型

嵌套类型也是Swift中靠近现代化语言的重要一步，各种主流高级设计语言中，对嵌套类型的定义都有细微区别。比如，Java中的成员内部类其实就是一个闭包，而静态内部类才是和Swift内部类类似的存在。不过，**嵌套类型的设计，基本都是为了提供更严格的访问控制，和隔离实现**。除此之外，由于Swift中没有名称空间（_Name Space_）和包（_Package_），只有模块（_Module_）的概念，嵌套类型也常用来组织一系列相关的类，用以类更精细化的管理。参考下面这样一段代码：

```swift
protocol GraphicElement {
    func draw()
}

final class GraphicElementFactory {

    class func createElement(text: String) -> GraphicElement {
        return TextGraphicElement(text: text)
    }

    private class TextGraphicElement: GraphicElement {
        private let text: String

        private init(text: String) {
            self.text = text
        }

        private func draw() {
            print(self.text)
        }
    }
}

let element = GraphicElementFactory.createElement("hello world")
element.draw()
```

上面的示例，我们通过嵌套类来对外屏蔽实现细节，从面向对象的角度来考虑，这提供了良好的封装性，约束了使用者必须通过某种唯一途径来获得接口的实例。考虑下Foundation中的类簇，这种更严格的访问控制使得我们能设计出对客户代码侵入性更小的类库，减少使用者对某些他们并不关心类的困惑。

### 安全的覆写

最后稍微提及一下Swift在`override`上的改进，也就是面向对象中的覆写。这一点在Objective-C中简直是糟透了，因为当你继承一个类时，一不小心你就可能覆写掉了父类的某个私有方法，结果当然是你无法预计的。所以，为了防止这样的情况出现，我们会在私有方法命名前加上一些毫无意义的标识，这对追求优雅的人来说，是极度痛苦的（_有段时间我一直在比较，究竟用几个下划线比较好看_）。好在Swift里对此做出了很好的改进，**如果子类要覆写父类中的方法，那么必须使用`override`关键字，如果子类中出现了与父类签名相同的方法，并且没有标记`override`则编译不会通过**。这很棒！不是么？编译器向我们保证了继承链中不会存在意外覆写的状况，又为我们减少了一个可能会掉入的坑，所以，现在的程序员，真是太幸福了。

```swift
class Foo {
    func foo() {
        print("foo")
    }
}

class Bar : Foo  {
    func foo() { // 编译不通过

    }
}
```


## 重新定义函数

众所周知，Swift是一个支持函数式编程的语言，所以在函数这块与传统的命令式编程有较大的区别。首先，我们要搞清楚，什么是**函数**，什么是**方法**？函数是统称，而方法是主体的行为，也就是定义在类或其它主体中的函数。

### 高阶函数

在函数式编程里，我们不得不说说高阶函数，这是函数式与命令式最大的区别。在函数式编程里，函数是可以做为函数的输入参数和返回值，而**高阶函数便是参数或返回值中有函数的函数**。Swift中的高阶函数定义使用的是闭包表达式，这在闭包的章节里已经有所提及，参看下面这个高阶函数：

```swift
public extension Array {
    public func select<T>(trans: (Element) -> T) -> [T] {
        var result = [T]()
        for ele in self {
            result.append(trans(ele))
        }
        return result
    }
}

func translateInt(i: Int) -> String {
    return "\(i)"
}

let array = [1, 2, 3, 4, 5]
let strArray = array.select(translateInt)

print(strArray)
```

上面`Array`扩展中的`select`方法便是一个高阶函数，因为它接受一个方法参数，当然，我们也可以用闭包直接代替。高阶函数的使用，可以简化一些算法的实现，并且能有效的减少一些多余的中间变量。也因为有高阶函数的存在，使得函数和普通变量站在了同等的地位，这是函数式编程很大的特点。在其它的函数式编程语言中，还会有一些更加高级的函数式特性，相信在不久的将来，这些特性也都会加入到Swift中，参考下面`F#`的一段代码：

```fsharp
let pa f x = (f (x - 1.0), f (x + 1.0))
let g1 x y = x ** y
let h = pa g1 2.0 // 函数的局部应用
```


### 函数参数

谈到函数，那不得不说说它的参数了，这也是Swift区别与很多其它语言的地方。在古老的Objective-C中，方法的参数命名与其它同等级语言差别是巨大的，虽然褒贬不一，但不得不说，相比于Java或C#，它的可读性是最强的。苹果似乎一直想要保持这种的`代码即文档`的作风，所以在Swift中保留了这样类似的特性，并且对它做了简化，对比一下定义即可：

**Objective-C**：

```objc
- (BOOL)loginWithUsername:(NSString *)username password:(NSString *)password;
```

**Swift**：

```swift
func loginWithUsername(username: String, password: String) -> Bool
```

算一算，一共帮你省略了多少字符，这对保护你珍贵Mac键盘还是很有好处的！另外与参数话题相关的，就是**参数默认值**了，因为Swift参数是携有命名的特性，所以参数的默认值并不像其它语言中那样必须放置在最后的几个参数，这又让Swift能对自己拥有命名参数而引以为豪了：

```swift
// 放置在第一位的默认值
func loginWithUsername(username: String = "admin", password: String) -> Bool {
    return true
}
```

然后要说的便是**可变参数列表**了，在Objective-C中也有可变参数列表，比如`NSLog`中后续的参数，但是，在不进行任何文档查阅和网络搜索的情况下，你能默写出来么？我觉得大多数人都写不出来，而在Swift中，这种情况得以改变，可变参数列表直接与数组使用类似：

```swift
func Log(format: String, args: String ...) {
    for arg in args {
        print(arg)
    }
}
```

上面代码片段中的`args`，其实就是一个类似与数组的参数，这比起Objective-C中的使用要简单的很多，也与其它编程语言中类似。


### 自定义操作符

自定义操作符是一个非常酷的特性，它可以帮我们**将一些嵌套调用的代码变得更加清晰**，在Swift中，操作符其实就是一个特定的函数，这也是与众多函数式编程语言保持一致的地方。假设我们要做一个图片滤镜的程序，也就是说，可以对图片应用各种滤镜效果，那么应该有以下这样类似的代码：

```swift
class Image {
    var filterNames = [String]()
}

protocol ImageFilter {
    func apply(image: Image) -> Image
}

class GrayFilter : ImageFilter {
    func apply(image: Image) -> Image {
        image.filterNames.append("gray")
        return image
    }
}

class BlurFilter : ImageFilter {
    func apply(image: Image) -> Image {
        image.filterNames.append("blur")
        return image
    }
}

class ContrastFilter : ImageFilter {
    func apply(image: Image) -> Image {
        image.filterNames.append("contrast")
        return image
    }
}
```

当我们要对图片应用滤镜时，则可能会写出类似下面这样的嵌套调用：

```swift
let image = Image()

let gray = GrayFilter()
let blur = BlurFilter()
let contrast = ContrastFilter()

contrast.apply(blur.apply(gray.apply(image)))
```

这时候，我们可以用自定义操作符来解开这样的嵌套，类似下面代码：

```swift
infix operator |> { associativity left precedence 140 }

func |> (left: Image, right: ImageFilter) -> Image {
    return right.apply(left)
}

let outputImage = image |> gray |> blur |> contrast
print(outputImage.filterNames)

```

通过自定义`|>`这样一个操作符，我们使用者的代码表述性变得更强，也将相关性的处理放置在了同一条语句里。这种感觉是不是非常棒？似乎已经看到你们在YY一些奇怪的操作符了。关于自定义操作符的语法，这里简单的说明下：

```swift
infix operator |> { associativity left precedence 140 }

infix			: 代表操作符类型，可以为 prefix(前置) infix(中置) postfix(后置)
operator		: 固定关键字
|>				: 要定义的操作符
associativity	: 可以为 left right none，表示当两个同等优先级的中置操作符出现时，优先使用哪个。如果为 none 则不能将操作符连接。
precedence	: 操作符的优先级，值越大，优先级越高，+ 的优先级为 140
```

介绍完操作符，函数这块的内容应该也可以告一段落了，接下来介绍下属性相关的特性！


## 强大的属性

属性在面向对象的设计中，也是非常重要的一个概念，属性是对对象某种状态值的抽象，比如颜色、大小、重量等。在Objective-C中，属性又称之为自动合成属性，因为是编译器将**设置**和**获取**方法，按照属性的关键字进行自动合成的。这种方式在其它语言里也很常见，比如C#的属性也是类似，可以通过反射获取到单独的设置和获取方法。Swift中的属性也是传承了Objective-C属性的一些特性，并做了一些调整，比如去除了原子性描述，由于目前并没有太多关于Swift运行时的文档，也没有做一些关于这方面的Hack，所以原理性的东西这边就不提及了。


### 延时属性

在Objective-C中，我们可以手动的实现一个延时属性，也就是只有当属性第一次被调用时，才真正的去构建属性的实例。这种特性在Swift中，已经在语法层级得到了支持，这样对**处理一些占用内存较大，但又不是很常用的属性**时，能有效的降低内存使用率。下面是在Objective-C中实现延时属性的代码：

```objc
@interface DataManager : NSObject

@property (nonatomic, strong, readonly) NSData *data;

@end

@implementation DataManager

@synthesize data = _data;

- (NSData *)data {
    if (_data == nil) {
        _data = [NSData dataWithContentsOfFile:@"a/big/file"];
    }
    return _data;
}

@end
```

上面的写法虽然代码量不大，但毕竟会增加工作量，另外这种写法在多线程环境中并不严谨，如果再加上线程互斥的代码，整个延时属性的实现就会有更多的代码量和复杂度。所以，在Swift中，多线程并发的这种需求完全由编译器来保证，那么，我们实现这个延时属性，使用下面的代码即可：

```swift
class DataManager {
    lazy private(set) var data: NSData = NSData(contentsOfFile: "a/big/file")!
}
```

Bingo！！这样的特性太棒了，为我们省略了不少工作量啊！


### 属性监听

属性监听也是Swift中从语法层级支持的特性，当然，没有语法层级的支持，我们也可以手动撸出同样效果的代码，但总归是麻烦了点。首先我们要区别`KVO`和属性监听的区别，`KVO`是对其它对象属性的变化进行监听，而属性监听是对自身属性的变化进行监听。在语法层支持属性监听，使得我们可以将属性的存储逻辑和监听逻辑分离，这会使得我们代码结构更加清晰。

```swift
class StepCounter {

    var step: Int = 0 {
        willSet(newValue) {
            print("new value \(newValue)")
        }

        didSet {
            print("old value \(oldValue)")
        }
    }
}

var stepCounter = StepCounter()
stepCounter.step = 200
stepCounter.step = 201

// new value 200
// old value 0
// new value 201
// old value 200
```

在属性监听代码块里，我们可以获取到新设定的值和原始的值，而且这个块中的操作是线程安全的。所以，我们是可以在这个块里，**做一些与属性值相关的策略逻辑**，比如只在特定某些值下触发的逻辑。


### 下标

下标在Objective-C中是使用非正式协议的方式实现的，但对下标类型有所限制，而在Swift中对下标类型和数量是没有什么限制的，并且下标是支持**重载**的。这又给了我们创造的空间，比如用字符串或索引来取自定义配置文件中的值：

```swift
class ConfigurationFile {

    subscript(key: String) -> String {
        get {
            return "hello"
        }
    }

    subscript(index: Int) -> String {
        get {
            return "world"
        }
    }
}

let config = ConfigurationFile()

print(config["a key"] + " " + config[1])
```

下标主要是适用场景是**需要一种能通过索引快速取值**设计，它和属性非常相似，所以将其归类到了属性这一块。


## 运行时的安全

前面谈过了类型安全，这里再简单谈谈Swift在运行时是如何处理异常的。与很多传统的编程语言一样，Swift引入了`try - catch`机制，在Objective-C中也有`try`和`catch`，但与其它语言中的不同，Objective-C的异常处理在内存管理上存在泄露的风险。所以我们一直都在用`NSError`这样的错误处理模型，苹果提供的类库中也都采取了这样的处理方式，而在Swift中，这点得以改进，Swift中的`throws`函数必须用`try`去调用，所以很容易在ARC环境下生成`retain`和`release`代码，所以再也不用当心内存泄露的问题了。

在Swift中，所有可以抛出的异常，必须实现`ErrorType`协议，当然`NSError`实现了这样的协议。而如果我们要自定义异常，则必须使用枚举类型，原因很简单，因为枚举配合它的关联值特别适合做这样的事情：

```swift
enum LoginError: ErrorType {
    case InvalidUsername, InvalidPassword
    case Other(String)
}

func login(username: String, password: String) throws -> Int {
    if username != "makee" {
        throw LoginError.InvalidUsername
    } else if password != "sun" {
        throw LoginError.InvalidPassword
    } else {
        throw LoginError.Other("unknow error")
    }
}

do {
    try login("makee", password: "sun")
} catch LoginError.InvalidUsername {
    // ...
} catch LoginError.InvalidPassword {
    // ...
} catch LoginError.Other(let msg) {
    print(msg)
}
```

当然，如果确定没有任何异常，我们只要使用`try!`去调用即可，这样可以省略掉`do - cacth`这样的代码结构。

```swift
try! login("username", password: "password")
```

这时候需要注意，就和可选类型的强制解包一样，如果失败了是会导致Crash的。Swift引入了这样的异常处理机制，虽然在语法的角度上与其它语言大相径庭，但如果从ARC的角度去考虑，就会觉得，这不失为一种很好的妥协。比较Swift没有垃圾回收机制，所有的内存管理都是靠程序本身去处理，有了这样的异常处理机制，我们应该更少的使用`nil`，这样我们的程序会更加健壮。

与Java的异常处理类似，Swift中的异常处理也存在**冒泡机制**，也就是异常向上传递，这种特性使得我们的异常是可传递的，但为了内存管理考虑，还是必须要使用上`try`关键字。比如下面的代码：

```swift
func test() throws {
   try login("usr", password: "pwd")
}
```

这时候，调用`test`函数时，异常是从`login`冒上来的，这就是异常传递机制。通过这样的设定，我们不会忽略掉任何异常，配合**模式匹配**中所讲述的内容，我们的`catch`块可控性也是非常灵活的。唯一不足的是，并不能像Java那样知道到底会抛出何种异常，这点和`C#`中倒是有点类似，也只能靠文档来弥补了。


## 最后再说一点

作为单篇文章，本次所讲述的内容可能有点过多，因为我觉得Swift真的有太多比Objective-C强大的地方，也非常愿意作为Swift的传道士。碍于篇幅，这篇文章中还有很多Swift的小特性没有提及，比如表达式的`where`子句，枚举类型的`rawValue`等，这些就留给在座各位自己去摸索了。

本篇文章中，我想要达到的目的并不仅仅是让在座各位了解到Swift的特性，更希望能让大家明白在什么场景下去使用这样的特性。所以文章中花了很大的篇幅描述使用场景和我认为的设计初衷，语法细节都是很简单的略过了，因为语法是很容易从官方文档中找到说明的，而使用场景和相应的一些思想是很难从文档上找到的。

那么，本篇就到这里了，希望大家能够有所收获，**一同学习，成就更好的自己**！