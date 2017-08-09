## Functions  方法

<<<<<<< HEAD
#### 目录

- [Defining and Calling Functions](#defining-and-calling-functions)
- [Function Parameters and Return Values](#function-parameters-and-return-values)
  - [Functions Without Parameters](#functions-without-parameters)
  - [Functions With Multiple Parameters](#functions-with-multiple-parameters)
  - [Functions Without Return Values](#functions-without-return-values)
  - [Functions with Multiple Return Values](#functions-with-multiple-return-values)
  - [Optional Tuple Return Types](#optional-tuple-return-types)
- [Function Argument Labels and Parameter Names](#function-argument-labels-and-parameter-names)
  - [Specifying Argument Labels](#specifying-argument-labels)
  - [Omitting Argument Labels](#omitting-argument-labels)
  - [Default Parameter Values](#default-parameter-values)
  - [Variadic Parameters](#variadic-parameters)
  - [In-Out Parameters](#in-out-parameters)

- [Function Types](#function-types)
  - [Using Function Types](#using-function-types)
  - [Function Types as Parameter Types](#function-types-as-parameter-types)
  - [Function Types as Return Types](#function-types-as-return-types)
- [Nested Functions](#nested-functions)


---
=======
- [22222](#我们的祖国-define)

- [hhhhh](#function-parameters-and-return-values)

- [function](#function)
>>>>>>> origin/master

### Defining and Calling Functions


### Function Parameters and Return Values


#### Functions Without Parameters



#### Functions With Multiple Parameters



#### Functions Without Return Values



#### Functions with Multiple Return Values



<<<<<<< HEAD
#### Optional Tuple Return Types





### Function Argument Labels and Parameter Names




#### Specifying Argument Labels



#### Omitting Argument Labels



#### Default Parameter Values



#### Variadic Parameters



#### In-Out Parameters






### Function Types




#### Using Function Types



#### Function Types as Parameter Types



#### Function Types as Return Types




### Nested Functions

=======
### Function Parameters and Return Values



All of this information is rolled up into the function’s definition, which is prefixed with the func keyword. You indicate the function’s return type with the return arrow -> (a hyphen followed by a right angle bracket), which is followed by the name of the type to return.

The definition describes what the function does, what it expects to receive, and what it returns when it is done. The definition makes it easy for the function to be called unambiguously from elsewhere in your code:

print(greet(person: "Anna"))
// Prints "Hello, Anna!"
print(greet(person: "Brian"))
// Prints "Hello, Brian!"
You call the greet(person:) function by passing it a String value after the person argument label, such as greet(person: "Anna"). Because the function returns a String value, greet(person:) can be wrapped in a call to the print(_:separator:terminator:) function to print that string and see its return value, as shown above.

NOTE

The print(_:separator:terminator:) function doesn’t have a label for its first argument, and its other arguments are optional because they have a default value. These variations on function syntax are discussed below in Function Argument Labels and Parameter Names and Default Parameter Values.


### Function

string and see its return value, as shown above.

NOTE

The print(_:separator:terminator:) function doesn’t have a label for its first argument, and its other arguments are optional because they have a default value. These variations on function syntax are discussed below in Function Argument Labels and Parameter Names and Default Parameter Values.



### 我们的祖国 define

string and see its return value, as shown above.

NOTE

The print(_:separator:terminator:) function doesn’t have a label for its first argument, and its other arguments are optional because they have a default value. These variations on function syntax are discussed below in Function Argument Labels and Parameter Names and Default Parameter Values.
>>>>>>> origin/master

