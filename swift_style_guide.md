Official SlideShare Swift Style Guide
===========================
This is the SlideShare Swift Style Guide we are using for our upcoming iOS 8 only app written in Swift. Please feel free to submit pull requests, so that this may be helpful for anyone else using Swift.

### Table Of Contents

* [Xcode Preferences](#xcode-preferences)
* [Switch](#switch)
* [Properties](#properties)
* [Closures/Functions](#closures-functions)
* [Identifiers](#identifiers)
* [Singleton](#singleton)
* [Collections](#collections)
* [Optionals](#optionals)
* [Strings](#strings)
* [Enums](#enums)
* [Documentation](#documentation)
* [Protocols](#protocols)

---


#### Xcode Preferences
- Use spaces for tabs and 4 spaces per tab (Change the default in Xcode->Preferences->Text Editing->Indentation)


#### Switch
- Switch statements should have each case statement not indented and all code executed for that case indented below:

```swift
var value = 2
var test: String?

switch value {
case 1:
    test = "abc"
default:
    test = "xyz"
}
```

- If you want to match multiple values within an object or struct, create a tuple with the two values:

```swift
struct TestValue {
    enum Val {
        case A
        case B
    }
    var value: Val = .A
    var detail: String = "Test"
}
var testValue = TestValue()

switch (testValue.value, testValue.detail) {
case (.A, "Test"):
    println("This is printed")
default:
    println("This is not printed")
}
```

#### Properties
- If making a read-only computed variable, provide the getter without the get {} around it:

```swift
var computedProp: String {
    if someBool {
        return "Hello"
      }
}
```

- If making a computed variable that is readwrite, have get {} and set{} indented:

```swift
var computedProp: String {
    get {
        if someBool {
            return "Hello"
          }
    }
    set {
          println(newValue)
    }
}
```

- Same rule as above but for willSet and didSet:

```swift
var property = 10 {
    willSet {
        println("willSet")
    }
    didSet {
        println("didSet")
    }
}
```

- Though you can create a custom name for the new or old value for willSet/didSet and set, use the standard newValue/oldValue identifiers that are provided by default:

```swift
var property = 10 {
    willSet {
        if newValue == 10 {
            println("It’s 10")
         }
    }
    didSet {
         if oldValue == 10 {
               println("It was 10")
         }
    }
}
```

- Class constants are currently not supported, therefore, if you need that functionality, make it a computed variable:

```swift
class Test {
    class var ConstantValue: String {
        return "TestString"
    }
}
```
    Reasoning: When class constants are supported, the way the constant is accessed will not change.

#### Closures
- Do not use parameters when declaring parameter names to use in a closure. Also, keep parameter names on same line as opening brace for closures:

```swift
doSomethingWithCompletion() { param1 in
    println("\(param1)")
}
```

- Always use trailing closure syntax if there is a single closure as the last parameter of a method:

```swift
// Definition
func newMethod(input: Int, onComplete methodToRun: (input: Int) -> ()) {
    // content
}

// Usage
newMethod(10) { param in
    println("output: \(param)"")
}
```

- However, if there are 2 closures as the last parameters, do not use trailing closure syntax for the last one as this is ambiguous. Also, when creating a closure inline as a method parameter, put the parameter name on a new line and follow the following indentation rules:

```swift
testMethod(param: 2.5,
      success: {
        println("success")
      },
      failure: {
        println("failure")
      })
```

- Use trailing closure syntax also if a closure is the only parameter:

```swift
array1.map { /* content */ }
```

- If declaring the type of a function or closure with no return type, specify this by using Void as the return type. Also, put a space before and after -> when declaring a closure type:

```swift
func takeClosure(aClosure: () -> Void) {
    // content
}
```

- If creating a function or closure with no return type, do not specify one:

```swift
func noReturn() {
    // content
}
```

- If creating a closure that seems to be large (use your best judgement) do not declare inline; create a local variable.

```swift
func foo(something: () -> Void) {
    something()
}

func doEverything() {
    let doSomething = {
        var x = 1
        for 1...3 {
            x++
        }
        println(x)
    }
    foo(doSomething)
}
```

#### Identifiers
- Only use self.<parameter name> if you need to, which is when you have a parameter of the same name as the instance variable, or in closures:

```swift
class Test {

    var a: (() -> Void)?
    var b: Int = 3

    func foo(a: () -> Void) {
        self.a = a
    }

    func foo1() {
        foo() {
            println(self.b)
        }
    }
}
```

- If declaring a variable with its type, place the colon directly after the identifier with a space and then the type:

```swift
class var testVar: String
```

- When declaring dictionary types, include a space before the key type and after the colon:

```swift
var someDictionary: [String : Int]
```

- When declaring a constant, use camel case with the first letter uppercase.

```swift
class TestClass {
    let ConstantValue = 3
}
```

- To declare a set of constant variables not be used for switching, use a struct:

```swift
struct Constants {
    static let A = "A"
    static let B = "B"
}
```

#### Singleton
- Implement a singleton by having this at the top of your class definition:

```swift
class ClassA {
    class var sharedInstance: ClassA {
        struct Static {
            static let instance = ClassA()
        }
        return Static.instance
    }
}
```

#### Collections
- When appending to a collection, always use the append method instead of the += operator.

```swift
var array = [1, 2, 3]
array.append(4)
```

    Reasoning: The += operator will not append single elements.

#### Optionals
- When unwrapping optionals, rebind the optional to the same name, unless there is a reason not to. This is example shows this, but in this case it should be done within the binding.

```swift
func possibleBike() -> Bike? {
    // content
}

let bike = possibleBike()
if let bike = bike {
    // content
}
```

#### Strings
- When appending to a string, always use the += operator.

```swift
var newString = "Hello"
newString += " world!"
```

#### Enums
- When using an enum, always prefer the shorthand syntax when possible. The shorthand syntax should be possible whenever the type does not need to be inferred from the assigned value. Note: there are certain bugs that don't allow them to be used everywhere they should be possible.

```swift
enum TestEnum {
    case A
    case B
}

var theValue: TestEnum?
var shouldBeA = true

if shouldBeA {
    theValue = .A
} else {
    theValue = .B
}
```

#### Documentation
- When documenting a method, use /// if it is only a single line

```swift
/// This method does nothing
func foo() {
    // content
}
```

- If you are documenting a method, use /\*\* to begin and */ to end if it is multiline; do not indent.

```swift
/**
This method does something.
It's very useful.
*/
func foo2() {
    // content
}
```

- Use the standard Swift Documentation syntax (reST) in order to enable Quick Documentation. Follow the formatting below, exactly.

Note: Make sure to test your documentation by checking it's Quick Documentation by option-clicking on the method name.

```swift
/**
This method has parameters and a return type.

:param: input This is an input parameter.

:returns: True if it worked; false otherwise.
*/
func foo3(input: String) -> Bool {
    // content
}
```

> *Note: The following section refers to marks, which are Swift's version of #pragma mark in Objective-C to separate code. There are 2 types of marks: section marks and sub-section marks.*

> Section Marks:

>        // MARK: - Section Name

> Sub-section Marks:

>        // MARK: Sub-section Name

- Use marks to indicate new sections in code. Separate the mark comment with a new line.

```swift
class Stuff {

    // MARK: - Instance methods

    func newMethod() {
        // content
    }
}
```

- The class/struct file layout should be ordered as follows with respect to marks and code organization (Note: See naming convention specified in above note):

        - Section: Singleton
        - Section: Declared Types (Enums, Structs, etc.), Type Aliases
        - Section: Constants
        - Section: Class Properties
        - Section: Instance Properties
            - Sub-section: Stored
            - Sub-section: Computed
        - Section: Init/Deinit
        - Section: Class Methods
            - Sub-section: Public
            - Sub-section: Private
        - Section: Instance Methods
            - Sub-section: Public
            - Sub-section: Private
        - Section: Protocols
            - Sub-section: <Protocol Name>

- When a class implements a protocol, an extension should be created at the bottom of the file  that declares the protocol conformance and implements the protocol. 1 extension per protocol:

```swift
// NewProtocol.swift //
protocol NewProtocol {
    func reqMethod()
}

// Test.swift //
class Test {

    // content
}

// MARK: - Protocols
// MARK: NewProtocol

extension Test: NewProtocol {
    func reqMethod() {
        // content
    }
}
```

#### Protocols
- Do not use @objc in front of a protocol unless you need to specify optional methods. There may be cases where class will need to be added to the protocol definition to avoid use of @objc as in the example below:

```swift
protocol NewProtocol: class {
    // content
}

@objc protocol OptionalProtocol {
    func fooReq()
    optional func fooOpt()
}

class NewClass: OptionalProtocol {
	// Cannot declare a struct as weak. Therefore, the type that implements the protocol must be a class
    weak var delegate: NewProtocol?

    func fooReq() {

    }
}
```
