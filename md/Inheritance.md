# Inheritance

---

## 目录
- [Defining a Base Class-定义基类](#defining-a-base-class)
- [Subclassing-子类](#subclassing)
- [Overriding-重写](#overriding)
  - [Accessing Superclass Methods, Properties, and Subscripts-访问父类的属性和方法](#accessing-superclass-methods-properties-and-subscripts)
  - [Overriding Methods-重写方法](#overriding-methods)
  - [Overriding Properties-重写属性](#overriding-properties)
    - [Overriding Property Getters and Setters-重写属性的getter和setter方法](#overriding-property-getters-and-setters)
    - [Overriding Property Observers-给重写属性添加观察器](#overriding-property-observers)
- [Preventing Overrides-防止重写](#preventing-overrides)

---
## 前言

一个类可以继承另一个类的属性和方法，我们则称继承的类为子类，被继承的类为父类（或者超类）。继承也是类（*class*）区分于其他类型的一个重要特征。

Swift中子类可以访问父类的方法和属性，也可以重写父类的属性和方法，编译器也能检查出子类重写的合法性。

子类也可以对继承来的存储属性和计算属性添加属性观察器来监听值得变化。

---

## Defining a Base Class

基类即不继承自任何一个类的类。
>Swift里面的类不是由一个基类继承而来的，如果定义类的时候不指明它的父类，这个类就是基类。


下面我们定义一个基类`Vehicle`,该基类有一个默认值为`0.0`的存储属性`currentSpeed`,一个只读的计算属性`String`。一个方法`makeNoise()`。

```swift
class Vehicle {
    var currentSpeed = 0.0
    var description: String {
        return "traveling at \(currentSpeed) miles per hour"
    }
    func makeNoise() {
        // do nothing - an arbitrary vehicle doesn't necessarily make a noise
    }
}
```
然后我们初始化一个实例

```swift
let someVehicle = Vehicle()
```
然后获取其`description`属性

```swift
print("Vehicle: \(someVehicle.description)")
// Vehicle: traveling at 0.0 miles per hour
```
上面我们定义了一个通用特性的机动车基类`Vehicle`(机动车)。因为定义的通用属性，所以对于具体的车辆来说基类并没有多大意义。这个时候就可以使用继承来实现具体的机动车类型扩展。也就是创建子类。



---
## Subclassing

子类即基于一个类（这个类称之为父类父类）的基础上创建的一个新的类。子类继承父类的特性，并且可以添加自己的特性。

👇是创建子类的语法：

```swift
//子类SomeSubclass
//父类SomeSuperclass
class SomeSubclass: SomeSuperclass {
    // subclass definition goes here
}
```
基于子类的概念，我们创建一个车辆类`Vehicle`的子类`Bicycle`

```swift
class Bicycle: Vehicle {
    var hasBasket = false
}
```
`Bicycle`继承`Vehicle`的一切特性，包括他的属性`currentSpeed`和`description`,以及方法`makeNoise()`。另外，`Bicycle`还定义了一个属于他自己的存储属性`hasBasket`，默认值为`false`

当创建一个`Bicycle`实例的时候，如果不设置`hasBasket`属性，则默认值是`false`

```swift
let bicycle = Bicycle()
print(bicycle.hasBasket)
//输出false
bicycle.hasBasket = true
```
同样该实例也可以修改其继承属性。
```swift
bicycle.currentSpeed = 15.0
print("Bicycle: \(bicycle.description)")
// Bicycle: traveling at 15.0 miles per hour
```

子类也可以被继承，比如我们创建一个`tandem`类继承自`Bicycle`类

```swift
class Tandem: Bicycle {
    var currentNumberOfPassengers = 0
}
```
同样`tandem`类继承`Bicycle`类的特性以及及`Vehicle`的特性，所以：

```swift
let tandem = Tandem()
tandem.hasBasket = true
tandem.currentNumberOfPassengers = 2
tandem.currentSpeed = 22.0
print("Tandem: \(tandem.description)")
// Tandem: traveling at 22.0 miles per hour

```
---
## Overriding

子类可以对继承自父类的属性和方法进行自定义的实现，我们称之为重写(*overriding*)

我们使用关键字`override`实现对父类特性的重写。`override`不可或缺，它会检查重写的合法性以保证重写的定义是正确的。


### Accessing Superclass Methods-Properties-and Subscripts

我们在重写的时候可能会使用到父类的其他属性或者方法，这个时候我们可以使用前缀`super`来访问父类的属性、方法或者下标。

- `super.someMethod()`调用父类方法
- `super.someProperty`获取父类属性
- `super[someIndex]`获取父类下标


### Overriding Methods

下面我们定义一个子类`Train`来重写父类的方法。

```swift
class Train: Vehicle {
    override func makeNoise() {
        print("Choo Choo")
    }
}
```
然后我们创建一个实例，并调用实例方法。

```swift
//重写实现了对父类方法的覆盖
let train = Train()
train.makeNoise()
// Prints "Choo Choo"

```


### Overriding Properties

同样子类可以重写父类属性,提供自定义的`getter`和`setter`方法，或者添加属性观察器监听属性变化。


#### Overriding Property Getters and Setters

子类可以自定义`getter`和`setter`实现对父类属性的重写。无论父类的属性书存储属性还是计算属性。因为子类并不知道继承属性是存储属性还是计算属性，他只知道继承属性的名字和类型。所以子类重写的时候，必须提供继承属性的名字和类型，这样编译器才能推断出你重写的是哪个继承属性。

子类可以将父类只读(*read-only *)属性重写为读写(*read-write*)属性,但是不能将读写属性重写为只读属性。

> 如果子类重写了属性的`setter`方法，那么必须也重写`getter`方法。如果不想在`getter`方法内改变父类的属性值，可以在`getter`方法内返回父类的属性值。（`return super.someProperty`）


下面我们创建一个子类`Car`继承自`Vehicle`
```swift
class Car: Vehicle {
    //定义一个子类存储属性
    var gear = 1
    //重写父类的属性
    override var description: String {
    //super.description返回父类Vehicle的属性值
        return super.description + " in gear \(gear)"
    }
}
```
👆的子类重写了父类的`description`属性的`getter`方法。下面我们来看看效果。

```swift
let car = Car()
car.currentSpeed = 25.0
car.gear = 3
print("Car: \(car.description)")
// Car: traveling at 25.0 miles per hour in gear 3

```


#### Overriding Property Observers

子类重写属性的同时也可以给属性添加观察器来监听继承属性值的变化。

> 是如果父类的属性为常量存储属性或者只读属性，那么它是不能添加属性观察器的，因为这些属性是不会变化的，所以设置观察器是没必要的。

> 另外属性的`setter`方法和观察器是不能同时重写的，因为观察器就是监听值的变化，而在`setter`方法中就可以监听出值的变化。


下面举个例子，比如一个自动挡的汽车，随着速度的增加，挡位也随之变化。我们用程序抽象为：
```swift
class AutomaticCar: Car {
    override var currentSpeed: Double {
        didSet {
            gear = Int(currentSpeed / 10.0) + 1
        }
    }
}
```

如上，我们重写车辆类的`currentSpeed`属性，并添加属性观察器，当速度变化的时候，挡位也随之变化。

```swift
let automatic = AutomaticCar()
automatic.currentSpeed = 35.0
print("AutomaticCar: \(automatic.description)")
// AutomaticCar: traveling at 35.0 miles per hour in gear 4

```

---

## Preventing Overrides

有些属性或者方法是不允许被子类重写的，这时候我们可以通过关键字`final`来防止被重写。

```swift
//防止属性重写
final var name
//防止实例方法重写
final func setName() {

}
//防止类方法重写
final class func set() {
    
}
//防止下标重写
final subscript(xx:Int){
    
}
//防止类被重写，即加final则类不可被继承
final class Car{
    
}
```

如果重写加`final`的属性或者方法会报编译错误。

另外加`final`的类是不能创建子类的。