## Control Flow 控制流
---

## 目录

- [For-In Loops for循环](#for-in-loops-for循环)
- [While Loops](#while-loops)
  - [While](#while)
  - [Repeat-While](#repeat-while)
- [Conditional Statements 条件语句](#conditional-statements-条件语句)
  - [If --if语句](#if-if语句)
  - [Switch  --Switch语句](#switch-switch语句)
  - [No Implicit Fallthrough](#-no-implicit-fallthrough)
  - [Interval Matching 区间匹配](#interval-matching-区间匹配)
  - [Tuples  元组](#tuples-元组)
  - [Value Bindings 值绑定](#value-bindings-值绑定)
  - [Where](#where)
  - [Compound Cases 复合条件](#compound-cases-复合条件)
- [Control Transfer Statements 控制转移语句](#control-transfer-statements-控制转移语句)
  - [Continue](#continue)
  - [Break](#break)
    - [Break in a Loop Statement](#break-in-a-loop-statement)
    - [Break in a Switch Statement](#break-in-a-switch-statement)
  - [Fallthrough](#fallthrough)
  - [Labeled Statements 标签声明](#labeled-statements-标签声明)
- [Early Exit ](#early-exit)
- [Checking API Availability   检查API的可用性](#checking-api-availability-检查api的可用性)



---

### 前言

Swift有很多的控制流语句。
例如`while`可以将一个任务执行多次，`if`、`guard`、`switch`进行条件判断执行不同的操作，`break`、`continue`能跳出循环或者跳到其他节点。

使用`for-in`能够很方便的遍历 arrays, dictionaries, ranges, strings, 以及其它序列。

Switch语句也比其它类C的语言强大很多，具体介绍看下面。


---

## For-In Loops  --for循环

使用for循环来遍历序列元素，比如数组元素，范围内数字，字符串字符

如下遍历数组元素


```Swift
let names = ["Anna", "Alex", "Brian", "Jack"]
for name in names {
    print("Hello, \(name)!")
}
// Hello, Anna!
// Hello, Alex!
// Hello, Brian!
// Hello, Jack!
```

遍历字典的键和值，如下

```Swift
let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
for (animalName, legCount) in numberOfLegs {
    print("\(animalName)s have \(legCount) legs")
}
// ants have 6 legs
// spiders have 8 legs
// cats have 4 legs
```

数字范围遍历
```Swift
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}
// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25

```

如果用不到遍历的序列元素，也可以忽略，用 `_ `代替


```Swift
let base = 3
let power = 10
var answer = 1
for _ in 1...power {
    answer *= base
}
print("\(base) to the power of \(power) is \(answer)")
// Prints "3 to the power of 10 is 59049"
```

遍历半开区间

```Swift
let minutes = 60
for tickMark in 0..<minutes {
    // render the tick mark each minute (60 times)
}
```

是时候遍历的间隔不是1，比如每五分钟为间隔，可以使用`stride(from:to:by:)`跳过


```Swift
let minuteInterval = 5
for tickMark in stride(from: 0, to: minutes, by: minuteInterval) {
    // render the tick mark every 5 minutes (0, 5, 10, 15 ... 45, 50, 55)
}
//右侧为开区间（0<=tickMark<60）
```
如果要两头的值都能取到，使用stride(from:through:by:)方法

```Swift
let hours = 12
let hourInterval = 3
for tickMark in stride(from: 3, through: hours, by: hourInterval) {
    // render the tick mark every 3 hours (3, 6, 9, 12)
}

```
---
## While Loops 

`while`循环是一直执行到条件不成立为止，经常用于不知道循环开始点，只知道结束点的时候。

有两种while循环
- `while `在每次循环前做条件判断
- `repeat-while `在每次循环结束后做条件判断


### While

`while`循环格式如下

```Swift
while condition {
    statements
}
```

每次循环前都先判断`condition`是否成立，如果为`true`就执行`statements`语句，一直到`condition`为`false`为止。

下面举个例子，一个用`while`循环实现的小游戏，爬梯子游戏

![image](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/snakesAndLadders_2x.png)

下面说下游戏规则
- 如上图有25格，走出25格游戏结束
- 从左下角开始
- 轮到你的时候，掷骰子，掷到几前进几步，按照上右图的路线走
- 如果走到梯子的下端的格子，你可以顺着梯子到达梯子的顶端的格子
- 如果走到蛇口的格子，滑到蛇尾巴的格子


根据游戏规则我们抽象为代码如下

```Swift
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)

```
> 一共25个格子，初始化一个26个元素均为0的数组（数组索引从0开始，故为26）

根据上面左图设定游戏规则

```Swift
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
```
爬梯和滑梯通过数据来模拟，另外我们模拟一个🎲数据来写一次爬梯实验，如下


```Swift
var square = 0
var diceRoll = 0
while square < finalSquare {
    // roll the dice
    diceRoll += 1
    if diceRoll == 7 { diceRoll = 1 }
    // move by the rolled amount
    square += diceRoll
    if square < board.count {
        // if we're still on the board, move up or down for a snake or a ladder
        square += board[square]
    }
}
print("Game over!")
```

如上代码，🎲本应是个1-6的随机数，我们先固定以1, 2, 3, 4, 5, 6, 1, 2，3...的数据来模拟🎲数据，然后从左下角开始，当当前格子数超出25格的时候，game over。真实情况下，🎲的数据是随机的，所以我们不知道游戏从哪开始，只知道只要超过25就游戏结束了，所以用while循环再合适不过。

### Repeat-While

和`while`不同，`Repeat-While`是先执行语句在检查条件是否为真

> `repeat-while`类似其他语言中的`do-while`循环

格式如下

```Swift
repeat {
    statements
} while condition
```
同样用`repeat-while`来实现爬梯子游戏，实现如下：


```Swift
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
var square = 0
var diceRoll = 0

repeat {
    // move up or down for a snake or ladder
    square += board[square]
    // roll the dice
    diceRoll += 1
    if diceRoll == 7 { diceRoll = 1 }
    // move by the rolled amount
    square += diceRoll
} while square < finalSquare
print("Game over!")

```

相比于`while`循环，`repeat-while`更适合这个游戏的逻辑，因为相比于`while`,少了一步`square`和`board.count`的检查。

---
## Conditional Statements 条件语句

如果需要根据不同的条件来执行不同的代码块的时候，就需要使用条件语句。
Swift中有两种条件语句，一般说来
- if语句 当判断条件较少的时候使用
- switch语句 用于复杂的条件判断或者更多的条件判断


### If --if语句

简单的`if`语句，只有一个判断条件，当条件为真时执行里面的代码，如下：

```Swift
var temperatureInFahrenheit = 30
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
}
// Prints "It's very cold. Consider wearing a scarf."
```

也可以用`else`来执行当条件为假时的代码

```Swift
temperatureInFahrenheit = 40
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else {
    print("It's not that cold. Wear a t-shirt.")

```
用`else if `也可以增加多条件判断


```Swift
temperatureInFahrenheit = 90
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else if temperatureInFahrenheit >= 86 {
    print("It's really warm. Don't forget to wear sunscreen.")
} else {
    print("It's not that cold. Wear a t-shirt.")
}
```
可以根据自己需要选择是否需要else，比如也可以成这样：

```Swift
temperatureInFahrenheit = 72
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else if temperatureInFahrenheit >= 86 {
    print("It's really warm. Don't forget to wear sunscreen.")
}
```


### Switch  --Switch语句

`Switch`语句是判断一个值出现的不同可能性，根据实际值来判断执行不同的代码块，`switch`还能对不同的可能性执行同一个代码块。一般格式如下：

```Swift
switch some value to consider {
case value 1:
    respond to value 1
case value 2,
     value 3:
    respond to value 2 or 3
default:
    otherwise, do something else
}
```

如上，每种可能值都用` case` 修饰，如果要匹配多个可能值，用 ```,``` 隔开。当所有的值都不匹配的时候，可以设置`default`来执行当前的操作。


```Swift
let someCharacter: Character = "z"
switch someCharacter {
case "a":
    print("The first letter of the alphabet")
case "z":
    print("The last letter of the alphabet")
default:
    print("Some other character")
}
// Prints "The last letter of the alphabet"
```

###  No Implicit Fallthrough

和C、OC不同的是，Swift中的`Switch`在执行完匹配条件下的代码后就会结束`switch`语句，不会在执行下一个分支，所以也就不需要使用`break`，代码简洁，也不会因为`break`而出错。（但是也可以使用，具体使用见下面*Break in a Switch Statement*）

Switch中的每个`case下`都必须至少有一句执行代码，不然就是无效的。

```Swift
let anotherCharacter: Character = "a"
switch anotherCharacter {
case "a": // Invalid, the case has an empty body
case "A":
    print("The letter A")
default:
    print("Not the letter A")
}
// This will report a compile-time error.
```

如上代会报`compile-time error （case "a": does not contain any executable statements）`，即分支a下没有执行语句。这种机制能够避免分支穿透，使代码更安全，更清晰。

如果想要匹配多个值，可以用","隔开，如下

```Swift
let anotherCharacter: Character = "a"
switch anotherCharacter {
case "a", "A":
    print("The letter A")
default:
    print("Not the letter A")
}
// Prints "The letter A"
```


### Interval Matching 区间匹配

`switch`可以匹配数字区间，如下

```Swift
let approximateCount = 62
let countedThings = "moons orbiting Saturn"
let naturalCount: String
switch approximateCount {
case 0:
    naturalCount = "no"
case 1..<5:
    naturalCount = "a few"
case 5..<12:
    naturalCount = "several"
case 12..<100:
    naturalCount = "dozens of"
case 100..<1000:
    naturalCount = "hundreds of"
default:
    naturalCount = "many"
}
print("There are \(naturalCount) \(countedThings).")
// Prints "There are dozens of moons orbiting Saturn."
```

### Tuples  元组

Switch可以匹配元组的元素是否匹配或者部分元素是否匹配。可以使用（_）来代替任意元素。如下例：

```Swift
let somePoint = (1, 1)
switch somePoint {
case (0, 0):
    print("\(somePoint) is at the origin")
case (_, 0):
    print("\(somePoint) is on the x-axis")
case (0, _):
    print("\(somePoint) is on the y-axis")
case (-2...2, -2...2):
    print("\(somePoint) is inside the box")
default:
    print("\(somePoint) is outside of the box")
}
// Prints "(1, 1) is inside the box"
```
![image](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/coordinateGraphSimple_2x.png)

如上图，👆的代码分别匹配`somePoint`是否在坐标原点、在x轴，在y轴，在蓝色框内及蓝线上。

> 不像C,Swift的 case值允许包含重复的范围，只要有一个case满足条件，就会结束switch语句，后面的case就会被忽略。
  

```Swift
let vel = (1,1)
switch vel {
case (0...1,1):
    print("one")
case (0...1,1...2):
    print("two")
case (1...2,1...2):
    print("three")
default:
    print("four")
}
//虽然前三个条件都满足，但是print one，后面的被忽略


```

### Value Bindings 值绑定

我们可以重命名`case`匹配到的值，并作为一个临时常量或者变量在`case`的执行语句中使用，这种行为我们称为 ***value binding*** (值绑定)。如下例：

```Swift
let anotherPoint = (2, 0)
switch anotherPoint {
case (let x, 0):
    print("on the x-axis with an x value of \(x)")
case (0, let y):
    print("on the y-axis with a y value of \(y)")
case let (x, y):
    print("somewhere else at (\(x), \(y))")
}
// Prints "on the x-axis with an x value of 2"
```

![image](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/coordinateGraphMedium_2x.png)

👆的switch语句要验证`anotherPoint`是否在红色的x轴上，还是在黄色的y轴上，或者在其他轴上。

其中每个`case`语句中声明了占位符常量x、y来临时作为元组中一个或两个元素， `case (let x, 0)`匹配任意y值等于0的点，并将该点的x值赋值给临时常量x，且可以在执行语句中使用这个常量。`case (0, let y)`同`case (let x, 0)`解释一样。

可以看到，上面switch语句中并没有`default case`，因为`case let (x, y)`表示了任意点，包含了所有点出现的可能性。所以不需要`default case`了。

### Where  

Switch `case`中可以使用`where`从句增加附加条件，例如：

```Swift
let yetAnotherPoint = (1, -1)
switch yetAnotherPoint {
case let (x, y) where x == y:
    print("(\(x), \(y)) is on the line x == y")
case let (x, y) where x == -y:
    print("(\(x), \(y)) is on the line x == -y")
case let (x, y):
    print("(\(x), \(y)) is just some arbitrary point")
}
// Prints "(1, -1) is on the line x == -y"
```

![image](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/coordinateGraphComplex_2x.png)

这个switch语句验证`yetAnotherPoint`是否在红线上还是在绿线上或者其他。

如上例所示，`where`相当于一个过滤器，只有当where后面的条件为真的时候才会执行`case`里面的语句（看着比`if else `啰嗦点）

### Compound Cases 复合条件

如果多个`case`的执行语句是一样的，我们就可以把这些值组合在一起，用逗号隔开，支持多行。只要满足其中的任意值都会执行该`case`下语句。如下：

```Swift
let someCharacter: Character = "e"
switch someCharacter {
case "a", "e", "i", "o", "u":
    print("\(someCharacter) is a vowel")
case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
     "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
    print("\(someCharacter) is a consonant")
default:
    print("\(someCharacter) is not a vowel or a consonant")
}
// Prints "e is a vowel"
```

在这种复合条件下也可以使用***value bindings***（值绑定），如：


```Swift
let stillAnotherPoint = (9, 0)
switch stillAnotherPoint {
case (let distance, 0), (0, let distance):
    print("On an axis, \(distance) from the origin")
default:
    print("Not on an axis")
}
// Prints "On an axis, 9 from the origin"
```
如上第一个`case`表示无论`stillAnotherPoint`在x轴或者y轴上，均输出该点到原点的距离

---
## Control Transfer Statements 控制转移语句

控制转移语句（***Control transfer statements***）是用来改变代码的执行顺序，Swift有五个转移语句：

- **`continue`**
- **`break`**
- **`fallthrough`**
- **`return`**
- **`throw`**

以上三种前三种会在下面介绍，`return`语句会在*Functions*里面介绍，`throw`语句会在 *Error Handling*中介绍。

### Continue

`continue`语句的作用就是提前结束当前循环，进入下一个循环（并不是跳出循环语句）。如下例：


````Swift``
let puzzleInput = "great minds think alike"
var puzzleOutput = ""
let charactersToRemove: [Character] = ["a", "e", "i", "o", "u", " "]
for character in puzzleInput {
    if charactersToRemove.contains(character) {
        continue
    } else {
        puzzleOutput.append(character)
    }
}
print(puzzleOutput)
// Prints "grtmndsthnklk"
```

### Break

`break`语句会立刻结束控制流语句的执行。可以使用在`switch`语句和`loop`循环语句中，`break`能提前终止`switch`和`loop`语句。

#### Break in a Loop Statement

在循环语句中，`break`语句作用就是立刻结束整个循环语句，跳出循环体。执行循环体外的后续代码。

#### Break in a Switch Statement

在switch语句中，`break`会立刻跳出switch语句，执行switch语句外代码。

因为switch的case body中必须有执行语句，所以要是想忽略这个case或者多个case，我们就可以在body中使用`break`，这样就能提前结束switch语句。如下例

```Swift
let numberSymbol: Character = "三"  // Chinese symbol for the number 3
var possibleIntegerValue: Int?
switch numberSymbol {
case "1", "١", "一", "๑":
    possibleIntegerValue = 1
case "2", "٢", "二", "๒":
    possibleIntegerValue = 2
case "3", "٣", "三", "๓":
    possibleIntegerValue = 3
case "4", "٤", "四", "๔":
    possibleIntegerValue = 4
default:
    break
}
if let integerValue = possibleIntegerValue {
    print("The integer value of \(numberSymbol) is \(integerValue).")
} else {
    print("An integer value could not be found for \(numberSymbol).")
}
// Prints "The integer value of 三 is 3."
```
如上当所有case不匹配的时候，默认结束switch语句。执行`if let..`语句

### Fallthrough 

上面曾介绍了Swift中switch case不具有穿透性（不会从一个case body穿过执行下一个case body），只要有一个`case`满足条件，执行完该case body就会结束switch语句。不同的是，在C中，你需要在每个case body末尾加上break来阻止穿透。因此相比于C，Swift还是比较简洁的，也能够避免匹配多个case造成错误。

如果想让Swift中的switch case具有穿透性，可以使用`fallthrough`关键字，如下例：

```Swift
let integerToDescribe = 5
var description = "The number \(integerToDescribe) is"
switch integerToDescribe {
case 2, 3, 5, 7, 11, 13, 17, 19:
    description += " a prime number, and also"
    fallthrough
default:
    description += " an integer."
}
print(description)
// Prints "The number 5 is a prime number, and also an integer."
```

如上switch语句如果不加`fallthrough`关键字，那么在case结束后就会直接执行`print(description)`。加了`fallthrough`后，使得case body执行完后继续执行default中body。所以`fallthrough`使得switch语句具有穿透性。


```Swift
let integerToDescribe = 5
var description = "The number \(integerToDescribe) is"
switch integerToDescribe {
case 2, 3, 5, 7, 11, 13, 17, 19:
    description += " a prime number, and also"
    fallthrough
case 5:
    description += " 5,and also"
    fallthrough
default:
    description += " an integer."
}
print(description)
//输出The number 5 is a prime number, and also 5,and also an integer.
```
（fallthrough可以让我们匹配多个case）

### Labeled Statements 标签声明

在Swift里，有时候需要循环语句嵌套循环语句来实现复杂的控制流。如果在嵌套循环中使用`continue`、`break`就会出问题。为了避免这些问题，我们可以创建
 ***Labeled Statements***（标签声明），相当于给`loop`起个名字，使用`break`、`continue`的时候标明作用于哪个`loop`语句。
 
 标签声明格式如下
 
```Swift
label name: while condition {
    statements
}

```

还是上面的爬梯子游戏，不过改下规则：

- 只有到达第25格（不是超过）才算赢。

![image](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/snakesAndLadders_2x.png)

这次使用while循环和switch嵌套使用，如下


```Swift
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
var square = 0
var diceRoll = 0

gameLoop: while square != finalSquare {
    diceRoll += 1
    if diceRoll == 7 { diceRoll = 1 }
    switch square + diceRoll {
    case finalSquare:
        // diceRoll will move us to the final square, so the game is over
        break gameLoop
    case let newSquare where newSquare > finalSquare:
        // diceRoll will move us beyond the final square, so roll again
        continue gameLoop
    default:
        // this is a valid move, so find out its effect
        square += diceRoll
        square += board[square]
    }
}
print("Game over!")
```

如上，我们给while循环起了个名字gameLoop，在zwitch中我们使用break gameLoop和continue gameLoop来控制代码执行顺序。在Switch语句中

- 第一个case表示当点数等于25的时候，跳出while循环，game over.
- 第二个case表示当点数超过25的时候，就需要重新掷骰子，执行下一次循环
- default表示点数不到25，就继续前进。


> 使用标签声明控制流语句能够使代码更清晰，也能够避免因关键字（`break`、`continue`）造成的代码顺序紊乱


---
## Early Exit 

一个`guard`语句，和if语句一样，都是根据表达式的`boolean`值来决定执行哪块代码。使用`guard`语句是为了保护`guard`语句后面执行的代码的安全性。`guard`语句一定会有`else`从句来执行条件为`false`的代码。如下

```Swift
func greet(person: [String: String]) {
    guard let name = person["name"] else {
        return
    }
    
    print("Hello \(name)!")
    
    guard let location = person["location"] else {
        print("I hope the weather is nice near you.")
        return
    }
    
    print("I hope the weather is nice in \(location).")
}
 
greet(person: ["name": "John"])
// Prints "Hello John!"
// Prints "I hope the weather is nice near you."
greet(person: ["name": "Jane", "location": "Cupertino"])
// Prints "Hello Jane!"
// Prints "I hope the weather is nice in Cupertino."
```

如上代码，如果`guard`后面的条件为真的话，就会执行`guard`后的代码，而在条件中声明的常量或者变量就可以放心大胆的在后续的代码中使用。

如果`guard`后面的条件不满足的话，就会执行性`else`分句中的代码，在该代码中必须跳出当前`guard`语句所在的代码块。我们可以使用控制转移语句`（control transfer statement）`，比如： `return`,` break`, `continue`, or `throw`或者 调用一个没有返回的`func`（比如`fatalError(_:file:line:)`）。


使用`guard`语句可以提高代码的可读性，
举个例子，如下

```Swift
func greet(person: [String: String]) {
    
    let name = person["name"]
    if name == nil {
        print("name not exist")
    } else {
        print("--\(String(describing: name))")
    }
    
    guard let g_name = person["name"]  else{
        return
    }
    print("--\(g_name)")
}

greet(person: ["name": "John"])
//print  --Optional("John")  --John

```

从上面代码就可以看出`if else `和`guard`的区别，和`if else`相比，`guard`语句并没有把通常要执行的代码封装在`else`从句中，而是放在`guard`语句后面，先检查条件是否为真，然后执行`guard`语句后面代码，保证了后面代码的安全性。

> 综上可以看出，当我们要使用一个Optional值，但不确定其是否存在，这时候用guard语句就会比较方便。

---
## Checking API Availability   检查API的可用性


Swift提供了检测api在当前部署环境下是否可用的方法

如下格式

```Swift
if #available(platform name version, ..., *) {
    statements to execute if the APIs are available
} else {
    fallback statements to execute if the APIs are unavailable
}
```

举例：

```Swift
if #available(iOS 10, macOS 10.12, *) {
    // Use iOS 10 APIs on iOS, and use macOS 10.12 APIs on macOS
} else {
    // Fall back to earlier iOS and macOS APIs
}
```

以上代码可以判断一个api是否能在iOS 10及以后和macOS 10.12的版本中运行，如果不行会报一个编译错误，可根据提示修正错误。