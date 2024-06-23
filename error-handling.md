# Error handling

### Swift Tilida Xatolarni Boshqarish

#### Xatolarni Boshqarish Nima?

Xatolarni boshqarish — bu dasturda yuzaga kelishi mumkin bo’lgan kutilmagan vaziyatlar va muammolarni aniqlash va ularga javob berish usuli. Swift tilida xatolarni boshqarish uchun `do-catch` bloklari, `throw` va `throws` kalit so’zlari, hamda `try` operatori ishlatiladi.

#### Xatolarni Aniqlash

Swift tilida xatolar `Error` protokoliga mos keladigan tur sifatida aniqlanadi. Odatda, bu uchun `enum` ishlatiladi

```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
```

#### Xatolarni Tashlash (Throwing Errors)

Xatolarni tashlash (`throwing`) funktsiyalar yoki metodlar ichida sodir bo’ladi. Xato yuzaga kelganda, `throw` kalit so’zi ishlatiladi:

```swift
func vend(itemNamed name: String) throws {
    guard let item = inventory[name] else {
        throw VendingMachineError.invalidSelection
    }
    
    guard item.count > 0 else {
        throw VendingMachineError.outOfStock
    }
    
    guard item.price <= coinsDeposited else {
        throw VendingMachineError.insufficientFunds(coinsNeeded: item.price - coinsDeposited)
    }
    
    coinsDeposited -= item.price
    inventory[name]!.count -= 1
    print("Dispensing \(name)")
}
```

#### Xatolarni Qo’lga Olish (Catching Errors)

Xatolarni qo’lga olish `do-catch` bloki yordamida amalga oshiriladi. do blokida xatolarni tashlaydigan kod yoziladi, `catch` bloklarida esa xatolarni tutib, ularga javob beriladi:

```swift
do {
    try vend(itemNamed: "Chips")
    print("Enjoy your snack!")
} catch VendingMachineError.invalidSelection {
    print("Invalid selection.")
} catch VendingMachineError.outOfStock {
    print("Out of stock.")
} catch VendingMachineError.insufficientFunds(let coinsNeeded) {
    print("Insufficient funds. Please insert an additional \(coinsNeeded) coins.")
} catch {
    print("Unexpected error: \(error).")
}
```

#### `try?` va `try!` Operatorlari

Agar xatolarni qo’lga olishni xohlamasangiz, lekin xato yuzaga kelsa `nil` qiymat qaytarishni istasangiz, `try?` operatorini ishlatishingiz mumkin. Bu operator xato tashlanganida `nil` qiymat qaytaradi:

```swift
if let result = try? vend(itemNamed: "Chips") {
    print("Enjoy your snack!")
} else {
    print("Unable to vend the item.")
}
```

Agar xato yuzaga kelmasligiga amin bo’lsangiz va uni qo’lga olishni xohlamasangiz, try! operatorini ishlatishingiz mumkin. Bu operator xato tashlanganida dastur nosozligiga olib keladi:

```swift
let result = try! vend(itemNamed: "Chips")
print("Enjoy your snack!")
```

#### Xatolarni Qayta Tashlash (Rethrowing Errors)

Funktsiya yoki metod boshqa tashlaydigan funktsiyalarni chaqirishi va xatolarni qayta tashlashi mumkin. Buning uchun rethrows kalit so’zi ishlatiladi:

```swift
func process(items: [String], vendingFunction: (String) throws -> Void) rethrows {
    for item in items {
        try vendingFunction(item)
    }
}
```

#### Xulosa

Swift tilida xatolarni boshqarish kuchli va moslashuvchan vositalar orqali amalga oshiriladi. Xatolarni aniqlash, tashlash va qo’lga olish orqali dastur kutilmagan vaziyatlarga tayyorlanadi va ularga mos javob beradi. Bu kodni barqaror va xavfsiz qilishda muhim rol o’ynaydi.
