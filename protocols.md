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

