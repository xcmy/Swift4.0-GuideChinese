## Basic Operators 基础运算符
### Terminology 术语

- 一元运算符作用于一个目标(例如- a)。前缀操作符(如! b)，后缀运算符(如c !)
- 二元操作符作用于两个目标之间（如：a+b）
- 三元运算符作用于三个目标。像C,Swift只有一个三元操作符,三元条件操作符如(a?b:c)。

    操作符影响是操作数的值。+是一个二元运算符,它的两个操作数是1和2


---

### Assignment Operator 赋值运算符

例a = b ，a初始化，更新a的值为b。
    
```
let b = 10
var a = 5
a = b
// a is now equal to 10
    
```
如果右侧是一个元组与多个值,它的元素可以分解成多个常量或变量

```
let (x, y) = (1, 2)
// x is equal to 1, and y is equal to 2
```
与C和objective - C的赋值运算符不同,赋值运算符本身不返回值。下面的声明是无效的

```
if x = y {
    // This is not valid, because x = y does not return a value.
}
```

---
### Arithmetic Operators 算数运算符

Swift支持所有数字类型的四种基本算术运算符

- Addition (+)  加
- Subtraction (-)  减
- Multiplication (*)  乘
- Division (/)  除


```
1 + 2       // equals 3
5 - 3       // equals 2
2 * 3       // equals 6
10.0 / 2.5  // equals 4.0
```

与C和objective - C的算术运算符不同,Swift算术运算符不允许默认值溢出。

Addition (+) 同样支持String

```
"hello, " + "world"  // equals "hello, world"

```

####  Remainder Operator 取余

> ==NOTE==
> 在其他语言取余操作符(%)也被称为模运算符。然而,在Swift中负数也是可以参与运算的,严格地说,不算是模操作

余数的写法

```
9 % 4    // equals 1
```

负值计算余数

```
-9 % 4   // equals -1
```

如a % b，b的正负是被忽略的，a % b = a % -b

```
9 % -4    // equals 1
```

#### Unary Minus Operator 取反运算符-

数值前面可以加前缀 -（直接加在数字前面，中间没有空格），表示取反值
```
let three = 3
let minusThree = -three       // minusThree equals -3
let plusThree = -minusThree   // plusThree equals 3, or "minus minus three"
```
#### Unary Plus Operator +运算符

没有什么实际意义，写在数字之前，对值没有什么影响


```
let minusSix = -6
let alsoMinusSix = +minusSix  // alsoMinusSix equals -6
```

### Compound Assignment Operators  组合运算符

像C、Swift都有组合运算符，如（+=），仅仅是运算符，没有返回值（不能写成b = a+=）
```
var a = 1
a += 2
// a is now equal to 3
```

### Comparison Operators 比较运算符

Swift支持所有C的标准的比较运算符
- Equal to (a == b) 等于
- Not equal to (a != b) 不等于
- Greater than (a > b) 大于
- Less than (a < b) 小于
- Greater than or equal to (a >= b) 大于等于
- Less than or equal to (a <= b) 小于等于

> Swift还提供了两个身份操作符(= = =和! = =),可以用它来检测两个对象引用是否指向同一对象实例。


每个比较运算符都返回一个bool值来判断表达式的正确与否
```
1 == 1   // true because 1 is equal to 1
2 != 1   // true because 2 is not equal to 1
2 > 1    // true because 2 is greater than 1
1 < 2    // true because 1 is less than 2
1 >= 1   // true because 1 is greater than or equal to 1
2 <= 1   // false because 2 is not less than or equal to 1
```

比较运算符经常被使用与条件语句中，如if语句

```
let name = "world"
if name == "world" {
    print("hello, world")
} else {
    print("I'm sorry \(name), but I don't recognize you")
}
// Prints "hello, world", because name is indeed equal to "world".
```
元组类型一致也是可以比较的（字符串比较同位的字母先后顺序），比如

```
(1, "zebra") < (2, "apple")   // true because 1 is less than 2; "zebra" and "apple" are not compared
(3, "apple") < (3, "bird")    // true because 3 is equal to 3, and "apple" is less than "bird"
(4, "dog") == (4, "dog")      // true because 4 is equal to 4, and "dog" is equal to "dog"
```
> ==NOTE==Swift比较运算符只支持元素少于七位的元组进行比较，多于七位只能自己实现比较运算

### Ternary Conditional Operator 条件运算符
条件运算符的格式：question ? answer1 : answer2；如果 question为真返回answer1，为假返回answer2 ，如下：

```
if question {
    answer1
} else {
    answer2
}
```
举例

```
let contentHeight = 40
let hasHeader = true
let rowHeight: Int
if hasHeader {
    rowHeight = contentHeight + 50
} else {
    rowHeight = contentHeight + 20
}
// rowHeight is equal to 90
```
也可以简写为

```
let contentHeight = 40
let hasHeader = true
let rowHeight = contentHeight + (hasHeader ? 50 : 20)
// rowHeight is equal to 90
```
> 三元运算符也可以实现简单的条件判断，代码看起来很简洁，过度使用会增加代码的可读性


### Nil-Coalescing Operator  ?? 操作符
如果a是optional类型，a ?? b表示当a = nil的时候，给a默认一个值b(b的类型必须和a声明的类型一致)。

代码实现原理，解释如下：

```
a != nil ? a! : b

```
举例

如果a != nil ,a = !a 
```
userDefinedColorName = "green"
colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName is not nil, so colorNameToUse is set to "green"

```
如果a = nil ，a = b

```
let defaultColorName = "red"
var userDefinedColorName: String?   // defaults to nil
 
var colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName is nil, so colorNameToUse is set to the default of "red"
```

### Range Operators 区间运算符

#### Closed Range Operator 闭区间
闭区间（a...b）,表示从a到b的区间范围，也包括a和b,前提条件是（a < b）,常用于数据遍历，例：

```
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}
// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25
```
#### Half-Open Range Operator 半开区间
半开区间（a..<b）,表示从a到b,包括a,但不包括b,如果a == b,这就是个空区间。例：

```
let names = ["Anna", "Alex", "Brian", "Jack"]
let count = names.count
for i in 0..<count {
    print("Person \(i + 1) is called \(names[i])")
}
// Person 1 is called Anna
// Person 2 is called Alex
// Person 3 is called Brian
// Person 4 is called Jack

```
#### One-Sided Ranges 单边区间
闭区间和半开区间都是一个值到一个值，单边区间指从一个值到末尾或者从开头到某个值。比如：

```
for name in names[2...] {
    print(name)
}
// Brian
// Jack
 
for name in names[...2] {
    print(name)
}
// Anna
// Alex
// Brian
```
当然也有半单边区间，如


```
for name in names[..<2] {
    print(name)
}
// Anna
// Alex
```
也可以用于检测范围内是否包含某一个值，如

```
let range = ...5
range.contains(7)   // false
range.contains(4)   // true
range.contains(-1)  // true

```
### Logical Operators 逻辑运算符
如下三种逻辑运算符
- Logical NOT (!a)  非
- Logical AND (a && b)  且\并
- Logical OR (a || b) 或

#### Logical NOT Operator 非运算符

非运算符是一个前缀操作符，中间没有空格，也可以理解为“不是”。

```
let allowedEntry = false
if !allowedEntry {
    print("ACCESS DENIED")
}
// Prints "ACCESS DENIED"

```
#### Logical AND Operator 并\且运算符

运算符（&&）表示当两个表达式均为true的条件下才为true。（当第一个条件为false的时候，第二个条件是不会判断的）。

```
let enteredDoorCode = true
let passedRetinaScan = false
if enteredDoorCode && passedRetinaScan {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// Prints "ACCESS DENIED"

```
#### Logical OR Operator 或运算符
运算符（||）表示当两个表达式只要有一个为true的条件就为true。

```
let hasDoorKey = false
let knowsOverridePassword = true
if hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// Prints "Welcome!"

```

#### Combining Logical Operators 复合逻辑运算符

如下（只要enteredDoorCode为true并且passedRetinaScan、hasDoorKey、knowsOverridePassword其中一个为true整个表达式就为true）
```
if enteredDoorCode && passedRetinaScan || hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// Prints "Welcome!"
```
#### Explicit Parentheses 括号+逻辑运算符

如下，（）里面整体表示一个单独的逻辑，使用括号有助于使逻辑更加清晰。

```
if (enteredDoorCode && passedRetinaScan) || hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// Prints "Welcome!"
```





















 









    


