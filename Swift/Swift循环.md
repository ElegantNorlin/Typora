---

---











### For循环

Swift有很多种书写循环的方式，它们底层的机制是相同的：重复执行某段代码直到某个条件不再满足。

最常见的循环是`for`循环：它在数组和范围上循环，每次拉出一个值然后把它赋予一个常量。

举个例子，这里是一个数字的范围：

```swift
let count = 1...10
```

我们可以用一个`for`循环打印里面的每一个值，就像这样：

```swift
for number in count {
  print("数字是\(number)")
}
```

数组的操作方式也一样：

```swift
let albums = ["Red", "1989", "Reputation"]
for album in albums {
  print("\(album)在Apple Music上有售卖。")
}
```

如果你不需要用到`for`循环提供给你的常量，你可以用下划线替代，这样Swift就会忽略这个你没有用到的值：

```swift
for _ in 1...5 {
  print("Zzz...")
}
```

### While循环

第二种书写循环的方式是使用`while`：给定一个检查的条件，循环运行代码直到条件不成立。

举个例子，我们可以使用`while`循环来模拟一个躲猫猫游戏：从1开始数数，数到的数会打印出来。数完20个数之后打印“准备好了没？我来啦！”

Swift代码长这样：

```swift
var number = 1
while number <= 20 {
  print(number)
  number += 1
}

print("准备好了没？我来啦！")
```

### Repeat循环

第三种循环的写法不常用，它是 `repeat` 循环。除了把条件检查放在后面，它基本上跟 `while` 循环是一样的。

因此，我们可以用 `repeat` 循环重写我们的躲猫猫游戏：

```swift
var number = 1
repeat {
  print(number)
  number += 1
} while number <= 20

print("准备好了没？我来啦！")
```

因为检查的条件是放在后面，所以 `repeat` 循环里的代码至少会被执行一次。而 `while` 循环则是在首次运行前就会检查条件。

举个例子， 下面代码里的 `print()` 函数永远都不会被运行，因为 `false` 永远是false：

```swift
while false {
  print("这是false")
}
```

Xcode 会警告我们上面代码中的 `print()` 代码永远都不会被执行。

而在下面这个代码里，`print()` 函数则至少运行一次，因为 `repeat` 只有在条件检查没有通过时才会停止执行：

```swift
repeat {
  print("这是false")
} while false
```

### 退出循环

你可以使用 `break` 关键字来终止循环。让我们以一个常规的 `while` 循环为例——火箭发射倒计时：

```swift
var countDown = 10
while countDown >= 0 {
  print(countDown)
  countDown -= 1
}

print("发射！")
```

想象一下，宇航员感觉这个倒计时过程很无聊，决定跳过后面的数，直接发射：

```swift
while countDown >= 0 {
  print(countDown)
  if countDown == 4 {
    print("好无聊啊，让我们直接发射吧！")
    break
  }

  countDown -= 1
}
```

通过这次改动，只要 `countDown` 达到4，那么宇航员的消息就会被打印，剩余的计数将会被忽略。

### 退出多重循环

把循环放在另一个循环里，叫做*嵌套* 循环。有的时候，你会有这种需求：同时跳出内部的循环和外部的循环。

举个例子， 我们可以用嵌套循环实现一个从 1 到 10 的乘法表：

```swift
for i in 1...10 {
  for j in 1...10 {
    let product = i * j
    print ("\(i) * \(j) is \(product)")
  }
}
```

如果想退出循环，我们需要做两件事。首先，给外层循环加一个标签，像这样：

```swift
outerLoop: for i in 1...10 {
  for j in 1...10 {
    let product = i * j
    print ("\(i) * \(j) 等于 \(product)")
  }
}
```

然后，在内层循环里添加条件，在条件满足时用 `break outerLoop` 同时退出内外层循环：

```swift
outerLoop: for i in 1...10 {
  for j in 1...10 {
    let product = i * j
    print ("\(i) * \(j) 等于 \(product)")
    if product == 50 {
      print("这是一个靶心。")
      break outerLoop
    }
  }
}
```

如果只使用`break`，就只能退出内层循环，外层循环会继续运行。

### 跳过循环项

如你所见，`break`关键字可以用于退出循环。但是假如你想要跳过当前项然后继续执行下一次循环，你可以使用 `continue` 关键字。

让我们写一个从 1 到 10 的循环，然后利用Swift的取余操作符来跳过所有的奇数：

```swift
for i in 1...10 {
  if i % 2 == 1 {
    continue
  }

  print(i)
}
```

### 无限循环

通常来说，我们用 `while` 循环来实现无限循环：它指的是那种不会自动停止的循环，或者说只有我们想要它停止时才停止的循环。你的智能手机里的App都用到了无限循环， 因为它们一旦开始运行，就会持续运行，不断地接收输入事件，做出响应。

最简单的无限循环是用 `true` 作为条件。`true` 使得循环可以无限重复地执行。警告：如果你采用`while true`来实现循环，确保你在循环内会有一个条件检查以退出这个无限循环。

举个例子， 我们用 `while true` 来模拟约翰·凯奇的《4’33”》这首歌。这首歌真的很奇葩，4分33秒都是沉默，没声。

```swift
var counter = 0
while true {
  print(" ")
  counter += 1
  if counter == 273 {
    break
  }
}
```

### 总结

让我们来总结一下。

* 循环可以重复执行某段代码，直到条件不再满足。

* 最常见的循环是 `for` 循环，它内部会计数一个临时的常量来记录循环的次数。

* 如果你用不上这个 `for` 循环给到你的常量，可以使用下划线来告知Swift忽略它。

* 对于 `while` 循环，你需要显式地提供一个条件检查来决定循环是否执行。

* 尽管和 `while` 循环很像，`repeat` 循环至少会执行一次循环体里的代码。

* 你可以用 `break` 结束循环，但是在嵌套循环里， `break` 之后还要再加上标签才能跳出特定的外层循环。

* 你可以用 `continue` 来跳过循环中特定的项。

* 无限循环只有在你要求它停止时才会结束。如果你用了 `while true` 来实现无限循环，确保你有一个条件检查用于终止循环！