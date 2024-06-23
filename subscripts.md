---
description: Swift Tilida Subscripts
---

# Subscripts

Subscripts (subskriptlar) — Swift tilida klasslar, strukturalar va enummarda indekslash orqali elementlarga kirish imkonini beradi. Odatda subscriptslar bir obyektni indekslash orqali uning elementlariga tezkor kirish uchun ishlatiladi.

#### Subskriptlar qanday ishlaydi?

Subskriptlar o’ziga xos sintaksisga ega bo’lib, u funksiyalarga o’xshaydi, lekin ular subscript kalit so’zi bilan e’lon qilinadi. Mana oddiy misol:

```swift
struct Matrix {
    let rows: Int, columns: Int
    var grid: [Double]

    init(rows: Int, columns: Int) {
        self.rows = rows
        self.columns = columns
        self.grid = Array(repeating: 0.0, count: rows * columns)
    }

    func indexIsValid(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && column >= 0 && column < columns
    }

    subscript(row: Int, column: Int) -> Double {
        get {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            return grid[(row * columns) + column]
        }
        set {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            grid[(row * columns) + column] = newValue
        }
    }
}
```

#### Subskriptlarni E’lon Qilish

Subskriptlar odatda to’plamlar yoki massivlar kabi ma’lumot tuzilmalariga kirish uchun ishlatiladi. Ular klass, struktur yoki enumda quyidagicha e’lon qilinadi:

```swift
subscript(index: Int) -> Element {
    get {
        // elementni qaytarish
    }
    set(newValue) {
        // elementni o'rnatish
    }
}
```

#### Subskriptlar Bilan Ishlash

Subskriptlarni chaqirish xuddi massiv elementlariga kirish kabi amalga oshiriladi:

```swift
var matrix = Matrix(rows: 2, columns: 2)
matrix[0, 1] = 1.5
let value = matrix[0, 1]
print(value) // 1.5
```

#### Kengaytirilgan Subskriptlar

Subskriptlar bir necha parametrlarni qabul qilishi mumkin va hatto parametrlar uchun turli xil turlarni ishlatishi mumkin:

```swift
struct TicTacToe {
    enum Mark: String {
        case X = "X", O = "O", Empty = "_"
    }
    var board: [Mark] = Array(repeating: .Empty, count: 9)

    subscript(row: Int, column: Int) -> Mark {
        get {
            return board[(row * 3) + column]
        }
        set {
            board[(row * 3) + column] = newValue
        }
    }
}

var game = TicTacToe()
game[0, 1] = .X
print(game[0, 1].rawValue) // X
```

#### Xulosa

Subskriptlar Swift tilida qulay va kuchli vosita bo’lib, ular obyektlar ichidagi elementlarga tezkor va intuitiv kirishni ta’minlaydi. Ularni to’g’ri tushunish va ishlatish orqali kodning samaradorligi va o’qilishi yaxshilanadi.
