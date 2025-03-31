---
description: Questions dealing with data and data structures.
---

# Data

<details>

<summary>How is a dictionary different from an array?</summary>

**Arrays:**

* Ordered collections of elements accessed by index (0, 1, 2...)
* Elements are stored sequentially in memory
* Lookups by index are fast (O(1))
* Searching for a value requires iteration (O(n))
* Example: `[1, 5, 7, 9]` - you'd access the second element with `array[1]`

**Dictionaries:**

* Key-value pairs where each value is accessed by its unique key
* Data is stored in an unordered manner
* Lookups by key are fast (O(1) average case)
* No sequential indexing
* Example: `{"name": "John", "age": 30}` - you'd access John's age with `dictionary["age"]`

</details>

<details>

<summary>What are the main differences between classes and structs in Swift?</summary>

**Classes:**

* Reference types (passed by reference)
* Heap allocated
* Support inheritance
* Have deinitializers
* Reference counting for memory management
* Instances can be mutated even if declared with `let`

**Structs:**

* Value types (passed by value)
* Stack allocated when possible
* No inheritance
* No deinitializers
* Automatic memberwise initializers
* Cannot be mutated if declared with `let`&#x20;

</details>

<details>

<summary>What are tuples and why are they useful?</summary>

**What they are:**

* Ordered, finite groups of values potentially of different types
* Lightweight compound values

**Why they're useful:**

* Return multiple values from a function without creating a custom type
* Group related values temporarily
* Unpacking/destructuring values
* Typed access to fixed-size collections
* Example: `let person = ("John", 30)` or with labels `let person = (name: "John", age: 30)`

</details>

<details>

<summary>What does the <code>Codable</code> protocol do?</summary>

The `Codable` protocol in Swift is a type alias that combines the `Encodable` and `Decodable` protocols. It provides a standardized way to convert Swift data types to and from external representations like JSON or Property Lists.

When you conform a type to `Codable`:

* You can encode it to formats like JSON using `JSONEncoder`
* You can decode JSON data back into Swift objects using `JSONDecoder`
* Swift automatically synthesizes the encoding/decoding logic for types with simple properties
* You can customize encoding/decoding by implementing `encode(to:)` and `init(from:)` methods

```swift
struct User: Codable {
    var name: String
    var age: Int
}

// Encoding
let user = User(name: "John", age: 30)
let jsonData = try JSONEncoder().encode(user)

// Decoding
let decodedUser = try JSONDecoder().decode(User.self, from: jsonData)
```

</details>

<details>

<summary>What is the difference between an array and a set?</summary>

**Arrays:**

* Ordered collection that can contain duplicate elements
* Elements are stored and accessed in sequence by index
* Maintains insertion order
* Better for when order matters or you need duplicates
* O(n) lookup time in worst case

**Sets:**

* Unordered collection that stores unique elements only
* No defined sequence or indexing
* Automatically removes duplicates
* Optimized for testing membership (contains operations)
* O(1) average lookup time
* Efficient for set operations like union, intersection, etc.

</details>

<details>

<summary>What is the difference between the <code>Float</code>, <code>Double</code>, and <code>CGFloat</code> data types?</summary>

**Float:**

* 32-bit floating-point number
* Less precision (about 7 decimal digits)
* Uses less memory
* Faster on some hardware but rarely meaningful in modern devices

**Double:**

* 64-bit floating-point number
* More precision (about 15-16 decimal digits)
* Swift's default floating-point type
* Preferred for most calculations

**CGFloat:**

* Platform-dependent floating-point type used in Core Graphics framework
* On 32-bit systems, it's the same as Float
* On 64-bit systems (all modern iOS/macOS devices), it's the same as Double
* Used for graphical operations like drawing and layout

In modern iOS/macOS development, since all current devices are 64-bit, `CGFloat` and `Double` are essentially the same, differing mainly in their semantic meaning in the API.\


</details>

<details>

<summary>What’s the importance of key decoding strategies when using <code>Codable</code>?</summary>

Key decoding strategies determine how JSON keys map to Swift property names when using `JSONDecoder`. They're important because:

* They allow you to decode JSON with naming conventions different from Swift (like snake\_case vs camelCase)
* They prevent you from having to write custom coding keys for every property
* They simplify working with inconsistent APIs

```swift
decoder.keyDecodingStrategy = .useDefaultKeys // Default: exact match required
decoder.keyDecodingStrategy = .convertFromSnakeCase // Converts snake_case to camelCase
decoder.keyDecodingStrategy = .custom { ... } // Custom conversion logic
```

```swift
// JSON: {"user_name": "John", "user_age": 30}
struct User: Codable {
    var userName: String
    var userAge: Int
}

let decoder = JSONDecoder()
decoder.keyDecodingStrategy = .convertFromSnakeCase
let user = try decoder.decode(User.self, from: jsonData)
```

</details>

<details>

<summary>When using arrays, what’s the difference between <code>map()</code> and <code>compactMap()</code>?</summary>

* `map()`: Transforms each element in a collection using a closure, maintaining the same number of elements
* `compactMap()`: Transforms elements AND removes nil values from the result

```swift
let numbers = ["1", "2", "three", "4"]

// Using map
let mapped = numbers.map { Int($0) }
// [Optional(1), Optional(2), nil, Optional(4)]

// Using compactMap
let compactMapped = numbers.compactMap { Int($0) }
// [1, 2, 4]
```

</details>

<details>

<summary>Why is immutability important?</summary>

Immutability (using `let` instead of `var`) is important because it:

1. **Prevents bugs**: Once initialized, values can't change unexpectedly
2. **Simplifies concurrency**: No race conditions when values can't change
3. **Improves performance**: Compiler optimizations for immutable data
4. **Functional programming**: Enables pure functions with predictable behavior
5. **Thread safety**: Safe to share immutable data across threads
6. **Easier reasoning**: Code is more predictable and easier to understand

</details>

<details>

<summary>What are one-sided ranges and when would you use them?</summary>

One-sided ranges specify a sequence that's bounded on only one side. Swift supports:

1. **Start-only range (`startIndex...`)**: Everything from the start index to the end
2. **End-only range (`...endIndex`)**: Everything from the beginning up to and including the end index
3. **End-excluding range (`..<endIndex`)**: Everything from the beginning up to but not including the end index

Examples:

```swift
swiftCopylet items = ["A", "B", "C", "D", "E"]

// From index 2 to the end
let fromC = items[2...] // ["C", "D", "E"]

// From the start through index 2
let throughC = items[...2] // ["A", "B", "C"]

// From the start up to index 2
let upToC = items[..<2] // ["A", "B"]
```

One-sided ranges are useful for:

* Slicing collections when you only care about one boundary
* Working with collections of unknown size
* String manipulation
* Pattern matching in switch statements

</details>

<details>

<summary>What does it mean when we say “strings are collections in Swift”?</summary>

In Swift, strings are collections of characters, which means:

* You can iterate over a string character by character using `for...in` loops
* String conforms to `Collection` protocol, providing functionality like `map`, `filter`, etc.
* Characters are accessed via string indices, not integers (`string[index]` not `string[0]`)
* String indices are complex types that account for variable-width Unicode characters
* You can use collection methods like `count`, `isEmpty`, `first`, and `last`

Example:

```swift
let greeting = "Hello"
for character in greeting {
    print(character)
}
// Outputs: H, e, l, l, o

let firstChar = greeting[greeting.startIndex] // "H"
let thirdChar = greeting[greeting.index(greeting.startIndex, offsetBy: 2)] // "l"
```

</details>

<details>

<summary>What is a <code>UUID</code>, and when might you use it?</summary>

A `UUID` (Universally Unique Identifier) is a 128-bit value used to uniquely identify information. In Swift, it's represented by the `UUID` struct.

Use cases:

* Generating unique identifiers for database records
* Creating unique keys for caching
* Tracking unique user sessions
* Uniquely identifying devices or instances
* Implementing offline-first syncing strategies

Example:

```swift
let id = UUID() // Creates a random UUID
let stringRepresentation = id.uuidString // e.g., "E621E1F8-C36C-495A-93FC-0C247A3E6E5F"
```

</details>

<details>

<summary>What's the difference between a value type and a reference type?</summary>

**Value Types** (structs, enums, tuples):

* Copied when assigned to variables or passed to functions
* Each copy is independent; modifications don't affect other copies
* Memory typically managed on the stack when possible
* Automatically deallocated when out of scope

**Reference Types** (classes, functions):

* Passed by reference; variables hold a reference to the same instance
* Multiple variables can reference the same instance
* Changes to one reference affect all others pointing to the same instance
* Memory managed on the heap
* Reference counting tracks when to deallocate memory

Example:

```swift
// Value type
struct Point { var x, y: Int }
var point1 = Point(x: 1, y: 1)
var point2 = point1
point2.x = 2
print(point1.x) // 1 (unchanged)

// Reference type
class Rectangle { var width, height: Int }
let rect1 = Rectangle(width: 1, height: 1)
let rect2 = rect1
rect2.width = 2
print(rect1.width) // 2 (changed)
```

</details>

<details>

<summary>When would you use Swift’s <code>Result</code> type?</summary>

`Result<Success, Failure>` is an enum with two cases: `.success(Success)` and `.failure(Failure)`. It's used to:

* Represent the outcome of operations that can either succeed or fail
* Encapsulate success values or error information in a type-safe way
* Enable cleaner error handling compared to throwing functions
* Support asynchronous error handling in completion handlers

Example:

```swift
func fetchUser(id: String, completion: @escaping (Result<User, NetworkError>) -> Void) {
    // Network request
    if successful {
        completion(.success(user))
    } else {
        completion(.failure(.notFound))
    }
}

fetchUser(id: "123") { result in
    switch result {
    case .success(let user):
        print("Found user: \(user.name)")
    case .failure(let error):
        print("Error: \(error)")
    }
}
```

</details>

<details>

<summary>What is type erasure and when would you use it?</summary>

Type erasure is a technique for hiding concrete types behind protocol interfaces while preserving type safety. You use it when:

* You need to store different implementations of a protocol with associated types
* You want to hide implementation details while maintaining protocol conformance
* You need to work around Swift's limitations with protocols that have Self or associated type requirements

Common examples include `AnyCollection`, `AnyHashable`, and `AnyPublisher` in Combine.

Example:

```swift
// Without type erasure
// Error: Protocol 'Sequence' can only be used as a generic constraint
// let sequences: [Sequence] = [...]

// With type erasure
let sequences: [AnySequence<Int>] = [
    AnySequence([1, 2, 3]),
    AnySequence(Set([4, 5, 6]))
]
```

</details>
