# Inheritance

Swift Tilida Vorislik (**Inheritance**)

#### Vorislik nima?

Vorislik — bu bir klass boshqa bir klassdan xususiyatlar (properties) va metodlarni meros qilib olishi. Bu OOP (Object-Oriented Programming) ning asosiy tushunchalaridan biridir va kodni qayta ishlatish va tashkillashtirishni osonlashtiradi.

#### Asosiy Klass va Subklass

Vorislikka ega bo’lgan klass “asosiy klass” (`base class`) deb ataladi, undan meros olgan klass esa “subklass” (`subclass`) deb ataladi. Swift tilida bir klass faqat bitta asosiy klassdan vorislik olishi mumkin (yagona vorislik).

#### Asosiy Klassni Yaratish

Asosiy klass oddiy klassdek e’lon qilinadi:

```swift
class Vehicle {
    var currentSpeed = 0.0
    
    func describe() -> String {
        return "Hozirgi tezlik \(currentSpeed) km/soat."
    }
}
```

#### Subklassni Yaratish

Subklass `:` belgisidan keyin asosiy klass nomini ko’rsatish orqali e’lon qilinadi:

```swift
class Bicycle: Vehicle {
    var hasBasket = false
}
```

Endi Bicycle klassi Vehicle klassining barcha xususiyatlari va metodlariga ega bo’ladi.

#### Subklassda Xususiyatlarni Qayta E’lon Qilish

Subklass o’ziga xos xususiyatlarni qo’shishi yoki mavjud xususiyatlarni o’zgartirishi mumkin:

```swift
class Car: Vehicle {
    var gear = 1
    
    override func describe() -> String {
        return super.describe() + " va \(gear)-pog'onali uzatma."
    }
}
```

Bu yerda Car klassi Vehicle klassining describe metodini qayta e’lon qiladi (override so’zi yordamida) va unga yangi ma’lumot qo’shadi.

#### `super` Kalit So’zi

`super` kalit so’zi subklassda asosiy klassning metodlari va xususiyatlariga murojaat qilish uchun ishlatiladi. Yuqoridagi misolda `super.describe()` asosiy klassning describe metodini chaqiradi.

#### Quruvchilar (`Initializers`)

Subklass o’zining o’ziga xos quruvchilarga ega bo’lishi mumkin, lekin u asosiy klassning barcha e’lon qilingan xususiyatlarini to’liq boshlang’ich qiymat bilan ta’minlashi kerak:

```swift
class ElectricCar: Car {
    var batteryLevel: Int
    
    init(batteryLevel: Int) {
        self.batteryLevel = batteryLevel
        super.init()
    }
    
    override func describe() -> String {
        return super.describe() + " Batarеya darajasi: \(batteryLevel)%."
    }
}
```

Bu yerda ElectricCar o’ziga xos batteryLevel xususiyatiga ega va yangi quruvchi (initializer) e’lon qilgan.

#### Xulosa

Vorislik Swift tilida kuchli va moslashuvchan mexanizm bo’lib, kodni qayta ishlatish va tashkillashtirishni osonlashtiradi. Asosiy va subklasslar yordamida murakkab obyektlar o’rtasidagi munosabatlarni ifodalash mumkin.
