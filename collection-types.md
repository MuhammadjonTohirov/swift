---
description: Array, Set, Dictionary, Tuple
---

# 🔄 Collection Types

<details>

<summary>Array</summary>

Array bir xil turdagi qiymatlarni tartiblangan ro'yxatda saqlaydi. Xuddi shu qiymat arrayda turli pozitsiyalarda bir necha marta paydo bo'lishi mumkin.

```swift
// Empty (bo'sh) arrayni yaratish

var someInts: [Int] = []
print("Integerlar soni \(someInts.count) ta")

```

```swift
// N ta bir xil qiymatiga ega bo'lgan array yaratish

var array: [Int] = Array.init(repeating: 0, count: 4)

print(array)

// [0, 0, 0, 0]
```

```swift
// Arrayga ma`lumot qo'shish
someInts.append(3)
```

```swift
// Arraydagi malumotlarni yo'q qilish
someInts = []
```

```swift
// Array ni boshlang'ich qiymat bilan yaratish
var shoppingList: [String] = ["Eggs", "Milk"]

```

```swift
// Arrayga boshqacha uslubda ma'lmuot qo'shish

shoppingList += ["Baking Powder"]
// Endi shoppingList da 3 ta ma'lumot bor
```

```swift
// Arrayni (n) indexdagi ma'lumotini olish
print(shoppingList[0])
// Eggs
```

```swift
// Arrayni (n) indexdagi ma'lumotini o'zgartirish

shoppingList[0] = "Coffee"
print(shoppingList)

// ["Coffee", "Milk", "Baking Powder"]
```

```swift
// Arrayni n indexiga malumot kirgizish

shoppingList.insert("Apple", at: 1)
print(shoppingList)

// ["Coffee", "Apple", "Milk", "Baking Powder"]
```



</details>

<details>

<summary>Multi dimentional array</summary>

Multi dimentional array - 2 va undan ortiq o'chamga ega bo'lgan array. Masalan 2 o'lchamlik array, bizga bu array matrix yoki matritsa nomi bilan ma'lum.

```swift
// Masalan
let matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

// yuridagi 2 o'lchamli array matematikada quidagicha yoziladi
// 1 2 3
// 4 5 6
// 7 8 9
```

Agar biz `(3, 2)` o'rindagi ma'lumotni olmoqchi bo'lsak (yani 8 ni). U xolda biz `matrix[2][1]` deb yozishimiz kerak bo'ladi

```swift
// Masalan

print(matrix[2][1]) // 8
```

Quida 2 o'lchamlik array ga misol

```swift
var workingHours = [  ["Week Day", "Monday", "Tuesday"],
  ["Hours", "9 AM - 5 PM", "9 AM - 5 PM"],
]

// hafta kunlari va u kunlardagi ish soatlari

let day = workingHours[0][1]
let whour = workingHours[1][1]

print("Bizning \(day) dagi ish soatimiz \(whour)")

// Biznig Monday dagi ish soatimiz 9 AM - 5 PM
```

Budan ham array kabi berilgan o'ridagi qiymatni o'zgartirish mumkin

```
// Masalan

workingHours[0][1] = "Dushamba" // "Monday" -> "Dushamba" ga o'zgardi

let day = workingHours[0][1]
let whour = workingHours[1][1]

print("Bizning \(day) dagi ish soatimiz \(whour)")

// Biznig Dushamba dagi ish soatimiz 9 AM - 5 PM
```

</details>

<details>

<summary>Set</summary>

Set arrayga o'xshash lekin unda ma\`lumotlar faqat bir marotaba uchraydi

```
// Set va Array
var meals: Set<String> = [
    "sho`rva", "osh"
]

var fruits: [String] = [
    "apple", "cherry"
]

print(meals)
// ["osh", "sho`rva"]
print(fruits)
// ["apple", "cherry"]

// farqiga misol

meals.insert("osh")
fruits.append("apple")

print(meals)
// ["osh", "sho`rva"]
print(fruits)
// ["apple", "cherry", "apple"]

```

Yuqoridagi misolda ko'ringanidek set ga o'zini ichida mavjud bo'lgan malumot qo'shilganda set uni qabul qilmadi. Arrayda esa bosh qabul qildi.\
\
Demak set o'z nomi bilan set, unda har xil narsadan bittada bo'ladi.

```
// Set ga ma`lumot qoshish

var meals: Set<String> = [
    "sho`rva", "osh"
]

meals.insert("mastava")

print(meals)
// ["sho`rva", "osh", "mastava"]

// ! setni ketma ket tarzda joylashmaydi, u ning ichidagi 
// ma`lumotlar joylashuv o'rni o'zgarib qoladi, yuqoridagi misolga o'xshab
```



</details>

<details>

<summary>Dictionary</summary>

Swift-dagi `Dictionary` kalit-qiymat juftliklari to'plami bo'lib, har bir kalit yagona xisbolanadi. U arrayga o'xshaydi, lekin elementlarga kirish indeks emas, balki kalit yordamida amalga oshiriladi. `Dictionary`dagi kalit-qiymat juftliklari tartibsizdir, ya'ni ularning lug'atga qo'shilish tartibi ular olinadigan tartibda bo'lishi kafolatlanmaydi.

Swift-da `Dictionary` kvadrat qavslar `[ ]` yordamida aniqlanadi va ularning kalit va qiymat turlari ko'rsatilishi kerak. Mana, meva nomlarini ularning narxiga ko'rsatadigan lug'at yaratish misoli:

```swift
// Masalan
// Ushbu misolda kalit turi String va qiymat turi Double.
var fruitPrices: [String: Double] = [
        "Apple": 0.5, 
        "Banana": 0.25, 
        "Orange": 0.75
]
```

Bo'sh `Dictionary` yaratish va keyinroq kalit-qiymat juftlarini qo'shish uchun qisqacha sintaksisidan ham foydalanishingiz mumkin.

```
// Masalan
var fruitPrices: [String: Double] = [:]
fruitPrices["Apple"] = 0.5
fruitPrices["Banana"] = 0.25
fruitPrices["Orange"] = 0.75

```

```
// Kvadrat qavs ichidagi kalitdan foydalanib, 
// kalit qiymatini olishingiz mumkin, 
// masalan:
let applePrice = fruitPrices["Apple"]
```

`Dictionary` dagi kalit orqali uni qiymatini o'zgartirsh mumkin

```
// Masalan

fruitPrices["Apple"] = 11
print(fruitPrices)

// endi natija
// ["Apple": 11.0, "Orange": 0.75, "Banana": 0.25]


// boshqacha uslubi ham bor u esa `updateValue` dan foydalanish

fruitPrices.updateValue(4, forKey: "Apple")
print(fruitPrices)

// endi natija
// ["Apple": 4.0, "Orange": 0.75, "Banana": 0.25]

```

`Dictionary` dagi kalit ga qiymatni `nil` berish orqali uni o'chirish mumkin yoki `removeValue` metodidan foydalanish kerak

```
// Masalan

// = nil
fruitPrices["Apple"] = nil
print(fruitPrices)

// ["Orange": 0.75, "Banana": 0.25]


// removeValue
fruitPrices.removeValue(forKey: "Banana")
print(fruitPrices)

// ["Orange": 0.75]
```



</details>

<details>

<summary>Tuple</summary>

Swift-dagi `Tuple` turli xil bo'ligan tartiblangan qiymatlar to'plamidir. Ular `array`ga o'xshaydi, lekin `tuple`dagi elementlarga indeks yordamida kirish mumkin emas, ular o'z pozitsiyalari yordamida kirishadi. `Tuple` - bu bir nechta qiymatlarni bitta qo'shma qiymatga guruhlash usuli.

Swift-da `tuple` qavslar `( )` yordamida aniqlanadi va elementlarning turlarini ko'rsatish shart emas. \
Ism va yoshni o'z ichiga olgan `tuple` yaratish misoli:

```
let person = ("John", 30)
print(person)

// ("John", 30)

print(person.0)
```

Shuningdek, `tuple`da alohida konstantalar yoki o'zgaruvchilarga ajratishingiz mumkin,&#x20;

```
// Masalan
let (name, age) = person

print(name, age)

// John 30
```

Shuningdek, `tuple` elementlariga ularning joylashuvi bo'yicha malumotni olishingiz mumkin

```
// Masalan
let name = person.0
let age = person.1

print(name, age)

// Jogn 30
```

Shuningdek, `tuple`da har bir elementdan oldin yorliqni qo'shish orqali `tuple` elementlarini nomlashingiz mumkin. Bu unga yanada o'qilishi va foydalanishni osonlashtiradi.

```
// Masalan

let person = (name: "John", age: 30)

print(person.name)
print(person.age)

// John
// 30
```



</details>
