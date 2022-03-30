---
title: Playground实践
date: 2021-4-28 16:05:00
tags: Swift
toc: true
description: "设计一个小游戏：
    我们来设计一个卡牌游戏叫做西游记，一共有三个角色：唐僧、孙悟空和妖怪。类似于剪子包袱锤！3局两胜！"
---







设计一个小游戏：
    我们来设计一个卡牌游戏叫做西游记，一共有三个角色：唐僧、孙悟空和妖怪。类似于剪子包袱锤！3局两胜！

```swift
import UIKit
/*
 设计一个小游戏：
    我们来设计一个卡牌游戏叫做西游记，一共有三个角色：唐僧、孙悟空和妖怪。类似于剪子包袱锤！3局两胜！
 */
print("Start Game!")
//定义了卡牌人物
var persionTypes = ["👨‍🦲","🐒","👻"]

//设计卡牌对局规则
func battle(a:String,b:String) -> String {
    if a == "👨‍🦲" && b == "🐒"{
        return "👨‍🦲"
    }else if a == "👨‍🦲" && b == "👻"{
        return "👻"
    }else if a == "🐒" && b == "👻"{
        return "🐒"
    }else{
        return ""
    }
}


//电脑的出招顺序（随机）
//解包的时候加！是强制解包。这样打印出来的数据就没有optional了，直接就是内容。
//集合的randomElement()方法是从任意集合中随机取出一个元素.
var PCFirstRound = persionTypes.randomElement()!
var PCSecondRound = persionTypes.randomElement()!
var PCThirdRound = persionTypes.randomElement()!

//我的出招顺序
var myFirstRound = "👨‍🦲"
var mySecondRound = "🐒"
var myThirdRound = "👻"

//我的出招顺序数组
var myList = [myFirstRound,mySecondRound,myThirdRound]
//电脑的出招顺序数组
let PCList = [PCFirstRound,PCSecondRound,PCThirdRound]

var total = 0

for index in 0..<myList.count{
    let result = battle(a: myList[index], b: PCList[index])
    if result == myList[index]{
        print("Round\(index + 1),You win!")
        total += 1
    }else if result == ""{
        print("Round\(index + 1),The same!")
    }else{
        print("Round\(index + 1),You lose!")
        total -= 1
    }
}

if total > 0{
    print("You win the game!")
}else if total < 0{
    print("You lose the game!")
}else{
    print("Try again!")
}

```

