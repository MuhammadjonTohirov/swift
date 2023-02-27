# Methods

`method` - bu klass yoki structdagi funktsiyalardir. Methodlar, bir nechta turli ma'lumotlarni (`argument`lar) qabul qilish va xususiyatlarga (`property`) murojaat qilish imkoniyatini beradi. Methodlar klass yoki structdagi xususiyatlar bilan birga ishlaydigan, u o'zining obyektlariga murojaat qilish uchun foydalaniladi.

Methodlarni bir nechta turdagi xususiyatlarga murojaat qilish uchun foydalanish mumkin. Masalan, `instance method` - bu klass yoki structning har bir obyekti uchun mo'ljallangan. Bu metodlar `self` ma'lumotiga murojaat qilish uchun ishlatiladi. `Class method` - bu esa o'zining klassi uchun mo'ljallangan. Bu metodlar `type` xususiyatiga murojaat qilish uchun foydalaniladi.

Quyidagi misolda, `Person` klassida `instance method` va `class method` yaratilgan:

```swift
class Person {
    var name: String
    var age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
    
    func printName() {
        print("My name is \(self.name)")
    }
}
```

Bu yerda `Person` klassida `printName()` methodi obyektga `name` ni chop etadi, va `getPersonDescription()` methodi `class method` hisoblanadi va string qiymat qaytaradi.

`printName()` metodiga murojaat quyidagi kabi amalga oshiriladi:

```swift
let person = Person(name: "John", age: 30)
person.printName() // Output: My name is Johnwi
```
