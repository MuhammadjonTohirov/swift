# Control Flow

### For Loop

Arraydagi elementlar, raqamlar diapazonlari yoki string belgilar kabi ketma-ketliklarni takrorlash uchun `for-in` yoki shunchaki `for loop` siklidan foydalanasiz.

```swift
// Masalan

let names = ["Anna", "Alex", "Brian", "Jack"]
for name in names {
    print("Hello, \(name)!")
}
// Hello, Anna!
// Hello, Alex!
// Hello, Brian!
// Hello, Jack!
```

Shuningdek, `dictionary` elementlarini ham takrorlashingiz mumkin. `Dictionary` takrorlanganda undagi har bir element (kalit, qiymat) `tuple` sifatida qaytariladi va siz (kalit, qiymat) `tuple` a'zolarini `for-in` siklida ishlatish uchun aniq nomlangan o'zgarmas qiymat sifatida ajratishingiz mumkin. Quyidagi kod misolida `dictionary` `animalName` va qiymatlari esa `legCount` deb nomlangan.

```swift
let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]

for (animalName, legCount) in numberOfLegs {
    print("\(animalName)s have \(legCount) legs")
}

// cats have 4 legs
// ants have 6 legs
// spiders have 8 legs
```

Raqamlar diapazoni orqali ham `for-in` siklidan foydalanishingiz mumkin. Ushbu misol besh martalik jadvaldagi dastlabki bir nechta yozuvlarni chop etadi:

```swift
for index in 1...5 { // bu yerda [1, 5] oraliqdagi sonlar.
    print("\(index) times 5 is \(index * 5)")
}
// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25

// agar oraliqni [1, 5) gacha qilmoqchi bo'lsangiz 1..<5
```

Ba'zilar interfeysida kamroq belgi qo'yishni xohlashlari mumkin. Buning o'rniga ular har 5 qadamda bitta belgini afzal ko'rishlari mumkin. Keraksiz belgilarni o'tkazib yuborish uchun `stride(from:to:by:)` funksiyasidan foydalaning.

```swift
// Masalan: 0 dan 20 gacha har qadami 5 taga teng bo'lgan loop ni yaratamiz

for i in stride(from: 0, to: 21, by: 5) {
    print(i)
}

//0
//5
//10
//15
//20
```

`For-In` loop ni yana bir ajoyib xususiyatlaridan biri, loop ni muayyan bir shart asosida bajarishimiz mumkin

```swift
// Masalan
let number: [Int] = [1, 2, 3, 4, 5, 6, 7]
// aytaylik biz toq sonlarni ekranga chiqarishimiz kerak

for num in numbers where num % 2 != 0 {
    print(num)
} 

// 1
// 3
// 5
// 7
```

### While Loop

`while` sikli kod blokini bajarishdan oldin shartni tekshiradi. Shart to'g'ri bo'lsa, tsikl bajarilishda davom etadi. Swift-dagi `while` tsiklining sintaksisi:

```swift
while condition {
    // code
}
```

```swift
// Masalan

var count = 0

while count < 5 {
    print("count is \(count)")
    count += 1
}
```

`releat-while` `while` tsikliga o'xshaydi, lekin shartni tekshirishda bir oz farq bor. `repeat-while` sikli ichidagi kod bloki kamida bir marta bajarilishi kafolatlanadi va shart to'g'ri bo'lsa, tsikl bajarilishda davom etadi. Swift-da takrorlash-while siklining sintaksisi:

```swift
repeat {
    // code
} while condition
```

```swift
// Masalan

var count = 0

repeat {
    print("count is \(count)")
    count += 1
} while count < 5
```
