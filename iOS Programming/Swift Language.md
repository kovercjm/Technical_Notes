# 0 Reference

This article is summarized from the [Offical Swift Book](https://docs.swift.org/swift-book/).

# 1 Swift Basics

## 1.1 Variable and Constant

| Type                        | Value                                                        |
| --------------------------- | ------------------------------------------------------------ |
| **Int**                     | Integers                                                     |
| **Double** & **Float**      | Floating-point values                                        |
| **Bool**                    | Boolean values                                               |
| **String**                  | Textual values                                               |
| Collection - **Array**      | Ordered collections of values                                |
| Collection - **Set**        | Unordered collections of unique values                       |
| Collection - **Dictionary** | Unordered collections of key-value associations              |
| Tuple **( , )**             | A single compound value of mutiple values                    |
| Optional **?** & **!**      | either “there *is* a value, and it equals *x*”or “there *isn’t* a value at all” |

<div align=center>
<img src="https://docs.swift.org/swift-book/_images/CollectionTypes_intro_2x.png"/>
<br />
Collection Types Illustration
</div>

Several points to be noted:

- The constants are declared by `let` and variables by `var`.

- Something especial for string is to use `\(variable)` as a *string interpolation* to replace in string of a variable's current value.

- To refer an existing type by a name, using `typealias` key word like the following.

  ``` swift
  typealias AudioSample = UInt16
  ```

- Non-Boolean value cannot been substituted in condition statement.

  ``` swift
  let i = 1
  if i { }						// This cannot compile and will report an error
  ```
  
- Tuple can contain different types and can be decomposed.

  ``` swift
  let http404Error = (404, "Not Found")
  let (statusCode, _) = http404Error
  ```

## 1.2 Optionals

Optional value is used when a variable has a value or don't.

``` swift
var optional: String? = nil						// variable has no value
optional = "something"								// variable has value
```

There are several ways to unwrap an optional value:

- `if` statement
- force unwrapping
- implicitly unwrapping

``` swift
// safer way to get optional value by Optional Binding
if let getOptionalValue1 = optional {
  print("Optional value got: \(getOptionalVaule2)")
}

// force unwrap an optional value
// will report error if optional is nil
let getOptionalValue2 = optional!	

// implicitly unwrapping
let getOptionalValue3: String! = optional
```

When an optional value is nested, the Optional Binding can be used like a chain call.

``` swift
let label: UILabel?
if let hashText = label?.text?.hashText {
  // both label and text are not nil
} else {
  // either label or text is nil
}
```

Using `??` to set default value for getting value from an optional variable.

``` swift
let text = optional ?? "placeholder"
```

# 2 Basic Operators

## 2.1 Assignment Operator

An assignment operator (`a = b`) can initialize or update the value on the right size, and if the right side is a tuple with multiple values, its elements can be decomposed.

``` swift
let (x, y) = (1, 2)						// now x equals to 1 and y equals to 2
```

The assignment operator doesn't return a value, so that `if x = y { }` will be invalid.

## 2.2 Arithmetic Operators

Swift supports for standard arithmetic operators (`+`, `-`, `*`, `/`) for all number types and String types. And Remainder Operator (`a % b`), Unary Minus Operator (`-a`) and Compound Assignment Operators (`a += b`) are also supported.

## 2.3 Comparison Operators

Swift supports the following comparison operators:

- Equal to (`a == b`)
- Not equal to (`a != b`)
- Greater than (`a > b`)
- Less than (`a < b`)
- Greater than or equal to (`a >= b`)
- Less than or equal to (`a <= b`)

Tuples can also be compared.

``` swift
(1, "zebra") < (2, "apple")   // true - because 1 is less than 2; "zebra" and "apple" are not compared
(3, "apple") < (3, "bird")    // true - because 3 is equal to 3, and "apple" is less than "bird"
(4, "dog") == (4, "dog")      // true - because 4 is equal to 4, and "dog" is equal to "dog"
("blue", false) < ("purple", true)  // Error because compare Boolean values
```

## 2.4 Ternary Conditional Operator

Ternary Conditional Operator (`condition ? trueAction : falseAction`) is a shorthand for the code below:

``` swift
if condition {
    trueAction
} else {
    falseAction
}
```

## 2.5 Nil-Coalescing Operator

The Nil-Coalescing Operator (`a ?? b`) unwraps an optional `a` if contains value or returns default value `b`, which is a shorthand for the code below:

``` swift
a != nil ? a! : b
```

## 2.6 Range Operators

Swift includes Close Range Operator (`a...b`), Half-Open Range Operator (`a..<b`) and One-Sided Ranges (`...2`).

## 2.7 Logical Operators

Swift supports three standard logic operators: 

- Logical NOT (`!a`)
- Logical AND (`a && b`)
- Logical OR (`a || b`)

# 3 Strings and Characters

## 3.1 String Literals

A string literal surrounded by `"`, and a multiline string literal surrounded by `"""`.

``` swift
// The following literals are the same.
let singleLineString = "These are the same."
let multilineString = """
These are the same.
"""

// \ stands for line break, below literal is the same with following.
let multilineStringWithLineBreak = """
These are \
the same.
"""
```

![../_images/multilineStringWhitespace_2x.png](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/multilineStringWhitespace_2x.png)

String literals can include the following special characters:

- The escaped special characters `\0` (null character), `\\` (backslash), `\t` (horizontal tab), `\n` (line feed), `\r` (carriage return), `\"` (double quotation mark) and `\'` (single quotation mark)
- An arbitrary Unicode scalar value, written as `\u{`*n*`}`, where *n* is a 1–8 digit hexadecimal number

## 3.2 Extended String Delimiters

A number sign (`#`) will include special characters in a string without invoking their effect.

``` swift
print(#"\n"#)			// Prints "\n"
print(#"\#n"#)		// Prints a new line
```

## 3.3 String Interpolation

Items can be insert into the string literal wrapped in a pair of parentheses, prefixed by `\`.

``` swift
let multiplier = 3
let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)"
// message is "3 times 2.5 is 7.5"
```

# 4 Collection Types

Swift supports Array, Set and Dictionary.

# 5 Control Flow

## 5.1 For-In Loops

For-in loops can used to iterate a sequence, such as an array, number ranges or String.

``` swift
let array = ["a", "b", "c"]
for arrayItem in array { }

let dictionary = ["key1": "value1", "key2": "value2"]
for (key, value) in dictionary { }

for number in 1..5 { }

for index in stride(from: 1, to: 5, by: 2) {}
```

## 5.2 While Loops

Swift supports two kinds of while loops:

- `while` evaluates its condition at the start of each pass through the loop.
- `repeat`-`while` evaluates its condition at the end of each pass through the loop.

## 5.3 Conditional Statements

Swift provides two ways to add conditional branches to your code: the `if` statement and the `switch` statement. In `switch` statement, case can be a value, a tuple, a range or multiple values, or `default` case. A swtich case can also have a `where` condition.

``` swift
// Value bindings in switch statement
let anotherPoint = (2, 0)
switch anotherPoint {
case (let x, 0):
    print("on the x-axis with an x value of \(x)")
case (0, let y):
    print("on the y-axis with a y value of \(y)")
case let (x, y):
    print("somewhere else at (\(x), \(y))")
}
// Prints "on the x-axis with an x value of 2"
```

## 5.4 Control Transfer Statements

Swift has five control transfer statements:

- `continue`: tells a loop to stop what it is doing and start again at the beginning of the next iteration through the loop
- `break`: ends execution of an entire control flow statement immediately
- `fallthrough`: fall through the bottom of each case and into the next one
- `return`
- `throw`

Swift can have Labeled Statements for Control Transfer Statements to point at directly.

In addition, a `guard` statement to require that a condition must be true in order for the code after the `guard` statement to be executed, and always has an `else` clause to executed if the condition is not true.

## 5.5 API Availability

To use an *availability condition* in an `if` or `guard` statement to conditionally execute a block of code, depending on whether the APIs you want to use are available at runtime.

``` swift
if #available(platform name & version, ..., *) {
    statements to execute if the APIs are available
} else {
    fallback statements to execute if the APIs are unavailable
}
```

# 6 Functions

## 6.1 Function Parameters

A function has a name, and can have or have no input and output parameter(s). Parameter can have Argument Labels before its name, or have Omitting Arugument Label (`_`). And also, parameter can have default value. The output parameter can be a tuple for multiple output results.

``` swift
func noParamFunc() { }
func multiParamFunc(oneParam: String, _ twoParam: String, and threeParam: String, fourParam: Int = 4) -> (Int, Int){ }
let (a, b) = multiParamFunc(oneParam: "one", "two", and: "three")
```

Function can have one and only one Variadic Parameter and also In-Out Parameters.

``` swift
func variadicParam(numbers: Int...) { }
variadicParam(numbers: [1, 2, 3])

func inOutParam(_ a: inout Int, _ b: inout Int) {
  let temp = a; a = b; b = temp;
}
var a = 1, b = 2
inOutParam(&a, &b)				// now a equals to 2 and b equals to 1
```



## 6.2 Function Types

Function types just like any other types in Swift.

``` swift
var newFunction: (Int, Int) -> Int = oldFunction

func funcParam(_ function: (Int, Int) -> Int) -> (Int) -> Int { }
```

If functions are defined inside the bodies of other functions, known as *nested functions*, they are hidden from the outside world by default. An enclosing function can also return one of its nested functions to allow the nested function to be used in another scope.

``` swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backward ? stepBackward : stepForward
}
var currentValue = -2
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero now refers to the nested stepForward() function
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
print("zero!")
// -2...
// -1...
// zero!
```

# 7 Closures

Global and nested functions are actually special cases of closures. Closures take one of three forms:

- Global functions are closures that have a name and don’t capture any values.
- Nested functions are closures that have a name and can capture values from their enclosing function.
- Closure expressions are unnamed closures written in a lightweight syntax that can capture values from their surrounding context.

# 9 Structures and Classes

Structures and classes in Swift have many things in common. Both can:

- Define properties to store values
- Define methods to provide functionality
- Define subscripts to provide access to their values using subscript syntax
- Define initializers to set up their initial state
- Be extended to expand their functionality beyond a default implementation
- Conform to protocols to provide standard functionality of a certain kind

Classes have additional capabilities that structures don’t have:

- Inheritance enables one class to inherit the characteristics of another.
- Type casting enables you to check and interpret the type of a class instance at runtime.
- Deinitializers enable an instance of a class to free up any resources it has assigned.
- Reference counting allows more than one reference to a class instance.

## 9.1 Structures and Enumerations Are Value Types

A *value type* is a type whose value is *copied* when it’s assigned to a variable or constant, or when it’s passed to a function.

## 9.2 Classes Are Reference Types

Unlike value types, *reference types* are *not* copied when they are assigned to a variable or constant, or when they are passed to a function. Rather than a copy, a reference to the same existing instance is used.

Swift provides two identity operators to find out whether two constants or variables refer to exactly the same instance of a class:

- Identical to (`===`)
- Not identical to (`!==`)

# 10 Properties

Property association:

- Computed properties are provided by classes, structures and enumerations;
- Stored properties are provided by classes and structures.

Properties are usually associated with instances of a particular type, but can also associated with the *type* itself.

## 10.1 Stored properties

Stored property is a constant (`let`) or variable (`var`) that is stored as **part** of an instance. 

``` swift
struct StoredProperties {
  var variable: Int = 0			// can provide default value or not
  let constant: Int
}

let constantItem = StoredProperties(variable: 1, constant: 2)
// Cannot change a constant Item of its variable property
// constItem.variable = 0;
```

## 10.2 Lazy stored properties

A lazy stored property (`lazy var`) is a property whose initial value is not calculated until the first time it is used.

> NOTE: If a property marked with the `lazy` modifier is accessed by multiple threads simultaneously and the property has not yet been initialized, there is no guarantee that the property will be initialized only once.

## 10.3 Computed properties

Instead of actually store a value, computed properties provide a getter and a *optional* setter to retrieve and set other properties and values. If no setter, the property will be known as a *read-only* computed property.

``` swift
struct ComputedProperties {
  var variable: Int
  var computedProperty: Int {
    set(newData) {
      variable = newData
    }
    get {
      return variable
    }
  }
  var shorthandComputedProperty: Int {
    // if not define a name for the new value
    // a default name of newValue is used
    set {
      variable = newValue
    }
    // if entire body of getter is a single expression
    // the expression will implicitly return expression
    get {
      variable
    }
  }
}
```

## 10.4 Property observers

Property observers are called every time a property’s value is **set**, even if the new value is the same as the property’s current value. Observers can be **added** to any inherited property by **overriding** the property within a subclass.

- `willSet` is called just before the value is stored, the default parameter name is `newValue`
- `didSet` is called immediately after the new value is stored, the default parameter name is `oldValue`

``` swift
class A {
  var number: Int {
    get {
      print("get")
      return 1
    }
    set {
      print("set")
    }
  }
}

class B: A {
  override var number: Int {
    willSet {
      print("willSet")
    }
    didSet {
      print("didSet")
    }
  }
}

let b = B()
b.number = 0
// Output will be:
// get				-> didSet() will get oldValue, so getter is called
// willSet
// set
// didSet

```

## 10.5 Property wrapper

To provide some safety-check, a property wrapper can be defined. A structure, enumeration, or class can define a `wrappedValue` property.

``` swift
@propertyWrapper
struct TwelveOrLess {
  private var number: Int
  init() {
    self.number = 0
  }
  var wrappedValue: Int {
    get {
      return number
    }
    set {
      number = min(newValue, 12)
    }
  }
}
```

The wrapper will make sure new value is less than 12.

``` swift
struct SmallRectangle {
  @TwelveOrLess var height: Int
  @TwelveOrLess var width: Int
}
var rectangle = SmallRectangle()
print(rectangle.height)
// 0

rectangle.height = 10
print(rectangle.height)
// 10

rectangle.height = 24
print(rectangle.height)
// 12
```

