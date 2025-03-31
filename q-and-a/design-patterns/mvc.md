---
description: MVC on Apple's Platforms
---

# MVC

Model-View-Controller (MVC) is a fundamental design pattern that Apple has embraced across its platforms, particularly in iOS and macOS development. It's a way to organize your code by separating it into three distinct components.

### The Core Components

1. **Model**
   * Represents your data and business logic
   * Independent of the user interface
   * Responsible for data storage, processing, and validation
   * Notifies observers when its state changes
2. **View**
   * The user interface components
   * Displays data to the user and captures user interactions
   * Passive and typically unaware of the model's specifics
   * In iOS/macOS: `UIView`/`NSView` and their subclasses
3. **Controller**
   * The mediator between Model and View
   * Responds to user actions and updates the model
   * Observes model changes and updates the view
   * In iOS/macOS: `UIViewController`/`NSViewController`

### Apple's Implementation

Apple's implementation of MVC is sometimes called "Cocoa MVC" and has some specific characteristics:

```swift
// Model Example
struct User {
    var name: String
    var email: String
}

// Controller Example
class ProfileViewController: UIViewController {
    @IBOutlet weak var nameLabel: UILabel!
    @IBOutlet weak var emailLabel: UILabel!
    
    var user: User? {
        didSet {
            updateView()
        }
    }
    
    func updateView() {
        guard let user = user else { return }
        nameLabel.text = user.name
        emailLabel.text = user.email
    }
}
```

### Communication Flow

In Apple's MVC:

1. The View notifies the Controller of user actions via target-action, delegate patterns, etc.
2. The Controller updates the Model based on those actions
3. The Controller observes the Model (via KVO, Notifications, callbacks) for changes
4. When the Model changes, the Controller updates the View

### Practical Use

In practice on Apple platforms:

* ViewControllers tend to become massive as they handle both view lifecycle and business logic
* Apple's "Massive View Controller" problem leads many developers to adopt alternative patterns like MVVM or VIP
* For simpler apps, MVC remains an excellent choice due to its straightforward implementation

### Evolution Beyond MVC

While MVC is the foundation, Apple has subtly shifted toward supporting other patterns:

* SwiftUI introduces a more declarative approach closer to MVVM
* Combine framework facilitates reactive programming
* UIKit remains MVC-centric but can adapt to other patterns
