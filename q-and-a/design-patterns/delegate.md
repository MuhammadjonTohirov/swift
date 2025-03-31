---
description: Understanding Delegates in Swift
---

# Delegate

Delegates in Swift are a design pattern that allows one object to send messages to another object when specific events happen. Think of a delegate as a trusted assistant that handles specific tasks for the main object.

Here's how delegates work in Swift:

### Core Concept

Imagine you're at a restaurant:

* The waiter (main object) takes your order
* The chef (delegate) actually cooks the food
* The waiter doesn't need to know how to cook, they just need to communicate with someone who does

### Technical Implementation

In Swift, delegates typically use protocols:

```swift
protocol RestaurantOrderDelegate {
    func prepareFood(dish: String)
    func alertWhenFoodIsReady(dish: String)
}

class Waiter {
    var delegate: RestaurantOrderDelegate?
    
    func takeOrder(dish: String) {
        print("Order received for \(dish)")
        delegate?.prepareFood(dish: dish)
    }
}

class Chef: RestaurantOrderDelegate {
    func prepareFood(dish: String) {
        print("Preparing \(dish)")
        // Cooking logic...
        alertWhenFoodIsReady(dish: dish)
    }
    
    func alertWhenFoodIsReady(dish: String) {
        print("\(dish) is ready to serve!")
    }
}
```

### Real-World Example

A common example in iOS development is table views:

```swift
class MyViewController: UIViewController, UITableViewDelegate {
    let tableView = UITableView()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        tableView.delegate = self // "I'll handle delegate responsibilities"
    }
    
    // Implementing a delegate method
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        print("User tapped row \(indexPath.row)")
    }
}
```

### Why Use Delegates?

1. **Separation of concerns**: Objects focus on their core responsibilities
2. **Reusability**: The delegate pattern allows for flexible, modular code
3. **Communication**: Provides a structured way for objects to talk to each other
