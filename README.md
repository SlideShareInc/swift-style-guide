Official SlideShare Swift Style Guide
===========================

###This is the SlideShare Swift Style Guide we are using for our upcoming iOS 8 only app written in Swift. Please feel free to submit pull requests, so that this may be helpful for anyone else starting with Swift.

Use spaces for tabs and 2 spaces per tab (Change the default in Xcode->Preferences->Text Editing->Indentation)

Switch statements should have each case statement not indented and all code executed for that case indented below:

```swift
switch value {
case 1:
  test = "abc"
default:
  test = "xyz"
}
```

If making a read-only computed variable, provide the getter without the get {} around it:

```swift
var computedProp: String {
  if someBool {
    return "Hello"
  }
}
```

If making a computed variable that is readwrite, have get {} and set{} not indented:

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

Same rule as above but for willSet and didSet:

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

Though you can create a custom name for the new or old value for willSet/didSet and set, use the standard newValue/oldValue identifiers that are provided by default:

```swift
var property = 10 {
willSet {
  if newValue == 10 {
    println("It’s 10")
 }
didSet {
 if oldValue == 10 {
   println("It was 10")
 }
}
```

Do not use parameters when declaring parameter names to use in a closure. Also, keep parameter names on same line as opening brace for closures:

```swift
doSomethingWithCompletion() { param1 in
  println("\(param1)")
}
```

If declaring a variable with its type, place the colon directly after the identifier with a space and then the type:

```swift
	class var testVar: String
```

Always use trailing closure syntax if a closure is the last parameter of a method:

```swift
func newMethod(input: Int, onComplete methodToRun: (input: Int) -> ()) {
  // content
}
```

```swift
newMethod(10) { param in
  println("output: \(param)"")
}
```

However, if there are 2 closures as the last parameters, do not use trailing closure syntax on the last one as this is ambiguous. Also, when creating a closure inline with for a method parameter, put the parameter name on a new line and follow the following indentation rules:

```swift
testMethod(param: 2.5,
  success: {
    println("success")
  },
  failure: {
    println("failure")
  })
```

Use trailing closure syntax also if a closure is the only parameter:

```swift
	array1.map { /* content */ }
```

Only use self.<parameter name> if you need to, which is when you have a parameter of the same name as the instance variable, or in closures:

If declaring the type of a function or closure with no return type, specify this by using () as the return type. Also, put a space before and after -> when declaring a closure type:

```swift
func takeClosure(aClosure: () -> ()) {
  // content
}
```

If creating a function or closure with no return type, do not specify one:

```swift
func noReturn() {
  // content
}
```

Implement a singleton by having this at the top of your class definition:

```swift
class ClassA {
  func sharedInstance() -> ClassA {
    struct Static {
      static let instance = ClassA()
    }
    return Static.instance
  }
}
```

When declaring dictionary types, include a space before after the key type and after the colon:

```swift
var someDictionary: [String : Int]
```
