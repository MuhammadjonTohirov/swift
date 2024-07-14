# Nested types

`Nested types` - bu turlarni boshqa tur (`class`, `struct`, `enum`) ichida joylashtirish imkonini beradi. Joylangan turlar kodni mantiqiy bo’limlarga ajratish va tashqi kodni tartibga solish uchun ishlatiladi

#### `Nested types` qayerlarda ishlatilinadi?

`Nested types` asosan kodni mantiqiy guruhlash, tashqi kod bilan ishlashni soddalashtirish va kodni yanada o’qiladigan qilish uchun ishlatiladi.

#### Namunalar

Skeleton - `struct` misolida&#x20;

```swift
struct OuterStruct {
    var name: String
    
    struct InnerStruct {
        var value: Int
    }
    
    var inner: InnerStruct
}

let outer = OuterStruct(name: "Outer", inner: OuterStruct.InnerStruct(value: 42))
print("Outer name: \(outer.name), Inner value: \(outer.inner.value)")
```

`enum` misolida

```swift
class OuterClass {
    var name: String
    
    enum InnerEnum {
        case caseOne
        case caseTwo
    }
    
    var inner: InnerEnum
    
    init(name: String, inner: InnerEnum) {
        self.name = name
        self.inner = inner
    }
}

let outer = OuterClass(name: "Outer", inner: .caseOne)
print("Outer name: \(outer.name), Inner enum: \(outer.inner)")
```

`class` misolida

```swift
struct OuterStruct {
    var name: String
    
    class InnerClass {
        var description: String
        
        init(description: String) {
            self.description = description
        }
    }
    
    var inner: InnerClass
}

let outer = OuterStruct(name: "Outer", inner: OuterStruct.InnerClass(description: "This is an inner class"))
print("Outer name: \(outer.name), Inner description: \(outer.inner.description)")
```

#### Amaliy misol

Joylangan turlar kodni yanada tartibli va o’qilishi oson qilish uchun keng ko’lamda qo’llanilishi mumkin. Masalan, quyidagi misolda `BlackjackCard` strukturasida `Rank` va `Suit` joylangan enumlar ishlatilgan.

```swift
struct BlackjackCard {
    // Rank joylangan enum
    enum Rank: Int {
        case two = 2, three, four, five, six, seven, eight, nine, ten
        case jack, queen, king, ace
    }
    
    // Suit joylangan enum
    enum Suit: Character {
        case spades = "♠", hearts = "♥", diamonds = "♦", clubs = "♣"
    }
    
    let rank: Rank
    let suit: Suit
    
    var description: String {
        return "Card: \(rank) of \(suit.rawValue)"
    }
}

let card = BlackjackCard(rank: .ace, suit: .spades)
print(card.description)
```

• `BlackjackCard` strukturasida `Rank` va `Suit` joylangan enumlari bor.\
• `Rank` va `Suit` enumlari kartaning rank va suitlarini ifodalaydi.\
• `BlackjackCard` ob’ekti yaratilib, kartaning tavsifini qaytaradi.

Joylangan turlar yordamida kodni yanada tartibli va yaxshi tashkil etilgan holda saqlash mumkin. Bu esa kodni tushunishni osonlashtiradi va kodni boshqarishni yaxshilaydi.
