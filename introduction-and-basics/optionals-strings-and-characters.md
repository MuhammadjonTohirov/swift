# 🍹 Optionals, Strings and Characters

String bilan biz avval tanishgan bo'ldik lekin bu mavzu ko'ringanidan kengroq, hozir sizlar bilan string va unga tegishli amallarni ko'rib chiqamiz.&#x20;

## Optionals

{% hint style="info" %}
Optional bu **?** belgisi bilan tugaydigan har qanday tipga aytiladi
{% endhint %}

{% code lineNumbers="true" %}
```
// Masalan

var id: Int?

// bu yerda id integer tipiga mansub lekin u optional. 
// ya'ni qiymati nil yoki bo'shliq desak ham bo'ladi

print(id)

// natija: nil

// agar biz id ga biror qiymat bersak
id = 3

print(id)

// natija Optional(3)
```
{% endcode %}

> Yuqoridagi holatdan bilish mumkinki&#x20;
>
> `Int` != `Int?` ya'ni integer bu optional integer emas.
>
> Optional tip nil qiymatni qabul qilishi mumkin. Va optional bo'lmagani esa unda qila olmaydi&#x20;

```
// Masalan

var age: Int = 8

age = nil // bu xato xolat xisoblanadi

var grade: Int? = 3

grade = nil // bu to'g'ri xolat
```

> Xo'sh unda biz qanday qilib Int? ustida amal bajara olamiz desangiz
>
> Unwrap - ochish uslubi orqali.
>
> Force unwrap orqali yoki o'riniga boshqa qiymat taklif qilish orqali
>
> Force unwrap (`!`) belgisi bilan belgilanadi. O'rniga boshqa qiymat taklif qilish `??` belgisi orqali bo'ladi

```
// Masalan

var age: Int? = 3

print(age!) 

// natija: 3

var grade: Int?

print(grade ?? 0) // agar grade nil bo'lsa o'rniga 0 ni oladi

// natija: 0

grade = 4

print(grade ?? 0) // bu yerda grade nil emas shuning uchun 0 emas 4 print qlinadi

// natija: 4


```



## Strings and Characters

<details>

<summary><code>append()</code></summary>

`Append` string oxiriga belgi yoki so'z qo'shishda foydalaniladi

```
// Masalan
var greeting = "salom"

greeting.append(" dunyo")

print(greeting)

// salom dunyo
```

</details>

<details>

<summary><code>hasPrefix()</code></summary>

`hasPrefix` string ni berilgan belgi yoki so'z bilan boshlanish yoki aksini aniqlaydi

```
// Masalan
var greeting = "salom"

let isStartedWithSal = greeting.hasPrefix("sal")

print("Is started with sal", isStartedWithSal)

// Is started with sal true

let isStartedWithDun = greeting.hasPrefix("dun")

print("Is started with dun", isStartedWithDun)

// Is started with dun false
```

</details>

<details>

<summary><code>hasSuffix()</code></summary>

`hasSuffix` string ni berilgan belgi yoki so'z bilan yanlanishini tekshiradi

```
// Masalan

let isEndedWithYo = greeting.hasSuffix("yo")
print(isEndedWithYo)

print("Is ended with yo", isEndedWithYo)

//Is ended with yo true

let isEndedWithLom = greeting.hasSuffix("lom")
print(isEndedWithLom)

print("Is ended with lom", isEndedWithLom)
//Is ended with lom false
```

</details>

<details>

<summary><code>insert()</code></summary>

`insert` stringni berilgan indexiga belgi kiritadi

```
// Masalan
var greeting = "salom dunyo"

// string ga yangi belgi ko'rsatilgan indexga kiritilsin
greeting.insert("1", at: greeting.startIndex) 
print(greeting)
//1salom dunyo

// stringni 2-o'rindagi indexiga belgi kiritilsin
greeting.insert("2", at: String.Index(utf16Offset: 2, in: greeting))
print(greeting)
//1s2alom dunyo

```

</details>

<details>

<summary><code>remove()</code></summary>

`remove` stringni berilgan indexidagi belgini o'chiradi

```
// Masalan

var greeting = "salom dunyo"

greeting.remove(at: greeting.startIndex)
print(greeting)

//alom dunyo
```

</details>

<details>

<summary><code>replacingOccurrences()</code></summary>

`replacingOccurrences` stringni ichida berilgan so'z yoki belgini boshqa bir belgi yoki so'z bilan almashtiradi

```
// Masalan

var greeting = "salom dunyo"

let result1 = greeting.replacingOccurrences(of: "sa", with: "CA")
print(result1)

// CAlom dunyo
```

</details>

<details>

<summary><code>split()</code></summary>

`split` stringni berilgan belgi uchragan joydan stringni bo'ladi va array xosil qiladi

```
// Masalan

var greeting = "salom dunyo qalaysan"

let result = greeting.split(separator: " ")
print(result)

// ["salom", "dunyo", "qalaysan"]

// bu xolatda `space` belgisidan foydalanib stringni bo'laklarga bo'ldik
```

</details>

<details>

<summary><code>init(repeating: , count: )</code></summary>

Berilgan `string` yoki `character` ni bir necha marotaba yozish uchun foydalaniladi.

```swift
// 1. n soni va c character berilgan, bu characterni n marotaba chiqaring

let n: Int = 4
let c: Character = "a"

let str = String.init(repeating: c, count: n)
print(str) 
// aaaa
```

```
// 2. n soni va s string b berilgan, bu stringni n marotaba chiqaring

let n: Int = 4
let s: String = "ab"

let str = String.init(repeating: s, count: n)
print(str) 
// abababab
```

</details>



