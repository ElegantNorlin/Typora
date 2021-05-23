---

---











### 算术操作符

到目前为止，你已经了解了Swift的所有基本类型，现在让我们利用操作符把它们放在一起来使用。操作符指的是那些看起来像数学符号的玩意，比如+和-。Swift中有大量的操作符。

下面有一些测试用的变量（这里特指数学里的变量，不局限于Swift的var）：

```swift
let firstScore = 12
let secondScore = 4
```

让我们用+和-把它们相加或者相减：

```swift
let total = firstScore + secondScore
let difference = firstScore - secondScore
```

我们还可以用 * 和 / 来执行乘法和除法：

```swift
let product = firstScore * secondScore
let divided = firstScore / secondScore
```

Swift有一个用于计算除法的余数的特殊操作符：%。它可以计算一个数A用若干个数B填充后，剩余的空间。

举个例子，如果我们把secondScore设置为4，那么当我们做13 % secondScore这个操作时，我们会得到1，因为4可以填充13三次，余数为1：

```swift
let remainder = 13 % secondScore
```

### 操作符重载

Swift支持“操作符重载”，这是一种简明的说法，具体指的是操作符的行为可以根据它使用时的具体情境来决定。举个例子，+可以用来相加整数，就像这样：

```swift
let meaningOfLife = 42
let doubleMeaning = 42 + 42
```

同时，+也可以用来连接字符串，就像这样：

```swift
let fakers = "Fakers gonna "
let action = fakers + "fake"
```

你甚至还可以使用+来连接数组，就像这样：

```swift
let firstHalf = ["John", "Paul"]
let secondHalf = ["George", "Ringo"]
let beatles = firstHalf + secondHalf
```

记住，Swift是一门类型安全的语言。这意味着它不允许你混用类型。举个例子，你不能把一个整数加到一个字符串上，因为这么做没有意义。

### 复合赋值操作符

Swift提供了一些把操作符和赋值组合起来的速记操作符，以便你可以用一次操作同时完成计算和赋值。它们看起来很像你已经认识的那些操作符，比如+，-，*，和/，不过需要在尾部再加上一个=，以表示把计算结果赋给操作数。

举个例子，如果有人考试考了95分，但是需要罚去5分，你可以这么写：

```swift
var score = 95
score -= 5
```

类似地，你可以利用+=来拼接字符串：

```swift
var quote = "The rain in Spain falls mainly on the "
quote += "Spaniards"
```

### 比较操作符

Swift提供了几个比较操作符，这些符号的工作方式跟你在数学中使用它们的方式很相似。

先来两个测试用的变量：

```swift
let firstScore = 6
let secondScore = 4
```

有两个可以用来检查相等的操作符。==（发音“等于”）检查两个值是否相等，而!=（发音“不等于”）则检查两个值是否不相等：

```swift
firstScore == secondScore
firstScore != secondScore
```

还有四个用来检查两个值哪一个比较大，哪一个比较小或者相等的操作符。就跟数学里的符号一样：

```swift
firstScore < secondScore
firstScore >= secondScore
```

上面的这些操作符用在字符串上也是可以的，因为字符串都有一个自然的字母顺序。

```swift
"Taylor" <= "Swift"
```

### 条件

你现在应该已经意识到你可以使用if语句来书写一些条件了。当你给到Swift一个条件时，如果条件成立，Swift会运行你在if语句块里写的代码。

尝试一下。你可以用上Swift最基础的函数，它叫做print()：你提供一些文本给它，它将这些文本打印出来。

让我们用条件来检测二十一点扑克游戏的赢家：

```swift
let firstCard = 11
let secondCard = 10
if firstCard + secondCard == 21 {
  print("21点！")
}
```

花括号 { 和 } 之间的代码，在条件成立时会被执行。如果条件不成立时你想执行另外的代码，可以使用 else 语句：

```swift
if firstCard + secondCard == 21 {
  print("21点！")
} else {
  print("普通点数。")
}
```

你还可以使用 else if 来串联多个条件：

```swift
if firstCard + secondCard == 2 {
  print("Aces，好手气!")
} else if firstCard + secondCard == 21 {
  print("21点！")
} else {
  print("普通点数。")
}
```

### 组合条件

Swift提供两种操作符，以便我们把条件组合在一起，它们是 &&（发音“与”）和 || （发音“或”）。

举个例子，为了检查两个人的年龄是否都超过某个值，我们可以这么写：

```swift
let age1 = 12
let age2 = 21
if age1 > 18 && age2 > 18 {
  print("两个人都超过18岁。")
}
```

上面的 print() 调用只有当两个年龄都大于18时才会被执行，而给到的两个年龄并不满足都大于18。实际上，Swift并不会真的把两个条件都检查一遍， 它不需要检查age2是否大于18，因为当它发现age1大于18这个条件已经不成立时，就不会再继续检查后面的条件。

与 && 不同的是，只要有任意一个条件通过测试， || 检查就会被认定为通过。举个例子，下面的代码在任一年龄大于18时就会打印一条消息：

```swift
if age1 > 18 || age2 > 18 {
  print("有一个人超过18岁。")
}
```

你可以在一个检查中使用多次&&和||，但是切记不要把条件写得太过复杂，因为那样很难阅读！

### 三元操作符

Swift还提供一种不常用的操作符，叫做三元操作符。它的名字源自它可以一次协同三个操作数工作的特点。首先，它会先检查第一个数里指定的条件是否满足，如果满足则返回第二个数，否则返回第三个数。

三元操作符是把 true 和 false 两种情况一次性考虑进去的符号，它包含问号和冒号两个部分，因此阅读起来更麻烦一些。下面是一些例子：

```swift
let firstCard = 11
let secondCard = 10
print(firstCard == secondCard ? "牌是一样的" : "牌不一样")
```

上面的代码检查两张牌是否一样，如果一样则打印“牌是一样的”，否则打印“牌不一样”。我们也可以换成常规的条件语句来达到一样的检测效果：

```swift
if firstCard == secondCard {
  print("牌是一样的")
} else {
  print("牌不一样")
}
```

### switch语句

如果你需要用到多个 if 和 else if ，那你可以使用另一种结构：switch case，它会让你的代码看起来更清晰。采用 switch 语句，你只需要写一次条件，然后列出所有可能的结果，并针对所有的结果编写对应的处理代码。

让我们来尝试一下，下面是一个值为 sunny 字符串的名叫 weather 的常量：

```swift
let weather = "sunny"
```

我们可以使用 switch 语句块来打印下面四种消息中的一种：

```swift
switch weather {
case "rain":
  print("记得带伞")
case "snow":
  print("记得保暖")
case "sunny":
  print("记得戴墨镜")
default:
  print("天气不错！")
}
```

最后一条case是 default ，它是不能省略的，因为Swift需要确保你覆盖了所有可能的case ，即不能有遗漏的情况。所以只要天气不是rain，snow，或者sunny， 默认的情况的case就会被运行。

Swift只会运行某一个case里的代码。如果你希望继续执行下一个case的代码，你需要用到 fallthrough 关键字，就像这样：

```swift
switch weather {
case "rain":
  print("记得带伞")
case "snow":
  print("记得保暖")
case "sunny":
  print("记得戴墨镜")
  fallthrough
default:
  print("天气不错！")
}
```

### 范围操作符

Swift提供了两种方式给我们创建范围：它们是 ..< 和 ... 操作符。半开放范围操作符 ..< ，创建的范围不包含右边的值。而闭合范围操作符 ... ，创建的范围包含右边的值。

范围 1..<5 包含数字1，2，3和4， 而范围 1...5 包含数字1，2，3，4和5。

对于switch语句块来说，范围非常有用。因为你可以把它们用于你的每条case。举个例子，假设我们根据某人的考试成绩打印不同的消息：

```swift
let score = 85
switch score {
case 0..<50:
  print("你需要加把劲了。")
case 50..<85:
  print("做的不错。")
default:
  print("你真棒！")
}
```

如前面提到的，这里必须有一个default case来确保所有的可能都被覆盖到。

### 总结

让我们来总结一下。

- Swift提供了用于算术和比较的操作符，它们的工作机制就像我们在数学中所熟知的那样。
- 有一些算术操作符的复合变体，它们可以一次性完成算术和赋值两个操作，比如 +=， -=，等等。
- 你可以使用 if ， else 和 else if 语句，基于条件判定的结果来运行不同的代码。
- Swift提供一个三元操作符，把 true 和 false 两种条件检查和语句块组合在一起。尽管有的时候会看到大家在使用它们，我个人不建议你使用这个操作符。
- 如果你基于同一个值做多种条件判定，那么你可以使用switch语句，这么做会使代码更清晰。
- 我们可以用 ..< 或者 ... 来创建范围，具体用哪一个取决于要不要包含后面那个值，即范围是否闭合。