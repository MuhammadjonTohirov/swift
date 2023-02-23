# ♻ Functions

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

### O'zgaruvchan (variadic) parametrlar

Variadic parametr - belgilangan turdagi nol yoki undan ortiq qiymatlarni qabul qiladigan parametr. Funktsiya ta'rifi doirasida parametr turi nomiga uchta nuqta (...) qo'shish orqali variadik parametrni aniqlaymiz. Variadik parametrning qiymatlari funksiyaga belgilangan turdagi massiv sifatida taqdim etiladi.

```swift
// Masalan

func sayHello(greeting: String, names: String...) {
     for name in names {
       print("\(greeting) \(name)")
     }
}

sayHello(greeting: "Salom", names: "Ahmad")
// Salom Ahmad

sayHello(greeting: "Salom", names: "Ahmad", "Karim")
// Salom Ahmad
// Salom Karim
```

### Inout&#x20;

Inout dasturchiga funksiaga parameter sifatida berilgan o'zgaruvchini funksia ichida o'zgartirganda berilgan parameterini qiymati ham o'zgarishi taminlash da yordam beradi.

```swift
// Masalan
func add(number: Int, to anotherNumber: inout Int) {
    anotherNumber += number
}
var a = 20
let b = 12

print(a) // 20

add(number: b, to: &a)

print(a) // 32
```

Yuqoridagi misolda o'zgaruvchi `a` ning qiymati funksia chaqirilishidan avval `20` undan keyin esa `32` ga teng bo'lib qolganini ko'rish mumkin.

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

#### Recursive funksiyalarga misollar



<details>

<summary>1 dan boshlab n gacha sonlar yig'indisi</summary>

```swift
// Masalan

func sum(upto n: Int) -> Int {
    if n <= 0 {
        return 0
    }
    
    return n + sum(upto: n - 1)
}

// birdan boshlab 5 gacha sonni yig'indisi
print(sum(upto: 5))

// natija: 15


// n = 5 -> 5 + {5 - 1 + {4 - 1 + {3 - 1 + {2 - 1 + {1 - 1}}}}}
```

</details>

<details>

<summary>n! ni xisoblash</summary>

1 dan n gacha sonlar ko'paytmasini topish

```swift
// Masalan

func factorial(of n: Int) -> Int {
    if n == 1 {
        return 1
    }
    
    return n * factorial(of: n - 1)
}

print(factorial(4))
// 24

// 4 * {(4 - 1) * {(3 - 1) * {(2 - 1)}}}
// 4 * 3 * 2 * 1 ni xisoblaydi va natija 24 bo'ldi
```

![](<.gitbook/assets/image (1).png>)

Bu yerda recursive uslubda n factorial ni xisoblashni grafik uslubda ko'rish mumkin.

</details>

<details>

<summary>fibonacci n</summary>

n - o'rinda joylashga fibonachi sonini topish

```swift
// Masalan

func fibonacci(of n: Int) -> Int {
    if n <= 1 {
        return n
    }
    
    let left = fibonacci(of: n - 1)
    let right = fibonacci(of: n - 2)
    
    return left + right
}
```

```
// Agar yuqoridagi funksiyaga 5 ni bersak. nima sodir 
// bo'lishi quidagi rasmda ekltirilgan

print(fibonacci(of: 5))
// 5 - o'rindagi fibonacci soni 
// natija: 5
// 0, 1, 1, 2, 3, 5

```



</details>



