---

---





### 闭包简介

OC有Block，C++有lambda表达式，swift则有闭包。



闭包采用如下三种形式之一：

- 全局函数是一个有名字但不会捕获任何值的闭包
- 嵌套函数是一个有名字并可以捕获其封闭函数域内值的闭包
- 闭包表达式是一个利用轻量级语法所写的可以捕获其上下文中变量或常量值的匿名闭包

### 闭包的语法

来定义一个闭包

```swift
let driving = {
  print("我要去开车。")
}
```

看起来一点都不像闭包，很怀疑到底是不是闭包，别着急，继续往下看

```swift
let driving = {() -> Void in
	print("我要去开车。")              
}
```

现在是不是看明白了，其实这两种方法是等价的。

```swift
{ (parameters) -> returntype in
    statements
}


//可以直接在花括号后面赋值
{
  (s1:Int,s2:Int) -> Int in
  return s1 + s2
}(1,2)

//$0表示拿到第一个参数
//¥1表示拿到第二个参数
{
  $0 + $1
}(1,2)
```

```swift
// 还有很多精巧的语法
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
// 单行表达式闭包可以通过省略 return 关键字来隐式返回单行表达式的结果
reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )
// 直接通过 $0，$1，$2 来顺序调用闭包的参数，以此类推。
reversedNames = names.sorted(by: { $0 > $1 } )
// Swift 的 String 类型定义了关于大于号（>）的字符串实现，其作为一个函数接受两个 String 类型的参数并返回 Bool 类型的值
reversedNames = names.sorted(by: >)
```

另外要注意的是闭包表达式是引用类型。	



### 在闭包中接收参数

举个例子，我们来创建一个闭包，接收一个叫 place 的字符串作为唯一的参数，就像这样：

```swift
let driving = { (place: String) in
  print("我要开车去\(place)。")
}
//这两种闭包的写法其实是等价的，看懂就好了，这种形式看到要懂。
let driving = {{place: String} -> Void in
	print("我要开车去\(place)。")              
}
```



函数和闭包的一个区别是运行闭包的时候你不会用到参数标签。因此，调用 `driving()` 的时候，我们是这样写的：

```swift
driving("北京")
```



### 从闭包中返回值

闭包也能返回值，写法和闭包的参数类似：写在闭包内部， `in` 关键字前面。

还是以 `driving()` 闭包为例， 让它返回一个字符串。原来的函数是这样的：

```swift
let driving = { (place: String) in
  print("我要开车去  \(place)。")
}
```

改成返回字符串而不是直接打印那个字符串，需要 `in` 之前添加 `-> String`，然后像常规函数那样用到 `return` 关键字:

```swift
let drivingWithReturn = { (place: String) -> String in
  return "我要开车去 \(place)。"
}
```

现在我们运行这个闭包并且打印出它的返回值：

```swift
let message = drivingWithReturn("北京")
print(message)
```

### 闭包作为参数

既然闭包可以像字符串和整数一样使用，你就可以将它们传入函数。闭包作为参数的语法乍一看一看挺伤脑筋的，让我们慢慢来。

首先，还是基本的 `driving()` 闭包。**以下两种闭包的写法在例子中是等价的,有助于“闭包作为参数的理解。”**

```swift
let driving = {
  print("我正在开车。")
}

或者写成这样

let driving = {() -> Void in
	print("我正在开车。")
}
```

如果我们打算把这个闭包传入一个函数，以便函数内部可以运行这个闭包。我们需要把函数的参数类型指定为 `() -> Void`。它的意思是“不接收参数，并且返回 `Void`”。在Swift中，`Void`是什么也没有的意思。

好了，让我们来写一个 `travel()` 函数，接收不同类型的 traveling 动作， 并且在动作前后分别打印信息：

```swift
func travel(action: () -> Void) {
  print("我准备出发了。")
  action()
  print("我到达目的地了。")
}
```

现在可以用上 `driving` 闭包了，就像这样：

```swift
travel(action: driving)
```

### 拖尾闭包（尾随闭包）

如果一个函数的最后一个参数是闭包，Swift允许你采用一种被称为 “*尾随闭包语法”* 的方式来**调用**这个闭包。你可以把闭包传入函数之后的花括号里，而不必像传入参数那样。

又用到我们的 `travel()` 函数了。它接收一个 `action` 闭包。闭包在两个 `print()` 调用之间执行:

```swift
func travel(action: () -> Void) {
  print("我准备出发了。")
  action()
  print("我到达目的地了。")
}
```





由于函数的最后一个参数是闭包，我们可以用拖尾闭包语法来调用 `travel()` 函数，就像这样：

```swift
//一下两种写法完全一样，两种方法都要能看懂。
travel() {
  print("我正在开车。")
}


travel(){
    () -> Void in
    print("我要开车去了。")
}
```





实际上，由于函数没有别的参数了，我们还可以将圆括号完全移除：

```swift
travel {
  print("我正在开车。")
}
```

**尾随闭包语法在Swift中非常常见，所以你要适应它。**

### 使用接收参数的闭包作为函数的参数

接下来要说到的闭包用法会有点复杂: 当你把闭包作为函数参数时，闭包本身也接收参数。

前面我们用 `() -> Void` 来表示“不接收参数，并且什么也不返回“，但实际上你可以在 `()` 里填上你任何想要闭包接收的参数类型。

再次用到 `travel()` 函数。函数只接收一个闭包作为参数，但这次闭包会接收一个字符串参数：

```swift
func travel(action: (String) -> Void) {
  print("我准备出发了。")
  //action调用填了参数值得好好体会。好好理解参数是从这里传入的，并不是调用的时候传入参数。
  action("北京")
  print("我到达目的地了。")
}
```

现在，当我们采用拖尾闭包语法调用 `travel()` 时，我们的闭包代码会要求接收一个字符串：

```swift
travel { (place: String) in
  print("我准备开车去\(place)。")
}
```

### 使用有返回值的闭包作为函数的参数

我们之前用 `() -> Void` 来表示“不接收参数，并且什么也不返回”。你可以把 `Void` 替换成任意的类型从而让闭包可以返回值。

还是 `travel()` 函数，这次闭包会返回一个字符串。

```swift
func travel(action: (String) -> String) {
  print("我准备出发了。")
  let description = action("北京")
  print(description)
  print("我到达目的地了")
}
```

仍然用拖尾闭包语法来调用 `travel()`，闭包要求接收一个字符串并且返回一个字符串：

```swift
//方法一调用
travel { (place: String) -> String in
  return "我要开车去\(place)。"
}

//方法二调用
travel(){(place:String) -> String in return("我准备开车去\(place)。")}

//方法三调用
travel{(place:String) -> String in return("我准备开车去\(place)。")}
```



### 速记参数名

前面我们了构建 `travel()` 函数。它接收一个闭包作为参数，这个闭包本身接收一个参数并且返回一个字符串，它在两个 `print()` 调用之间运行。

代码如下：

```swift
func travel(action: (String) -> String) {
  print("我准备出发了。")
  let description = action("北京")
  print(description)
  print("我到达目的地了。")
}
```

我们可以像这样调用 `travel()`：

```swift
travel { (place: String) -> String in
  return "我要开车去\(place)。"
}
```

不过，Swift知道提供给闭包的参数必须是一个字符串，所以调用的代码可以简写成这样：

```swift
travel { place -> String in
  return "我要开车去\(place)。"
}
```

Swfit也知道闭包必须返回一个字符串，于是进一步简写：

```swift
travel { place in
  return "我要开车去\(place)。"
}
```

由于这里的闭包只有一行代码，这行代码肯定是返回值的那行代码，因此Swift允许我们把 `return` 关键字也移除：

```swift
travel { place in
  "我要开车去\(place)。"
}
```

最后，Swift还提供一种速记语法，让你可以把代码变得更短。我们可以让Swift为闭包的参数自动提供一个名字，而不必自行写下 `place in`。这些自动生成的名字以$开头，然后跟着一个从0开始的整数，就像下面这样：

```swift
//还有困惑参数是从哪里传如的吗？参数都是从我们定义的函数中传入，并不是调用的时候传入参数。
travel {
  "我要开车去\($0)。"
}
```

### 有多个参数的闭包

让我们把闭包这个概念一次讲透吧。接下来举一个接收两个参数的闭包的例子。

将 `travel()` 函数改造一下，不仅接收旅行目的地，也接收速度。闭包的类型会变成 `(String, Int) -> String`：

```swift
func travel(action: (String, Int) -> String) {
  print("我准备出发了。)
  let description = action("北京", 60)
  print(description)
  print("我到达目的地了。")
}
```

再一次用速记闭包参数名来调用函数。由于这次闭包有两个参数了，于是自动参数名分别是 `$0` 和 `$1`：

```swift
travel {
  "我要开车去\($0)，时速\($1)公里每小时。"
}
```

有些人可能不喜欢用速记参数名，因为它们的语义不是很清晰。你可以根据自己的喜好来决定是否采用它们。不过了解一下这个语法还是必要的，这样读到别人的代码时就不会感到困惑。

### 从函数中返回闭包

就如同你可以把闭包传入函数那样，你也可以从函数中返回闭包。

返回闭包的语法看起来有点绕，因为用了两次 `->`：第一次用于指定函数的返回值，第二次用于指定闭包的返回值。

又又又要把 `travel()` 函数拉出来了。这次它不接收参数，但返回一个闭包。这个返回的闭包在用的时候必须传入一个字符串，但闭包本身没有返回值。

Swift代码长这样：

```swift
//(String) -> Void是闭包类型,这样看应该就理解代码了
func travel() -> (String) -> Void {
  return {
    print("我要动身去\($0)")
  }
}
```

**接下来我们通过调用 `travel()` 拿到闭包，然后作为函数来调用：**

```swift
let result = travel()
result("北京")
```

**留意下面的代码，它是直接调用 `travel()` 的返回值。这个写法虽然在语法上完全没问题，但是可读性较差，建议尽量不要这样写。**

```swift
let result2 = travel()("北京")
```

### 捕获变量

如果你想要使用闭包之外的对象，Swift会为你“捕捉”它们，并把它们和闭包一同存储，以便外部作用域已经失效的情况下闭包内部还可以使用它们。

最后一次用到 `travel()` 函数，它返回一个闭包，这个闭包接收字符串作为唯一的参数并且什么也不返回：

```swift
func travel() -> (String) -> Void {
  return {
    print("我准备去\($0)")
  }
}
```

调用 `travel()` 拿到闭包，然后自由使用：

```swift
let result = travel()
result("北京")
```

闭包捕获变量可以发生在什么情况下呢？举个例子，当 `travel()` 函数内创建了一个变量，这个变量需要在闭包里面用到，那么这个变量就会被闭包捕获。比如，我们想知道闭包被调用的次数：

```swift
func travel() -> (String) -> Void {
  var counter = 1
  return {
    print("第\(counter)次，我将前往\($0)")
    counter += 1
  }
}
```

尽管 `counter` 变量是在 `travel()` 里被创建的，它被闭包捕获，因而会在闭包内部存续。

当我们多次调用 `result("北京")`，计数器会持续增加：

```swift
result("北京")
result("北京")
result("北京")
```

### 可选绑定

使用可选绑定（optional binding）来判断可选类型是否包含值(是否为nil)，如果包含(不为nil)就把值赋给一个临时常量或者变量。

```swift
import UIKit

var myString:String?

myString = "Hello, Swift!"

if let yourString = myString {
   print("你的字符串值为 - \(yourString)")
}else{
   print("你的字符串没有值")
}
```

### 总结

* 你可以把闭包赋值给变量，之后再用变量名来调用闭包。

* 闭包和常规函数一样可以接收参数和返回值。

* 你可以将闭包作为参数传入函数，并且这些闭包也可以有自己的参数和返回值。

* 如果函数的最后一个参数是闭包，你可以使用拖尾闭包语法。

* Swift为拖尾闭包语法自动生成了 `$0` 和 `$1` 这样的速记闭包参数名，但不是所有人都习惯这种速记法。

* 如果你在闭包中使用了外部变量，这些变量将被闭包“捕捉”以便后续引用。

