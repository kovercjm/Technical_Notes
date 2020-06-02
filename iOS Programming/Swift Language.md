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
  if i {							// It will not compile and report an error
  }
  ```

- Tuple can contain different types and can be decomposed.

  ``` swift
  let http404Error = (404, "Not Found")
  let (statusCode, _) = http404Error
  ```

- 

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

