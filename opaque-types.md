---
description: >-
  Swiftda qiymatning turini yashirish uchun ikkita usul bor: opaque turlar va
  boxed protokol turlari. Bu usullar qiymatning turini yashirish uchun
  foydalidir, ayniqsa modul va modulni chaqiradigan kod o
---

# Opaque type

#### Opaque turlar nima?

Swift 5.1 da joriy qilingan bir imkoniyat bo‘lib, u protokollardan foydalanish va Swift API dizaynidagi ba’zi muammolarni hal qilish uchun mo‘ljallangan. Bu funksiya, metod yoki xususiyat uchun qiymat qaytarganda, qiymatning aniq turini API foydalanuvchisidan yashirish imkonini beradi.

Protokolni qaytish turi sifatida ishlatish haqida barchamiz yaxshi tushunchaga egamiz. Protokolni qaytish turi sifatida ishlatish degani, biz funksiya uchun qaytish turi sifatida protokolni belgilaymiz. Quyidagi misolda, `getShapeType()` funksiyasi `Shape` protokoli turini qaytarmoqda va keyinchalik bu qaytgan protokolni o‘z ehtiyojimizga ko‘ra turga o‘tkazishimiz mumkin.

```swift
protocol Shape {
    func draw() -> Int
}

class Circle: Shape {
    var name: String = "circle"
    func draw() -> Int {
        return 1
    }
}

func getShapeType() -> Shape { 
    return Circle()
}

let circle = getShapeType() as? Circle

print(circle?.name ?? "-") 

// output: circle
```

Endi, `Shape` protokolidagi `draw()` metodining aniq qaytarish turini (`return type`) bilmaymiz deb tasavvur qilaylik. Narsalarni umumlashtirishga harakat qilamiz. Ha, bu yerda `associatedType` haqida gap ketyapti. Protokolga `associatedType` qo‘shganimizdan so‘ng, protokolni qaytish turi sifatida ishlatishga harakat qilganimizda kompilyator xatolik beradi. Bu xatolikning sababi shundaki, protokol umumiy (`Generic`) holatga ega va o‘ziga xos ma’lumot turiga ega emas, aksincha mos keluvchi `struct`, `class` yoki `enum`’dan ma’lumot turini oladi.

```swift
protocol Shape {
    associatedtype T
    func draw() -> Int
}

class Circle: Shape {
    typealias T = Int
    var name: String = "circle"
    
    func draw() -> T {
        return 1
    }
}

func getShapeType() -> Shape { 
// error: Use of protocol 'Shape' as a type must be written 'any Shape'
    return Circle()
}
```

Protokol turidan foydalanishda Shape so‘zining o‘rniga any Shape yoki some Shape deb yozilishi kerak.

#### Bu muammoni qanday hal qilamiz?&#x20;

Noaniq tur (`Opaque Type`) bu yerda yordamga keladi. Noaniq tur protokolning ichki ma’lumot turini bilmasligi mumkin, lekin some kalit so‘zidan foydalanib, qaytish turining ushbu protokol turi ekanligini kompilyatorga bildirish mumkin.

```swift
protocol Shape {
    associatedtype T
    func draw() -> Int
}

class Circle: Shape {
    typealias T = Int
    var name: String = "circle"
    
    func draw() -> T {
        return 1
    }
}

func getShapeType() -> some Shape {
    return Circle()
}
```

#### Hulosa

Swift tilida noaniq qaytish turlari (`Opaque Return Types`) qiymatlarning aniq turini yashirib, protokollarga mos kelishini ta’minlash imkoniyatini beradi. Bu imkoniyat associatedtype ga ega protokollarni qaytish turi sifatida ishlatishda yuzaga keladigan muammolarni hal qiladi. some kalit so‘zi yordamida dasturchilar ichki implementatsiyani oshkor qilmasdan, yanada moslashuvchan va umumiy kod yozishlari mumkin. Bu yondashuv `API` dizaynini soddalashtirish bilan birga, tur xavfsizligini oshiradi va abstraksiyani saqlab qoladi, bu esa Swift kodini yanada ishonchli va qulay boshqariladigan qiladi. Shunday qilib, noaniq qaytish turlari Swift’ning zamonaviy dasturiy ta’minot ishlab chiqishida, ayniqsa protokollar va generikalar bilan ishlashda samarali yechim taklif etadi.
