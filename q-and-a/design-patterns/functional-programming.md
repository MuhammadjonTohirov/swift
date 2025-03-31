# Functional Programming

## Functional Programming in Swift

Functional programming is a paradigm that treats computation as the evaluation of mathematical functions while avoiding changing state and mutable data. Swift supports many functional programming concepts, making it a multi-paradigm language that lets you blend functional techniques with object-oriented and protocol-oriented approaches.

### Key Concepts of Functional Programming

1. **First-class and higher-order functions**: Functions can be assigned to variables, passed as arguments, and returned from other functions.
2. **Pure functions**: Functions that always produce the same output for the same input and have no side effects.
3. **Immutability**: Once a value is created, it cannot be changed; instead, new values are created from existing ones.
4. **Function composition**: Building complex functions by combining simpler ones.
5. **Declarative code**: Focusing on what to accomplish rather than how to accomplish it.

### Swift's Functional Features

Swift incorporates many functional features:

```swift
// Higher-order functions
let numbers = [1, 2, 3, 4, 5]
let doubled = numbers.map { $0 * 2 }
let evens = numbers.filter { $0 % 2 == 0 }
let sum = numbers.reduce(0, +)

// Function composition
func compose<A, B, C>(_ f: @escaping (B) -> C, _ g: @escaping (A) -> B) -> (A) -> C {
    return { f(g($0)) }
}

// Immutability
let immutableArray = [1, 2, 3]
// Create a new array instead of modifying
let newArray = immutableArray + [4]
```

**Error Handling**

```swift
// Using functors (map) with Optional and Result types
func fetchUserData(id: String) -> Result<User, APIError> {
    // Implementation...
}

// Transform the successful result without unwrapping
let userViewModel = fetchUserData(id: "123")
    .map { user in UserViewModel(name: user.name, age: user.age) }
```

**Asynchronous Operations**

```swift
// Using functional concepts with async/await
func processImage(_ image: UIImage) async -> UIImage {
    // Chain transformations in a functional way
    return await image
        .applyingFilter(.blur)
        .resizing(to: .medium)
        .compressing(quality: 0.8)
}
```

**State Management**

```swift
// Immutable state updates (similar to Redux pattern)
struct AppState {
    let user: User?
    let items: [Item]
    let isLoading: Bool
}

// Pure function to create new state
func reducer(state: AppState, action: Action) -> AppState {
    switch action {
    case .login(let user):
        // Return new state, don't modify existing
        return AppState(user: user, items: state.items, isLoading: false)
    // Other cases...
    }
}
```

**Function Composition**

```swift
// Building complex operations from simple ones
let validateEmail = { (email: String) -> Bool in email.contains("@") }
let normalizeEmail = { (email: String) -> String in email.lowercased() }

// Compose functions
let prepareEmail = { (email: String) -> String? in
    let normalized = normalizeEmail(email)
    return validateEmail(normalized) ? normalized : nil
}
```

Functional programming is a mindset that encourages using pure functions, avoiding side effects, and treating data as immutable. While collections showcase these principles well through methods like `map`, `filter`, and `reduce`, the approach can revolutionize how you structure entire applications, not just how you process collections.

## Potential Answer for "What experience do you have of functional programming?"

"I've incorporated functional programming principles in several Swift projects. For example, in a recent inventory management app, I implemented a data pipeline that processed sales records using functional techniques:

```swift
func processInventoryData(records: [SalesRecord]) -> [ProductStatus] {
    return records
        .filter { $0.isValid && $0.date > lastUpdateDate }
        .map { record -> ProductTransaction in 
            ProductTransaction(
                productId: record.productId,
                quantity: record.isSale ? -record.quantity : record.quantity
            )
        }
        .reduce(into: [:]) { result, transaction in
            let currentCount = result[transaction.productId, default: 0]
            result[transaction.productId] = currentCount + transaction.quantity
        }
        .map { productId, count in
            ProductStatus(id: productId, remainingStock: count)
        }
        .sorted { $0.remainingStock < $1.remainingStock }
}
```

This chain transformed raw sales data into actionable inventory insights through a series of pure functions. What I love about this approach is how each step has a single responsibility - filtering valid records, transforming them to transactions, aggregating by product, and creating the final status objects.

The code was not only more concise compared to imperative alternatives but also easier to test since each operation could be verified independently. When we needed to add a new feature to flag low-stock products, I could simply add another transformation to the chain without disturbing the existing logic.

I've also worked extensively with Combine for reactive programming and find that these functional techniques lead to more predictable and maintainable code, especially for complex data flows.
