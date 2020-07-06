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