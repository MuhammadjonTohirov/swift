---
description: >-
  Swift bir necha murakkab operatorlarni o'z ichiga oladi, ular oddiy arifmetik,
  solishtirish va mantiqiy operatorlardan tashqari qo'shimcha imkoniyatlar
  beradi. Bu murakkab operatorlar bit manipulyatsi
---

# Advanced operators

### Bitwise (Bitli) Operatorlar

Bitwise operatorlar sonlarning ikkilik ko'rinishi bilan to'g'ridan-to'g'ri ishlaydi.

#### Bitwise NOT (\~)

Sondagi barcha bitlarni teskari aylantiradi.

```swift
let initialBits: UInt8 = 0b00001111
let invertedBits = ~initialBits  // 11110000
```

#### Bitwise AND (&)

Ikkala kiritilgan sonning ham bitlari 1 ga teng bo'lgan joylaridagina 1 ga teng bo'lgan bitlarni qaytaradi.

```swift
let firstBits: UInt8 = 0b11110000
let secondBits: UInt8 = 0b00001111
let andBits = firstBits & secondBits  // 00000000
```

#### Bitwise OR (|)

Kiritilgan sonlarning birontasida ham 1 ga teng bo'lgan bitlarni qaytaradi.

```swift
let orBits = firstBits | secondBits  // 11111111
```

#### Bitwise XOR (^)

Kirish bitlari farqli bo'lgan joyda 1 ga teng bo'lgan bitlarni qaytaradi.

```swift
let xorBits = firstBits ^ secondBits  // 11111111
```

#### Bitwise Left Shift (<<) va Right Shift (>>)

Sondagi barcha bitlarni belgilangan joylar soni bo'yicha chapga yoki o'ngga siljitadi.

```swift
let shiftBits: UInt8 = 4   // 00000100
let leftShift = shiftBits << 1  // 00001000
let rightShift = shiftBits >> 1  // 00000010
```

### Overflow (Toshib ketish) Operatorlari

Swift butun sonlar toshib ketish holatlarini boshqarish uchun maxsus operatorlarni taqdim etadi.

#### Overflow Addition (&+)

Ikkita sonni qo'shadi, agar natija turning maksimal qiymatidan oshib ketsa, boshiga qaytib keladi.

```swift
swiftCopylet maxUInt8 = UInt8.max  // 255
let overflowingResult = maxUInt8 &+ 1  // 0
```

#### Overflow Subtraction (&-)

Bitta sondan boshqasini ayiradi, agar natija turning minimal qiymatidan kam bo'lsa, boshiga qaytib keladi.

```swift
let minUInt8 = UInt8.min  // 0
let underflowingResult = minUInt8 &- 1  // 255
```

#### Overflow Multiplication (&\*)

Ikkita sonni ko'paytiradi, natija turning maksimal qiymatidan oshib ketsa, boshiga qaytib keladi.

```swift
let multiplyOverflow: UInt8 = 200 &* 2  // 144 (400 emas, UInt8 dan oshib ketgani uchun)
```

### Maxsus Operatorlar

Swift o'z maxsus operatorlaringizni yaratishga imkon beradi.

#### Maxsus Operator Yaratish

```swift
// Maxsus operator e'lon qilish
prefix operator +++

// Operatorni amalga oshirish
prefix func +++(value: Int) -> Int {
    return value + 2
}

let result = +++5  // 7 ni qaytaradi
```

#### Maxsus Infix Operatorlar

```swift
// Maxsus infix operator e'lon qilish
infix operator **: CustomPrecedenceGroup

// Ustunlik guruhini aniqlash
precedencegroup CustomPrecedenceGroup {
    associativity: left
    higherThan: MultiplicationPrecedence
}

// Operatorni amalga oshirish (daraja funksiyasi)
func **(base: Int, power: Int) -> Int {
    var result = 1
    for _ in 0..<power {
        result *= base
    }
    return result
}

let powerResult = 2 ** 3  // 8
```

### Operator Metodlari

Siz o'z maxsus turlaringizda operatorlarni metodlar sifatida amalga oshirishingiz mumkin.

```swift
struct Vector2D {
    var x = 0.0, y = 0.0
}

extension Vector2D {
    static func + (left: Vector2D, right: Vector2D) -> Vector2D {
        return Vector2D(x: left.x + right.x, y: left.y + right.y)
    }
    
    static prefix func - (vector: Vector2D) -> Vector2D {
        return Vector2D(x: -vector.x, y: -vector.y)
    }
}

let v1 = Vector2D(x: 2.0, y: 3.0)
let v2 = Vector2D(x: 1.0, y: 1.0)
let v3 = v1 + v2  // Vector2D x: 3.0, y: 4.0 bilan
let v4 = -v1      // Vector2D x: -2.0, y: -3.0 bilan
```

### Murakkab Tayinlash Operatorlari

Bular tayinlash (=) ni boshqa operatsiya bilan birlashtiradi.

```swift
extension Vector2D {
    static func += (left: inout Vector2D, right: Vector2D) {
        left = left + right
    }
}

var v5 = Vector2D(x: 1.0, y: 2.0)
v5 += v2  // v5 endi x: 2.0, y: 3.0 ga ega
```

### Tenglik Operatorlari

O'z maxsus turlaringiz uchun tenglik operatorlarini amalga oshiring.

```swift
extension Vector2D: Equatable {
    static func == (left: Vector2D, right: Vector2D) -> Bool {
        return (left.x == right.x) && (left.y == right.y)
    }
}

let v6 = Vector2D(x: 2.0, y: 3.0)
print(v1 == v6)  // true
print(v1 == v2)  // false
```

Swift'dagi murakkab operatorlar bit manipulyatsiyasi, toshib ketish holatlarini boshqarish va o'zingizning maxsus ehtiyojlaringizga mos keluvchi maxsus operatorlar yaratish uchun kuchli vositalarni taqdim etadi. Ular ayniqsa matematik hisob-kitoblar, maxsus ma'lumotlar strukturalari va quyi darajadagi dasturlash vazifalarida ishlayotganda foydalidir.
