Official SlideShare Cocoa/Swift Style Guide
===========================
This is the SlideShare Cocoa/Swift Style Guide we are using for our iOS 8 only app written in Swift. Please feel free to submit pull requests, so that this may be helpful for anyone else using Swift. This document lays out our conventions for using Swift with the Cocoa APIs for iOS.

You can download the app at http://lnkd.in/ssios

### Table Of Contents

* [UITableView](#uitableview)
* [Strings](#strings)
* [NSNotification](#nsnotification)
* [App Delegate](#app-delegate)
* [Core Foundation](#core-foundation)
* [View Controllers](#view-controllers)
* [UIView](#uiview)
* [Objective-C Interoperability](#objective-c-interoperability)

---


#### UITableView
- In a UITableViewCell subclass, create a read-only computed property for the reuse identifier for the cell. Use camel case with first letter uppercase, because it is a constant.

    *Note: Since Swift doesn't yet support stored class properties, that is why we use a read-only computed property for now.*

```swift
class TableViewCell: UITableViewCell {
    class var ReuseIdentifier: String {
        return "TableViewCell"
    }
}
```
> **Reasoning**: We are using this as the temporary solution as class stored properties are not yet supported, because the interface will be the same when that feature is available. Also, once it is supported we would like to create a protocol that has the reuseIdentifier property in it, so that we can just make an extension on UITableViewCell to implement that protocol, so that all UITableViewCell subclasses will need to have it.

- Similar to example above, in a UITableViewCell subclass, create a read-only computed property for the NIB name for the cell.

    *Note: See above explanation for why it is a read-only computed property.*

```swift
class TableViewCell: UITableViewCell {
    class var NibName: String {
        return "TableViewCell"
    }
}
```

#### Strings
- Put any user-facing string in the Localizable.strings file with a key in snake case. Use NSLocalizedString when accessing the strings in code.

```swift
// Localizable.strings //
// <App Section>
"user_facing_string_key" = "This is a user-facing string."

// Someting.swift //
var userFacing = NSLocalizedString("user_facing_string_key", comment: "")
```

#### NSNotification
- Name notifications in reverse domain format with the notification name in snake case

```swift
com.linkedin.slideshare.notification_name
```

- Create a struct to hold all notification names as constants.

```swift
struct GlobalNotifications {
    static let ABC = ""
}
```
- Create a registerNotifications() method and deregisterNotifications().

```swift
func registerNotifications() {
    let notificationCenter = NSNotificationCenter.defaultCenter()

    notificationCenter.addObserver(self, selector: "handleNotificationABC:", name: GlobalNotifications.ABC, object: nil)
}

func deregisterNotifications() {
    NSNotificationCenter.defaultCenter().removeObserver(self)
}
```

#### App Delegate
- Abstract initialization of the application into a separate class.

```swift
class AppInitializer {
    func onAppStart() {
        // content
    }
}
```

#### Core Foundation
- When using Core Graphics structs, such as CGRect, use the initializers instead of the older CGRectMake method.

```swift
var rect = CGRect(x: 10, y: 10, width: 45, height: 300)
```

- If you need to make an instance of a struct zeroed out, utilize the class constant.

```swift
var zeroRect = CGRect.zeroRect
```

#### View Controllers
- If the view controller is associated with a Storyboard, create a class method named newInstance to return an initialized instance of the view controller from the Storyboard.

```swift
class func newInstance() -> MasterViewController {
    return UIStoryboard.initialControllerFromStoryboard("Master") as MasterViewController
}
```

- If you have the situation described above, but have properties that need to be initialized, also create helper methods following the designated/convenience initializer type pattern like so.

```swift
class func newInstanceWithId(id: Int) -> MasterViewController {
        let masterViewController = newInstance()
        masterViewController.id = id
        return masterViewController
}
```

#### UIView
- If you have a class that inherits from UIView and has a XIB file where it is layed out, create a class method named newInstance similar to the example in the View Controllers section.

```swift
class CustomView: UIView {

    class var NibName: String {
        return "CustomView"
    }

    class func newInstance() -> CustomView {
        return NSBundle.mainBundle().loadNibNamed(nibName, owner: nil, options: nil)[0] as CustomView
    }
}
```

#### Objective-C Interoperability
- You must have a single Objective-C bridging header for Object-C interoperability. However, if a certain set of code you are importing has multiple header files; group them into another header file.

```objc
// <Product-Name>-Bridging-Header.h
#import "SDWebImageHeader.h"

// SDWebImageHeader.h
#import <SDWebImage/UIImageView+WebCache.h>
#import <SDWebImage/UIImage+MultiFormat.h>
#import <SDWebImage/SDWebImagePrefetcher.h>
```
