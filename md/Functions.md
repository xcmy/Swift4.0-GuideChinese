## Functions  方法

#### 目录

- [Defining and Calling Functions-函数定义和函数调用](#defining-and-calling-functions)
- [Function Parameters and Return Values-函数参数和返回值](#function-parameters-and-return-values)
  - [Functions Without Parameters-无参数函数](#functions-without-parameters)
  - [Functions With Multiple Parameters-多参数函数](#functions-with-multiple-parameters)
  - [Functions Without Return Values-无返回值函数](#functions-without-return-values)
  - [Functions with Multiple Return Values-多返回值函数](#functions-with-multiple-return-values)
  - [Optional Tuple Return Types-可选元组类型作为返回类型](#optional-tuple-return-types)
- [Function Argument Labels and Parameter Names-函数参数标签和参数名](#function-argument-labels-and-parameter-names)
  - [Specifying Argument Labels-特定的参数标签](#specifying-argument-labels)
  - [Omitting Argument Labels-省略参数标签](#omitting-argument-labels)
  - [Default Parameter Values-默认参数值](#default-parameter-values)
  - [Variadic Parameters-可变参数](#variadic-parameters)
  - [In-Out Parameters-输出参数](#in-out-parameters)

- [Function Types-函数类型](#function-types)
  - [Using Function Types-使用函数类型](#using-function-types)
  - [Function Types as Parameter Types-函数类型作为参数类型](#function-types-as-parameter-types)
  - [Function Types as Return Types-函数类型作为返回类型](#function-types-as-return-types)
- [Nested Functions-嵌套函数](#nested-functions)

---
## 前言

***Functions***(方法、函数，以下简称函数)就是一段封装起来去做一个特定的任务的代码块。每个函数都有它的名字，调用的使用会使用到函数名。  
Swift的func语法很灵活，既能实现像C风格的没有参数名称的简单函数，也能实现复杂的Objective-C风格的含有参数标签的函数。



---

## Defining and Calling Functions

函数的定义和调用。Swift函数定义时，可定义一个或多个*parameters*（参数）。也可定义一个*return type*（返回类型）。

每个函数都有它的名字，一般用函数的功能作用来命名。通过函数的名字和输入值来调用函数。输入值必须和函数定义的参数类型和顺序一致。



一个函数基本格式如下，关键字`func`加函数名，后括号内定义函数参数名和参数类型，在`->`后面设置函数返回类型，在`{}`内写函数执行语句，若定义有返回类型，则必须返回值。

```Swift
func func_name(parameter_name:parameter_type,parameter_name:parameter_type...) -> return_type {

    statument
    return value
}
```

如下例函数`greet(person:)`,定义了一个`String`类型的参数`person`和一个`String`类型的返回值`greeting`.


```swift
func greet(person: String) -> String {
    let greeting = "Hello, " + person + "!"
    return greeting
}
```

函数调用

```
print(greet(person: "Anna"))
// Prints "Hello, Anna!"
print(greet(person: "Brian"))
// Prints "Hello, Brian!"
```

因为👆的函数有一个String类型的返回值，所以可以直接用 `print(_:separator:terminator:)`来输出函数的返回值。

函数是可以被多次调用的，返回结果根据输入参数的变化有关。上面的函数也可以简写为：

```
func greetAgain(person: String) -> String {
    return "Hello again, " + person + "!"
}
print(greetAgain(person: "Anna"))
// Prints "Hello again, Anna!"

```


## Function Parameters and Return Values
***函数的参数和返回值***。
在Swift中，函数参数和返回值定义是非常灵活的，你可以根据不同的需求定义简单或者复杂的函数

### Functions Without Parameters

函数的参数不是必须的，如下函数就没有输入参数。

```
func sayHelloWorld() -> String {
    return "hello, world"
}
print(sayHelloWorld())
// Prints "hello, world"

```
就算函数没有输入参数，但是函数名后面的括号是必须要的，定义的时候需要，调用的时候也需要。



### Functions With Multiple Parameters

函数可以有多个输入参数，写在`()`里面，用`,`号隔开，如下示例：

```
func greet(person: String, alreadyGreeted: Bool) -> String {
    if alreadyGreeted {
        return greetAgain(person: person)
    } else {
        return greet(person: person)
    }
}
print(greet(person: "Tim", alreadyGreeted: true))
// Prints "Hello again, Tim!"
```

函数` greet(person:)`和函数`greet(person:alreadyGreeted:)`的函数名是一样的，但是携带参数是不一样的，调用的时候我们可以用携带的参数来区别。


### Functions Without Return Values

函数的返回值也不是必须的，如下示例

```
func greet(person: String) {
    print("Hello, \(person)!")
}
greet(person: "Dave")
// Prints "Hello, Dave!"
```

如上函数因为没有返回值，所以省略了 `->` .
> 严格的说，即时没有返回值，我们也需要定义一个`Void`类型的返回值，简单来说就是一个`()`（空元组），所以上面的方法也可写成`func greet(person: String) -> () {}`，看个人所好。

函数有返回值，但是我们既可以用也可以不用，如下例

```
func printAndCount(string: String) -> Int {
    print(string)
    return string.count
}
func printWithoutCounting(string: String) {
    let _ = printAndCount(string: string)
}
printAndCount(string: "hello, world")
// prints "hello, world" and returns a value of 12
printWithoutCounting(string: "hello, world")
// prints "hello, world" but does not return a value
```
如上两个函数都输出了`hello world`，达到了我们的目的，虽说第一个函数有返回值，但是并没有用到。

> 一旦函数定义了返回类型，必须在函数底部返回值，不然会报编译错误

### Functions with Multiple Return Values

函数返回多个返回值是通过返回元组类型来实现的。

如下函数返回数组的最大值和最小值：

```
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```
函数`minMax(array:) `返回了一个包含两个Int元素的元组,我们在函数中给这两个元素设置了标签，将来可以通过这个标签来获取各个返回值。如下

```
let bounds = minMax(array: [8, -6, 2, 109, 3, 71])
print("min is \(bounds.min) and max is \(bounds.max)")
// Prints "min is -6 and max is 109"
```

### Optional Tuple Return Types

如果函数的返回值有可能为nil，我们可以设置一个可选（Optional）类型的元组作为函数的返回值。比如`(Int, Int)?` 或者` (String, Int, Bool)?`

> 注意可选元组类型`(Int, Int)?`和元组元素为可选类型(Int?, Int?)是不一样的。

像上面计算数组最大值和最小值的函数，如果输入的参数为空数组`[]`，就会在`var currentMin = array[0]`这儿报错。为了保证输入值的安全性，我们需要定义一个返回类型为可选的元组类型，当输入参数为空数组的时候返回`nil`。如下

```
func minMax(array: [Int]) -> (min: Int, max: Int)? {
    if array.isEmpty { return nil }
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```

你可以使用`if`或者`guard`去判断返回值是否为`nil`

```
if let bounds = minMax(array: [8, -6, 2, 109, 3, 71]) {
    print("min is \(bounds.min) and max is \(bounds.max)")
}
// Prints "min is -6 and max is 109"

```

## Function Argument Labels and Parameter Names

函数的每个参数都有它的参数标签和参数名，参数标签在函数调用的时候使用，参数名在声明定义函数的时候使用，一般情况下，我们默认使用参数名作为参数标签。如下

```
//这儿的firstParameterName、secondParameterName为参数名
func someFunction(firstParameterName: Int, secondParameterName: Int) {
    // In the function body, firstParameterName and secondParameterName
    // refer to the argument values for the first and second parameters.
}

//这儿的firstParameterName、secondParameterName为参数标签
someFunction(firstParameterName: 1, secondParameterName: 2)
```
每个参数的参数名必须唯一，不然报`Definition conflicts with previous value`错误。虽然不同的参数名可以有相同的参数标签，但是唯一的参数标签能够让代码更加清晰。（关于定义特定的参数标签请看下节）举个例子

```
func click(name name:String,name age:Int,name gender:Bool) ->() {
    print("\(name) is \(age) years old,and \(gender ? "he":"she") is a \(gender ? "boy":"girl").")
}
click(name: "小明", name: 23, name: true)

//会报警告Extraneous duplicate parameter name; 'name' already has an argument label
//输出 小明 is 23 years old,and he is a boy.

```
如上，参数标签一样使得函数调用时看起来代码很混乱，虽说不会报错，不推荐这样写。


### Specifying Argument Labels

如果要给函数定义特定的参数标签，将它写在参数名前，用空格隔开，格式如下：

```
func someFunction(argumentLabel parameterName: Int) {
    // In the function body, parameterName refers to the argument value
    // for that parameter.
}
```

举个例子：

```
func greet(person: String, from hometown: String) -> String {
    return "Hello \(person)!  Glad you could visit from \(hometown)."
}
print(greet(person: "Bill", from: "Cupertino"))
// Prints "Hello Bill!  Glad you could visit from Cupertino."
```
使用参数标签可以使函数看起来更清晰，可读性更强。（也没多大卵用）

### Omitting Argument Labels

如果想让某个参数没有参数标签，可以用`_`代替参数标签。这样调用的时候就不会显示参数标签，如下：

```
func someFunction(_ firstParameterName: Int, secondParameterName: Int) {
    // In the function body, firstParameterName and secondParameterName
    // refer to the argument values for the first and second parameters.
}
someFunction(1, secondParameterName: 2)
```

如果定义函数的时候定义了参数标签，那么函数调用的时候必须写参数标签。

### Default Parameter Values

函数定义的时候我们可以给参数设置默认值，函数调用的时候我们也可以不用写这个参数，如下例：

```
func someFunction(parameterWithoutDefault: Int, parameterWithDefault: Int = 12) {
    // If you omit the second argument when calling this function, then
    // the value of parameterWithDefault is 12 inside the function body.
}
someFunction(parameterWithoutDefault: 3, parameterWithDefault: 6) // parameterWithDefault is 6
someFunction(parameterWithoutDefault: 4) // parameterWithDefault is 12
```
通常把没有默认值的参数写在前面，有默认值的参数放在后面，就当是惯例吧



### Variadic Parameters
*variadic parameter*（可变参数）即可以输入多个指定类型的值。写法是在参数指定类型的后面加`...`

在函数内我们可以通过一个可变参数指定类型的数组来获取可变参数的值，比如指定一个可变参数`numbers`，类型是`double`，那么在函数内就可以用`[Double]`类型的数组来获取参数，如下例：

```
func arithmeticMean(_ numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
arithmeticMean(1, 2, 3, 4, 5)
// returns 3.0, which is the arithmetic mean of these five numbers
arithmeticMean(3, 8.25, 18.75)
// returns 10.0, which is the arithmetic mean of these three numbers
```
> 一个函数只能有一个可变参数


### In-Out Parameters

函数的参数是一个常量，是不能修改的，如果在函数内赋值会遇到编译错误。如果想修改参数的值，就把它定义为一个*in-out parameter*参数（输出参数）。

*in-out parameter*是通过在参数指定类型前面加关键字`inout`来实现的。

> 注意：输出参数是不能有默认值的，可变参数也不能用`inout`来修饰

输出参数在函数调用的时候必须传入一个变量，因为常量或者字面量都是不能修改的，所以需要在输入参数前加一个`&`来区别这个参数在函数内是可被修改的(函数调用的时候会自动加)。如下例通过输出参数的函数修改了函数外的变量的值：

```
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}

var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// Prints "someInt is now 107, and anotherInt is now 3"

```

> 输出参数不同于函数返回值，像上面的例子，虽然没有返回值仍然修改了函数外变量的值。所以输出参数是函数能够影响函数外范围的一种有效方式。



## Function Types

每个函数都有它的类型，一般函数类型指的是他的输入参数类型和返回类型，举个例子：

```
func addTwoInts(_ a: Int, _ b: Int) -> Int {
    return a + b
}
func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {
    return a * b
}
```

如上，两个函数的内容不一样的，但是他们的类型都是一样的，都是` (Int, Int) -> Int`类型。
另一个例子：

```
func printHelloWorld() {
    print("hello, world")
}
```
函数类型为`() -> Void`

### Using Function Types

这个函数类型我们可以当做和其他的类型一样使用，我们可以定义一个常量或者变量的类型为函数类型，如下

```
func addTwoInts(_ a: Int, _ b: Int) -> Int {
    return a + b
}
var mathFunction: (Int, Int) -> Int = addTwoInts

```

如上，如果不写`mathFunction`的类型就会默认和函数addTwoInts的一致，如果要写，就必须和函数addTwoInts的函数类型一致（`(Int, Int) -> Int`）类型,使用和addTwoInts一样


```
print("Result: \(mathFunction(2, 3))")
// Prints "Result: 5"
```
因为我们定义的`mathFunction`是一个变量，所以如下：
```
func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {
    return a * b
}
mathFunction = multiplyTwoInts
print("Result: \(mathFunction(2, 3))")
// Prints "Result: 6"
```


### Function Types as Parameter Types

上面提到函数类型可以和其他的类型一样使用，所以也可以用函数类型作为另一个函数的参数类型，如下

```
func addTwoInts(_ a: Int, _ b: Int) -> Int {
    return a + b
}
func printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    print("Result: \(mathFunction(a, b))")
}
printMathResult(addTwoInts, 3, 5)
// Prints "Result: 8"
```

如上定义了一个函数名为**printMathResult**的函数，定义了一个参数名为`mathFunction`,参数类型为` (Int, Int) -> Int`类型的参数，还定义了两个`Int`类型的参数`a`和`b`.  
调用**printMathResult**函数的时候，我们将函数**addTwoInts*作为参数传了进去，所以执行结果相当于`addTwoInts(3,5)`即8.

### Function Types as Return Types

函数类型也可以作为另一个函数的返回值来使用，将函数类型写在`->`的后面，如下例子


```
func stepForward(_ input: Int) -> Int {
    return input + 1
}
func stepBackward(_ input: Int) -> Int {
    return input - 1
}

func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    return backward ? stepBackward : stepForward
}

var currentValue = 3
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero now refers to the stepBackward() function

print("Counting to zero:")
// Counting to zero:
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
print("zero!")
// 3...
// 2...
// 1...
// zero!

```

首先定义了。两个函数**stepForward**和**stepBackward**，定义了一个函数**chooseStepFunction**的，根据传入参数的不同返回不同的函数类型。

定义了一个变量`currentValue = 3`，定义了一个常量`moveNearerToZero = chooseStepFunction(backward: currentValue > 0)`，可以看出
`moveNearerToZero`指的就是**stepBackward**()函数



## Nested Functions

以上所有提到的函数都是全局函数（在全局范围内定义的）。我们也可以在函数内定义函数，叫做嵌套函数。

嵌套函数只能在他的父级函数内部使用，或者作为返回值在函数外部使用，如下我们把上面的例子用嵌套函数来写：


```
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backward ? stepBackward : stepForward
}
var currentValue = -4
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero now refers to the nested stepForward() function
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
print("zero!")
// -4...
// -3...
// -2...
// -1...
// zero!
```

