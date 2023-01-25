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

<summary>append()</summary>



</details>

<details>

<summary>hasPrefix()</summary>



</details>

<details>

<summary>hasSuffix()</summary>



</details>

<details>

<summary>insert()</summary>



</details>

<details>

<summary>remove()</summary>



</details>

<details>

<summary>replacingOccurrences()</summary>



</details>

<details>

<summary>reversed()</summary>



</details>

<details>

<summary>split()</summary>



</details>

<details>

<summary>trimmingCharacters()</summary>



</details>



