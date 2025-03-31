# Dependency Injection

## Understanding Dependency Injection

Dependency injection might sound complex, but it's actually a straightforward concept that can greatly improve your code. Let me explain it in simple terms:

### The Basic Concept

Imagine you're building a house. Instead of making your own furniture inside the house as you build it, you create doorways where pre-made furniture can be brought in. That's essentially dependency injection - instead of creating dependencies inside your class, you "inject" them from the outside.

### A Simple Example

Let's say you have a `UserService` that needs to make network requests:

```swift
// Without dependency injection
class UserService {
    // The service creates its own network client internally
    let networkClient = NetworkClient()
    
    func fetchUser(id: String, completion: @escaping (User?) -> Void) {
        networkClient.request(endpoint: "users/\(id)", completion: completion)
    }
}
```

With dependency injection:

```swift
// With dependency injection
class UserService {
    // The network client is injected from outside
    let networkClient: NetworkClientProtocol
    
    init(networkClient: NetworkClientProtocol) {
        self.networkClient = networkClient
    }
    
    func fetchUser(id: String, completion: @escaping (User?) -> Void) {
        networkClient.request(endpoint: "users/\(id)", completion: completion)
    }
}
```

### Why It's Useful

1.  **Testability**: You can easily inject mock dependencies for testing

    ```swift
    swiftCopy// For testing, inject a mock network client
    let mockNetwork = MockNetworkClient()
    let userService = UserService(networkClient: mockNetwork)
    ```
2.  **Flexibility**: You can swap implementations without changing the dependent class

    ```swift
    // In production code
    let realNetwork = RealNetworkClient()
    let userService = UserService(networkClient: realNetwork)

    // For offline mode
    let offlineNetwork = OfflineNetworkClient()
    let userService = UserService(networkClient: offlineNetwork)
    ```
3. **Decoupling**: Your classes don't need to know how to create their dependencies

### Common Methods of Injection

1. **Constructor Injection**: Pass dependencies in the initializer (as shown above)
2.  **Property Injection**: Set dependencies after initialization

    ```swift
    var userService = UserService()
    userService.networkClient = RealNetworkClient()
    ```
3.  **Method Injection**: Pass dependencies to specific methods

    ```swift
    func fetchUser(id: String, using networkClient: NetworkClientProtocol, completion: @escaping (User?) -> Void) {
        networkClient.request(endpoint: "users/\(id)", completion: completion)
    }
    ```

### Real-World Benefit

Think about it this way: if you change how your `NetworkClient` works, with dependency injection you only need to change it in one place. Without DI, you'd need to update every class that creates its own `NetworkClient`.

Remember, dependency injection is all about making your code more flexible, testable, and maintainable by decoupling the creation of objects from their usage.
