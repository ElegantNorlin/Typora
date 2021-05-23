---

---











### 协议

协议是一种描述某个类型必须有某些属性和方法的方式。你告知Swift某个类型将使用某个协议，这个过程称为协议适配或者协议遵循。

举个例子，我们可以写一个函数接收 `id` 属性，但我们并不精确地关心用的是哪一种数据类型。让我们从 `Identifiable` 协议开始，这个协议要求所有遵循协议的类型必须有一个 `id` 字符串属性，并且这个字符串可读写。

```swift
protocol Identifiable {
  var id: String { get set }
}sw
```

我们无法创建协议的实例，因为协议只是一种描述，它本身并非一种类型。但是我们可以创建遵循它的结构体。

```swift
struct User: Identifiable {
  var id: String
}
```

最后，我们还可以写一个 `displayID()` 函数，它接收 `Identifiable` 对象：

```swift
func displayID(thing: Identifiable) {
  print("My ID is \(thing.id)")
}
```

### 协议继承

一个协议可以继承另一个协议，这个过程称为协议继承。跟类不一样的是，你可以同一时间继承多个协议。

接下来我们将定义三个协议： `Payable` 要求遵循它的类型实现 `calculateWages()` 方法，`NeedsTraining` 要求遵循它的类型实现 `study()` 方法，而 `HasVacation` 要求遵循它的类型实现 `takeVacation()` 方法：

```swift
protocol Payable {
  func calculateWages() -> Int
}

protocol NeedsTraining {
  func study()
}

protocol HasVacation {
  func takeVacation(days: Int)
}
```

现在我们可以创建一个 `Employee` 协议，将上面的三个协议合并在一起。我们还不打算添加额外的东西，因此简单地写一对花括号就行：

```swift
protocol Employee: Payable, NeedsTraining, HasVacation { }
```

现在我们就可以创建遵循这个单一协议的类型，而不是分别遵循三个协议的类型。

### 扩展

扩展使得你可以为已经存在的类型添加方法，实现它们设计时没有做的事情。

举个例子，我们可以为 `Int` 类型添加一个扩展方法 `squared()`，用来返回当前数的平方。

```swift
extension Int {
  func squared() -> Int {
    return self * self
  }
}
```

尝试一下。创建一个整数，你会发现现在这个整数有了 `squared()` 方法：

```swift
let number = 8
number.squared()
```

Swift不允许你通过扩展添加存储属性，但可以用扩展添加计算属性。举个例子，我们给整数添加一个 `isEven` 的计算属性，这个属性返回当前数值是否为偶数：

```swift
extension Int {
  var isEven: Bool {
    return self % 2 == 0
  }
}
```

### 协议扩展

协议可以描述某个类型应当有某种方法，但并没有提供方法的代码。扩展实现有具体代码的方法，但一次只能作用于一个数据类型，你没办法同时给多个类型添加相同的代码。

协议扩展同时解决了这两个问题：它们就像常规扩展一样，差异只在于你并不是只扩展一个特定的类型，比如 `Int`，你扩展是的一个协议，因而所有遵循这个协议的类型都会发生改变。

举个例子，下面有一个包含了一些名字的数组和一个同样包含了一些名字的集合：

```swift
let pythons = ["Eric", "Graham", "John", "Michael", "Terry", "Terry"]
let beatles = Set(["John", "Paul", "George", "Ringo"])
```

Swift的数组和集合都遵循一个叫 `Collection` 的协议，因此我们可以给 `Collection` 协议扩展一个叫 `summarize()` 的方法，这个方法逐一打印collection里的元素。

```swift
extension Collection {
  func summarize() {
    print("There are \(count) of us:")
    for name in self {
      print(name)
    }
  }
}
```

`Array` 和 `Set` 都将获得这个方法。让我们来尝试一下：

```swift
pythons.summarize()
beatles.summarize()
```

### 面向协议编程

协议扩展可以为我们自己的协议方法提供默认实现。这使得类型遵循协议变得更加容易，并且允许我们“面向协议编程”——这是一种利用协议和协议扩展来加工代码的方式。

首先，我们这里有一个叫 `Identifiable` 的协议，它要求所有遵循协议的类型都有一个叫 `id`的属性和叫一个叫 `identify()` 的方法：

```swift
protocol Identifiable {
  var id: String { get set }
  func identify()
}
```

虽然我们可以让每个遵循这个协议的类型书写它们自己的 `identify()` 方法，但协议扩展允许我们可以提供一个默认实现：

```swift
extension Identifiable {
  func identify() {
    print("My ID is \(id).")
  }
}
```

现在当我们再声明一个遵循 `Identifiable` 协议的类型时，它会自动获得 `identify()` 方法的实现：

```swift
struct User: Identifiable {
  var id: String
}

let paul = User(id: "Paul")
paul.identify()
```

### 总结

让我们来总结一下。

1. 协议描述了一个遵循它的类型应该拥有的属性和方法，但并不提供那些方法的实现。
2. 你可以基于协议创建协议，这一点跟类相似。
3. 扩展允许你为类型添加方法和计算属性。
4. 协议扩展是为协议添加方法和计算属性。
5. 面向协议编程是这样一种实践：它把程序架构按照一系列协议来设计，然后利用协议扩展来提供默认实现。