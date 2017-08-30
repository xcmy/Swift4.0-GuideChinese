# Properties
---
## 目录


- [Stored Properties-存储属性](#stored-properties)
  - [Stored Properties of Constant Structure Instances-结构体实例常量存储属性](#stored-properties-of-constant-structure-instances)
  - [Lazy Stored Properties-延迟存储属性](#lazy-stored-properties)
  - [Stored Properties and Instance Variables-存储属性和实例变量](#stored-properties-and-instance-variables)
- [Computed Properties-计算属性](#computed-properties)
  - [Shorthand Setter Declaration-Set简写语法](#shorthand-setter-declaration)
  - [Read-Only Computed Properties-只读计算属性](#read-only-computed-properties)
- [Property Observers-属性观察器](#property-observers)
- [Global and Local Variables-全局变量和局部变量](#global-and-local-variables)
- [Type Properties-类型属性](#type-properties)
  - [Type Property Syntax-类型属性语法](#type-property-syntax)
  - [Querying and Setting Type Properties-类型属性的读写](#querying-and-setting-type-properties)



---
## 前言

属性（*Properties*）就是把值跟特定的类、结构体、枚举给关联起来。存储属性用来存储变量或者常量作为实例的一部分；而计算属性是计算一个值（不是存储）。

- 计算属性用于类、结构体、枚举。
- 存储属性用于类、结构体。（枚举中我们使用枚举值，而不是存储属性）

属性既可以和实例关联作为实例属性，也可以和类型关联作为类型属性（具体下面会有介绍）。

我们还可以通过属性观察器监听属性值的变化实现不同的操作。既可以给自定义的属性添加，也可以给继承属性添加。




---


## Stored Properties
存储属性，简单的来说就是存储在类或者结构体实例中的一个常量或者变量。存储属性可以是变量存储属性也可以是常量存储属性（分别用`var`和`let`声明）

我们可以给存储属性设定默认值，也可以在初始化的时候修改和设定存储属性的值（包括常量存储属性）

下面举个例子，创建一个结构体`FixedLengthRange`表示一个数字范围。

```swift
struct FixedLengthRange {
    var firstValue: Int
    let length: Int
}
var rangeOfThreeItems = FixedLengthRange(firstValue: 0, length: 3)
// the range represents integer values 0, 1, and 2
rangeOfThreeItems.firstValue = 6
// the range now represents integer values 6, 7, and 8

```

如上，创建变量实例的时候我们给属性赋值，因为`length`是常量属性，故不能通过打点赋值。


### Stored Properties of Constant Structure Instances

如果我们创建了一个常量结构体实例，那么它的属性是不能修改的，即使该属性是变量属性。

```swift
let rangeOfFourItems = FixedLengthRange(firstValue: 0, length: 4)
// this range represents integer values 0, 1, 2, and 3
rangeOfFourItems.firstValue = 6
//报错
// this will report an error, even though firstValue is a variable property
```
在上一节介绍中了常量类实例的属性是可以修改的，而这儿常量结构体实例的属性是不能修改的，因为类是引用类型，结构体是值类型，所以初始化一个常量结构体实例的时候，实例的属性都被常量化，不可修改。

### Lazy Stored Properties

延迟存储属性，就是当第一次被调用的时候才会计算初始值。使用关键字`lazy`来声明。

>因为常量存储属性只能在初始化之前完成赋值，所以`lazy`不能修饰常量存储属性。

对于那些对外部因素依赖较大，在初始化完成之前无法获取到值的属性；或者属性值需要在特定条件下进行复杂或者大量的计算才能获取的属性；延迟属性是很有用的。

下面举个例子用延迟属性来避免复杂类不必要的初始化。

```swift
class DataImporter {
    /*
     DataImporter is a class to import data from an external file.
     The class is assumed to take a nontrivial amount of time to initialize.
     这个类的主要作用就是数据导入；
     初始化可能需要花费大量的时间。
     */
    var filename = "data.txt"
    // the DataImporter class would provide data importing functionality here
    //这儿将会实现数据导入功能
}
 
class DataManager {
    lazy var importer = DataImporter()
    var data = [String]()
    // the DataManager class would provide data management functionality here
    //这个类主要用于数据管理
}
 
let manager = DataManager()
manager.data.append("Some data")
manager.data.append("Some more data")
// the DataImporter instance for the importer property has not yet been created
//manager的属性importer是类DataImporter的一个实例
//因为被声明为延迟属性，所以虽然初始化了manager实例，但是DataImporter实例还没有被创建
```
如上面所示，当`DataManager`要实现导入文件功能的时候就需要初始化属性`importer`，即一个`DataImporter`实例，而个`DataImporter`实例的初始化可能需要耗费大量的时间；而`DataManager`如果仅仅用来管理自己内部的数据的，不需要导入文件的时候，也没有必要初始化属性`importer`。

所以我们使用延迟属性来声明`importer`，只有当被调用的时候才会创建（如下）。

```swift
print(manager.importer.filename)
// the DataImporter instance for the importer property has now been created
// Prints "data.txt"
```

> 延迟属性在没有初始化的情况下若被多线程同时调用，不能保证只初始化一次。

```swift
//创建一个并发队列，分别调用importer属性
let concurrentQueue = DispatchQueue(label: "name", attributes: .concurrent)
concurrentQueue.async {
    manager.importer.filename = "1"
    print(manager.importer.filename)
}
concurrentQueue.async {
    manager.importer.filename = "2"
    print(manager.importer.filename)
}
concurrentQueue.async {
    manager.importer.filename = "3"
    print(manager.importer.filename)
}
concurrentQueue.async {
    manager.importer.filename = "4"
    print(manager.importer.filename)
}
sleep(1)
print("\(manager.importer.filename)")
```

### Stored Properties and Instance Variables

我们看一下*Objective-C*里面的属性声明：

```Objective-C
//声明一个属性myButton
@property (nonatomic, retain) UIButton *myButton;
//编译器自动生成实例变量 _myButton
```
简单说下，*Objective-C*里面的`@property`声明的是属性，并不是实例变量。如果在`interface`里声明了`@property`，编译器会根据属性自动生成实例变量（比如上面的`_myButton`就是属性`myButton`自动生成的实例变量）和对应的`getter`和`setter`方法。所以*Objective-C*里面的点语法其实本质上是`getter`和`setter`的调用。所以在*Objective-C*里面，事实上我们是通过实例变量来存取属性值的。


而*Swift*去除了*Objective-C*里面属性这些繁复的概念，使用一个简单的属性定义语句来实现，所有的信息包括名字、类型、存储特征都在定义语句中实现。避免了不同环境下获取方式的困扰。


---
## Computed Properties

除存储属性外，类、结构体和枚举类型都能定义计算属性（*computed properties*），计算属性实际上并不存储值，而是提供一个`getter`方法和一个可选的`setter`方法来间接设置其他的属性和值。

下面举个计算坐标系中一个矩形的中心点和原点的例子。

```swift
//定义一个点
struct Point {
    var x = 0.0, y = 0.0
}
//定义一个长和宽
struct Size {
    var width = 0.0, height = 0.0
}
//定义一个矩形，两个存储属性长和宽，一个计算属性center(矩形中心点)
struct Rect {
    var origin = Point()
    var size = Size()
    var center: Point {
        //通过宽高和原点计算中心点
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        //通过中心点和宽高计算原点
        set(newCenter) {
            origin.x = newCenter.x - (size.width / 2)
            origin.y = newCenter.y - (size.height / 2)
        }
    }
}
var square = Rect(origin: Point(x: 0.0, y: 0.0),
                  size: Size(width: 10.0, height: 10.0))
let initialSquareCenter = square.center
square.center = Point(x: 15.0, y: 15.0)
print("square.origin is now at (\(square.origin.x), \(square.origin.y))")
// Prints "square.origin is now at (10.0, 10.0)"
```

![image](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/computedProperties_2x.png)

如上将矩形抽象为结构体`Rect`,定义两个变量属性`origin`(原点)和`size`（长和宽），并定义一个计算属性`center`（中心点）,上面的例子中我们给计算属性`center`提供了一个`get`方法获取值和一个`set`方法设置其他属性值。所以我们只要给矩形初始化一个原点和长宽就能通过`center`属性计算出中心点，或者通过中心点和长宽设定矩形的原点。

### Shorthand Setter Declaration

我们看下上面的例子中`set`方法
```swift
set(newCenter) {
    origin.x = newCenter.x - (size.width / 2)
    origin.y = newCenter.y - (size.height / 2)
}
```
如上`set`方法定义了一个形参`newCenter`,代表新的中心点，并在方法内使用它计算原点。事实上，我们可以省去`newCenter`的定义，用一个默认值`newValue`来表示，这样`set`方法也可简写为：


```swift
 set {
    origin.x = newValue.x - (size.width / 2)
    origin.y = newValue.y - (size.height / 2)
}
```

### Read-Only Computed Properties

如果一个计算属性只有`getter`方法而没有`setter`方法，我们称其为只读计算属性。我们能通过点语法来调用获取值，却不能给其设置值。

>计算属性通常都用`var`来声明，因为计算属性的值往往是不固定的。而`let`声明的常量值在初始化之后就不能改变了，所以不能用。

下面我们举个计算立方体容积例子
```swift
struct Cuboid {
    var width = 0.0, height = 0.0, depth = 0.0
    var volume: Double {
        return width * height * depth
    }
}
let fourByFiveByTwo = Cuboid(width: 4.0, height: 5.0, depth: 2.0)
print("the volume of fourByFiveByTwo is \(fourByFiveByTwo.volume)")
// Prints "the volume of fourByFiveByTwo is 40.0"

//如果给只读属性设置值则会报错"Cannot assign to property: 'volume' is a get-only property"
fourByFiveByTwo.volume = 10

```



---

## Property Observers
属性观察器（*Property observers*）用来观察和监听属性值的变化，一旦属性值被重新赋值都会被调用（无论新值和旧值是否相等）。

你可以给任何除了延迟属性（*lazy stored properties*）外的存储属性添加属性观察器。也可以给子类的重写的继承属性添加属性观察器；在这里说一下，没必要为非重写的计算属性添加属性观察器，因为计算属性值的变化我们可以通过`setter`方法来监听。

你可以给属性添加一个或者两个属性观察器：
- `willSet`在属性值存储前被调用
- `didSet`在新值存储完成后调用

另外

- 在`willSet`观察器中，默认使用常量参数`newValue`表示新值；
- 在`didSet`观察器中，默认使用常量参数`oldValue`表示旧值；

> 如果子类在初始化构造器中给父类属性赋值，父类的`willSet`和`didSet`观察器会在父类初始化之后被调用;如果子类是给自己的属性赋值，则不会调用属性观察器方法。



```swift
class father {
    init() {
        print("父类初始化")
    }
    var totalSteps: Int = 0 {
        willSet(newTotalSteps) {
            print("--\(totalSteps)")
        }
        didSet {
            print("--\(totalSteps)")
        }
    }
}
class son:father{
    override init() {
        self.byp = 4
        super.init()
        super.totalSteps = 3
        
    }
    var byp:Int = 3
}
_ = son.init()

//依次输出
//父类初始化
//--0
//--3

```

下面我们举个日常步数统计器的例子来说下`willSet`和`didSet`观察器。如下：

```swift
class StepCounter {
    var totalSteps: Int = 0 {
        willSet(newTotalSteps) {
            print("About to set totalSteps to \(newTotalSteps)")
        }
        didSet {
            if totalSteps > oldValue  {
                print("Added \(totalSteps - oldValue) steps")
            }
        }
    }
}
let stepCounter = StepCounter()
stepCounter.totalSteps = 200
// About to set totalSteps to 200
// Added 200 steps
stepCounter.totalSteps = 360
// About to set totalSteps to 360
// Added 160 steps
stepCounter.totalSteps = 896
// About to set totalSteps to 896
// Added 536 steps
```

上面的步数统计例子中，我们看到，`willSet`比`didSet`先调用；当属性被赋值的时候，`willSet`观察器输出将要走到多少步，`didSet`输出相比于之前增加了多少步数。

> 如果类属性是作为函数的`in-out`参数，`willSet`和`didSet`也会被调用,虽然在函数中使用的是拷贝值，但是在函数结束的时候会被重新赋值。


---
## Global and Local Variables

上面提到的属性的计算和观察器同样适用于全局变量和局部变量。

- 全局变量即定义在函数、方法、闭包外的变量。
- 局部变量即定义在函数、方法、闭包内的变量。

我们之前遇到的全局变量和局部变量都是存储变量，和存储属性一样，能够存储特定数据类型值，而且可以读和写。

如下，全局变量和局部变量定义计算和观察器，写法和属性一样：



```swift
var totalSteps: Int = 0 {
    willSet(newTotalSteps) {
        print("About to set totalSteps to \(newTotalSteps)")
    }
    didSet {
        print("Added \(oldValue) steps")
    }
}
print("\(totalSteps)")
totalSteps = 12
print("\(totalSteps)")
```

```swift
var center: Int  {
    get {
        return totalSteps
    }
    set {
        totalSteps = newValue-6
    }
}
center = 19
print("\(center)")
print("\(totalSteps)")
```

> 全局的常量或者变量都是延迟计算的，和延迟存储属性不同的是，不需要使用关键字`lazy` ；局部变量和常量从不延迟计算。

---

## Type Properties

实例属性（*Instance properties *）是属于实例的一个特定类型的属性。每一个实例都有其自己的属性值。


我们也可以创建属于类型自己的属性，无论该类型被创建了多少实例，该属性都只有一个。这种属性我们称为类型属性（*type properties*）。

类型属性用来定义所有实例共有的特定类型值。比如C里面的静态常量（所有实例都能用的常量属性）；C里面的静态变量（所有实例都能访问的一个变量）。

存储类型属性可以是常量也可以是变量。计算类型属性同计算实例属性一样，必须是变量属性。

> 和存储实例属性不同的是，存储类型属性必须在定义的时候设定默认值。因为存储类型属性没有实例化构造器，也无法通过属性赋值来设定值。  
>存储类型属性是延迟初始化的（不需要使用关键字`lazy`），在被访问（不论是单线程访问还是多线程访问）的时候仅仅初始化一次。

### Type Property Syntax

在C和Objective-C中，一个类型相关联的静态常量和变量是作为全局静态变量定义的。而Swift的类型属性是定义在类型括号内的，作用范围在类型所支持的范围内。

类型属性使用关键字`static`来定义。再给类类型定义计算属性的时候，可以使用关键字`class`来代替实现子类对父类的重写。

举例：

```swift
struct SomeStructure {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 1
    }
}
enum SomeEnumeration {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 6
    }
}
class SomeClass {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 27
    }
    class var overrideableComputedTypeProperty: Int {
        return 107
    }
}
```

> 上面的例子中的计算类型属性都是只读的，也可以定义为可读写的属性，写法跟计算属性一样。

### Querying and Setting Type Properties

类型属性的读写均使用点语法，不同于实例属性的是，使用类名来调用。如下例：

```swift
print(SomeStructure.storedTypeProperty)
// Prints "Some value."
SomeStructure.storedTypeProperty = "Another value."
print(SomeStructure.storedTypeProperty)
// Prints "Another value."
print(SomeEnumeration.computedTypeProperty)
// Prints "6"
print(SomeClass.computedTypeProperty)
// Prints "27"
```

下面举个例子，如何用两个声道模拟立体声的音量。如下图：
![image](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/staticPropertiesVUMeter_2x.png)


我们抽象一个声道模型，如上左声道和右声道的最大值（或者阈值）都是10，初始默认值都是0，当前左声道为9，右声道为7。如下我们创建一个结构体模型：

```swift
struct AudioChannel {
    //类型常量属性：阈值为10
    static let thresholdLevel = 10
    //类型变量属性：所有声道输入最大音量
    static var maxInputLevelForAllChannels = 0
    var currentLevel: Int = 0 {
        didSet {
            if currentLevel > AudioChannel.thresholdLevel {
                // cap the new audio level to the threshold level
                //若某个声道当前音量超过阈值，则设置为阈值
                currentLevel = AudioChannel.thresholdLevel
            }
            if currentLevel > AudioChannel.maxInputLevelForAllChannels {
                // store this as the new overall maximum input level
               //若某个声道当前音量大于maxInputLevelForAllChannels，则将maxInputLevelForAllChannels设为当前音量  
               AudioChannel.maxInputLevelForAllChannels = currentLevel
            }
        }
    }
}
```

然后我们创建两个实例左声道和右声道：

```swift
var leftChannel = AudioChannel()
var rightChannel = AudioChannel()
```
然后当左声道或者右声道当前音量变化的时候，根据输入音量修改类型存储属性`maxInputLevelForAllChannels`的值。

```swift
leftChannel.currentLevel = 7
print(leftChannel.currentLevel)
// Prints "7"
print(AudioChannel.maxInputLevelForAllChannels)
// Prints "7"

```
当左声道或者右声道当前音量超过10的时候，就会`maxInputLevelForAllChannels`的值设定为`thresholdLevel`的值

```swift
rightChannel.currentLevel = 11
print(rightChannel.currentLevel)
// Prints "10"
print(AudioChannel.maxInputLevelForAllChannels)
// Prints "10"

```