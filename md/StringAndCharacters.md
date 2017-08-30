## Strings and Characters 字符串和字符
---
## 目录

- [ String Literals  字面量\字符串常量](#string-literals-字面量-字符串常量)
- [Initializing an Empty String 空字符串初始化](#initializing-an-empty-string-初始化一个空字符串)
- [String Mutability 字符串的可变性](#string-mutability-字符串的可变性)
- [Working with Characters 字符](#working-with-characters-字符)
- [Concatenating Strings and Characters 拼接字符和字符串](#concatenating-strings-and-characters-拼接字符和字符串)
- [String Interpolation 字符串内插](#string-interpolation-字符串内插)
- [Unicode 编码](#unicode-编码)
  - [Unicode Scalars （编码方式）](#unicode-scalars-编码方式)
  - [Special Characters in String Literals 特殊字符](#special-characters-in-string-literals-特殊字符)
  - [Extended Grapheme Clusters 扩展字形群集](#extended-grapheme-clusters-扩展字形群集)
- [Counting Characters 字符长度](#counting-characters-字符长度)
- [Accessing and Modifying a String 字符串操作](#accessing-and-modifying-a-string-字符串操作)
- [Substrings 子字符串](#substrings-子字符串)
- [Comparing Strings 字符串比较](#comparing-strings-字符串比较)
- [Unicode Representations of Strings](#unicode-representations-of-strings)
  - [UTF-8 Representation](#utf-8-representation)
  - [UTF-16 Representation](#utf-16-representation)
  - [Unicode Scalar Representation](#unicode-scalar-representation)



---
### 前言

> Swift的String类型是NSString类型的过渡，它扩展了NSString的方法，所以我们String类型可以使用NSString类型的方法。

Swift的字符串是有一系列characters组成的，像 "hello word" or "albatross",在Swift里面表示为String类型，我们可以通过多种方法获取String类型的值的内容，包括它的字符集合。

通过String类型和Character类型，我们能够简单快速的创建文本。字符串的创建和操作轻便，而且可读性强，可以像C的字符串一样用 + 运算符拼接，既可以定义为常量，也可以定义为变量。在字符串中我们可以插入常数、变量、常量，表达式，也叫字符串内插，方便与我们自定义字符串的展示，存储和打印。

尽管语法很简单，但是String 类型仍然是一个快速，潮流的字符串实现方式，每个字符串都由独特编码方式的字符组成，而且我们可以通过很多方法来获取这些字符。





---
## String Literals  字面量-字符串常量
字符串常量是由一系列字符组成，被""包裹，如下：

```Swift
let someString = "Some string literal value"
```
多行字符串由三个引号包裹，如下

```Swift
let quotation = """
The White Rabbit put on his spectacles.  "Where shall I begin,
please your Majesty?" he asked.
 
"Begin at the beginning," the King said gravely, "and go on
till you come to the end; then stop."
"""
```
如果一个多行字符串中包含三个引号，需要用\隔开，如：

```Swift
let threeDoubleQuotes = """
Escaping the first quote \"""
Escaping all three quotes \"\"\"
"""
```
对于单行和多行的字符串定义，下面这两个字符串是相同的

```Swift
let singleLineString = "These are the same."
let multilineString = """
These are the same.
"""
```

多行字符串以空行开始和结尾，如下

```Swift
"""
 
This string starts with a line feed.
It also ends with a line feed.
 
"""
```

多行字符串是以结尾的三个引号对齐的，只要文本和结尾的"""对齐，那就是文本无缩进，如果文本比结尾引号靠后，打印出来也是有缩进的（如果引号比文本靠后，就会报错）。

```Swift
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
---
## Initializing an Empty String 初始化一个空字符串

我们可以通过""或者字符串初始化语法来创建一个空字符串，如：

```Swift
var emptyString = ""               // empty string literal
var anotherEmptyString = String()  // initializer syntax
// these two strings are both empty, and are equivalent to each other
```
可以通过.isEmpty属性来判断是否是空字符串

```Swift
if emptyString.isEmpty {
    print("Nothing to see here")
}
// Prints "Nothing to see here"

```

---
## String Mutability 字符串的可变性

如果一个字符串是变量那么它是可以修改的，如果是常量，它是不能修改的。如

```Swift
var variableString = "Horse"
variableString += " and carriage"
// variableString is now "Horse and carriage"
 
let constantString = "Highlander"
constantString += " and another Highlander"
// this reports a compile-time error - a constant string cannot be modified
```

> 不同于Object-c和Cocoa通过NSString和NSMutableString来区别可变字符串和不可变字符串，Swift则是通过var和let来判断的。

## Strings Are Value Types
String类型是一个值类型，如果你创建了一个String类型的值，当这个值在function中被使用或者将它赋值给其他变量的时候，就会创建一个新值和该值相等用于function中使用和变量赋值，而不是使用原值。
Swift的copy-by-default(👆这种行为)保证了原值不受function和变量赋值的污染，你可以放心大胆的使用。

---

## Working with Characters 字符

使用 for-in loop遍历字符串可以获取String中的每一个独立的字符
```Swift
for character in "Dog!🐶" {
    print(character)
}
// D
// o
// g
// !
// 🐶
```
Character的创建

```Swift
let exclamationMark: Character = "!"
```
字符串也可以由字符数组（Character[]）来初始化，如下:

```Swift
let catCharacters: [Character] = ["C", "a", "t", "!", "🐱"]
let catString = String(catCharacters)
print(catString)
// Prints "Cat!🐱"

```
---
## Concatenating Strings and Characters 拼接字符和字符串

String类型可以通过 + 运算符拼接，如下

```Swift
let string1 = "hello"
let string2 = " there"
var welcome = string1 + string2
// welcome now equals "hello there"
```
可变String可以通过 += 运算符拼接,如下

```Swift
var instruction = "look over"
instruction += string2
// instruction now equals "look over there"
```
可以在String后面通过append()方法拼接字符，如下

```Swift
let exclamationMark: Character = "!"
welcome.append(exclamationMark)
// welcome now equals "hello there!"
```
> 不能给可变字符后面拼接字符串或者字符，因为字符只能包含一个字符元素

---
## String Interpolation 字符串内插
将常数，变量，常量，表达式等值拼接在一起组成一个新的字符串就叫String Interpolation，在字符串中这些值都用\\()包裹起来，如下：

```Swift
let multiplier = 3
let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)"
// message is "3 times 2.5 is 7.5"
```
如上，使用\\(multiplier)将multiplier的值插入到了字符串中，\\(Double(multiplier) * 2.5)将Double(multiplier) * 2.5计算后的结果插入到了字符串中
>  \\()中的表达式中不能有转义的\\，回车和换行。

---
## Unicode 编码
Swift的String和Character类型完全符合[Unicode](https://baike.baidu.com/item/Unicode/750500?fr=aladdin)编码规范和标准


```

graph TD
A[Unicode scalar]---B[String]

```



```
graph TD
A[Unicode scalar 编码单元]---|一个或多个组成|B[Extended Grapheme Clusters 扩展字形群集]
B---|相当于|C[Character 字符]
A---|部分 Unicode scalar 对应 |C
C---|组成|D[String 字符串]

```

### Unicode Scalars 编码方式
 Swift 的String 类型是基于 Unicode Scalar创建的。一个编码单元由21位数字或者修饰符组成，就像`U+0061`代表拉丁字母`A ("a")`，`U+1F425`代表表情"🐥"。
 
>  Unicode Scala编码支持从 `U+0000` -> `U+D7FF` 或者从 `U+E000` -> `U+10FFFF` 之间的所有码点。但不支持[surrogate pair](https://baike.baidu.com/item/surrogate%20pair/16820063?fr=aladdin)编码（一种支持从`U+D800` -> `U+DFFF` 码点的编码方式）。

并不是所有 21-bit 的编码单元都对应有相应的字符（但字符一定有对应的编码单元），这样有利于将来的扩展。按照惯例被分配的编码单元一般都有一个名字，如上面例子的`LATIN SMALL LETTER A`和`FRONT-FACING BABY CHICK`

---
### Special Characters in String Literals 特殊字符

字符串里面可以包含下面这些特殊字符
- 转义字符

转义字符 | 说明
---|---
\0 | null
\t | 水平缩进
\\\ | 反斜杠
\n | 换行
\r | 回车
\\" | 双引号
\\' | 单引号

-  `\u{n}` 形式的字符串（n是十六进制数）都对应一个码点，如下：


```Swift
let wiseWords = "\"Imagination is more important than knowledge\" - Einstein"
// "Imagination is more important than knowledge" - Einstein
let dollarSign = "\u{24}"        // $,  Unicode scalar U+0024
let blackHeart = "\u{2665}"      // ♥,  Unicode scalar U+2665
let sparklingHeart = "\u{1F496}" // 💖, Unicode scalar U+1F496

```

---

### Extended Grapheme Clusters 扩展字形群集

Swift里面的`Character`类型相当于一个*Extended Grapheme Clusters*(扩展字形群集)，而一个扩展字形群集则是由一个或多个Unicode scalar（编码单元）组成人类可以识别的`character`（字符）。

```Swift
let eAcute: Character = "\u{E9}"                         // é
let combinedEAcute: Character = "\u{65}\u{301}"          // e followed by ́
// eAcute is é, combinedEAcute is é
```
如上所示， é 既可以由一个编码单元组成，也可以由两个编码单元e和 ́组成。


*Extended grapheme clusters*可以通过单个的字符组成复杂的字符，例如韩语里面 `한` 字，如下：

```Swift
let precomposed: Character = "\u{D55C}"                  // 한
let decomposed: Character = "\u{1112}\u{1161}\u{11AB}"   // ᄒ, ᅡ, ᆫ
// precomposed is 한, decomposed is 한
```

*Extended grapheme clusters*可以将一个字符用封闭标记（o）包起来，如下`é -> é⃝`, 只需在后面加上  `U+20DD`，如下：

```Swift
let enclosedEAcute: Character = "\u{E9}\u{20DD}"
// enclosedEAcute is é⃝
```

*regional indicator symbols*（区域表示字母）,如🇦、🇺、🇸等，可以组合在一起形成一个 emoji flags （国旗），比如🇺和🇸可以组成🇺🇸

```Swift
let regionalIndicatorForUS: Character = "\u{1F1FA}\u{1F1F8}"
let China: Character = "\u{1F1E8}\u{1F1F3}"
print("----\(regionalIndicatorForUS)-----")
print("----\(China)-----")
// regionalIndicatorForUS is 🇺🇸
// China is🇨🇳
```


## Counting Characters 字符长度

计算字符串长度要使用`.count`属性

```Swift
let unusualMenagerie = "Koala 🐨, Snail 🐌, Penguin 🐧 ，Dromedary 🐪"
print("unusualMenagerie has \(unusualMenagerie.count) characters")
// Prints "unusualMenagerie has 40 characters"

```

*extended grapheme clusters*（扩展字符群集）虽然改变了字符的展示形式，但对于字符串的长度是没有影响的。字符串的长度是计算的Character的个数，如下`e`和`é`都算是一个长度

```Swift
var word = "cafe"
print("the number of characters in \(word) is \(word.count)")
// Prints "the number of characters in cafe is 4"
 
word += "\u{301}"    // COMBINING ACUTE ACCENT, U+0301
 
print("the number of characters in \(word) is \(word.count)")
// Prints "the number of characters in café is 4"

```

> 基于上，同样一个`character`（字符）可能是由不同的扩展字符群集组成的，所以，同样一个字符串，显示是一样的，分配的内存可能是不一样的。
> 包含相同字符的String类型和NSString类型，String的`count`属性和NSString的`length`属性返回值不一定相同，因为NSString的长度是基于 16-bit 编码单元和 `UTF-16 `的编码方式。

---
## Accessing and Modifying a String 字符串操作

> You access and modify a string through its methods and properties, or by using subscript syntax

## String Indices 字符串索引

每一个索引都对应着字符串里面的一个Character（字符）

正如上面提到的,不同的字符需要不同数量的内存来存储,所以为了确定哪些字符在哪个特定的位置,你必须遍历每个编码单元的开始或结束。出于这个原因,Swift字符串不能用整数索引值。

`startIndex`属性获取第一个字符的位置  
`endIndex`属性获取最后一个字符之后的位置(所以当String为空的时候，取`endIndex`是无意义的)  
`index(before:)`获取字符的前一位的位置  
`index(after:)`获取字符后一位的位置  
`index(_:offsetBy:)`获取偏移某个某个位置的位置


```Swift
let greeting = "Guten Tag!"
greeting[greeting.startIndex]
// G
greeting[greeting.index(before: greeting.endIndex)]
// !
greeting[greeting.index(after: greeting.startIndex)]
// u
let index = greeting.index(greeting.startIndex, offsetBy: 7)
greeting[index]
// a
```


如果获取的位置超出了字符的范围就会报`runtime` 错误

```Swift
greeting[greeting.endIndex] // Error
greeting.index(after: greeting.endIndex) // Error
```

通过`indices`属性可以获取字符串中每个字符的索引

```Swift
for index in greeting.indices {
    print("\(greeting[index]) ", terminator: "")
}
// Prints "G u t e n   T a g ! "
```

> 只要遵从Collection协议，`startIndex` 、 `endIndex` 属性 和 `the index(before:)`,` index(after:)`, and `index(_:offsetBy:)`这几个方法都是适用的，比如（String、Array、 Dictionary、 Set）都可以用这些属性和方法。

### Inserting and Removing 移动和插入

给字符串插入一个单一的字符（character）用`insert(_:at:) `这个方法。
给字符串插入另一段字符串用`insert(contentsOf:at:)`方法

```Swift
var welcome = "hello"
welcome.insert("!", at: welcome.endIndex)
// welcome now equals "hello!"
 
welcome.insert(contentsOf: " there", at: welcome.index(before: welcome.endIndex))
// welcome now equals "hello there!"
```

移除单一字符用`remove(at:)` 方法
移除一段字符串用`removeSubrange(_:)` 方法


```Swift
welcome.remove(at: welcome.index(before: welcome.endIndex))
// welcome now equals "hello there"
 
let range = welcome.index(welcome.endIndex, offsetBy: -6)..<welcome.endIndex
welcome.removeSubrange(range)
// welcome now equals "hello"
```

> 同样这几个方法也适用于String、Array、Dictionary、Set。前提是遵循RangeReplaceableCollection协议

---
## Substrings 子字符串

当你使用类似于`prefix(_:)`这样的方法获取一个字符串的子字符串的时候，得到的结果其实是[Substring](https://developer.apple.com/documentation/swift/substring)的一个实例，不是另一个字符串。但你可以像使用String类型一样使用它（大部分方法和属性都是通用的），但不是持久的，如果想一直使用它，需要重新赋值，分配内存。如下

```Swift
let greeting = "Hello, world!"
let index = greeting.index(of: ",") ?? greeting.endIndex
let beginning = greeting[..<index]
// beginning is "Hello"
 
// Convert the result to a String for long-term storage.
let newString = String(beginning)

```

简单的说，substring的使用是不会重新分配内存的。像上面例子中的`beginning`使用的是`greeting`的内存，而`newString`是重新分配的内存。


![image](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/stringSubstring_2x.png)

>  String 和 Substring都遵循StringProtocol协议。


## Comparing Strings 字符串比较

Swift有三种比较字符串的方法：
---
1. **string and character equality 字符相等**
1. **prefix equality 前缀相等**
1. **suffix equality 后缀相等**

###  string and character equality 字符比较相等

通过 比较运算符（[Comparison Operators](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/BasicOperators.html#//apple_ref/doc/uid/TP40014097-CH6-ID70)）来判断字符串是否相等


```Swift
let quotation = "We're a lot alike, you and I."
let sameQuotation = "We're a lot alike, you and I."
if quotation == sameQuotation {
    print("These two strings are considered equal")
}
// Prints "These two strings are considered equal"
```

就算两个字符串的 `Unicode scalars` 不相同，只要字符串的意义和样子是一样的，就是相等的，如下：

```Swift
// "Voulez-vous un café?" using LATIN SMALL LETTER E WITH ACUTE
let eAcuteQuestion = "Voulez-vous un caf\u{E9}?"
 
// "Voulez-vous un café?" using LATIN SMALL LETTER E and COMBINING ACUTE ACCENT
let combinedEAcuteQuestion = "Voulez-vous un caf\u{65}\u{301}?"
 
if eAcuteQuestion == combinedEAcuteQuestion {
    print("These two strings are considered equal")
}
// Prints "These two strings are considered equal"
```

但是，像英语里面的拉丁字母 "A"（`U+0041`）和西里尔字母（`CYRILLIC）的 "А"，虽然外观是一样的，但是语义是不一样的，所以他们两是不相等的。


```Swift
let latinCapitalLetterA: Character = "\u{41}"
 
let cyrillicCapitalLetterA: Character = "\u{0410}"
 
if latinCapitalLetterA != cyrillicCapitalLetterA {
    print("These two characters are not equivalent.")
}
// Prints "These two characters are not equivalent."

```
> Swift里的字符比较是不区分区域设置的（ locale-sensitive）


### Prefix and Suffix Equality 前缀和后缀相等

要检测一个字符串是否以某个字符串开头或者结尾，使用`hasPrefix(_:)` 和 `hasSuffix(_:)` 方法，均返回Boolean值。


如下一个字符串数组

```Swift
let romeoAndJuliet = [
    "Act 1 Scene 1: Verona, A public place",
    "Act 1 Scene 2: Capulet's mansion",
    "Act 1 Scene 3: A room in Capulet's mansion",
    "Act 1 Scene 4: A street outside Capulet's mansion",
    "Act 1 Scene 5: The Great Hall in Capulet's mansion",
    "Act 2 Scene 1: Outside Capulet's mansion",
    "Act 2 Scene 2: Capulet's orchard",
    "Act 2 Scene 3: Outside Friar Lawrence's cell",
    "Act 2 Scene 4: A street in Verona",
    "Act 2 Scene 5: Capulet's mansion",
    "Act 2 Scene 6: Friar Lawrence's cell"
]
```
你可以通过`hasPrefix(_:)` 方法计算出以"Act 1"打头的字符串的个数

```Swift

for scene in romeoAndJuliet {
    if scene.hasPrefix("Act 1 ") {
        act1SceneCount += 1
    }
}
print("There are \(act1SceneCount) scenes in Act 1")
// Prints "There are 5 scenes in Act 1"
```

同样 `hasSuffix(_:)`也有妙用

```Swift
var mansionCount = 0
var cellCount = 0
for scene in romeoAndJuliet {
    if scene.hasSuffix("Capulet's mansion") {
        mansionCount += 1
    } else if scene.hasSuffix("Friar Lawrence's cell") {
        cellCount += 1
    }
}
print("\(mansionCount) mansion scenes; \(cellCount) cell scenes")
// Prints "6 mansion scenes; 2 cell scenes"

```

>` hasPrefix(_:) `和 `hasSuffix(_:)` 这两个方法其实是依次对比每个字符是否等价，是遵循*String and Character Equality*规则的

---
## Unicode Representations of Strings 

字符串要被写入文本或者做其他用的时候，都要编码成为一个个编码单元来存储，常见的如`UTF-8` （8个编码单元），`UTF-16`（16个编码单元），`UTF-32`（32个编码单元）。

Swift里有好几种不同的方法来获取字符串的编码展示。可以通过`for-in`来获取字符串的扩展字符群集，可以参考上面的*Working with Characters*

另外，获取另外三种的编码展示
- UTF-8的编码单元集合通过字符串的`utf8`属性
- UTF-16的编码单元集合通过字符串的`utf16`属性
- 21-bit Unicode scalar 编码等同于UTF-32编码格式，都通过字符串的`unicodeScalars`属性来获取


```Swift
let dogString = "Dog‼🐶"

```


character | D | o | g | !! | 🐶 |
---|---|---|---|---|---|
**Unicode scalar**| U+0044 | U+006F |U+0067 | U+203C| U+1F436 |


### UTF-8 Representation 

获取String的UTF-8编码单元集合，可以遍历String的`utf8`属性，这个属性的类型是`String.UTF8View`（UInt8值的集合）

```Swift
for codeUnit in dogString.utf8 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// Prints "68 111 103 226 128 188 240 159 144 182 "
```

![image](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/UTF8_2x.png)


如上图，双引号需要三个UTF-8 编码单元，🐶则需要四位


### UTF-16 Representation

获取String的UTF-16编码单元集合，可以遍历String的`utf16`属性，这个属性的类型是`String.UTF16View`（UInt16值的集合）


```Swift
for codeUnit in dogString.utf16 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// Prints "68 111 103 8252 55357 56374 "
```
![image](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/UTF16_2x.png)


对于前三个字符（D、o、g）,无论是采用UTF-8还是UTF-16编码，编码单元都是一样的。

对于 !! 而言，UTF-16编码只需要一个编码单元就可以表示

而 🐶 ，UTF-16编码需要两个编码单元就可以表示


### Unicode Scalar Representation

获取String的Unicode scalar编码单元集合，可以遍历String的`unicodeScalars`属性，这个属性的类型是`UnicodeScalarView`（UnicodeScalar的集合）

每一个UnicodeScalar都有一个`value`属性（21-bit值，相当于UInt32值）


```Swift
for scalar in dogString.unicodeScalars {
    print("\(scalar.value) ", terminator: "")
}
print("")
// Prints "68 111 103 8252 128054 "
```

![image](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/UnicodeScalar_2x.png)

因为 

十进制 | 16进制
---|---
8252 | 203C
128054 | 1F436

所以!!和🐶均可以用一个Unicode scalar编码单元来表示


```Swift
for scalar in dogString.unicodeScalars {
    print("\(scalar) ")
}
// D
// o
// g
// ‼
// 🐶
```
每个UnicodeScalar都可以看做一个新的String值



