# Functions

Swift-dagi funksiya ma'lum bir vazifani bajaradigan mustaqil kod blokidir. Funktsiyalar qayta ishlatilishi mumkin va ularni kodingizning boshqa qismlaridan turib ishlatish ham mumkin, bu esa kodingizni tartibga solish va saqlashni osonlashtiradi.

```
// Funksiyani skileti quidagicha

func functionName(parameters) -> ReturnType {
   // funksiya tanasi
   // logika yoziladigan joy
   return value
}

```

* `func` - bu Swift-ga funksiyani aniqlayotganingizni bildiruvchi kalit so'z.&#x20;
* `functionName` - funksiyaning nomi.
* `parameters` - funksiyaga beriladigan tashqi qiymatlar.
* `-> ReturnType` - funksiyaning ixtiyoriy qismi bo'lib, funksiyaning qaytish turini belgilaydi. Agar funktsiya qiymat qaytarsa, qaytarish turi (`return type`) ko'rsatilishi kerak.
* `{...}` - funksiyaning tanasi. Bu erda siz funktsiya bajarishi kerak bo'lgan vazifalarni aniqlaysiz.
* `return value` - bu xolatda siz funksiyani `ReturnType` ga qarab qaytarilish ko'zda tutilgan ma'lumot sanaladi

```swift
// Masalan

func addNumbers(a: Int, b: Int) -> Int {
   let sum = a + b
   return sum
}

let result = addNumbers(a: 4, b: 5)
print(result)

// natija: 9
```

> Yuqoridagi funksiyada
>
> `addNumbers` - funksiya nomi
>
> `a: Int, b: Int` bular parameterlar
>
> `-> Int` - bu funksiyani qaytarish kerak bo'lgan qiymatning turi
>
> `return sum` - funksiya `sum` nomli o'zgaruvchini qaytaryapdi.

&#x20;
