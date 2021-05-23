---

---









先看代码

```swift
import UIKit

//定义一个类Pet
class Pet{
  	//定义类的属性
    var sex:Int
    var color:UIColor
    var rarity:String
  	
  	//类的方法，这里是宠物赚钱的一个方法
  	func makeMoney() -> Int{
        return 20
    }
    
  	//类的构造器
    init(sex:Int,color:UIColor,rarity:String) {
        self.sex = sex
        self.color = color
        self.rarity = rarity
    }
}

//定义一个类并且该类继承自Pet类
class Cat:Pet{
    var cloths:[String]
  	
  //重写父类的方法
  	override func makeMoney() -> Int {
        switch rarity {
        case "Legend":
            return 200
        case "Rare":
            return 100
        case "Normal":
            return 1
        default:
            return 0
        }
    }
  	
  	//本类的构造器
    init(cloths:[String],sex:Int,color:UIColor,rarity:String) {
        self.cloths = cloths
        super.init(sex: sex, color: color, rarity: rarity)
    }
}

//初始化一个Cat实例(实例化)
var kitty = Cat(cloths: ["裙子","衬衫"], sex: 1, color: .orange, rarity: "legend")

//定义了一个变量来存储赚到的钱，willSet和didSet的作用是：每次修改变量值，就会打印变量前后的数值。
var kittyMoney = 0 {
    willSet{
        print("xxx")
    }
    didSet{
        print("yyy")
    }
}

kittyMoney += kitty.makeMoney()

```

写类的默认规则：

* 先写类的属性
* 再写类的方法，也就是func
* 最后写类的构造方法

### 创建你自己的类

Swift的类也能让你创建带有属性和方法的新类型，这一点和结构体很相似，但是它们之间有五个显著的区别。下面让我一一为你说明。

类和结构体的第一个区别是类没有逐一成员构造器。这意味着只要你的类里有属性，你就必须自行创建构造器。

举个例子：

```swift
class Dog {
  var name: String
  var breed: String

  init(name: String, breed: String) {
    self.name = name
    self.breed = breed
  }
}
```

创建类的实例跟创建结构体的实例方式一样：

```swift
let poppy = Dog(name: "Poppy", breed: "Poodle")
```

### 类继承

类和结构体的第二个区别是类可以继承已经存在的类。新的类继承了原始类所有的属性和方法。

这个过程被称为 *类继承* 或者 *子类化*， 被继承的类称为 “父类” 或者 “超类”， 而新的类称为 “子类” 。

下面是一个 `Dog` 类：

```swift
class Dog {
  var name: String
  var breed: String

  init(name: String, breed: String) {
    self.name = name
    self.breed = breed
  }
}
```

现在让我们基于 `Dog` 来创建一个新的类 `Poodle`。默认情况下，它会继承 `Dog` 的所有属性以及构造器。

```swift
class Poodle: Dog {
}
```

不过，我们也可以为 `Poodle` 创建自己的构造器。我们知道这个类的 `breed` 属性总是 “Poodle”，因此我们可以创建一个只有 `name` 属性的构造器。并且，我们可以在 `Poodle` 的构造器里直接调用 `Dog` 的构造器，以便发生和 `Dog` 相同的构造过程。

```swift
class Poodle: Dog {
  init(name: String) {
    super.init(name: name, breed: "Poodle")
  }
}
```

出于安全性考虑，Swift会要求你总是在子类里调用 `super.init()`，以防止类在构造时来自父类的一些重要工作被遗漏。

### 重写方法

子类可以将父类的方法替换为自己的实现，这个过程被称为 *重写*。让我们给 `Dog` 类添加一个 `makeNoise()` 方法：

```swift
class Dog {
  func makeNoise() {
    print("Woof!")
  }
}
```

如果你创建一个 `Poodle` 类继承自 `Dog`，它会继承 `makeNoise()` 方法。“Woof!”:

```swift
class Poodle: Dog {
}
let poppy = Poodle()
poppy.makeNoise()
```

方法重写使得我们可以为 `Poodle` 类重新实现 `makeNoise()`。

Swift要求我们在重写方法时用 `override func` 而不是 `func`，这个限定防止你在自己不知情的情况下偶然重写方法。另外，试图重写一个父类中并不存在的方法，你会遭遇错误。

```swift
class Poodle: Dog {
  override func makeNoise() {
    print("Yip!")
  }
}
```

通过这个修改，`poppy.makeNoise()` 将打印出 “Yip!”，而不是 “Woof!”。

### Final类

尽管类继承十分有用，并且苹果的平台在许多地方要求你大量使用它，有的时候你会希望阻止其他开发者基于你的类构建新的类。

Swift给了我们 `final` 关键字用于实现这种意图：当你把一个类声明为final时，将没有类能够继承它。这意味着没有人能通过重写方法来改变这个类的行为。

只需要把 `final` 关键字放在类前面，就像这样：

```swift
final class Dog {
  var name: String
  var breed: String

  init(name: String, breed: String) {
    self.name = name
    self.breed = breed
  }
}
```

### 复制对象

类和结构体的第三个区别是它们被复制的方式。当你复制一个结构体时，原始对象和复制体是不一样的两个东西，改变其中一个并不会改变另外一个。当你复制一个 *类* 时，原始对象和复制体都指向相同的东西，所以改变其中一个也会改变另一个。

举个例子，有一个简单的 `Singer` 类，它有一个带有默认值的 `name` 属性：

```swift
class Singer {
  var name = "Taylor Swift"
}
```

当我们创建一个这个类的实例并且打印它的name时，我们会得到“Taylor Swift”：

```swift
var singer = Singer()
print(singer.name)
```

当我们基于第一个实例创建第二实例并且改变第二个实例的name时：

```swift
var singerCopy = singer
singerCopy.name = "Justin Bieber"
```

由于类的工作机制，`singer` 和 `singerCopy` 指向内存里的同一个对象。所以当我们打印 singer的name时，我们也会得到“Justin Bieber”：

```swift
print(singer.name)
```

另一方面，假如 `Singer` 是一个结构体，那第二次我们还将得到 “Taylor Swift”：

```swift
struct Singer {
  var name = "Taylor Swift"
}
```

### 析构器

类和结构体的第四个区别是类有 *析构器*，它是一个类的实例被销毁时执行的代码。

这里有一个 `Person` 类，它有一个 `name` 属性，一个简单的构造器，以及一个打印信息的 `printGreeting()` 方法：

```swift
class Person {
  var name = "John Doe"

  init() {
    print("\(name) is alive!")
  }

  func printGreeting() {
    print("Hello, I'm \(name)")
  }
}
```

我们将利用循环创建几个 `Person` 类的实例，每次循环流转的时候，一个新的 `Person` 都会被创建，之后被销毁：

```swift
for _ in 1...3 {
  let person = Person()
  person.printGreeting()
}
```

来到我们的析构器。每当 `Person` 实例被销毁时，它的析构器都会被执行。

```swift
deinit {
  print("\(name) is no more!")
}
```

### 可变性

类和结构体的最后一个区别是它们处理常量的方式。如果你有一个常量结构体，它有一个变量属性，那么这个变量属性是无法修改的。

但是，如果它是一个常量类，也有一个变量属性，那么这个变量属性是可以被修改的。基于这个区别，类的方法在改变属性时，并不需要 `mutating` 关键字，而结构体则需要。

这个区别意味着你可以修改类中的任何变量属性，即便类的实例本身被声明为常量。以下代码完全合法：

```swift
class Singer {
  var name = "Taylor Swift"
}

let taylor = Singer()
taylor.name = "Ed Sheeran"
print(taylor.name)
```

如果你不想属性被修改，那么你必须直接将属性声明为常量。

```swift
class Singer {
  let name = "Taylor Swift"
}
```

### 总结

让我们来总结一下。

1. 类和结构体很相似，它们都允许你创建自己的属性和方法。
2. 一个类可以继承自另一个类，它会获得父类的所有属性和方法。谈到类层次结构的时候，我们经常说一个类基于另一个类，也就是一个类继承自另一个类。
3. 你可以用 `final` 关键字来标记一个类，这样可以阻止它被继承。
4. 方法重写使得一个子类可以用全新的实现来替代父类中的实现。
5. 当两个变量指向同一个实例时，它们指代的对象在内存中占用同一块区域，改变其中一个也会改变另一个。
6. 类可以有析构器，它们是类的实例被销毁时执行的代码。
7. 类不像结构体那样受常量的强制约束。如果类的一个属性被声明为变量，那么无论类的实例是否以变量的方式创建，这个属性都可以被修改。