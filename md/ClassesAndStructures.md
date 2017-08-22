# Classes and Structures 
---
## 目录

- [Comparing Classes and Structures-比较类和结构体](#comparing-classes-and-structures)
  - [Definition Syntax-定义语法](#definition-syntax)
  - [Class and Structure Instances-实例初始化](#class-and-structure-instances)
  - [Accessing Properties-属性获取](#accessing-properties)
  - [Memberwise Initializers for Structure Types-逐个成员初始化构造器](#memberwise-initializers-for-structure-types)
- [Structures and Enumerations Are Value Types-结构体和枚举都是值类型](#structures-and-enumerations-are-value-types)
- [Classes Are Reference Types-类是引用类型](#classes-are-reference-types)
  - [Identity Operators-等价运算符](#identity-operators)
  - [Pointers-指针](#pointers)
- [Choosing Between Classes and Structures-如何选择类和结构体](#choosing-between-classes-and-structures)
- [Assignment and Copy Behavior for Strings-Arrays-and-Dictionaries](#assignment-and-copy-behavior-for-strings-arrays-and-dictionaries)


---

## 前言

类和结构体是创建自定义数据结构最普遍和灵活的构造器，但也必须严格遵循常量或者变量、函数语法来设计数据结构的属性和功能。

和其他语言不同的是，Swift不需要单独创建声明和实现文件来创建自定义结构模型。你只需要定义一个类或者结构体，系统就会自动帮你创建外部接口。

> 传统意义上我们称类的一个实例为对象（object）。然而相比与其他语言，Swift中类和结构体中功能更加贴近，以下介绍的大多数功能都适用于类或者结构体，所以我们通常用实例（instance）来统称类和结构体的实例。

---

## Comparing Classes and Structures

Swift的类和结构体有很多共同之处，包括：

- 使用属性存储值
- 使用方法实现功能
- 使用下标语法访问值
- 使用初始化器初始化值
- 使用扩展增加附加功能
- 遵循协议实现标准功能

类能实现一些结构体做不到的功能：

- 继承，允许一个类继承另一个类的特征
- 类型转换，允许使用运行时机制（runtime）检查实例类型
- 析构器（或解构器），允许实例释放任何被分配的资源
- 自动引用计数，允许一个实例被引用多次


> 通常在代码中的结构体使用的都是copy值，所以不需要使用自动引用计数


### Definition Syntax

类和结构体的定义语法类似，类使用关键字`class`，而结构体使用`struct`。如下：

```swift
class SomeClass {
    // class definition goes here
}
struct SomeStructure {
    // structure definition goes here
}

```
> 每当定义一个类或者结构体的时候都相当于创建了一个新的Swift类型。一般的，类名和结构体名称使用首字母大写的驼峰命名法（像`SomeClass` 和`SomeStructure`），相反的属性名称和方法名称使用首字母小写的驼峰命名法（像`frameRate`）。

👇举个类定义和结构体定义的例子：

```swift
struct Resolution {
    var width = 0
    var height = 0
}
//该结构体定义了两个Int类型的属性width和height，并初始化值为0

```
>存储属性就是类和结构体绑定和定义的常量或者变量
```swift
class VideoMode {
    var resolution = Resolution()
    var interlaced = false
    var frameRate = 0.0
    var name: String?
}
//该类定义了四个存储属性
//结构体类型的属性resolution，并初始化了一个结构体实例Resolution()
//Bool类型的属性interlaced，初始化值为false
//Double类型的属性frameRate，初始化值为0.0
//可选类型的属性name，初始化值为nil
```

### Class and Structure Instances

类和结构体定义只是属性和功能的抽象化。所以我们需要创建类和结构体实例来具象化一个对象。

创建类和结构体的实例类似，如下
```swift
let someResolution = Resolution()
let someVideoMode = VideoMode()
//如上实例创建的方式都是"类名或者结构体名称+()"，
//实例拥有类的所有属性及其属性默认值。

```


### Accessing Properties

点语法（*dot syntax*）即`.`以下简称打点，
我们通过打点访问实例属性。如下例：

```swift
print("The width of someResolution is \(someResolution.width)")
// Prints "The width of someResolution is 0"
```
使用打点访问子属性，如：

```swift
print("The width of someVideoMode is \(someVideoMode.resolution.width)")
// Prints "The width of someVideoMode is 0"
```
使用打点给变量属性赋值，如下
```swift
someVideoMode.resolution.width = 1280
print("The width of someVideoMode is now \(someVideoMode.resolution.width)")
// Prints "The width of someVideoMode is now 1280"
```

>和OC不同的是，Swift能直接设置子属性的值。（像上面这个例子，不需要重新设置resolution的属性值）。



### Memberwise Initializers for Structure Types

所有的结构体都有一个***memberwise initializer***（逐个成员初始化）构造器用来初始化实例所有的属性值。如下例：

```swift
let vga = Resolution(width: 640, height: 480)
```
>注：类（class）没有这种构造器

---

## Structures and Enumerations Are Value Types
简单说一下*value type*（值类型），如果将一个值类型赋值给一个常量或者变量或者其本身作为函数参数的时候，使用的其实都是一个copy值。

像我们之前使用的基础数据类型比如`integers`, `floating-point numbers`, `Booleans`, `strings`, `arrays` 和 `dictionaries`都是值类型，底层都是通过结构体的形式实现的。

枚举和结构体也是值类型，意味着在代码中传递使用的值都是其属性值的拷贝。

以下用例子来说明：
```swift
let hd = Resolution(width: 1920, height: 1080)
//初始化一个hd的结构体实例
var cinema = hd
//将该实例值赋值给变量cinema

```
因为结构体是值类型，所以上面例子中的`cinema`事实上是`hd`的一个拷贝，虽然他们的值是一样的，但是是两个不同的实例。

如下例子，当改变`cinema`值的时候，`hd`的值不受影响。

```swift
cinema.width = 2048

print("cinema is now \(cinema.width) pixels wide")
// Prints "cinema is now 2048 pixels wide"

print("hd is still \(hd.width) pixels wide")
// Prints "hd is still 1920 pixels wide"
```
或者，当我们改变原实例的值，并不影响拷贝值。如下例：

```swift
enum CompassPoint {
    case north, south, east, west
}
var currentDirection = CompassPoint.west
let rememberedDirection = currentDirection
currentDirection = .east
if rememberedDirection == .west {
    print("The remembered direction is still .west")
}
// Prints "The remembered direction is still .west"
```

---

## Classes Are Reference Types

和值类型不同的是，引用类型被赋值给一个常量或者变量或者作为函数参数的时候，引用的是其实例本身，而不是拷贝值。

如下例，我们使用上面定义过的`VideoMode`类

```swift
//创建一个类实例tenEighty，并给实例属性初始化值
let tenEighty = VideoMode()
tenEighty.resolution = hd
tenEighty.interlaced = true
tenEighty.name = "1080i"
tenEighty.frameRate = 25.0
```
下面我们将实例赋值给一个变量，并改变属性值
```swift
let alsoTenEighty = tenEighty
alsoTenEighty.frameRate = 30.0
```
因为类是引用类型，所以上例中的`alsoTenEighty`和`tenEighty`仅仅是名字不同而已，引用的都是同一个实例。

下面我们打点访问原实例的属性值，发现其值随着`alsoTenEighty`的属性值的变化而变化。
```swift
print("The frameRate property of tenEighty is now \(tenEighty.frameRate)")
// Prints "The frameRate property of tenEighty is now 30.0"

```
>上面的`alsoTenEighty`和`tenEighty`都是常量而不是变量。然而仍然可以修改其属性值，因为`alsoTenEighty`和`tenEighty`仅仅是类`VideoMode`的一种引用，而并非存储`VideoMode`实例，属性值的修改并不影响这种引用关系，所以`alsoTenEighty`和`tenEighty`本质上是没有改变的。

### Identity Operators

Swift中为了检测两个变量或者常量是否引用自同一个实例，提供了两个运算符：
- *Identical to* 等价于 （`===`）
- *Not identical to* 不等价于（`!==`）
>该运算符只能作用于引用类型的实例比较

如下例子

```swift
if tenEighty === alsoTenEighty {
    print("tenEighty and alsoTenEighty refer to the same VideoMode instance.")
}
// Prints "tenEighty and alsoTenEighty refer to the same VideoMode instance."
```
注意：
- *Identical to*（`===`）是比较两个class类型的常量或者变量是否引用自同一个实例。
- *equal to*（`==`）是比较两个实例的值是否相等。

当你自定义一个类或者结构体的时候，你可以设定他们相等的标准规范，具体在后面会有介绍（[Equivalence Operators](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html#//apple_ref/doc/uid/TP40014097-CH27-ID45)）。

### Pointers

在C、C++、和Objective-C中使用指针指向实例的内存地址来表示这种引用。在Swift里面，这种被赋值给引用类型实例的常量或者变量和指针类似，但却没有像指针一样指向内存地址，所以和其他的变量或者常量的定义方式一样，不需要在定义的时候使用*标明该变量是引用。

---

## Choosing Between Classes and Structures

在swift中，你可以使用类和结构能创建一些自定义的数据模型。结构体是值类型，类是引用类型，所以在创建数据模型的时候你要充分考虑该使用结构体还是类类型。

通常以下情况，如果我们创建的数据结构符合以下一条或多条条件时，我们会创建结构体类型：

- 数据结构主要用于封装少量简单的数据
- 当把数据结构实例对象赋值给常量或者变量的时候，使用是拷贝值而不是值引用
- 数据结构的所有属性都是值类型。
- 数据结构不需要继承其他类型的某些属性或者特征

下面举几个适合用结构体的例子：
1. 图形的高（height）和宽（width）,都是`double`类型
1. 路径范围，包含开始（start）和结束(end)，都是`int`类型
1. 3D模型中，坐标`x`、`y`、`z`,都是`double`类型


如果一个实例使用引用来管理值和传递值，那么该实例是一个类（class）类型。



---
## Assignment and Copy Behavior for Strings-Arrays-and-Dictionaries

在Swift中，许多基础数据类型如`String`, `Array`, `Dictionary` 都是通过结构体来实现的，所以它们被赋值的时候使用的是拷贝值而不是引用值。

而`NSString`, `NSArray`, `NSDictionary`是通过类来实现的，所以实例赋值使用的是值引用

下面是系统中`NSString`和`String`的定义。
```swift
open class NSString : NSObject, NSCopying, NSMutableCopying, NSSecureCoding {

    open var length: Int { get }

    open func character(at index: Int) -> unichar

    public init()

    public init?(coder aDecoder: NSCoder)
}
```

```swift
public struct String {
    //...
    //略
}
```
> 在Swift中，只会在必要的时候创建一个copy值，并不是在每次赋值或者传递操作时都创建copy值，系统会自动管理copy值，并做了优化，所以你不用担心因为赋值而影响系统性能。
