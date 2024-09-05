---
description: Generics nima va ular nima uchun kerak?
---

# Generics

`Generics` — bu Swift tilidagi kuchli vosita bo‘lib, u kodni bir necha turdagi ma’lumotlar bilan ishlaydigan qilib yozishga yordam beradi. Dastur yozishda ko‘p hollarda bir xil jarayonlar turli xil ma’lumot turlari bilan takrorlanadi. Misol uchun, siz sonlar yoki matnlar ustida bir xil operatsiyalarni bajarmoqchi bo‘lsangiz, har ikkisi uchun ham alohida kod yozishingiz kerak bo‘ladi. `Generics` esa shu vazifani bitta kod orqali hal qilish imkonini beradi.

#### Generics funksiyani qanday yozish mumkin?

Quida ikki kod yozish uslubini ko'rsatamiz, birida `Generic` dan foydalanmaymiz keyingisida foydalanamiz

1. Misol tariqasida, biz ikki o‘zgaruvchining qiymatlarini almashtiradigan funksiya yozamiz. Quyida oddiy kod keltirilgan:

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```



> <pre class="language-swift"><code class="lang-swift"><strong>// Yuqoridagi funksiya faqat Int (butun sonlar) bilan ishlaydi.
> </strong>// Ammo, agar biz String (matn) yoki boshqa turdagi 
> // ma’lumotlar bilan ishlashni xohlasak, yana boshqa funksiyalar 
> // yozishimiz kerak bo‘ladi. Bu esa ko‘p vaqt va joy oladi.
> </code></pre>

2. **Endi esa, shu funksiyani `Generics` yordamida yozamiz:**

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

> ```swift
> // Bu yerda kodimizga har qanday turdagi (son, matn, va hokazo)
> // ma’lumotlar bilan ishlashga imkon beradi. 
> // Endi bu funksiyani har qanday turdagi ma’lumotlar bilan
> // chaqirishimiz mumkin:
> ```

```swift
var x = 5
var y = 10
swapTwoValues(&x, &y)
print("x: \(x), y: \(y)")  // Natija: x: 10, y: 5

var firstName = "Ali"
var lastName = "Vali"
swapTwoValues(&firstName, &lastName)
print("firstName: \(firstName), lastName: \(lastName)")  
// Natija: firstName: Vali, lastName: Ali
```

#### Generics qanday afzalliklarga ega?

> 1\. Tez va oson kod yozish: Bir xil turdagi vazifalar uchun har xil turdagi kod yozishga hojat yo‘q.
>
> 2\. Moslashuvchanlik: Kodni ko‘proq ma’lumot turiga moslashtirish imkonini beradi.
>
> 3\. Xatolar sonini kamaytiradi: Har xil vazifalar uchun bitta umumiy kod yoziladi, shuning uchun xatolar ham kamroq bo‘ladi.

Generics dastur kodini yanada oddiy, qulay va samarali qilishga yordam beradi. Bu dasturlashda katta yordam beradi, ayniqsa dastur katta yoki murakkab bo‘lsa.
