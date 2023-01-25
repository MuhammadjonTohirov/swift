---
description: Array, Set, Dictionary, Tuple
---

# 🔄 Collection Types

<details>

<summary>Array</summary>

Array bir xil turdagi qiymatlarni tartiblangan ro'yxatda saqlaydi. Xuddi shu qiymat arrayda turli pozitsiyalarda bir necha marta paydo bo'lishi mumkin.

```
// Empty (bo'sh) arrayni yaratish

var someInts: [Int] = []
print("Integerlar soni \(someInts.count) ta")

```

```
// Arrayga ma`lumot qo'shish
someInts.append(3)
```

```
// Arraydagi malumotlarni yo'q qilish
someInts = []
```

```
// Array ni boshlang'ich qiymat bilan yaratish
var shoppingList: [String] = ["Eggs", "Milk"]

```

```
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



</details>

<details>

<summary>Tuple</summary>



</details>
