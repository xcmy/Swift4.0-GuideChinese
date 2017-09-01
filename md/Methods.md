# Methods

---
## 目录

- [Instance Methods-实例方法](#instance-methods)
  - [The self Property-](#the-self-property)
  - [Modifying Value Types from Within Instance Methods-](#modifying-value-types-from-within-instance-methods)
  - [Assigning to self Within a Mutating Method](#assigning-to-self-within-a-mutating-method)
- [Type Methods-类型方法](#type-methods)

---
## 前言

在Objective-C中我们可以给类（*class*）定义加号方法（类方法）和减号方法（实例方法）。而在Swift中我们把这些跟类型相关的函数都称为方法（*Methods*），不同的是Swift不但能给类（*class*）定义方法，而且能给枚举类型和结构体定义方法。Swift中的Methods也分为实例方法和类方法。相比于
Objective-C非常的灵活。
---

## Instance Methods

实例方法就是类型实例才调用的函数，写在类型括号里面，默认能够访问所有的类型属性和其他的实例方法，写法和函数定义一样。可以用来实现实例的属性读写或者其的它实例相关功能。

如下例：

```Swift
class Counter {
    var count = 0
    func increment() {
        count += 1
    }
    func increment(by amount: Int) {
        count += amount
    }
    func reset() {
        count = 0
    }
}

```
👆的例子中`Counter`类定义了三个实例方法，均用来修改`count`属性

- `increment()`实现`count`属性值加1
- `increment(by: Int)`实现`count`属性值加输入值
- `reset()`实现`count`属性值清零

实例方法和属性一样，通过点语法调用。如下例

```Swift
let counter = Counter()
// the initial counter value is 0
counter.increment()
// the counter's value is now 1
counter.increment(by: 5)
// the counter's value is now 6
counter.reset()
// the counter's value is now 0
```

>事实上方法就是函数，只不过跟类型相关联而已，函数有的功能方法都有。

### The self Property

每个实例都默认有一个`self`属性，代表实例本身。
比如上面的方法也可以写为
```Swift
func increment() {
    self.count += 1
}
```
一般情况下在方法内不必写`self`,系统会默认推断为实例的属性或者方法，就像上面的`count`。

但是如果方法的参数名和实例的属性名称一样，就需要使用`self`来区分属性和参数了。如下例子

```Swift
struct Point {
    var x = 0.0, y = 0.0
    func isToTheRightOf(x: Double) -> Bool {
        return self.x > x
        //self.x代表实例属性
        //x代表方法参数
    }
}
let somePoint = Point(x: 4.0, y: 5.0)
if somePoint.isToTheRightOf(x: 1.0) {
    print("This point is to the right of the line where x == 1.0")
}
// Prints "This point is to the right of the line where x == 1.0"
```

### Modifying Value Types from Within Instance Methods
枚举和结构体是值类型，默认情况下，其属性不能在方法内被修改。然而，如果实在需要修改，可以在关键字`mutating`修饰的方法内修改其属性值，系统会在方法结束的时候改变方法内作出的改变，并使用新的实例替换原来的实例。

如下面例子：


```Swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}
var somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveBy(x: 2.0, y: 3.0)
print("The point is now at (\(somePoint.x), \(somePoint.y))")
// Prints "The point is now at (3.0, 4.0)"
```

>当然修改实例属性值是建立在变量实例的基础上，一个常量实例的属性是不能修改的，因为其属性在初始化的时候也被初始化为常量。

```Swift
let fixedPoint = Point(x: 3.0, y: 3.0)
fixedPoint.moveBy(x: 2.0, y: 3.0)
// this will report an error

```

### Assigning to self Within a Mutating Method

上面的例子本质上是在方法内创建了一个新的实例替换了原来的实例。事实上我们也可以通过重新初始化给`self`属性一个新的实例。所以上面的例子同样可以写为：

```Swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}

```

在枚举类型中，`self`可以代表一个实例的不同枚举值。如下例：
```Swift
enum TriStateSwitch {
    case off, low, high
    mutating func next() {
        switch self {
        case .off:
            self = .low
        case .low:
            self = .high
        case .high:
            self = .off
        }
    }
}
var ovenLight = TriStateSwitch.low
ovenLight.next()
// ovenLight is now equal to .high
ovenLight.next()
// ovenLight is now equal to .off
```
上面实现了一个三态开关，通过调用`next()`方法实现三种状态的循环切换。


---
## Type Methods

上面介绍了实例方法，下面来介绍*type methods*(类型方法)。我们通过关键字`static`和`class`来定义类方法。不同的是`class`关键字允许子类重写父类方法的实现。

>不同于Objective-C的是，Swift可以给类（class）、结构体、枚举均能定义类型方法。


类型方法的调用和实例方法类似，只不过必须通过类型来调用。如下

```Swift
class SomeClass {
    class func someTypeMethod() {
        // type method implementation goes here
    }
}
SomeClass.someTypeMethod()
```

在类型方法中，`self`代表类型本身，同样我们使用`self`来消除类型属性和类型方法参数名称一致的歧义问题。

在没有歧义的情况下，我们可以在类型方法体内直接使用属性名称和通过方法名称调用，而不需要使用类型名来打点调用。


下面举个例子，一个游戏，所有玩家初始化等级为1，只要有一个玩家完成下一个等级，该等级对所有玩家解锁。

下面定义一个结构体来设定等级解锁：
```Swift
struct LevelTracker {
    static var highestUnlockedLevel = 1
    var currentLevel = 1
    
    static func unlock(_ level: Int) {
        if level > highestUnlockedLevel { highestUnlockedLevel = level }
    }
    
    static func isUnlocked(_ level: Int) -> Bool {
        return level <= highestUnlockedLevel
    }
    
    @discardableResult
    mutating func advance(to level: Int) -> Bool {
        if LevelTracker.isUnlocked(level) {
            currentLevel = level
            return true
        } else {
            return false
        }
    }
}

```
和玩家定义
```Swift
class Player {
    var tracker = LevelTracker()
    let playerName: String
    func complete(level: Int) {
        LevelTracker.unlock(level + 1)
        tracker.advance(to: level + 1)
    }
    init(name: String) {
        playerName = name
    }
}
```


```Swift
var player = Player(name: "Argyrios")
player.complete(level: 1)
print("highest unlocked level is now \(LevelTracker.highestUnlockedLevel)")
// Prints "highest unlocked level is now 2"

```

初始化一个玩家，当该玩家完成等级1的时候，调用解锁工具，等级2被解锁。

```Swift
player = Player(name: "Beto")
if player.tracker.advance(to: 6) {
    print("player is now on level 6")
} else {
    print("level 6 has not yet been unlocked")
}
// Prints "level 6 has not yet been unlocked"
```

若另一个玩家开始一个没有被解锁的等级，则会失败，如下：

```swift
player = Player(name: "Beto")
if player.tracker.advance(to: 6) {
    print("player is now on level 6")
} else {
    print("level 6 has not yet been unlocked")
}
// Prints "level 6 has not yet been unlocked"
```