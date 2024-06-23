# Initialization and Deinitialization

#### Swift Tilida Initialization (init)

`Init` — bu klass yoki struktura obyektini yaratishda uning barcha xususiyatlariga boshlang’ich qiymatlarni berish jarayoni. Bu jarayon maxsus funksiyalar, ya’ni quruvchilar (initializers) yordamida amalga oshiriladi.

#### Oddiy Quruvchi (init)

Oddiy quruvchi (initializer) funksiyasi init kalit so’zi bilan belgilanadi:

```swift
struct Celsius {
    var temperatureInCelsius: Double
    
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
}
```

Bu yerda Celsius strukturasi ikki xil quruvchi yordamida yaratilib, har xil harorat o’lchov birliklaridan qiymatlarni qabul qiladi.

#### Parametrsiz Quruvchi `(init)`

Agar barcha xususiyatlar boshlang’ich qiymatga ega bo’lsa, Swift avtomatik ravishda parametrsiz quruvchini yaratadi:

```swift
struct Point {
    var x = 0.0
    var y = 0.0
}

let defaultPoint = Point() // (x: 0.0, y: 0.0)
```

#### Klasslarda Initializer

Klasslardagi Initializerda ham init funksiyasi ishlatiladi. Klasslarda barcha xususiyatlar boshlang’ich qiymatga ega bo’lishi yoki quruvchida qiymat berilishi kerak:

```swift
class Vehicle {
    var numberOfWheels: Int
    var color: String
    
    init(numberOfWheels: Int, color: String) {
        self.numberOfWheels = numberOfWheels
        self.color = color
    }
}
```

#### Convenience Initializers

Qulay quruvchilar asosiy quruvchilarga qo’shimcha qulayliklar qo’shish uchun ishlatiladi. Ular convenience kalit so’zi bilan belgilanadi:

```swift
class Bicycle: Vehicle {
    var hasBasket: Bool
    
    init(hasBasket: Bool) {
        self.hasBasket = hasBasket
        super.init(numberOfWheels: 2, color: "Black")
    }
    
    convenience init() {
        self.init(hasBasket: false)
    }
}
```

### Swift Tilida Deinitialization (Yakunlash)

#### Yakunlash nima?

Yakunlash (deinitialization) — bu obyekt xotiradan o’chirilishidan oldin bajariladigan maxsus jarayon. Bu jarayon deinit funksiyasi orqali amalga oshiriladi va faqat klasslarda ishlatiladi.

#### Deinitializer (Yakunlovchi)

Yakunlovchi (deinitializer) obyekt xotiradan o’chirilishidan oldin kerakli tozalash ishlarini bajarish uchun ishlatiladi:

```swift
class Bank {
    static var totalCoinsInBank = 10_000
    var coinsInVault: Int
    
    init(coins: Int) {
        self.coinsInVault = coins
        Bank.totalCoinsInBank -= coins
    }
    
    deinit {
        Bank.totalCoinsInBank += coinsInVault
    }
}
```

Bu yerda Bank klassi deinitializer yordamida obyekt xotiradan o’chirilganda umumiy bankdagi tangalar sonini yangilaydi.

#### Xulosa

Boshlang’ichlash (`init`) va yakunlash (`deinit`) Swift tilida obyektlar hayot tsiklini boshqarish uchun juda muhim. To’g’ri boshlang’ichlash obyektning barcha xususiyatlari kerakli qiymatlarga ega bo’lishini ta’minlaydi, yakunlash esa obyekt xotiradan o’chirilishidan oldin zaruriy tozalash ishlarini bajaradi.
