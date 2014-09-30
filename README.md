Official SlideShare Swift Style Guide
===========================
This is the SlideShare Swift Style Guide we are using for our upcoming iOS 8 only app written in Swift. Please feel free to submit pull requests, so that this may be helpful for anyone else using Swift.

### Table Of Contents

* [Xcode Preferences](#xcode-preferences)
* [Switch](#switch)
* [Properties](#properties)
* [Closures](#closures)
* [Identifiers](#identifiers)
* [Singleton](#singleton)
* [Collections](#collections)
* [Optionals](#optionals)

---


#### Xcode Preferences
- Use spaces for tabs and 4 spaces per tab (Change the default in Xcode->Preferences->Text Editing->Indentation)


#### Switch
- Switch statements should have each case statement not indented and all code executed for that case indented below:

```swift
switch value {
case 1:
	test = "abc"
default:
	test = "xyz"
}
```

#### Propreties
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
- When appending to an Array or String, always use the append method instead of the += operator. This is because the += operator will not append single elements.

```swift
var array = [1, 2, 3]
array.append(4)
```

#### Optionals
- When unwrapping optionals, rebind the optional to the same name, unless there is a reason not to. This is example shows this, but in this case it should be done within the binding.

```swift
let bike = possibleBike() // this returns an optional
if let bike = bike {
	// do something with bike
}
```
