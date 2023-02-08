# Basic operators

### Assignment Operators (Tayinlash yoki tayinlash operatori)

```
// Example
let a = 10 // a yaratildi va qiymat qilib 10 tayinlandi yoki assign qilindi 
var b = 5 // b yaratildi va qiymat qilib 5 tayinlandi yoki assign qilindi 
b = a // b endi a ga yani 10ga teng bo'ldi. Demak b o'zgaruvchisi a orqali qimat oldi yoki qiymati o'zgardi
```

### Arithmetic Operators (Arifmetik amal)

> * Qo'shish (`+`)
> * Ayirish (`-`)
> * Ko'paytirish (`*`)
> * Bo'lish (`/`)

<pre><code>// Example
<strong>2 + 3    // 5 ga teng
</strong>4 - 2    // 2 ga teng
5 * 6    // 30 ga teng
8 / 4    // 2 ga teng
</code></pre>



### Reminder Operator (Qoldiq operatori)

> * Qoldiq olish (`%`)

```
// Example
5 % 2    // 1 ga teng
```

### Unary operations

> * Minus (`-`)
> * Denial yoki force unwrap (`!`)

{% code lineNumbers="true" %}
```
// Example
// minus
let a = 2
let b = -a    // b = -2

// denial
let hasBooks = true
let isThereBooks = !hasBooks    // isThereBooks = false 

// force unwrap (keyingi mavzularda)
```
{% endcode %}

### Comparison Operators (Solishtirish operatorlari)

> * Equal to ( `a == b` )
> * Not equal to (`a != b`)
> * Greater than (`a > b`)
> * Less than (`a < b`)
> * Greater than or equal to (`a >= b`)
> * Less than or equal to (`a <= b`)

{% code lineNumbers="true" %}
```swift
// Example

let a = 2
let b = 3
let c = a == b // c false ga teng bo'ladi ya'ni '2' ni '3' ga tengligi bo'lishi mumkin emas

let c1 = a != b // c1 true bo'ladi, sababi '2' chindan '3' ga teng emas

let c2 = a > b // c2 false chunki '2' '3'dan katta emas

let c3 = a < b // c3 true chunki '2' '3'dan kichik

let c4 = a >= b // c2 false chunki '2' '3'dan katta yoki teng emas

let c5 = a <= b // c2 true chunki '2' '3'dan kichik yoki teng
```
{% endcode %}

{% hint style="info" %}
Yuqoridagi lardan kelib chiqadiki solishtirish operatorlari o'zlaridan natija xosil qiladi, yani `true` yoki `false` ni.&#x20;
{% endhint %}

### &#x20;Casting numerals (Raqamlarni bir turdan ikkinchi turda o'girish)

```swift
// Masalan
let a: Int = 4
let b: Double = 8

print(a + b) // bu xato xolat, compiler da xatolik bo'ladi

print(Double(a) + b) // natija 12.0 double tipida

print(a + Int(b)) // natija 12 int tipida
```

> Yuqoridagi vaziyatda biz `Int` tipiga mansub sonni `Double` ga va `Double` tipidagi sonni `Int` ga o'girdik.
>
> Sababi biz bunga majburmiz.
>
> Swift arifmetik amallarni bajarish uchun bir xil tipga mansub qiymatlarni talab etadi.

### Identity Operators (=== va !===)

Keyingi mavzularda

### Logical Operators (Mantiq operatorlari)

> * Logical NOT (`!a`)
> * Logical AND (`a && b`)
> * Logical OR (`a || b`)
