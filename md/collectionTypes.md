## Collection Types 集合类型

## 目录

- [Mutability of Collections 集合的可变性](#mutability-of-collections-集合的可变性)
- [Arrays 数组](#arrays-数组)  
  - [Creating an Array with a Default Value  创建带默认值的数组](#creating-an-array-with-a-default-value-创建带默认值的数组)
  - [Creating an Array with an Array Literal](#creating-an-array-with-an-array-literal)
  - [Accessing and Modifying an Array 数组的存取和修改](#accessing-and-modifying-an-array-数组的存取和修改)
  - [Iterating Over an Array 数组遍历](#iterating-over-an-array-数组遍历)
- [Sets 集合](#sets-集合)
  - [Hash Values for Set Types](#hash-values-for-set-types)
  - [Set Type Syntax](#set-type-syntax)
  - [Creating and Initializing an Empty Set 初始化一个空集合](#creating-and-initializing-an-empty-set-初始化一个空集合)
  - [Creating a Set with an Array Literal  用数组字面量来创建集合](#creating-a-set-with-an-array-literal-用数组字面量来创建集合)
  - [Accessing and Modifying a Set 集合的访问和修改](#accessing-and-modifying-a-set-集合的访问和修改)
  - [Iterating Over a Set 集合遍历](#iterating-over-a-set-集合遍历)

- [Performing Set Operations 集合间比较](#performing-set-operations-集合间比较)
  - [Fundamental Set Operations 基本运算](#fundamental-set-operations-基本运算)
  - [Set Membership and Equality](#set-membership-and-equality)
  
- [Dictionaries 字典](#dictionaries-字典)
  - [Creating an Empty Dictionary](#creating-an-empty-dictionary)
  - [Creating a Dictionary with a Dictionary Literal 字面量创建](#creating-a-dictionary-with-a-dictionary-literal-字面量创建)
  - [Accessing and Modifying a Dictionary 字典存取和修改](#accessing-and-modifying-a-dictionary-字典存取和修改)
  - [Iterating Over a Dictionary 字典遍历](#iterating-over-a-dictionary-字典遍历)
  - 
---
### 前言
Swift主要有三种集合类型

- arrays(数组)有序值的集合
- sets(集合)无序不重复值的集合
- dictionaries(字典)键值对集合

![image](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/CollectionTypes_intro_2x.png)

以上三种类型对存入的数据类型有严格的控制，定义什么类型就只能存入什么类型，不会存入非定义的数据类型，所以取出来的数据类型肯定和定义的数据类型是一致的，可以放心大胆的使用。


---
## Mutability of Collections 集合的可变性

- 可变集合，在创建之后可以对它进行添加、删除、改变值的操作。
- 不可变集合，在创建之后，长度和内容都是不能改变的

---
## Arrays 数组

一个数组可以存储一样的值，但是所有的值的类型都是一致的

### Array Type Shorthand Syntax 数组简写语法

数组的完整定义是  `Array<Element>` 形式（Element是存储的类型），通常我们简写为 `[Element]` ,两种定义都一样，推荐简写。

### Creating an Empty Array 空数组创建

用初始化方法来创建一个空数组

```
var someInts = [Int]()
print("someInts is of type [Int] with \(someInts.count) items.")
// Prints "someInts is of type [Int] with 0 items."
```
Arrays的存储类型定义后不可改变


```
someInts.append(3)
// someInts now contains 1 value of type Int
someInts = []
// someInts is now an empty array, but is still of type [Int]

```

### Creating an Array with a Default Value  创建带默认值的数组

Array可以初始化一组相同的值，并设定它的个数。

```
var threeDoubles = Array(repeating: 0.0, count: 3)
// threeDoubles is of type [Double], and equals [0.0, 0.0, 0.0]

```
### Creating an Array by Adding Two Arrays Together 数组相加

如何两个数组类型一直的，就可以通过`+ `号运算符合并

```
var anotherThreeDoubles = Array(repeating: 2.5, count: 3)
// anotherThreeDoubles is of type [Double], and equals [2.5, 2.5, 2.5]
 
var sixDoubles = threeDoubles + anotherThreeDoubles
// sixDoubles is inferred as [Double], and equals [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]

```

### Creating an Array with an Array Literal

Array初始化的时候可以挨个写入值，如下格式

```
var shoppingList: [String] = ["Eggs", "Milk"]
// shoppingList has been initialized with two initial items

```
因为是用 `var `声明的，所以`shoppingList`是个可变数组。

```
shoppingList.append("ggg")
```

如果数组元素类型一致，可以简化初始化方法为

```
var shoppingList = ["Eggs", "Milk"]
```
Swift能自动判断出Array的存储类型

### Accessing and Modifying an Array 数组的存取和修改

查询数组个数，使用 `count `属性

```
print("The shopping list contains \(shoppingList.count) items.")
// Prints "The shopping list contains 2 items."
```
判断数组的`count`是否等于0，使用` isEmpty `属性


```
if shoppingList.isEmpty {
    print("The shopping list is empty.")
} else {
    print("The shopping list is not empty.")
}
// Prints "The shopping list is not empty."

```

使用`append(_:) `方法添加元素

```
shoppingList.append("Flour")
// shoppingList now contains 3 items, and someone is making pancakes
```
使用运算符`+=`添加元素


```
shoppingList += ["Baking Powder"]
// shoppingList now contains 4 items
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
// shoppingList now contains 7 items
```

通过下标索引获取元素值

```
var firstItem = shoppingList[0]
// firstItem is equal to "Eggs"
```
> ***Swift的索引是从0开始的***


修改某个索引的值（主要不要超出数组范围，不然报错）

```
shoppingList[0] = "Six eggs"
// the first item in the list is now equal to "Six eggs" rather than "Eggs"
```

也可以通过索引替换某个范围的元素，如下

```
shoppingList[4...6] = ["Bananas", "Apples"]
// shoppingList now contains 6 items
//"Chocolate Spread", "Cheese", and "Butter" 替换为 "Bananas" and "Apples"
```
在某个索引插入一个值，使用`insert(_:at:)`方法


```
shoppingList.insert("Maple Syrup", at: 0)
// shoppingList now contains 7 items
// "Maple Syrup" is now the first item in the list
```
移除某个索引的值使用`remove(at:)`方法（方法返回值为移除的元素）

```
let mapleSyrup = shoppingList.remove(at: 0)
// the item that was at index 0 has just been removed
// shoppingList now contains 6 items, and no Maple Syrup
// the mapleSyrup constant is now equal to the removed "Maple Syrup" string

```

> 为了避免runtime错误，在Array使用前先检查下数组的有效索引（最大索引为<array.count -1>）， 如果是空数组，那么就没有有效索引


一个元素移除后，它后面的元素索引均-1

```
firstItem = shoppingList[0]
// firstItem is now equal to "Six eggs"
```

移除最后一个元素，可以使用`removeLast()` 和`remove(at:)`方法，推荐前者，因为查询数组的个数


```
let apples = shoppingList.removeLast()
// the last item in the array has just been removed
// shoppingList now contains 5 items, and no apples
// the apples constant is now equal to the removed "Apples" string

```


### Iterating Over an Array 数组遍历

使用`for-in` 循环遍历

```
for item in shoppingList {
    print(item)
}
// Six eggs
// Milk
// Flour
// Baking Powder
// Bananas
```

如果需要元素索引，需要用到`enumerated()`方法

```
for (index, value) in shoppingList.enumerated() {
    print("Item \(index + 1): \(value)")
}
// Item 1: Six eggs
// Item 2: Milk
// Item 3: Flour
// Item 4: Baking Powder
// Item 5: Bananas

```

---
## Sets 集合

一个无序的、元素不重复的、同数据类型的数据集合。


### Hash Values for Set Types 

集合（set）的元素必须是 hashable（哈希化）类型。
hashable类型必须实现计算自己 hash value（哈希值）的方法。这个`hash value`是`int`类型，和其他对象一样，可以用来比较。
如

```
if a == b
事实上是判断
a.hashValue == b.hashValue
```

Swift的所有基础类型（像 String, Int, Double,  Bool）默认都是`hashable`类型，所以可以作为Set和Dictionary的元素类型，枚举类型也是是`hashable`类型的。

只要遵从Hashable协议（必须实现一个`Int`类型`hashValue`属性的`get`方法）的自定义类型都可以作为Set和Dictionary的元素类型。
Hashable协议遵循Equatable协议，Equatable协议要求只要遵循 == 就是等价的关系，所以遵循Hashable协议的类型也要实现一个 == 的运算。比如，要实现 == 的关系，a、b、c必须满足下面三个条件


```
a == a (Reflexivity 自反性)

a == b implies b == a (Symmetry  对称性)

a == b && b == c implies a == c (Transitivity 传递性)
```

### Set Type Syntax 

格式：`Set<Element>,Element`就是Set的元素类型。（没有简写）。

### Creating and Initializing an Empty Set 初始化一个空集合

使用初始化语法创建一个空Set

```
var letters = Set<Character>()
print("letters is of type Set<Character> with \(letters.count) items.")
// Prints "letters is of type Set<Character> with 0 items."
```

如果集合已经定义了元素类型并且插入了元素，就可以用空数组来创建一个空集合。

```
letters.insert("a")
// letters now contains 1 value of type Character
letters = []
// letters is now an empty set, but is still of type Set<Character>

```

### Creating a Set with an Array Literal  用数组字面量来创建集合

用数组字面量创建一个String类型的集合，如下

```
var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]
// favoriteGenres has been initialized with three initial items
```
如果数组字面量类型都是一致的，可简写为

```
var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"]
//系统自动识别
```

### Accessing and Modifying a Set 集合的访问和修改

获取集合数量使用`count`（只读）属性


```
print("I have \(favoriteGenres.count) favorite music genres.")
// Prints "I have 3 favorite music genres."
```

使用`isEmpty`属性判断`count`是否为0


```
if favoriteGenres.isEmpty {
    print("As far as music goes, I'm not picky.")
} else {
    print("I have particular music preferences.")
}
// Prints "I have particular music preferences."
```

使用`insert(_:)`插入一个新元素

```
favoriteGenres.insert("Jazz")
// favoriteGenres now contains 4 items
```

 移除元素使用`remove(_:)`方法，返回值为移除的元素，如果元素不存在，则返回`nil`，若要移除所有元素使用`removeAll()`方法
 
```
if let removedGenre = favoriteGenres.remove("Rock") {
    print("\(removedGenre)? I'm over it.")
} else {
    print("I never much cared for that.")
}
// Prints "Rock? I'm over it."
```

查看集合是否包含某个元素使用 `contains(_:)`方法


```
if favoriteGenres.contains("Funk") {
    print("I get up on the good foot.")
} else {
    print("It's too funky in here.")
}
// Prints "It's too funky in here."

```

### Iterating Over a Set 集合遍历

使用`for-in`遍历


```
for genre in favoriteGenres {
    print("\(genre)")
}
// Jazz
// Hip hop
// Classical

```

如果想让遍历后的数据有序使用`sorted()`方法

```
默认按字母升序
for genre in favoriteGenres.sorted() {
    print("\(genre)")
}
// Classical
// Hip hop
// Jazz

按降序排列
for genre in favoriteGenres.sorted({by:{ (a,b) -> Bool in
    return a > b
}) {
    print("\(genre)")
}
// Jazz
// Hip hop
// Classical

```
---
## Performing Set Operations 集合间比较

对集合进行对比运算，比如交集、并集、差集、补集

### Fundamental Set Operations 基本运算

以下是集合a和b关系的韦恩图（文氏图）
![image](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/setVennDiagram_2x.png)


-  `intersection(_:)` 取a和b的交集
-  `symmetricDifference(_:) `取交集的补集（或者补集的并集）
-  `union(_:) `取并集
-  `subtracting(_:) `取补集

```
let oddDigits: Set = [1, 3, 5, 7, 9]
let evenDigits: Set = [0, 2, 4, 6, 8]
let singleDigitPrimeNumbers: Set = [2, 3, 5, 7]
 
oddDigits.union(evenDigits).sorted()
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
oddDigits.intersection(evenDigits).sorted()
// []
oddDigits.subtracting(singleDigitPrimeNumbers).sorted()
// [1, 9]
oddDigits.symmetricDifference(singleDigitPrimeNumbers).sorted()
// [1, 2, 9]

```

### Set Membership and Equality 

下图
描绘了集合a、b、c三者的元素关系，
其中a是b的父集，因为b的元素a都包含，相反，b是a的子集。b与c没有交集的。

![image](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/setEulerDiagram_2x.png)

- 用`==`可以判断两个集合元素是否完全相等。
- `isSubset(of:)`判断是否是谁的子集（两个集合可以相等）
- `isSuperset(of:)`判断是否是谁的父集（两个集合可以相等）
- `isStrictSubset(of:)`和`isStrictSuperset(of:)`判断真子集和真父集（两个集合不能相同）
- `isDisjoint(with:)`判断是否有交集



```
let houseAnimals: Set = ["🐶", "🐱"]
let farmAnimals: Set = ["🐮", "🐔", "🐑", "🐶", "🐱"]
let cityAnimals: Set = ["🐦", "🐭"]
 
houseAnimals.isSubset(of: farmAnimals)
// true
farmAnimals.isSuperset(of: houseAnimals)
// true
farmAnimals.isDisjoint(with: cityAnimals)
// true

```

---
## Dictionaries 字典

dictionary（字典）就是一个无序的键值对组合，每一个值都对应唯一的键（唯一标识符）。类似于真实的字典一样，我们可以根据唯一标识符来查询词的定义。

### Dictionary Type Shorthand Syntax  简写语法

字典的定义格式 `Dictionary<Key, Value>`，`key`是键的数据类型，`value`是值的数据类型。

也可以简写为 `[Key: Value]`格式，推荐简写。

### Creating an Empty Dictionary

初始化一个空字典

```
var namesOfIntegers = [Int: String]()
// namesOfIntegers is an empty [Int: String] dictionary
```

如果已经定义了字典的键值类型，也可以通过` [:] `创建一个空字典。


```
namesOfIntegers[16] = "sixteen"
// namesOfIntegers now contains 1 key-value pair
namesOfIntegers = [:]
// namesOfIntegers is once again an empty dictionary of type [Int: String]

```

### Creating a Dictionary with a Dictionary Literal 字面量创建

直接写入字典元素来创建字典，如下

```
ar airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]

```
如果键类型一致，值类型一致，就可简写为

```
var airports = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```

### Accessing and Modifying a Dictionary 字典存取和修改

获取键值对的个数使用`count`（只读）属性

```
print("The airports dictionary contains \(airports.count) items.")
// Prints "The airports dictionary contains 2 items."
```

使用`isEmpty`属性判断`count`是否为0

```
if airports.isEmpty {
    print("The airports dictionary is empty.")
} else {
    print("The airports dictionary is not empty.")
}
// Prints "The airports dictionary is not empty."
```

插入新元素


```
airports["LHR"] = "London"
// the airports dictionary now contains 3 items
```

修改Value（键值）

```
airports["LHR"] = "London Heathrow"
// the value for "LHR" has been changed to "London Heathrow"
```
👆这个方法虽然能修改某个键值，但是不确定是否修改成功。所以可以使用updateValue(_:forKey:)`这个方法，返回更新前的值来判断是否更新成功。

```
if let oldValue = airports.updateValue("Dublin Airport", forKey: "DUB") {
    print("The old value for DUB was \(oldValue).")
}
// Prints "The old value for DUB was Dublin."

```

也可以使用下标语法来获取特定键的值，如果存在则返回该值，不存在返回nil

```
if let airportName = airports["DUB"] {
    print("The name of the airport is \(airportName).")
} else {
    print("That airport is not in the airports dictionary.")
}
// Prints "The name of the airport is Dublin Airport."
```

通过下标语法移除某个键值对（将其值置为nil即可）

```
airports["APL"] = "Apple International"
// "Apple International" is not the real airport for APL, so delete it
airports["APL"] = nil
// APL has now been removed from the dictionary
```

或者也可使用 `removeValue(forKey:)` 方法来移除

```
if let removedValue = airports.removeValue(forKey: "DUB") {
    print("The removed airport's name is \(removedValue).")
} else {
    print("The airports dictionary does not contain a value for DUB.")
}
// Prints "The removed airport's name is Dublin Airport."

```
###  Iterating Over a Dictionary 字典遍历

使用for-in循环

```
for (airportCode, airportName) in airports {
    print("\(airportCode): \(airportName)")
}
// YYZ: Toronto Pearson
// LHR: London Heathrow
```

获取字典里所有的键或者值

```
for airportCode in airports.keys {
    print("Airport code: \(airportCode)")
}
// Airport code: YYZ
// Airport code: LHR
 
for airportName in airports.values {
    print("Airport name: \(airportName)")
}
// Airport name: Toronto Pearson
// Airport name: London Heathrow
```

如果你想把字典里的键或者值封装成一个数组，则

```
let airportCodes = [String](airports.keys)
// airportCodes is ["YYZ", "LHR"]
 
let airportNames = [String](airports.values)
// airportNames is ["Toronto Pearson", "London Heathrow"]

```

字典是无序的，要想返回有序的，使用`sorted()` 方法，使用类似Set的有序遍历。