# Protocols

Protokol (`Protocol`) bu `Swift` tilida muayyan vazifa yoki funksionallik uchun kerakli bo‘lgan metodlar, xususiyatlar va boshqa talablar uchun shablon ta’riflaydi. Protokollar turli xil turlar o‘rtasida umumiy xulq-atvorni belgilashning bir usuli bo‘lib, ular ushbu xulq-atvorni qanday amalga oshirishini aniq belgilab bermaydi.

{% hint style="info" %}
**Hususiyatlari**:&#x20;

• Protokol o‘zi hech qanday amalga oshirish (`implementation`) taqdim etmaydi. Buning o‘rniga, u qaysi xususiyatlar yoki metodlarni qabul qiluvchi tur bajarishi kerakligini belgilaydi.

\
• Har qanday sinf (`class`), struktura (`struct`) yoki sanash (`enum`) protokolni qabul qilib, uning talablarini qondiruvchi natijani amalga oshirish uslubini taqdim etishi mumkin.
{% endhint %}

#### Protokolning Asosiy Sintaksisi

```swift
protocol ProtocolName {
    // Bu yerda protokolga xos metodlar va xususiyatlar kiritiladi
    var someProperty: String { get set }
    func someMethod()
}
```

1. `protocol` kalit so‘zi protokolni aniqlash uchun ishlatiladi.
2. `ProtocolName` — bu protokolning nomi, uni ma’noli qilib tanlash kerak.
3. Protokol tanasi ichida xususiyatlar (`var`) va metodlar (`func`) kiritiladi.
   1. Xususiyatlar `get` yoki `get` `set` degan qism bilan tugaydi. \
      `get` bu xususiyat faqat o’qilishi mumkinligini anglatadi.\
      `get set` esa o’qilishi va yozilishi mumkinligini bildiradi.
   2. Metodlar esa faqat imzosidan iborat bo‘lib, ular uchun amalga oshirish (`body`) kiritilmaydi.

### Bir necha protokol ni turlarga qabul qilinishi - Maxsus turlar (custom types)

Maxsus turlar (custom types) protokolni qabul qilganligini e’lon qilish uchun o‘zlarining ta’riflarida tur nomidan keyin, ikki nuqta orqali protokol nomini kiritadilar. Bir nechta protokolni ro‘yxatga kiritish ham mumkin, ular vergul bilan ajratiladi:

```swift
// Masalan:
protocol Named {
    var name: String { get }
}

protocol Aged {
    var age: Int { get }
}

struct Person: Named, Aged {
    var name: String
    var age: Int
}
```

Bu yerda `Person` strukturasi `Named` va `Aged` protokollarini qabul qiladi. Demak, u ikkala protokolda talab qilingan `name` va `age` xususiyatlarini taqdim etishi kerak.

Quidagi misolda `FlyingCar` sinfi `Drivable` va `Flyable` protokollarini qabul qiladi va ularning talab qilingan metodlarini (`drive()` va `fly()`) amalga oshiradi.

```swift
// Masalan:
protocol Drivable {
    func drive()
}

protocol Flyable {
    func fly()
}

class FlyingCar: Drivable, Flyable {
    func drive() {
        print("Driving on the road")
    }

    func fly() {
        print("Flying in the sky")
    }
}
```

> **Eslatma:** Protokollar ham tur (type) hisoblanganligi sababli, ularning nomlarini boshqa Swift turlari nomlariga (masalan, Int, String va Double) mos kelishi uchun bosh harf bilan boshlang (masalan, FullyNamed va RandomNumberGenerator).

### Protokol Kengaytmalari (Protocol Extensions)

Protokol kengaytmalari yordamida protokolda ta’riflangan funksionallikni kengaytirish va uning mos keluvchi turlari tomonidan avtomatik ravishda foydalanilishi mumkin bo‘lgan standart amalga oshirishlarni taqdim etish mumkin. Kengaytma (`extension`) kalit so’zi yordamida protokolga qo’shimcha funksionallik qo’shiladi.

```swift
// Protocollarda kengaytma sintaksisi
protocol SomeProtocol {
    func requiredMethod()
}

extension SomeProtocol {
    func optionalMethod() {
        print("Optional method implementation")
    }
}
```

Bu yerda SomeProtocol protokoli requiredMethod() metodini talab qiladi, lekin kengaytma orqali qo‘shimcha optionalMethod() metodini taqdim etadi. Barcha protokolga mos keluvchi turlar ushbu kengaytma orqali taqdim etilgan metoddan foydalanishlari mumkin.

#### Protokol Kengaytmalarining Afzalliklari

> 1\. Standart Amalga Oshirishlar (`Default Implementations`): Protokol kengaytmalari yordamida metodlar uchun standart amalga oshirishlarni taqdim etishingiz mumkin, shunda mos keluvchi turlar ushbu metodlarni o‘z ichida qayta amalga oshirishni talab qilmaydi.
>
> 2\. Kodni Qayta Foydalanish: Agar bir nechta tur uchun bir xil metod yoki funksionallikni taqdim etishingiz kerak bo‘lsa, protokol kengaytmasidan foydalanib kodni qayta yozmaslik mumkin.
>
> 3\. Moslashuvchan Kod: Kengaytmalar protokollarning dastur kodiga qo‘shimcha funksionallik qo‘shishni yanada osonlashtiradi va kodning modularligini oshiradi.

Quida bir qancha missolar bilan tushuntirishga harakat qildik

```swift
// 1 - misol

protocol Greetable {
    func greet()
}

extension Greetable {
    func greet() {
        print("Hello!")
    }
}

struct Person: Greetable {}

let person = Person()
person.greet()  // "Hello!"
```

Bu misolda `Greetable` protokoli `greet()` metodini talab qiladi. Kengaytma orqali ushbu metodning standart amalga oshirilishi berilgan. `Person` strukturasi ushbu protokolga mos keladi va standart amalga oshirilishdan foydalanadi.

```swift
// 2 - misol

protocol Shape {
    var area: Double { get }
}

extension Shape {
    func printArea() {
        print("Area is \(area)")
    }
}

struct Circle: Shape {
    var radius: Double
    var area: Double {
        return Double.pi * radius * radius
    }
}

let circle = Circle(radius: 5)
circle.printArea()  // "Area is 78.53981633974483"
```

Yuqoridagi misolda `Shape` protokoli faqat `area` xususiyatini talab qiladi. Kengaytma esa qo‘shimcha `printArea()` metodini qo‘shadi, bu metod mos keluvchi tur tomonidan avtomatik foydalanilishi mumkin.

```swift
// 3 - misol

protocol Describable {
    var description: String { get }
}

extension Describable {
    var uppercaseDescription: String {
        return description.uppercased()
    }
}

struct Book: Describable {
    var description: String
}

let book = Book(description: "Swift Programming")
print(book.uppercaseDescription)  // "SWIFT PROGRAMMING"
```

Yuqoridagi misolda `Describable` protokoli faqat description xususiyatini talab qiladi, lekin kengaytma yordamida hisoblovchi `uppercaseDescription` xususiyati qo‘shiladi.

{% hint style="info" %}
Hulosa o'rnida shuni aytish mumkinki `Swift`’da protokollar ko‘p tomonlama va kuchli vosita bo‘lib, ular yordamida siz kodni moslashuvchan, modulli, va qayta foydalanish mumkin bo‘lgan qilib tashkil etishingiz mumkin. Protokollar va ularning kengaytmalari yordamida siz murakkab loyihalarda oddiy va toza kod yozishingiz, hamda talablar va amalga oshirishlarni ajratib qo‘yishingiz mumkin. Protokollardan to‘g‘ri va samarali foydalanish dasturingizning sifatini oshiradi va uni boshqarishni osonlashtiradi.
{% endhint %}
