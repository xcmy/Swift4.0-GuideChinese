## Strings and Characters 字符串和字符

Swift的字符串是有一系列characters组成的，像 "hello word" or "albatross",在Swift里面表示为String类型，我们可以通过多种方法获取String类型的值的内容，包括它的字符集合。

通过String类型和Character类型，我们能够简单快速的创建文本。字符串的创建和操作轻便，而且可读性强，可以像C的字符串一样用 + 运算符拼接，既可以定义为常量，也可以定义为变量。在字符串中我们可以插入常量、变量、字面量，表情，也叫字符串内插，方便与我们自定义字符串的展示，存储和打印。

尽管语法很简单，但是String 类型仍然是一个快速，潮流的字符串实现方式，每个字符串都由独特编码方式的字符组成，而且我们可以通过很多方法来获取这些字符。

> ==NOTE==Swift的String类型是NSString类型的过渡，它扩展了NSString的方法，所以我们String类型可以使用NSString类型的方法。

### String Literals  字面量\字符串常量
字符串常量是由一系列字符组成，被""包裹，如下：

```
let someString = "Some string literal value"
```
多行字符串由三个引号包裹，如下

```
let quotation = """
The White Rabbit put on his spectacles.  "Where shall I begin,
please your Majesty?" he asked.
 
"Begin at the beginning," the King said gravely, "and go on
till you come to the end; then stop."
"""
```
如果一个多行字符串中包含三个引号，需要用\隔开，如：

```
let threeDoubleQuotes = """
Escaping the first quote \"""
Escaping all three quotes \"\"\"
"""
```
对于单行和多行的字符串定义，下面这两个字符串是相同的

```
let singleLineString = "These are the same."
let multilineString = """
These are the same.
"""
```

多行字符串以空行开始和结尾，如下

```
"""
 
This string starts with a line feed.
It also ends with a line feed.
 
"""
```

多行字符串是以结尾的三个引号对齐的，只要文本和结尾的"""对齐，那就是文本无缩进，如果文本比结尾引号靠后，打印出来也是有缩进的（如果引号比文本靠后，就会报错）。

```
func generateQuotation() -> String {
    let quotation = """
        The White Rabbit put on his spectacles.  "Where shall I begin,
        please your Majesty?" he asked.
 
        "Begin at the beginning," the King said gravely, "and go on
        till you come to the end; then stop."
        """
    return quotation
}
print(quotation == generateQuotation())
// Prints "true"
```
### Initializing an Empty String 初始化一个空字符串

我们可以通过""或者字符串初始化语法来创建一个空字符串，如：

```
var emptyString = ""               // empty string literal
var anotherEmptyString = String()  // initializer syntax
// these two strings are both empty, and are equivalent to each other
```
可以通过.isEmpty属性来判断是否是空字符串

```
if emptyString.isEmpty {
    print("Nothing to see here")
}
// Prints "Nothing to see here"

```
### String Mutability 字符串的可变性

如果一个字符串是变量那么它是可以修改的，如果是常量，它是不能修改的。如

```
var variableString = "Horse"
variableString += " and carriage"
// variableString is now "Horse and carriage"
 
let constantString = "Highlander"
constantString += " and another Highlander"
// this reports a compile-time error - a constant string cannot be modified
```

> 不同于Object-c和Cocoa通过NSString和NSMutableString来区别可变字符串和不可变字符串，Swift则是通过var和let来判断的。

### Strings Are Value Types
String是一个值类型，如果你创建了一个String类型的值，当这个值在function中被使用或者将它赋值给其他变量的时候，就会创建一个新值和该值相等用于function中使用和变量赋值，而不是使用原值。
Swift的copy-by-default(👆这种行为)保证了原值不受function和变量赋值的污染，你可以放心大胆的使用。









