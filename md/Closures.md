#  Closures 闭包

## 目录

- [Closure Expressions- 闭包表达式](#closure-expressions)
  - [The Sorted Method-](#the-sorted-method)
  - [Closure Expression Syntax-闭包表达式语法](#closure-expression-syntax)
  - [Inferring Type From Context-根据上下文推断类型](#inferring-type-from-context)
  - [Implicit Returns from Single-Expression Closures-隐式返回](#implicit-returns-from-single-expression-closures)
  - [Shorthand Argument Names-缩写参数名](#shorthand-argument-names)
  - [Operator Methods-运算符方法](#operator-methods)
- [Trailing Closures-尾随闭包](#trailing-closures)
- [Capturing Values-捕获值](#capturing-values)
- [Closures Are Reference Types-闭包是引用类型](#closures-are-reference-types)
- [Escaping Closures-逃逸闭包](#escaping-closures)
- [Autoclosures-自动闭包](#autoclosures)

---
## 前言

***Closures***(闭包)就是能在代码中能够传递和使用的自包含函数代码块。Swift里面的***Closures***(闭包)和C、OC里面的`blocks`是相似的。和其他编程语言里面的`lambdas`类似。

闭包能够捕获所在环境中的任意常量和变量，并自动管理和存储这些捕获值（关于捕获值下面有详细介绍）。

> 闭包就是能够读取其他函数内部变量的函数  
> 定义在一个函数内部的函数  
> 闭包就是将函数内部和函数外部连接起来的一座桥梁  
> ———  阮一峰


全局函数和嵌套函数其实就是一种特殊的闭包，一般闭包有以下三种形式：

1. 全局函数，不需要捕获任何值的有名字的闭包  
1. 嵌套函数，能够读取函数内部值的有名字的闭包
1. 闭包表达式，能通过轻量级的语法获取其上下文的值的匿名闭包

Swift的闭包表达式语法简单，并在相应的场景做了优化，包括：

- 通过上下文环境推测参数和返回值
- 隐式返回单表达式（省略`return`关键字）
- 参数名简写
- Trailing（尾随）闭包语法

---
## Closure Expressions

嵌套函数是一个能在较复杂函数中方便进行命名和定义自包含代码模块。但是有些情况下，一些没有定义和命名的结构也是很有用的，特别是函数作为函数的参数的时候。  
所以闭包表达式就是一种利用简单的语法构建一个内联闭包，经过语法优化后，使用起来更加方便。

### The Sorted Method

Swift提供了一个`sorted(by:)`方法，它能够根据你提供的排序闭包来给一个数组元素排序，并返回一个排好序的数组。而且原来的数组也没有被修改。

如下的例子，

```Swift
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
func backward(_ s1: String, _ s2: String) -> Bool {
    return s1 > s2
}
var reversedNames = names.sorted(by: backward)
// reversedNames is equal to ["Ewa", "Daniella", "Chris", "Barry", "Alex"]

```
如上`sorted(by:)`方法接收一个闭包，该闭包有两个和数组元素类型一样的参数，并且返回一个Bool值来确定哪个元素排序在前。

如果数组类型是`[String]`，那么闭包的的函数类型就是`(String, String) -> Bool`

然而上面的函数其实本质上是实现了一个表达式`s1>s2`，如果用`Closure Expression Syntax会`更简洁，👇

### Closure Expression Syntax
一般的闭包表达式语法如下：

```
{ (parameters) -> return type in
    statements
}
```

闭包表达式中的参数可以为输出参数，但是不能有默认值。在闭包内可以使用定义的变量，元组类型也可以作为参数或者返回类型。

如下用闭包表达式来写先前` backward(_:_:)`方法


```Swift
func backward(_ s1: String, _ s2: String) -> Bool {
    return s1 > s2
}

```
闭包表达式写法
```Swift
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
    return s1 > s2
})
```

如上可以看出，闭包表达式的参数和返回类型和` backward(_:_:)`函数都是一样的。都写为`(s1: String, s2: String) -> Bool`,不过闭包表达式是写在`{ }`内的。


闭包表达式的内容写在关键字的`in`的后面。因为上面的例子闭包内容很简单，所以也可以写在一行。如下

```Swift
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in return s1 > s2 } )
```
对比` sorted(by:) `来说，闭包表达式本质上和`backward(_:_:)`函数并没有什么区别，只不过闭包表达式写在了括号内，并且一行代码就可以实现。


### Inferring Type From Context

如上面看到的，排序闭包是作为` sorted(by:) `的输入参数。Swift能够根据
` sorted(by:) `的调用来推断出他的输入参数和返回类型。如上` sorted(by:) `被`names`调用，所以他的参数类型肯定是函数类型`(String, String) -> Bool`，所以在闭包表达式中我们可以忽略参数类型和返回类型的定义，所以我们也可以简写为

```Swift
reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )

```
所以当闭包作为一个函数的参数的时候，我们可以省略参数和返回类型的定义，当然，为了代码的可读性也可以补全。


### Implicit Returns from Single-Expression Closures

我们知道`sorted(by:)`的返回值类型肯定是`Bool`值，像上面的例子，闭包的内容就是一个简单的表达式`s1 > s2`,所以我们也可以隐去`return`关键字，直接写为：

```Swift
reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )
//这种省略return的写法即Implicit Returns from Single-Expression Closures
```

### Shorthand Argument Names
Swift在单行闭包中可以使用参数名缩写（$0、$1、$2...etc）来代替闭包参数。

使用参数名缩写可以省略参数的定义和`in`关键字，所以上面闭包表达式也可以写为：

```Swift
reversedNames = names.sorted(by: { $0 > $1 } )
//这儿的$0、$1相当于闭包第一个和第二个参数 
```

### Operator Methods

其实像上面的闭包表达式，我们还能更简单，如下：
```Swift
reversedNames = names.sorted(by: >)
```
Swift自动推断运算符`>`的前后参数及类型

---
## Trailing Closures

如果函数的最后一个参数是闭包，我们调用函数的时候就可以用***Trailing Closures***（尾随闭包）写法来代替原始的函数调用写法，尾随闭包写法能够省略闭包参数标签，如下例：
```swift
func someFunctionThatTakesAClosure(closure: () -> Void) {
    // function body goes here
}
 
// Here's how you call this function without using a trailing closure:
//正常写法
 
someFunctionThatTakesAClosure(closure: {
    // closure's body goes here
})
 
// Here's how you call this function with a trailing closure instead:
//尾随闭包的写法1
 
someFunctionThatTakesAClosure() {
    // trailing closure's body goes here
}

//尾随闭包的写法2(一般xcode会帮你自动选择这种写法)
 
someFunctionThatTakesAClosure {
    // trailing closure's body goes here
}

```
上面的数组排序如果用尾随闭包来写：
```swift
reversedNames = names.sorted() { $0 > $1 }
```
或者
```swift
//当在只有一个参数且为闭包的时候省略括号
reversedNames = names.sorted { $0 > $1 }
```
尾随闭包对于长闭包或者说多行闭包更适用。举个例子，`Array`有一个`map(_:) `方法只有一个闭包参数，该闭包实现遍历数组元素并返回一个映射数组，映射关系和返回类型都是由闭包决定的。

如下例用`map(_:) `方法将一个数字数组转换为字符串数组：
```swift
let digitNames = [
    0: "Zero", 1: "One", 2: "Two",   3: "Three", 4: "Four",
    5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine"
]
let numbers = [16, 58, 510]
let strings = numbers.map { (number) -> String in
    var number = number
    var output = ""
    repeat {
    
        output = digitNames[number % 10]! + output
        //这个!保证了下标的绝对存在
        number /= 10
    } while number > 0
    return output
}
// strings is inferred to be of type [String]
// strings is ["OneSix", "FiveEight", "FiveOneZero"]
//闭包的作用就是将数组元素由数字通过遍历digitNames字典转化为字母
//map方法则输出字符串数组
```

---
## Capturing Values

之前说过闭包能够捕获上下文环境（其内和其外）中的变量或者常量并在闭包内使用，即时原变量或者常量不存在了。

举个例子，嵌套函数能够捕获函数外的变量或者常量在其内部使用。

如下例：

```swift
func makeIncrementer(forIncrement amount: Int) -> () -> Int {
    var runningTotal = 0
    func incrementer() -> Int {
        runningTotal += amount
        print("内函数")
        return runningTotal
    }
    print("外函数")
    return incrementer
}
let incrementByTen = makeIncrementer(forIncrement: 10)
print("\(incrementByTen())")
print("\(incrementByTen())")
print("\(incrementByTen())")
 //输出顺序如下     
//        外函数
//        内函数
//        10
//        内函数
//        20
//        内函数
//        30

```
如上嵌套函数`incrementer`内部使用了函数内变量`runningTotal`和函数外输入参数`amount`。函数`makeIncrementer`返回`incrementer`函数。

然后我们将函数`makeIncrementer`赋给一个常量
`incrementByTen`。所以当调用`incrementByTen()`就相当于访问嵌套函数。通过上面的输出我们可以看到外函数调用了一次 ，而内函数调用了三次，而我们知道内函数可以捕获上下文的变量，由于在嵌套函数内我们改变了的
`runningTotal`的值，所以每次进来都拿到不同的`runningTotal`值。因为外函数只执行了一次，所以`amount`值一直是10。

>Swift闭包内使用的捕获值是一个copy值，可以放心的使用，不用担心原值改变而影响闭包内的使用。

像上面的例子，如果我们将函数赋予一个新值。

```swift
let incrementBySeven = makeIncrementer(forIncrement: 7)
incrementBySeven()
// returns a value of 7
//但是
incrementByTen()
// returns a value of 40
```
如上例，可以看出两个常量都是同一个函数`makeIncrementer()`，但是值却互不干扰。

>如果一个对象的属性为闭包，如果闭包捕获值指向了该对象或其属性，那么闭包和该对象就要建立强引用，具体详见[**Strong Reference Cycles for Closures**](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AutomaticReferenceCounting.html#//apple_ref/doc/uid/TP40014097-CH20-ID56)


---
## Closures Are Reference Types

上面的例子中`incrementBySeven`和`incrementByTen`都是常量，但是其所指向的闭包仍然可以改变闭包外`runningTotal`变量的值。这是因为闭包和函数都是*reference types*（引用类型）。  
所以将函数（闭包）赋值给一个变量（常量），实际上是给常量（变量）和函数（闭包）建立了一个引用关系，并不指向函数或者闭包本身。
故如果两个变量（常量）的引用关系是一样的，那么指向的闭包也是一致的。如下例：

```swift
let alsoIncrementByTen = incrementByTen
alsoIncrementByTen()
// returns a value of 50
```
---
## Escaping Closures
当闭包作为函数参数，却在在函数执行结束之后才被调用的闭包我们叫做逃逸闭包，逃逸闭包在声明的时候在类型前加关键字` @escaping`.
 
一种实现逃逸闭包的方式就是在函数外定义变量来存储闭包。举个例子，异步函数使用闭包来处理程序，如下：

```swift
var completionHandlers: [() -> Void] = []
func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
    completionHandlers.append(completionHandler)
}
//如果没有 @escaping关键字会报compile-time 错误
```

使用了`@escaping`关键字就意味着逃逸闭包，那么在闭包内使用捕获值的时候必须用`self`来修饰，而非逃逸闭包不用，如下：

```swift
func someFunctionWithNonescapingClosure(closure: () -> Void) {
    closure()
}
 
class SomeClass {
    var x = 10
    func doSomething() {
        someFunctionWithEscapingClosure { self.x = 100 }
        someFunctionWithNonescapingClosure { x = 200 }
    }
}
 
let instance = SomeClass()
instance.doSomething()
print(instance.x)
// Prints "200"
 
completionHandlers.first?()
print(instance.x)
// Prints "100"

```


---
## Autoclosures

***Autoclosures***（自动闭包）就是自动创建的闭包表达式作为函数参数，它没有参数，当被调用的时候返回表达式的值。这种语法很方便，能够省略闭包的括号，用一个普通的表达式代替闭包的完整写法。

我们经常会调用采用自动闭包的函数，但是很少去实现这样的函数。举个例子，`assert(condition:message:file:line:)` 函数接受自动闭包作为它的 `condition` 参数和 `message `参数；它的 `condition` 参数仅会在 `debug` 模式下被求值，它的 `message` 参数仅当 `condition` 参数为 `false` 时被计算求值。

自动闭包能够用来延迟求值，因为只有被调用这个闭包的时候代码才会被执行。一旦代码有副作用或者计算量很大，延迟求值就会很有用，因为它能够控制该什么时候执行代码。下面一个延时求值的例子：

```swift
var customersInLine = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
print(customersInLine.count)
// Prints "5"
 
let customerProvider = { customersInLine.remove(at: 0) }
print(customersInLine.count)
// Prints "5"
 
print("Now serving \(customerProvider())!")
// Prints "Now serving Chris!"
print(customersInLine.count)
// Prints "4"
```
注意`customerProvider`的类型是 `() -> String`,只有当`customerProvider`被调用的时候，闭包内的代码才会被执行，数组的元素才会被移除。

同样闭包作为函数参数也能够延迟求值，如下：

```swift
// customersInLine is ["Alex", "Ewa", "Barry", "Daniella"]
func serve(customer customerProvider: () -> String) {
    print("Now serving \(customerProvider())!")
}
serve(customer: { customersInLine.remove(at: 0) } )
// Prints "Now serving Alex!"
```
👆的这个函数调用时接收了一个显示的闭包参数。如果使用自动闭包，可以这样写，如下


```swift
// customersInLine is ["Ewa", "Barry", "Daniella"]
//使用@autoclosure标明是自动闭包
func serve(customer customerProvider: @autoclosure () -> String) {
    print("Now serving \(customerProvider())!")
}
serve(customer: customersInLine.remove(at: 0))
//调用时省略括号，直接执行闭包表达式
// Prints "Now serving Ewa!"
```

>当然，如果过度的使用自动闭包让代码可读性变差。上下文和函数名应该很清晰的表达出求值被延迟。

如果想让自动闭包能够逃逸，需要使用 `@autoclosure `和 `@escaping`两个标志，如下：

```swift
// customersInLine is ["Barry", "Daniella"]
//注意这定义了一个闭包数组
var customerProviders: [() -> String] = []
func collectCustomerProviders(_ customerProvider: @autoclosure @escaping () -> String) {
    //将传进来的闭包添加到数组中
    customerProviders.append(customerProvider)
}
//传入闭包
collectCustomerProviders(customersInLine.remove(at: 0))
collectCustomerProviders(customersInLine.remove(at: 0))
 
print("Collected \(customerProviders.count) closures.")
// Prints "Collected 2 closures."
for customerProvider in customerProviders {
    //遍历数组，调用闭包，因为闭包在函数执行完才被调用，故为逃逸闭包。
    print("Now serving \(customerProvider())!")
}
// Prints "Now serving Barry!"
// Prints "Now serving Daniella!"

```
