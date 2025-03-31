# Singleton

## Appropriate Use Cases for Singletons

While singletons can be overused and create dependencies that are hard to test, there are legitimate cases where they make sense. Here are some examples:

### System-wide Resource Management

1. **Device Hardware Access**
   * `AVAudioSession.sharedInstance()` - Manages audio session configuration for the entire app
   * `UIDevice.current` - Provides centralized access to device characteristics
   * `FileManager.default` - Offers system-wide access to the file system
2. **Global Configuration**
   * `UserDefaults.standard` - Manages app preferences
   * `URLSession.shared` - Provides a shared network session with common configuration
   * `NotificationCenter.default` - Centralizes the management of notifications

### Cases Where Shared State Is Logical

1.  **Analytics Manager**

    ```swift
    class AnalyticsManager {
        static let shared = AnalyticsManager()
        
        private init() {}
        
        func logEvent(_ event: String, parameters: [String: Any]? = nil) {
            // Log to analytics service
        }
    }
    ```

    Tracking analytics typically requires a consistent, centralized approach across the app.
2.  **Authentication Service**

    ```swift
    class AuthenticationManager {
        static let shared = AuthenticationManager()
        
        private init() {}
        
        var currentUser: User?
        
        func login(credentials: Credentials) async throws -> User {
            // Implementation
        }
    }
    ```

    Authentication state should be consistent across the app.

### Context-Specific Persistence

1.  **Core Data Stack**

    ```swift
    class CoreDataStack {
        static let shared = CoreDataStack()
        
        private init() {
            setupPersistentContainer()
        }
        
        var viewContext: NSManagedObjectContext {
            return persistentContainer.viewContext
        }
        
        private var persistentContainer: NSPersistentContainer!
        
        private func setupPersistentContainer() {
            // Setup code
        }
    }
    ```

    Managing a single persistence stack helps prevent conflicts and data inconsistencies.
2.  **Cache Manager**

    ```swift
    class ImageCache {
        static let shared = ImageCache()
        
        private init() {
            // Setup cache configuration
        }
        
        private let cache = NSCache<NSString, UIImage>()
        
        func image(for key: String) -> UIImage? {
            return cache.object(forKey: key as NSString)
        }
        
        func store(_ image: UIImage, for key: String) {
            cache.setObject(image, forKey: key as NSString)
        }
    }
    ```

    A centralized cache prevents duplicate memory usage for the same resources.

### Best Practices When Using Singletons

1.  **Dependency Injection Friendly**

    ```swift
    protocol NetworkServiceType {
        func fetch<T: Decodable>(from url: URL) async throws -> T
    }

    class NetworkService: NetworkServiceType {
        static let shared = NetworkService()
        
        // Public init allows for testing with mocks
        init() {}
        
        func fetch<T: Decodable>(from url: URL) async throws -> T {
            // Implementation
        }
    }
    ```

    This allows for using the singleton in production but injecting mocks in tests.
2.  **Lazy Initialization**

    ```swift
    class Logger {
        static let shared: Logger = {
            let logger = Logger()
            // Configure logger
            return logger
        }()
        
        private init() {}
    }
    ```

    This ensures the singleton is only created when needed.

Remember, even when using singletons, it's generally good practice to:

* Make them conformable to protocols for better testability
* Consider whether the singleton pattern is truly necessary
* Limit their responsibilities to truly global concerns
* Use dependency injection where possible, even with singletons
