---
title: Swift结构体
date: 2021-4-28 16:05:00
tags: Swift
toc: true
description: "Swift允许你用两种方式创建自己的类型。其中一种最常见的叫做结构体，即 *struct*。Struct可以拥有自己的变量、常量以及函数，而你可以在任意时候创建和使用它们。"
---









### 创建你自己的结构体

Swift允许你用两种方式创建自己的类型。其中一种最常见的叫做结构体，即 *struct*。Struct可以拥有自己的变量、常量以及函数，而你可以在任意时候创建和使用它们。

让我们以一个简单的例子开始：创建一个 `Sport` 结构体，它有一个叫 `name` 的字符串变量。在结构体中，这种变量被称为 *属性*。因此，这是一个拥有一个属性的结构体。

```swift
struct Sport {
  var name: String
}
```

类型定义完成，现在让我们来创建和使用它的实例：

```swift
var tennis = Sport(name: "Tennis")
print(tennis.name)
```

`name` 和 `tennis` 都是变量，因而我们可以像常规变量那样修改它们：

```swift
tennis.name = "Lawn tennis"
```

属性可以像常规变量那样拥有默认值，并且依赖Swift的类型推断。

### 计算属性

我们刚刚创建了 `Sport` 结构体：

```swift
struct Sport {
  var name: String
}
```

它有一个叫 *name* 的属性，存储 `String` 类型。这种属性叫做 *存储* 属性，因为Swift还有另外一种属性，它叫 *计算* 属性。这是一种通过运行代码来获得值的属性。

让我们为 `Sport` 结构体再增加一个存储属性，然后是一个计算属性。代码如下：

```swift
struct Sport {
  var name: String
  var isOlympicSport: Bool

  var olympicStatus: String {
    if isOlympicSport {
      return "\(name)是一项奥林匹克运动。"
    } else {
      return "\(name)不是一项奥林匹克运动。"
    }
  }
}
```

如你所见，`olympicStatus` 看起来像一个常规的 `String`，但它其实是依据其他的属性返回不同的值。

让我们来创建一个 `Sport` 实例：

```swift
let chessBoxing = Sport(name: "Chessboxing", isOlympicSport: false)
print(chessBoxing.olympicStatus)
```

### 属性观察者

属性观察者允许我们可以在属性变化前后运行代码。让我们来写一个名叫 `Progress` 的结构体，这个结构体追踪一个任务以及它完成的百分比：

```swift
struct Progress {
  var task: String
  var amount: Int
}
```

现在，创建这个结构体的实例，随着时间的推移调整它的进度：

```swift
var progress = Progress(task: "加载数据", amount: 0)
progress.amount = 30
progress.amount = 80
progress.amount = 100
```

我们期望Swift在每一次 `amount` 改变的时候都打印信息，这里可以用到一个叫 `didSet` 属性观察者。它可以用于每一次 `amount` 改变之后运行代码：

```swift
struct Progress {
  var task: String
  var amount: Int {
    didSet {
      print("\(task) 已完成 \(amount)%。")
    }
  }
}
```

你还可以用到叫 `willSet` 的属性观察者。它是在属性改变之前作用，相对来说更不常用。

### 方法

Struct的内部可以拥有函数，它们在必要时可以使用结构体的属性。这种函数被称为 *方法*，关键字也是 `func`。

现在我们通过一个叫 `City` 的结构体来演示。它有一个 `population` 属性，用于存储城市里的人口。此外，它还有一个 `collectTaxes()` 方法，这个方法返回人口数乘以1000。由于方法是属于 `City` 的，它可以读取当前城市的 `population` 属性。

代码如下：

```swift
struct City {
  var population: Int

  func collectTaxes() -> Int {
    return population * 1000
  }
}
```

基于结构体调用方法的代码如下：

```swift
let xiamen = City(population: 4_110_000)
xiamen.collectTaxes()
```

### 可变方法

如果一个结构体拥有一个变量属性，但是这个结构体的实例是以常量的方式创建的，那么在实例中，这个变量属性是不能修改的。这是因为结构体本身已经是常量了，所以它的所有属性也是常量。

这里面有一个问题，Swift无从得知你将以常量还是变量的方式使用结构体。所以安全起见，Swift的默认策略是：不允许你在方法里修改属性，除非你显式地要求这一点。

当你想要改变属性值时,，需要在方法前使用 `mutating` 关键字，就像这样：

```swift
struct Person {
  var name: String

  mutating func makeAnonymous() {
    name = "Anonymous"
  }
}
```

由于这个方法改变了属性值，所以Swift只会允许这个方法在变量型的 `Person` 实例上调用。

```swift
var person = Person(name: "Ed")
person.makeAnonymous()
```

### String的属性和方法

目前为止我们已经大量地使用了字符串。你发现了吗？其实 `String` 类型是一个结构体类型。它有许多属性和方法，用于查询和维护字符串本身。

首先，我们创建一个测试字符串：

```swift
let string = "Do or do not, there is no try."
```

你可以用 `count` 属性来读取这个字符串里的字符数量：

```swift
print(string.count)
```

字符串有一个 `hasPrefix()` 方法，可以用来检测字符串是否以特定字符开头：

```swift
print(string.hasPrefix("Do"))
```

你还可以用 `uppercased()` 方法把一个字符串转换成全大写的版本：

```swift
print(string.uppercased())
```

你甚至可以让Swift将字符串中的字母重新排序成一个数组：

```swift
print(string.sorted())
```

String类型有大量的属性和方法。你可以利用 Xcode 的代码补全，用 `string.` 调取这些选项看一看它们都有些什么能力。

### 数组的属性和方法

数组同样也是结构体，这意味着它们也有可以用来查询和操作数组的属性和方法。

从一个简单的数组开始：

```swift
var toys = ["Woody"]
```

你可以用 `count` 属性来读取数组的元素个数：

```swift
print(toys.count)
```

如果你需要增加一个元素，可以使用 `append()` 方法，就像这样：

```swift
toys.append("Buzz")
```

你可以用 `firstIndex()` 方法来定位元素在数组里的位置，就像这样：

```swift
toys.firstIndex(of: "Buzz")
```

上面的代码会返回1，因为数组位置从0开始计数。

跟字符串一样，你可以让Swift以字母表顺序给数组的元素重新排序。

```swift
print(toys.sorted())
```

最后，如果你想要移除数组里的一个元素，可以使用 `remove()` 方法，就像这样：

```swift
toys.remove(at: 0)
```

数组类型也有大量的属性和方法。你可以利用 Xcode 的代码补全，用 `toys.` 调取这些选项看一看它们都有些什么能力。

### 构造器

构造器是一种可以用来支持不同方式创建结构体的特殊方法。所有的结构体都有一个默认的构造器，这个构造器被称为 *逐一成员构造器*，它要求你为结构体的每一个属性都提供一个值。

让我们来声明一个 `User` 结构体，它有一个属性：

```swift
struct User {
  var username: String
}
```

当我们创建这个结构体的实例时，我们需要提供一个username：

```swift
var user = User(username: "Paul")
```

当然，我们也可以创建自己的构造器用以替换默认的。举个例子，我们可能希望所有的新用户默认都叫 “Anonymous”，并且会打印一条信息，就像这样：

```swift
struct User {
  var username: String

  init() {
    username = "Anonymous"
    print("创建新用户！")
  }
}
```

你并不需要在构造器前面写 `func` 关键字，但你必须确保构造器结束前所有的属性都被赋值。

现在让我们来用上这个没有参数的构造器，就像这样：

```swift
var user = User()
user.username = "Paul"
```

### 引用当前实例

方法的内部，有一个特殊的常量叫 `self`，它指向当前正在使用的结构体实例。当你在构造器中遇到参数名和属性名相同的情况时，这个 `self` 会很有用。

举个例子，如果你声明一个 `Person` 的结构体，它有一个 `name` 属性，并且你尝试写一个接收名为 `name` 参数的构造器，那么 `self` 可以帮助你区分属性和参数。`self.name` 指代属性，而 `name` 指代参数。

代码如下：

```swift
struct Person {
  var name: String

  init(name: String) {
    print("\(name) was born!")
    self.name = name
  }
}
```

### 懒加载属性

作为一种性能优化手段，Swift允许你在用到的时候才真正创建属性。举个例子，这里有一个叫 `FamilyTree` 的结构体，它做的工作很容易描述。但理论上为一个人创建族谱可能会是一个很耗时的过程：

```swift
struct FamilyTree {
  init() {
    print("创建族谱！")
  }
}
```

我们可以在 `Person` 结构体内将 `FamilyTree` 作为一个属性来使用，就像这样：

```swift
struct Person {
  var name: String
  var familyTree = FamilyTree()

  init(name: String) {
    self.name = name
  }
}

var ed = Person(name: "Ed")
```

假如某些情况我们并不需要用到某个人的族谱信息呢？我们可以在 `familyTree` 属性前添加 `lazy` 关键字，Swift将会在 `familyTree` 属性第一次被访问的时候才执行创建代码：

```swift
lazy var familyTree = FamilyTree()
```

因此，如果你想要看到 “创建族谱！” 这条信息被打印，你至少需要访问 `familyTree` 属性一次：

```swift
ed.familyTree
```

### 静态属性和方法

目前为止我们认识的所有属性和方法都是属于独立的结构体实例，这意味着假如我们有一个叫 `Student` 的结构体，我们可以创建几个student实例，每个实例都有各自的属性和方法：

```swift
struct Student {
  var name: String

  init(name: String) {
    self.name = name
  }
}

let ed = Student(name: "Ed")
let taylor = Student(name: "Taylor")
```

你可以要求Swift在不同的结构体实例之间共享属性和方法，这些属性和方法被称为静态属性和静态方法，实现的方式是添加 *static* 声明。

现在，让我们给 `Student` 结构体添加一个静态属性，用以存放班级学生的总数。每当我们创建一个新的 student 实例时，我们将这个属性的数值加1：

```swift
struct Student {
  static var classSize = 0
  var name: String

  init(name: String) {
    self.name = name
    Student.classSize += 1
  }
}
```

`classSize` 属性是属于结构体本身而非结构体的实例，因此我们需要用 `Student.classSize`来访问它：

```swift
print(Student.classSize)
```

### 访问控制

访问控制使得我们可以限制哪些代码能够访问属性和方法。这个机制在你想要保护属性免于被直接读取的时候很有用，举个例子：

我们仍然创建一个 `Person` 结构体，它有一个 `id` 属性，用来存放社保ID：

```swift
struct Person {
  var id: String

  init(id: String) {
    self.id = id
  }
}

let ed = Person(id: "12345")
```

一旦person实例被创建，我们希望它的 `id` 是私有的。私有的意思是你不能从结构体外部读取它，像 `ed.id` 这样的代码会变得不合法。

要做到这一点，你需要用到 `private` 关键字，像这样：

```swift
struct Person {
  private var id: String

  init(id: String) {
    self.id = id
  }
}
```

这么写之后，只有 `Person` 内部的方法才能读取 `id` 属性。举个例子：

```swift
struct Person {
  private var id: String

  init(id: String) {
    self.id = id
  }

  func identify() -> String {
    return "我的社保ID是 \(id)"
  }
}
```

还有一个常见的选项是 `public`，它使得所有的其他代码都能够访问到目标属性或者方法。

### 总结

让我们一起来总结一下。

1. 你可以创建自己的结构体类型，它们有自己的属性和方法。
2. 你学习了存储属性，以及每一次通过临时计算得到值的计算属性。
3. 当你希望在方法中修改属性时，需要把方法标记成`mutating`。
4. 构造器是创建结构体时的特殊方法。默认情况下，你会得到一个逐一成员构造器。不过，如果你想要自行实现构造器的话，需要确保所有的属性都被赋值。
5. 你可以在方法中用 `self` 常量来引用当前正在使用的结构体实例。
6. `lazy` 关键字告诉Swift你希望属性在第一次访问时才被创建。
7. 可以利用`static`关键字在结构体的所有实例间共享属性和方法。
8. 访问控制使得我们可以限制哪些代码能够访问属性和方法。

