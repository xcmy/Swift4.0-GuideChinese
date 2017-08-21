# Enumerations  枚举

---
## 目录
- [Enumeration Syntax-枚举定义](#enumeration-syntax)
- [Matching Enumeration Values with a Switch Statement-Switch语句使用枚举](#matching-enumeration-values-with-a-switch-statement)
- [Associated Values-关联值](#associated-values)
- [Raw Values-原始值](#raw-values)
  - [Implicitly Assigned Raw Values-原始值默认](#implicitly-assigned-raw-values)
  - [Initializing from a Raw Value-原始值初始化构造器](#initializing-from-a-raw-value)
- [Recursive Enumerations-递归枚举](#recursive-enumerations)

---
## 前言

枚举就是定义了一组共有类型的相关值，我们可以通过它安全的使用这些值。

我们知道，C语言中的枚举值，每个成员都有其名称和默认的整型值。而Swift的枚举值更加灵活，我们可以为每个成员自定义默认值（或原始值）。可以是字符串，字符或者数字类型。另外枚举类型可以给其成员设置任何不同类型的关联值，也可以设置该枚举类型的成员为其关联值。

枚举在Swift中属于顶层类类型（ ***first-class***），它采用了一些只被传统类所支持的特性，如属性获取，初始化构造器。这样我们可以使用枚举初始化构造器来初始化一个值，也可以使用枚举来实现一些扩展功能（见下面递归枚举）。

---
## Enumeration Syntax

使用关键字`enum`来定义枚举类型，格式如下：
```swift
enum SomeEnumeration {
    // enumeration definition goes here
}
```
下面使用枚举类型定义了四个方向（东南西北）。

```swift
enum CompassPoint {
    case north
    case south
    case east
    case west
}
//north, south, east, 和 west都是这个枚举的成员，
//case关键字用来创建新的成员变量

```
>不像在C和OC中，Swift创建枚举类型的时候不会给成员分配integer值，像上面的例子中的成员 north, south, east 和 west不会被默认赋值为0、1、2、3，成员本身就是就是值，它们的类型就是`CompassPoint`

多个成员值可以定义在单行内，用逗号隔开：
```swift
enum Planet {
    case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
}

```

每个枚举类型都是一个全新的类型，和其他的类型一样，枚举类型的命名也是首字母大写（如：`CompassPoint and Planet`）,一般命名都是单名，这样使用方便，读起来也容易。

```swift
var directionToHead = CompassPoint.west

```
如上，定义`directionToHead`为`CompassPoint`类型，使用时就可以简写为：

```swift
directionToHead = .east
//系统自己推断，在明确知道枚举类型的情况下，这种写法可读性更强
```

---

## Matching Enumeration Values with a Switch Statement

我们可以使用枚举值作为Switch语句的条件值：如下

```swift
directionToHead = .south
switch directionToHead {
case .north:
    print("Lots of planets have a north")
case .south:
    print("Watch out for penguins")
case .east:
    print("Where the sun rises")
case .west:
    print("Where the skies are blue")
}
// Prints "Watch out for penguins"
//很容易理解，值匹配就进来
```
一般`Switch`语句要考虑到所有的情况，如果不能穷举所有的枚举值，就要使用`default`，如下：

```swift
let somePlanet = Planet.earth
switch somePlanet {
case .earth:
    print("Mostly harmless")
default:
    print("Not a safe place for humans")
}
// Prints "Mostly harmless"

```


---

## Associated Values

关联值，上面介绍了如何定义枚举类型，和在Switch语句中使用枚举类型来做条件判断。但有时候把其他类型的关联值和成员值放在一起更加有用，这样既能存储更多的信息，也能保证每次使用时可以有不同的关联值。

枚举类型的关联值可以是任何类型，每个成员值的关联值也可以是不同的数据类型。枚举类型和其他程序语言中的标签联合（tagged union）也称可辨识联合（discriminated union）或者变体类型（variant type）类似。

举个例子，假设我们需要两种不用类型的条形码来跟踪商品，有些商品使用UPC格式（由0~9数字组成）的一维码标签，如下图（每个竖条都有对应的数字，第一位是前缀，紧接着五位是厂商代码<manufacturer code>，然后五位是商品代码<product code>，最后一位是校验码）

![image](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/barcode_UPC_2x.png)

有些商品使用[**QR Code码**](https://baike.baidu.com/item/QRCode/10336647?fr=aladdin&fromid=10462339&fromtitle=QR+Code)(矩阵二维码)标签，使用 ISO 8859-1 编码，最多可编码2953个字符。如下图：
![image](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/barcode_QR_2x.png)

我们可以使用枚举类型很方便的存储商品的UPC码和二维码，如下定义：


```swift
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}
//枚举类型Barcode
//其中一个成员为upc,关联值为四个Int类型元素的元组类型
//一个关联值为string类型的成员qrCode
```
如上我们在定义枚举类型的时候并没有给关联值赋值，只是设置了关联值的数据类型，只有定义Barcode常量或者变量的时候再给关联值赋值，如下：

```swift
var productBarcode = Barcode.upc(8, 85909, 51226, 3)
//也可以
productBarcode = .qrCode("ABCDEFGHIJKLMNOP")
```
在switch语句中我们用let、var声明关联参数，并在case语句中使用如下：


```swift
switch productBarcode {
case .upc(let numberSystem, let manufacturer, let product, let check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case .qrCode(let productCode):
    print("QR code: \(productCode).")
}
// Prints "QR code: ABCDEFGHIJKLMNOP."
```
如果关联参数都是变量或者都是常量则可以简写为：

```swift
switch productBarcode {
case let .upc(numberSystem, manufacturer, product, check):
    print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")
case let .qrCode(productCode):
    print("QR code: \(productCode).")
}
// Prints "QR code: ABCDEFGHIJKLMNOP."

```

---
## Raw Values
👆说了如何创建枚举类型成员不同类型的关联值，我们知道关联值是在枚举类型被赋值的时候才赋值的，枚举类型也能在定义的时候给每个成员设定默认值（叫***raw values***），前提是默认值的类型一致。如下例

```swift
enum ASCIIControlCharacter: Character {
    case tab = "\t"
    case lineFeed = "\n"
    case carriageReturn = "\r"
}
```
***raw values***（原始值）类型可以是字符、字符串、及任何的数字类型。且默认值不能重复。

>***raw values***（原始值）跟关联值不一样，必须在枚举类型定义的时候赋值，且数据类型一致，默认值不会改变。而关联值在枚举类型被赋值给变量或者常量的时候赋值，数据类型可以不一致，关联值也可以随着常量或者变量的不同而不同。


### Implicitly Assigned Raw Values
如果我们不去逐个设置每个成员的默认值，系统会默认的设置原始值。如原始值类型若为`Int`类型，如无设置默认第一个枚举成员原始值为0，若设置了其中一个，则前面枚举成员的原始值从0递增，后面的枚举成员从设定值依次递增。若原始值类型为字符串，原始值默认为枚举成员的名称，如下例

```swift
enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
}

enum CompassPoint: String {
    case north, south, east, west
}

let earthsOrder = Planet.earth.rawValue
// earthsOrder is 3
 
let sunsetDirection = CompassPoint.west.rawValue
// sunsetDirection is "west"

```
>使用`rawValue`属性来获取默认值


### Initializing from a Raw Value

如果一个枚举类型设定了原始值，我们则可以通过原始值来反查枚举成员，我们使用原始值初始化一个枚举实例，若存在则返回枚举成员，不存在则返回`nil`。如下例

```swift
let possiblePlanet = Planet(rawValue: 7)
// possiblePlanet is of type Planet? and equals Planet.uranus


```
如上该初始化方法返回值为一个可选类型（因为不确定到底能不能初始化成功）。

如果输入的参数在枚举类型原始值中不存在，则返回`nil`，如下例：


```swift
let positionToFind = 11
if let somePlanet = Planet(rawValue: positionToFind) {
    switch somePlanet {
    case .earth:
        print("Mostly harmless")
    default:
        print("Not a safe place for humans")
    }
} else {
    print("There isn't a planet at position \(positionToFind)")
}
// Prints "There isn't a planet at position 11"

```

---
## Recursive Enumerations

递归枚举类型，就是枚举成员中的一个或者多个成员的关联值为该枚举类型的成员。我们使用`indirect`来声明，如下例：


```swift
enum ArithmeticExpression {
    case number(Int)
    indirect case addition(ArithmeticExpression, ArithmeticExpression)
    indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```
也可以写成：

```swift
indirect enum ArithmeticExpression {
    case number(Int)
    case addition(ArithmeticExpression, ArithmeticExpression)
    case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```
下面我们用递归枚举类型和递归函数来实现`(5+4)*2`的计算。（具体看例子吧）
```swift
//常量赋值
let five = ArithmeticExpression.number(5)
let four = ArithmeticExpression.number(4)
let sum = ArithmeticExpression.addition(five, four)
let product = ArithmeticExpression.multiplication(sum, ArithmeticExpression.number(2))

//返回值为Int类型
func evaluate(_ expression: ArithmeticExpression) -> Int {
    switch expression {
    //第一步
    case let .number(value):
        return value
    //第二步
    case let .addition(left, right):
        return evaluate(left) + evaluate(right)
        //evaluate(left) 和 evaluate(right)分别返回第一步，
    //第三步
    case let .multiplication(left, right):
        return evaluate(left) * evaluate(right)
        //evaluate(left)会执行第二步，返回值（5+4），evaluate(right)会执行第一步返回值为2
    }
}
 
print(evaluate(product))
// Prints "18"

```
