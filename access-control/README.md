---
description: >-
  Swift'da kirish nazorati (Access Control) Swift'da kirish nazorati dastur
  modullarining tuzilishini belgilash va qaysi qismlar boshqalardan yashirin
  bo'lishi kerakligini ko'rsatish uchun ishlatiladi.
---

# Access control

### Asosiy kirish nazorati darajalari

Swift 5 versiyasida beshta kirish nazorati darajasi mavjud (eng ochiqdan eng yopiqgacha tartiblangan):

1. **`open`**: Eng ochiq daraja
2. **`public`**: Umumiy foydalanish uchun
3. **`internal`**: Modul ichida foydalanish uchun (standart)
4. **`fileprivate`**: Faqat aniq fayl ichida foydalanish uchun
5. **`private`**: Faqat aniq deklaratsiya ichida foydalanish uchun

### Har bir daraja haqida batafsil

#### 1. `open`

* Faqat **class** va uning **member**lari uchun mavjud
* Boshqa modullardan ham ko'rinadi
* Boshqa modullardan subclass yaratish mumkin
* Boshqa modullardan class member'larini override qilish mumkin

```swift
open class OpenClass {
    open func openMethod() {
        print("Bu metod boshqa modullardan override qilinishi mumkin")
    }
}
```

#### 2. `public`

* Boshqa modullardan ko'rinadi
* Boshqa modullardan subclass **yaratib bo'lmaydi** (faqat o'z moduli ichida)
* Boshqa modullardan member'larni override **qilib bo'lmaydi** (faqat o'z moduli ichida)

```swift
public class PublicClass {
    public func publicMethod() {
        print("Bu metod boshqa modullardan ko'rinadi, lekin override qilinmaydi")
    }
}
```

#### 3. `internal`

* **Standart** kirish darajasi (hech narsa ko'rsatilmaganda)
* Faqat **o'z modulida** ko'rinadi
* Boshqa modullardan ko'rinmaydi

```swift
class InternalClass { // 'internal' so'zi yozilmasa ham bo'ladi
    func internalMethod() {
        print("Bu metod faqat o'z modulida ko'rinadi")
    }
}
```

#### 4. `fileprivate`

* Faqat **aniq fayl ichida** ko'rinadi
* Bir fayl ichidagi turli xil tiplar o'rtasida foydalanish mumkin

```swift
fileprivate class FilePrivateClass {
    fileprivate func filePrivateMethod() {
        print("Bu metod faqat shu fayl ichida ko'rinadi")
    }
}

// Shu fayl ichida boshqa class bu classga kirishi mumkin
class AnotherClassInSameFile {
    func useFilePrivate() {
        let instance = FilePrivateClass()
        instance.filePrivateMethod() // ✅ Ruxsat etilgan
    }
}
```

#### 5. `private`

* Eng cheklangan kirish darajasi
* Faqat **aniq deklaratsiya ichida** ko'rinadi
* Extension'lar ham kirishga ega emas (Swift 4 dan boshlab extension'lar kirishga ega)

```swift
class PrivateExample {
    private var privateProperty = 42
    
    func usePrivateProperty() {
        print(privateProperty) // ✅ Ruxsat etilgan
    }
}

// Boshqa class bu propertyga kira olmaydi
class AnotherClass {
    func tryToAccess() {
        let instance = PrivateExample()
        // instance.privateProperty // ❌ Kompilyatsiya xatosi
    }
}
```

### Amaliy misollar

#### Framework yoki library yaratish

```swift
// AnimalKit modulida
open class Animal {
    public let species: String
    private var healthPoints: Int
    
    public init(species: String) {
        self.species = species
        self.healthPoints = 100
    }
    
    open func makeSound() {
        // Subclasslar override qilishi kerak
        print("Some generic sound")
    }
    
    public final func eat() {
        // Override qilinmasligi kerak
        healthPoints += 10
    }
    
    fileprivate func sleep() {
        healthPoints += 20
    }
}

// Foydalanuvchi dasturida
class Dog: Animal {
    override func makeSound() {
        print("Woof!")
    }
    
    // sleep() metodini override qilib bo'lmaydi, chunki u fileprivate
}
```

#### UIKit yoki SwiftUI dastur

```swift
class ProfileViewController: UIViewController {
    // Public API
    public var username: String?
    
    // Internal implementation
    private var userData: [String: Any]?
    
    // Faqat shu fayl ichida ishlatiladigan helper funksiya
    fileprivate func formatUserData() -> String {
        // Implementation
        return ""
    }
    
    // Faqat shu class ichida ishlatiladigan funksiya
    private func validateUserData() -> Bool {
        // Implementation
        return true
    }
}
```

### Getter va setter uchun kirish nazorati

Swift'da propertyning getter va setter uchun turli xil kirish darajalarini o'rnatish mumkin:

```swift
class DataManager {
    // Getter public, setter private
    private(set) public var lastUpdated: Date = Date()
    
    func updateData() {
        // Ma'lumotlarni yangilash...
        lastUpdated = Date() // Private setter ichkarida o'zgartirilishi mumkin
    }
}

let manager = DataManager()
print(manager.lastUpdated) // ✅ Ruxsat etilgan (public getter)
// manager.lastUpdated = Date() // ❌ Kompilyatsiya xatosi (private setter)
```

### Protocollar uchun kirish nazorati

```swift
public protocol PublicProtocol {
    // Bu protocol boshqa modullardan ko'rinadi
}

internal protocol InternalProtocol {
    // Bu protocol faqat o'z modulidan ko'rinadi
}

fileprivate protocol FilePrivateProtocol {
    // Bu protocol faqat shu fayldan ko'rinadi
}
```

### Extension'lar uchun kirish nazorati

```swift
swiftCopypublic class MyPublicClass {
    private var privateProperty = "Private"
}

// Private propertylarga kirish uchun extension
extension MyPublicClass {
    func accessPrivate() {
        print(privateProperty) // ✅ Swift 4+ versiyalarda ruxsat etilgan
    }
}
```

### Subsystem'lar uchun `fileprivate` misollar

```swift
// NetworkUtils.swift faylida
fileprivate class NetworkSession {
    fileprivate func createRequest() -> URLRequest {
        // Implementation
        return URLRequest(url: URL(string: "https://example.com")!)
    }
}

// Shu fayldagi boshqa class bu classga kirishi mumkin
class NetworkManager {
    private let session = NetworkSession()
    
    public func fetchData() {
        let request = session.createRequest() // ✅ Ruxsat etilgan
        // Fetch logic
    }
}
```

### Amaliy dastur dizayni uchun tavsiyalar

1. **API dizaynida e'tiborli bo'ling** - Foydalanuvchilarga faqat kerakli elementlarga kirish huquqini bering
2. **Default `internal` ni qo'llang** - Zarur bo'lmasa, kirish darajasini ko'rsatmang
3. **`private(set)` dan foydalaning** - Faqat o'qish huquqini berish uchun
4. **`open` va `public` ni ajratib oling** - `open` inheritance va override uchun, `public` esa faqat ko'rish uchun
5. **Ma'lumotlarni `private` qiling** - Implementatsiya detallarini yashiring

### Xulosa

Swift'dagi kirish nazorati qo'shimcha xavfsizlik va encapsulation ta'minlab, kod tuzilishini yaxshilaydi. To'g'ri kirish darajalarini tanlash orqali:

* Toza va tushunarli API yarating
* Kod implementatsiyasini yashiring
* Tushunarsiz tarzda o'zgarishlardan himoya qiling
* Framework va library'lar yaratishda ishonchli interface taqdim eting

Kirish nazorati - bu Swift'ning eng muhim xususiyatlaridan biri bo'lib, kodni saqlash va rivojlantirishni osonlashtiradi.
