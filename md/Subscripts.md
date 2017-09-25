# Subscripts

---
## 目录

- [Subscript Syntax-下标语法](#subscript-syntax)
- [Subscript Usage-下标用途](#subscript-usage)
- [Subscript Options-下标选项](#subscript-options)

---
## 前言

类、结构体和枚举类型都能定义下标来实现快速访问集合、列表、序列的元素。我们可以通过索引快速获取值而不需要单独的存取方法，比如通过`someArray[index]`获取数组元素和`someDictionary[key]`获取字典值。


一个类型（类型、结构体、枚举）可以定义多个下标，Swift会根据下标的输入参数类型和数量来推断使用哪个下标。下标的输入参数也可以根据需要为多个参数（具体见下面二维矩阵）。

---
## Subscript Syntax
下标语法，即使用实例名称后面跟方括号的方式来进行实例的读写操作，定义和实例方法、就算属性相似，使用关键字`subscript`修饰，有一个或多个输入参数和一个返回类型。与实例方法不同的是，下标可以像计算属性一样通过`getter`和`setter`来设定读写操作或者只读操作。
举个例子：

```swift
subscript(index: Int) -> Int {
    get {
        // return an appropriate subscript value here
    }
    set(newValue) {
        // perform a suitable setting action here
    }
}
```
如果`set`方法中没有传入参数的话，系统会默认一个参数`newValue`代表新值。

和只读计算属性一样，我们只读下标语法亦可简写为：


```swift
subscript(index: Int) -> Int {
    // return an appropriate subscript value here
}
```

下面我们举一个只读下标的实现，定义一个`TimesTable`的结构体来实现乘法口诀。

```swift
struct TimesTable {
    let multiplier: Int
    subscript(index: Int) -> Int {
        return multiplier * index
    }
}
let threeTimesTable = TimesTable(multiplier: 3)
print("six times three is \(threeTimesTable[6])")
// Prints "six times three is 18"
```
上面的这个结构体可以通过初始化`multiplier`为`3`，很方便的通过下表来实现`3`的倍数计算，比如`threeTimesTable[6]`计算`3*6`即`18`


---
## Subscript Usage

下标的确切含义是由使用环境而决定的，而通常下标用来快捷的获取集合、列表和序列的成员。你也可以给自定义的类或者结构体设置下标来实现一些功能。

举个例子，Swift里面的`Dictionary`类型通过实现下标来存取键值对，你可以通过下标来设置键值，也可以通过下标获取值。如下


```swift
var numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
numberOfLegs["bird"] = 2
```

上面的例子定义了一个`[String:Int]`类型的`Dictionary`类型，通过下标给`numberOfLegs`添加了一个新的键值对`"bird":2`


> `Dictionary`使用下标返回的是一个`optional`(可选)类型的值，因为不是每个键都有对应的值，下标也给我门提供了一种通过键来删除键值的方式。

---
## Subscript Options

下标可以有多个输入参数，输入参数可以为任何类型，也能返回任何类型的值。下标可以使用变量参数，但是不能使用输入输出参数（ *in-out parameters*）或者给参数设置默认值
。

类和结构体可以提供多个下标的实现，系统可以通过下标的输入参数类型和数量来推断使用哪个下标（这个有点像函数的推断），这个定义了多个下标的实现我们称之为下标重载（*subscript overloading*）

一般情况下下标只有一个参数，但是也可以根据情况定义多个参数，如下例多参数的下标实现。


```swift
struct Matrix {
    let rows: Int, columns: Int
    var grid: [Double]
    init(rows: Int, columns: Int) {
        self.rows = rows
        self.columns = columns
        grid = Array(repeating: 0.0, count: rows * columns)
    }
    func indexIsValid(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && column >= 0 && column < columns
    }
    subscript(row: Int, column: Int) -> Double {
        get {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            return grid[(row * columns) + column]
        }
        set {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            grid[(row * columns) + column] = newValue
        }
    }
}

```

我们提供上面的结构体`Matrix`来实现一个二维矩阵。

首先，我们来初始化一个结构体
```swift
var matrix = Matrix(rows: 2, columns: 2)

```
通过上面的初始化给结构体的属性赋值

```swift
rows = 2
columns = 2
guid = [0.0,0.0,0.0,0.0]
```
然后我们通过下标实现矩阵赋值操作

```swift
matrix[0, 1] = 1.5
matrix[1, 0] = 3.2
```
事实上是执行下标的set方法


```swift
grid[(row * columns) + column] = newValue
//即
grid[(0 * 2) + 1] = 1.5
grid[(1 * 2) + 0] = 3.2
//即
grid[1] = 1.5
grid[2] = 3.2
```
则`guid`变为

```swift
grid = [0.0,1.5,3.2,0.0]
```
即矩阵

![image](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/subscriptMatrix02_2x.png)

在上例中我们看到`getter`和`setter`中都含有断言（*assert*），就是验证矩阵下标合法性的布尔表达式

```swift
func indexIsValid(row: Int, column: Int) -> Bool {
    return row >= 0 && row < rows && column >= 0 && column < columns
}
//仅仅当0<row<2,且0<column<2的时候返回为真，否则返回false

```
比如设置超出范围的下标的时候

```swift
let someValue = matrix[2, 2]
// this triggers an assert, because [2, 2] is outside of the matrix bounds
//断言触发，提示超出范围。

```

