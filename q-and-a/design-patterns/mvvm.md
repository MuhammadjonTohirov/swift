---
description: MVVM on Apple's Platforms
---

# MVVM

## MVVM on Apple's Platforms

Model-View-ViewModel (MVVM) is a design pattern that evolved to address some of the limitations of MVC, particularly the "Massive View Controller" problem. It's become increasingly popular on Apple's platforms, and is especially well-suited for SwiftUI.

### Core Components

1. **Model**
   * Similar to MVC: represents your data and business logic
   * Responsible for data operations and business rules
   * Independent of the UI
2. **View**
   * UI components that display data and capture user interactions
   * In UIKit: typically `UIView` and `UIViewController` together
   * In SwiftUI: declarative View structs
3. **ViewModel**
   * Acts as a bridge between Model and View
   * Transforms Model data into View-friendly formats
   * Handles user interactions from the View
   * Contains no direct references to UI components

### Implementation in Swift

Here's how you might implement MVVM:

```swift
// Model
struct User {
    let id: String
    let name: String
    let joinDate: Date
}

// ViewModel
class UserProfileViewModel {
    private let user: User
    
    init(user: User) {
        self.user = user
    }
    
    var displayName: String {
        return user.name
    }
    
    var memberSince: String {
        let formatter = DateFormatter()
        formatter.dateStyle = .medium
        return "Member since: \(formatter.string(from: user.joinDate))"
    }
    
    func updateProfile() {
        // Handle business logic for updating profile
    }
}

// UIKit View
class UserProfileViewController: UIViewController {
    @IBOutlet private weak var nameLabel: UILabel!
    @IBOutlet private weak var memberSinceLabel: UILabel!
    
    var viewModel: UserProfileViewModel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        updateUI()
    }
    
    private func updateUI() {
        nameLabel.text = viewModel.displayName
        memberSinceLabel.text = viewModel.memberSince
    }
    
    @IBAction func updateProfileTapped(_ sender: UIButton) {
        viewModel.updateProfile()
    }
}
```

### SwiftUI + MVVM

MVVM pairs exceptionally well with SwiftUI:

```swift
// ViewModel with ObservableObject for SwiftUI binding
class ProfileViewModel: ObservableObject {
    @Published var user: User
    
    var formattedJoinDate: String {
        let formatter = DateFormatter()
        formatter.dateStyle = .medium
        return formatter.string(from: user.joinDate)
    }
    
    init(user: User) {
        self.user = user
    }
    
    func updateUsername(_ newName: String) {
        // Validation logic
        user.name = newName
    }
}

// SwiftUI View
struct ProfileView: View {
    @ObservedObject var viewModel: ProfileViewModel
    
    var body: some View {
        VStack {
            Text(viewModel.user.name)
                .font(.title)
            
            Text("Member since: \(viewModel.formattedJoinDate)")
                .font(.subheadline)
            
            Button("Update Profile") {
                viewModel.updateUsername("New Name")
            }
        }
    }
}
```

### Benefits of MVVM on Apple Platforms

1. **Testability**: ViewModels contain business logic but no UI references, making them easy to unit test
2. **Code organization**: Separates UI code from business logic
3. **Reusability**: ViewModels can be reused across different views
4. **SwiftUI compatibility**: Works naturally with SwiftUI's data flow model
5. **Binding**: Combine framework or SwiftUI's property wrappers make data binding straightforward

### Practical Considerations

* Simpler apps might not need the extra abstraction of MVVM
* For complex views, MVVM dramatically reduces controller complexity
* MVVM works well with reactive programming paradigms (Combine, RxSwift)
* Apple's introduction of SwiftUI has implicitly encouraged MVVM adoption
