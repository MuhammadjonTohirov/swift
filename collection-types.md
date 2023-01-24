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

```
// Arrayni (n) indexdagi ma'lumotini olish
print(shoppingList[0])
// Eggs
```

```
// Arrayni (n) indexdagi ma'lumotini o'zgartirish

shoppingList[0] = "Coffee"
print(shoppingList)

// ["Coffee", "Milk", "Baking Powder"]
```



</details>

<details>

<summary>Set</summary>



</details>



<details>

<summary>Dictionary</summary>



</details>

<details>

<summary>Tuple</summary>



</details>
