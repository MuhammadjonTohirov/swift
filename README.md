---
cover: >-
  https://images.unsplash.com/photo-1532289402244-3cbf8bdeb722?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw0fHxzd2lmdHxlbnwwfHx8fDE2NzI4MDgzNDQ&ixlib=rb-4.0.3&q=80
coverY: -1437.6237623762377
---

# 🍏 Introduction and basics

## About Swift

**Swift** - bu xavfsizlik, unumdorlik va dasturiy ta'minot tamoillariga asoslangan holda yaratilgan umumiy maqsadli dasturlash tili. (**General purpose programming language**)

**Safe (Havfsiz)**  - Dasturchining xato qilish extimolligini keskin kamaytiradi.

**Fast (Tez)** - `Swift` `Objective-C`, `C++` yoki `C` tillarini o'rniga Apple mahsulotlarini dasturiy taminoti uchun ishlab chiqilgan. Bu bilan aytish mumkin aslida tezlik bilan maqtana oladigan bu tillarga o'rindosh bo'lish tezlik bilan bo'ladi.

**Expressive(Qulay)** - Qo'llash uchun juda qulay bo'lgan til. Code yozish jarayoni maroqli mashg'ulotga aylanadi.

Swift open source dasturlash tili bo'lib uni nafaqat MacOS, balki windows va linux OS larida ham ishlatish mumkin. Lekin, Apple mahsulotlari uchun dastur yaratish faqat MacOS orqali amalga oshiriladi.

## Basics

#### Constants and Variables (O'zgarmaslar va o'zgaruvchilar)

• O'zgaruvchi yoki o'zgarmas bu nom bo'lib (ex: maxNumber yoki message) qandaydir qiymatga ega bo'ladi (`10` yoki `"Hello World"`)

#### Declaring Constant and variable (O'zgarmas va o'zgaruvchini yaratish)

• O'zgarmas malumotni siz `let` kalit so'zi orqali, o'zgaruvchini esa `var` kalit so'zi orqali yaratish mumkin

`let maxNumber = 10 // o'zgarmas`

`var message = "Hello World" // o'zgaruvchi`

&#x20;• `var` lar yoki `let` larni bir qatorda bir nechtasini yaratish usuli

`var x = 0.0, y = 0.0, z = 0.0`  \
Bu yerda biz `x`, `y`, va `z` o'zgaruvchilarini `0.0` qiymat bilan yaratdik.

{% hint style="info" %}
**Note -** o'zgaruvchi yoki o'zgarmaslar (`var` va `let`) nomlarida boʻshliq belgilari, matematik belgilar, oʻqlar, shaxsiy foydalanish uchun Unicode skalyar qiymatlari yoki chiziq va quti chizilgan belgilar boʻlishi mumkin emas. Raqamlar nomning boshqa joylariga kiritilishi mumkin bo'lsa-da, ular raqam bilan boshlana olmaydi.
{% endhint %}

1. `let π = 3.14159`❌
2. `let 你好 = "你好世界"`❌
3. `let 🐶🐮 = "dogcow"`❌
4. `let welcome = "Welcome user" ✅`
5. `var grade1 = 90.4 ✅`

#### Examples

`var name = "User"` `✅`\
`name = "Gamer" ✅`

Yuqoridagi misol o'zgaruvchi u-n bo'lib uni yaratib qiymat bergandan keyin ham o'zgartirish mumkin

`let name = "User" ✅`\
`name = "Gamer"` ❌

O'zgarmaslarda esa uni yaratib va so'ngra keyinchalik o'zgartirishga imkon bermaydi.

### Integers

**Butun sonlar kasrga boʻlmagan sonlardir**.\
Butun sonlar manfiy yoki musbat bo'ladi, masalan, 11 va -20.

Swift 8, 16, 32 va 64 bitli shakllarda butun sonlarni taqdim etadi. 8 bitli belgisiz butun son UInt8 turiga va 32 bitli imzolangan butun son Int32 tipiga ega. Swift-dagi barcha turlar singari, bu butun sonlar ham bosh harf bilan yozilgan nomlarga ega.

### Floating-Point Numbers

Bu turdagi raqamlar kasrga ega bo'lga raqamlar. Masalan: 12.42, -39.11

• `Float` 32 bit joy oladigan `(R)` haqiqiy son.\
• `Double` 64 bit joy oladigan `(R)` haqiqiy son.

> Example:
>
> `let pi = 3.14` -> bu o'rinda pi `Double` sifatida qaraladi.
>
> `let pi: Float = 3.14` -> endi esa pi `Float` sifatida ko'rinadi.

{% hint style="info" %}
Bu vaziyatdan bilish mumkinki agar biror bir o'zgaruvchi yoki o'zgarmas real sonni qiymat sifatida olganda, buni swift compiler `Double` deb biladi. Qachonki aniqlik kiritilib bu son `Float` deb ko'rsatilsagina berilgan `var` yoki `let` `Float` deb qaraladi.
{% endhint %}

### Bool

Kengaytmasi boolean ham deyiladi, bu orqali biz vaziyatni **ha** yoki **yo'q** sifatida mantiqiy talqin qilishimiz mumkin.

Boshqacha qilib aytganda `true` or `false`.

> Example:
>
> `let hasBook = false` -> bu xolatda `hasBook` boolean ga aylanadi\
> `let hasBook: Bool = false` -> yuqorida ko'rsatilgan xolat bilan bir xil natija. Farqi shuki, ikkinchi xolatda `hasBook` ni turi avvaldan bilgilanyapdi

> `let hasBook: Bool = "yes"` ❌ - bu xolat da dastur ishlamaydi. Ya'ni siz ha yoki yo'q mantiqiy amalni boshqa bir qiymat bilan aniqlashtirmoqchisiz.&#x20;

### String

String bu so'z demakdir, yoki belgilar ketma ketligi.

> `let name = "Bob"` -> name yaratilidi va "Bob" degan string ni qiymat sifatida qabul qildi

{% hint style="info" %}
String ichida keyword ishlatmoqchi bo'lsangiz `\()` belgidan foydalaning
{% endhint %}

> Bir necha qator ga string yozish uchun (`"""`) 3 ta qo'shtirnoqdan foydalaning

```
// Ex:
// Ekranga malumotni chop etish uchun print funksiyasidan foydalaning

let name = "User"
print("My name is \(name)) // My name is User

print(
"""
My name is \(name).
Do you know that?
"""
)
// My name is User
// Do you know that?
```
