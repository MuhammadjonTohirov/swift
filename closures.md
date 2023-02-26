# Closures

Swift-da `Closure` - bu o'z-o'zidan tuzilgan kod bloki bo'lib, uni o'tkazish va dasturingizda ishlatish mumkin. `Closure`lar funksiyalarga o'xshaydi, chunki ular kirish parametrlarini olishi va qiymatlarni qaytarishi mumkin.&#x20;

```swift
// Closure tuzlishi quidagicha

{
      (<#parameterlar#>) -> <#return-type#>
      in <code or logic>
}
```

`Closure`ni yaratish uchun ishlatiladigan sintaksis biz funksiyalar yaratishda foydalanadigan sintaksisga juda o'xshaydi va Swiftda funksiyalar aslida `closure`lardir. `Closure` va `funktsiya`lar o'rtasidagi formatdagi eng katta farq `in` kalit so'zidir. `Closure` parametri va `qaytish tur` (`return type`)larining ta'rifini `Closure` tanasidan ajratish uchun `in` kalit so'zi gullik qavslar ichida ishlatiladi.

```swift
// Masalan
let clos1 = { () -> Void in
      print("Hello World")
}

clos1()
// Hello World
```

Yuqoridagi misolda biz `closure` yaratamiz va uni `clos1` ga qiymat sifatida beramiz. Qavslar orasida hech qanday parametr aniqlanmaganligi sababli, bu `closure` hech qanday parametrni qabul qilmaydi. Shuningdek, qaytarish turi (`return type`) `void` sifatida aniqlanadi; shuning uchun bu `closure` hech qanday qiymatni qaytarmaydi. `Closure` korpusida konsolga `Hello World`-ni chop etadigan bitta qator mavjud.

Keling yana bir misolni ko'rib chiqamiz. Bu misolda closure `name` deb nomlanga parameter qabul qiladi va hech nima qaytarmaydi

```swift
let clos2 = { (name: String) -> Void in
    print("Hello \(name)")
}

clos2("Developer")
// Hello Developer

clos2("Gamer")
// Hello Gamer
```

Yana bir `closure` yaratsak. Bunda `closure` parameter sifatida `name` string qabul qilsin va unga hello so'zi biriktirib natijani qaytarsin.

```swift
// Masalan

let clos3 = { (name: String) -> String in
    return "Hello \(name)"
}

print(clos3("Fergana"))
// Hello Fergana
```

Bu yerda clos3 string qabul qilyapdi va string qaytaryapdi va biz qaytgan qimatni print funksiasi orqai konsolga chiqaryapmiz

Shuningdek `closure` larni boshqa bir `closure` yoki `funksiya`ga parameter sifatida berishimiz mumkin

```swift
// Masalan

 func testClosure(handler: (String) -> Void) {
      handler("Dasher")
}

testClosure(handler: { text in
      print(handler + "!")
})

// Dasher!
```

#### Closurega doir masalalar

```swift
// Masalan, a va b ni qo'shish uchun closure

let addClosure: (Int, Int) -> Int = { a, b in
    return a + b
}
let result = addClosure(2, 3) // result = 5
```

```swift
//  Masalan, eng uzun string topish

let longestStringClosure: ([String]) -> String? = { strings in
    return strings.max(by: { $0.count < $1.count })
}
let strings = ["apple", "banana", "cherry"]
let longest = longestStringClosure(strings) // longest = "banana"
```

```swift
// Masalan dictinoary dagi kalitlarni saralash uchun closure

let sortedKeysClosure: ([String: Any]) -> [String] = { dictionary in
    return dictionary.keys.sorted()
}
let dictionary = ["c": 3, "b": 2, "a": 1]
let sortedKeys = sortedKeysClosure(dictionary) // sortedKeys = ["a", "b", "c"]
```
