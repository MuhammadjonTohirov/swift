---
description: Key-Value Observing (KVO) in Swift
---

# KVO

Key-Value Observing (KVO) is an Objective-C mechanism that allows objects to be notified when properties of other objects change. It's part of Apple's Key-Value Coding (KVC) infrastructure and provides a way to implement the observer pattern.

### Core Concepts of KVO

KVO works by registering an observer object to watch for changes in a specific property of another object. When that property changes, the observing object receives a notification.

#### How KVO Works

1. **Registration**: An observer registers interest in a specific property of an object
2. **Notification**: When the property changes, the system automatically notifies the observer
3. **Response**: The observer implements logic to respond to the property change

### Implementation in Modern Swift

While KVO originated in Objective-C, it's available in Swift through several mechanisms:

#### 1. Traditional KVO (NSObject-based)

For Swift classes that inherit from NSObject:

```swift
class Person: NSObject {
    @objc dynamic var name: String
    
    init(name: String) {
        self.name = name
        super.init()
    }
}

class Observer {
    var observation: NSKeyValueObservation?
    
    func startObserving(_ person: Person) {
        observation = person.observe(\.name, options: [.old, .new]) { person, change in
            print("Name changed from \(change.oldValue ?? "") to \(change.newValue ?? "")")
        }
    }
}

// Usage
let person = Person(name: "Alice")
let observer = Observer()
observer.startObserving(person)

// This triggers the observation
person.name = "Bob"
```

Key points:

* The observed property must be marked with `@objc dynamic`
* The class must inherit from NSObject
* Swift uses key paths (e.g., `\.name`) to identify properties

#### 2. Combine Framework (iOS 13+)

For newer applications, Apple's Combine framework provides a more Swift-native way to observe property changes:

```swift
import Combine

class ModernPerson {
    @Published var name: String
    
    init(name: String) {
        self.name = name
    }
}

class ModernObserver {
    var cancellables = Set<AnyCancellable>()
    
    func startObserving(_ person: ModernPerson) {
        person.$name
            .sink { newValue in
                print("Name changed to \(newValue)")
            }
            .store(in: &cancellables)
    }
}
```

This approach uses the `@Published` property wrapper and doesn't require NSObject inheritance.

### Important Considerations

1. **Memory Management**: Traditional KVO observations must be invalidated to prevent memory leaks. The modern API handles this automatically when the observation object is deallocated.
2. **Performance**: KVO has some runtime overhead. For high-frequency updates or performance-critical code, consider alternatives.
3. **Thread Safety**: KVO notifications are delivered on the same thread where the change occurred, which may not be the main thread.
4. **SwiftUI Alternative**: For SwiftUI applications, consider using the `@State`, `@ObservedObject`, `@EnvironmentObject`, or `@StateObject` property wrappers instead of KVO.

### Common Use Cases

1. **UI Updates**: Updating interface elements when a model changes
2. **Synchronization**: Keeping multiple objects in sync
3. **Data Binding**: Binding model data to UI controls
4. **Debugging**: Monitoring property changes during development

### Apple Framework Usage

Many Apple frameworks use KVO internally:

* **Core Animation**: For animatable properties
* **NSOperationQueue**: For operation dependencies and state changes
* **UIKit**: For various UI state changes
* **Core Data**: For change notifications

### Limitations

1. KVO only works on classes, not structs or enums
2. Traditional KVO requires inheritance from NSObject
3. Only instance properties (not computed properties) can be observed directly
4. Changes made directly to collection objects (arrays, dictionaries) don't trigger KVO notifications unless you use specific KVO-compliant methods

### Best Practices

1. Use Swift's modern KVO API with key paths where possible
2. Consider Combine for Swift-native reactive programming
3. Document which properties are observable in your classes
4. Be cautious with KVO in multithreaded environments
5. Always properly manage observation lifetimes to prevent memory leaks
