# Functions

Swift-dagi funksiya ma'lum bir vazifani bajaradigan mustaqil kod blokidir. Funktsiyalar qayta ishlatilishi mumkin va ularni kodingizning boshqa qismlaridan turib ishlatish ham mumkin, bu esa kodingizni tartibga solish va saqlashni osonlashtiradi.

```swift
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

## Recursive functions

{% hint style="info" %}
Recursive funksiya bu o'z o'zini chaqiruvchi funksiyalarga aytiladi. U murakkab muammoni kichikroq, bir xil kichik muammolarga ajratish va asosiy holatga erishilgunga qadar ushbu kichik muammolarni rekursiv hal qilish orqali ishlaydi.
{% endhint %}

```swift
// Masalan
func printNumbers(n: Int) {
   if n == 0 {
      return
   }
   print(n)
   printNumbers(n: n - 1)
}
```

&#x20;Bu funksiya bitta `n` argumentini oladi va asosiy holat `n == 0` ga yetguncha o'zini `n - 1` argumenti bilan qayta-qayta chaqiradi. Asosiy holatga erishilganda, funktsiya natijalarni qaytarishni boshlaydi.

`PrintNumbers` funksiyasi `3`-argument bilan chaqirilganda nima sodir bo'ladi:

```swift

printNumbers(n: 3)

// Funktsiyaga birinchi chaqiruvda argument 3 bilan amalga oshiriladi.
// Funktsiya 3 ni chop etadi va keyin argument 2 bilan o'ziga chaqiradi.
// Funktsiyaga ikkinchi chaqiruv argument 2 bilan amalga oshiriladi.
// Funktsiya 2 ni chop etadi va keyin argument 1 bilan o'ziga chaqiradi.f
// Funksiyaga uchinchi chaqiruv 1-argument bilan amalga oshiriladi.
// Funktsiya 1 ni chop etadi va keyin 0 argumenti bilan o'ziga chaqiradi.
// Funktsiyaga to'rtinchi chaqiruv 0 argumenti bilan amalga oshiriladi.
// Asosiy holatga erishildi va funktsiya boshqa chaqiriqlarsiz qaytadi.

// 3
// 2
// 1
```
