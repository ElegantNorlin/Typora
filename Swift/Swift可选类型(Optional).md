---
title: Swift可选类型(Optional)
date: 2021-4-28 16:05:00
tags: Swift
toc: true
description: "其实我一开始对Optional也不是很理解，看了网上的介绍还是感觉云里雾里的感觉，直到有一天穿暖花开、万物复苏，仿佛我的脑海也注入了新的生机，不扯了。。。。。。"
---







其实我一开始对Optional也不是很理解，看了网上的介绍还是感觉云里雾里的感觉，直到有一天穿暖花开、万物复苏，仿佛我的脑海也注入了新的生机，不扯了。。。。。。

想学会怎么用Optional其实还是理解为什么要有Optional这个东西吧

有时候我们要定一个变量，但是又不确定现在这个变量的值是什么，又不能随便给变量赋一个值，要是有一个变量能先赋值为nil，需要赋值的时候再赋值那该多好！苹果的开发者也考虑的很周到，所以就有了Optional类型。

Optional的用法：

* 如果可选类型变量的值为nil，打印的时候仍为nil。
* 如果可选类型变量的值不为nil，那么则打印该值。

### 语法

* 如何创建一个可选变量（两种方式）

  ```swift
  //方法一：(推荐使用)
  var score:Int？ = nil
  
  //方法二：
  var score:Int! = nil
  ```

* 如何拿到可选变量的值(建议使用前两种方式解包，第三种方式比较老)

  ```swift
  var score:Int？ = nil
  
  var defaultscore = 10
  //如果score为nil，则输出defaultscore
  //如果score不为nil，则输出本身的值
  print(score ?? defaultscore)
  ```

  ```swift
  var score:Int? = nil
  //如果score为nil，则跳过输出
  //如果score不为nil，则输出本身的值
  if let score = score{
    print(score)
  }
  ```

  ```swift
  var score:Int？ = nil
  
  if score != nil{
    print(score!)
  }else{
    print("score值为nil")
  }
  ```


### 处理缺失的数据

我们已经会使用 `Int` 这样的类型来存储像 5 这样的数值。不过，当你想要存储用户年龄这样的属性，并且你还不知道该用户的年龄时你该怎么做呢？

你可能会说，“我可以暂时存成0”，但这样一来你就会混淆新生儿和你不知道年龄的用户。你应该用一个特殊的数字，比如 1000 或者 -1 来代表 “未知”，这两个数字都不可能是年龄，但你能记得住这些特殊数字的含义吗？

Swift的解决方案称为 *optional*，即可选型。你可以基于任意类型创建可选型。一个可选型整数可以有诸如1或者1000这样的数字，也可能没有任何值，即值可以缺失，Swift里表示缺失是用 `nil`。

为了把一个类型变成可选型，只需要在类型后面加一个问号。举个例子，我们可以这样把一个整数变成可选型：

```
var age: Int? = nil
```

这个可选型现在并不保有任何整数，但是如果之后我们知道了年龄值，我们可以给它重新赋值。

```
age = 38
```

### 解包可选型

可选型字符串可能包含一个 “Hello” 或者 nil。

考虑这样一个可选字符串：

```
var name: String? = nil
```

当我们调用 `name.count` 时会发生什么呢？一个真的字符串有 `count` 属性，它存储字符串的字符数量。但它是 `nil` 时，并没有 `count` 属性。

因此，试图直接使用 `name.count` 是不安全的，Swift也不允许。取而代之的是，我们必须先检查可选型里面有些什么，这个过程被称为 *解包* 。

解包可选型的一种常见做法是采用 `if let` 语法，它是一种有条件的解包。如果可选型里有值，你就可以使用它，如果没有则条件失败。

举个例子：

```
if let unwrapped = name {
  print("\(unwrapped.count) letters")
} else {
  print("Missing name.")
}
```

如果 `name` 持有一个字符串，这个字符串会被放进一个叫 `unwrapped` 的常规字符串，然后我们就可以读取它的 `count` 属性。另一种情况，如果 `name` 是 nil， 那么 `else` 中的代码将被执行。

### 用guard解包可选型

`if let` 的一个替代方案是 `guard let`，后者也可以用来解包可选型。`guard let` 会为你解包，但是当它发现可选型里是 `nil` 时会期望你退出它所处的函数，循环或者条件。

因此，`if let` 和 `guard let` 的主要区别在于 `guard let` 之后可选型还可以继续使用。

让我们尝试一下 `greet()` 函数。它将接收一个可选字符串作为唯一的参数，当它解包发现这个参数是nil时会打印消息并且退出函数。因为可选型 unwrapped 在 `guard let` 的语句块结束之后作用，我们可以在函数最后打印这个解包后的字符串。

```
func greet(_ name: String?) {
  guard let unwrapped = name else {
    print("You didn't provide a name!")
    return
  }
  print("Hello, \(unwrapped)!")
}
```

通过使用 `guard let`，你可以在函数的前面解决可选型的解包问题，在这之后的代码就可以愉快地使用解包后的值而不必担心出错了。

### 强制解包

可选型代表可能存在也可能不存在的数据，而某些时候你是 *确切* 知道一个可选型的值并不是nil。在这种情况下，Swift允许你强制解包这个可选型：通过把一个可选型转换为非可选型的方式。

举个例子，下面你试图将一个字符串转换成 `Int`，就像这样：

```
let str = "5"
let num = Int(str)
```

上面的代码中 `num` 其实是一个可选型 `Int`。因为你有可能尝试转换一个不是数字的字符串导致转换失败。

尽管Swift不确定转换能否成功，你自己是知道你的结果是否可以安全地执行强制解包。这个强制解包是通过在`Int(str)` 之后添加 `!`来完成的。

```
let num = Int(str)!
```

通过强制解包，Swift将立刻解包 `num`，把它变成一个常规的 `Int` 而不是 `Int?`。但是假如你是错的，比方说 `str` 是某个不能转换成数字的字符串，那么你的代码将崩溃。

强制解包的操作符常常被戏称为崩溃操作符。因此，只有当你确信强制解包是安全的你才能这么做。

### 隐式解包可选型

像常规可选型一样，隐式解包可选型可能包含一个值也可能包含 `nil`。不过，跟常规可选型不一样的是，隐式解包可选型时不需要再解包，你可以把它们当做非可选型一样使用。

隐式解包可选型是通过类型名之后添加感叹号来声明的，就像这样：

```
let age: Int! = nil
```

由于它们表现起来就像已经被解包过的样子，你并不需要 `if let` 或者 `guard let` 这样的代码来测试它们。不过，当你试图使用它们而它们实际上没有值时，即它们的值是 `nil`时，你的代码将崩溃。

隐式解包可选型存在的意义在于，有些情况某个变量一开始时是nil，但当你需要用到它之前它总会有值。如果你可以确信这一点，那么采用隐式解包可选型可以省去一直写 `if let` 的麻烦。

值得一提的是，如果条件允许采用常规可选型，安全起见最好总是采用常规可选型。

### 空合运算符

空合运算符解包一个可选型，如果可选型包含值则返回这个值，如果可选型不包含值，即可选型的值是 `nil`，那么返回某个默认值。两种情况，返回值都不再是可选型：它要么是可选型里包含的有效值，要么是一个备选的默认值。

下面是一个接收整数作为唯一参数并且返回可选型字符串的函数：

```
func username(for id: Int) -> String? {
  if id == 1 {
    return "Taylor Swift"
  } else {
    return nil
  }
}
```

如果我们以ID 15来调用这个函数，我们将得到 `nil`。通过空合运算符，我们可以提供一个叫 “Anonymous” 的默认值，就像这样：

```
let user = username(for: 15) ?? "Anonymous"
```

它将检查 `username()` 函数返回的值：如果是一个字符串，它将被解包并放入 `user`，如果是 `nil`，则使用 “Anonymous” 替代。

### 可选链

在使用可选型时，Swift为我们提供了一种快捷方式：假如你试图访问形如 `a.b.c` 这样的代码并且 `b` 是可选型，你可以在 `b` 后面写一个问号来启用 *可选链*： `a.b?.c`。

当代码运行时，Swift会检查 `b` 是否有值，如果它是 `nil`，那么这行代码剩下的部分将被忽略。Swift会立即返回 `nil`。但是如果 `b` 有值，它将被解包，代码执行将继续。

下面是一个名字的数组：

```
let names = ["John", "Paul", "George", "Ringo"]
```

我们将使用数组的 `first` 属性，如果数组里第一个名字有值的话返回这个名字，如果数组为空则返回 `nil`。在结果之上，我们调用 `uppercased()` 把它变成一个全大写的字符串：

```
let beatle = names.first?.uppercased()
```

上面的问号就是可选链。如果 `first` 返回 `nil`，那么Swift就不会尝试将它转换成全大写，它会将 `beatle` 立即设置为 `nil`。

### 可选型try

让我们回忆一下可能抛出错误的函数那一节的知识，看下面的代码：

```
enum PasswordError: Error {
  case obvious
}

func checkPassword(_ password: String) throws -> Bool {
  if password == "password" {
    throw PasswordError.obvious
  }

  return true
}

do {
  try checkPassword("password")
  print("That password is good!")
} catch {
  print("You can't use that password.")
}
```

通过 `do`， `try` 和 `catch`，我们得以运行可能抛出错误的函数，并且优雅地处理错误。

上面的 `try` 写法其实有另外两种选择，这两种选项都能加深你对可选型和强制解包的理解。

第一个是 `try?`，它将可能抛出错误的函数转换成返回可选型的函数。如果函数抛出错误，那你就会得到 `nil` 作为函数的执行结果，否则你会得到将返回值包装之后的可选型。

尝试使用 `try?` 来执行 `checkPassword()`，像下面这样：

```
if let result = try? checkPassword("password") {
  print("Result was \(result)")
} else {
  print("D'oh.")
}
```

另外一种选择是 `try!`，当你确信函数一定不会失败时你可以采用它。如果函数实际抛出了错误，你的代码将崩溃。

使用 `try!` 来重写前面的代码：

```
try! checkPassword("sekrit")
print("OK!")
```

### 失败构造器

当我们说到强制解包的时候，我用了下面的代码：

```
let str = "5"
let num = Int(str)
```

它将一个字符串转换成一个整数，但由于你可以传入任意字符串，你得到的实际上是一个可选型整数。

这里用到一种叫做 *失败构造器* 的东西：它是一种可能成功也可能失败的构造器。你在结构体或者类里面用 `init?()` 来实现失败构造器。如果某些东西出错，它将返回 `nil`。因此这种构造器返回的是某种类型的可选型，你用之前需要解包。

举个例子，我们现在要求 `Person` 结构体必须通过一个9字符的ID字符串来构造。只要不是9个字符，都会返回 `nil`。

Swift代码如下：

```
struct Person {
  var id: String

  init?(id: String) {
    if id.count == 9 {
      self.id = id
    } else {
      return nil
    }
  }
}
```

### 类型转换

Swift知道每个变量的类型，但有的时候你知道的信息比Swift更多。举个例子，这里有三个类：

```
class Animal { }
class Fish: Animal { }
class Dog: Animal {
  func makeNoise() {
    print("Woof!")
  }
}
```

我们创建几个Fish和几个Dog，然后把它们放进一个数组，像下面这样：

```
let pets = [Fish(), Dog(), Fish(), Dog()]
```

Swift 知道 `Fish` 和 `Dog` 都继承自 `Animal` 类，因此它通过类型推断将 `pets` 创建为一个 `Animal` 类型的数组。

如果我们想遍历 `pets` 数组，让所有的狗发出叫声，我们需要执行一次类型转换：Swift将检查每个pet是否 `Dog` 对象，以便我们调用 `makeNoise()` 方法。

这里用到了一个关键字 `as?`，它将返回一个可选型：类型转换失败时返回 `nil`，成功则返回转换后的类型。

Swift代码如下：

```
for pet in pets {
  if let dog = pet as? Dog {
    dog.makeNoise()
  }
}
```

### 总结

让我们来总结一下。

1. 可选型让我们可以用一种清晰无歧义的方式表示值缺失的情况。
2. Swift不允许不经解包就使用可选型，解包可以用 `if let` 或者 `guard let`。
3. 你可以用感叹号强制解包可选型，不过如果解出来的是 `nil` ，你的代码将会崩溃。
4. 隐式解包可选型没有做常规可选型的安全性检查。
5. 你可以使用空合运算符解包一个可选型，以便可选型里没有值时提供一个默认值。
6. 可选链用于操作可选型，如果可选型的值是空的，后续代码将被忽略。
7. 你可以使用 `try?` 将一个可能抛出错误的函数转换成一个可选型的返回值，或者使用 `try!`在抛出错误时崩溃。
8. 如果你需要在输入不合理时让构造过程失败，可以使用 `init?()` 来创建失败构造器。
9. 你可以使用类型转换将一个类型转换为另一个类型。