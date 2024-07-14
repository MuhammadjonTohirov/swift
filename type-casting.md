---
description: >-
  Type casting - bu bitta turdagi ob’ektni boshqa turga aylantirish jarayonidir.
  Swift’da type casting asosan sinflar va protokollar bilan ishlatiladi. Bu
  ob’ektning aniq turini tekshirish yoki uni bosh
---

# Type Casting

#### Type Casting Operatorlari

1. `is` operatori: Bu operator ob’ektning turini tekshiradi va natija sifatida `true` yoki `false` qaytaradi.
2. `as?` operatori: Bu operator xavfsiz (`optional`) type casting amalga oshiradi va agar casting muvaffaqiyatli bo’lsa, optional qiymat qaytaradi, aks holda `nil` qaytaradi.
3. `as!` operatori: Bu operator majburiy (`forced`) type casting amalga oshiradi va agar casting muvaffaqiyatli bo’lmasa, runtime xatosi (`runtime error`) yuzaga keladi.

#### `is` operatori

```swift
class Animal {
    var name: String
    init(name: String) {
        self.name = name
    }
}

class Dog: Animal {
    func bark() {
        print("Woof!")
    }
}

class Cat: Animal {
    func meow() {
        print("Meow!")
    }
}

let pet: Animal = Dog(name: "Buddy")

if pet is Dog {
    print("This pet is a dog.")
} else {
    print("This pet is not a dog.")
}
```

#### `as?` operatori

```swift
let pet: Animal = Dog(name: "Buddy")

if let dog = pet as? Dog {
    dog.bark()
} else {
    print("Casting to Dog failed.")
}

if let cat = pet as? Cat {
    cat.meow()
} else {
    print("Casting to Cat failed.")
}
```

#### `as!` operatori

```swift
let pet: Animal = Dog(name: "Buddy")

let dog = pet as! Dog
dog.bark()

// Quyidagi qator runtime xatosi (crash) keltirib chiqaradi, chunki pet aslida Cat emas
// let cat = pet as! Cat
// cat.meow()
```

#### Qo’shimcha misollar

Type casting ko’pincha massivlar (`array`s) bilan ishlatiladi, chunki massivlar bir nechta turdagi elementlarga ega bo’lishi mumkin.

```swift
let items: [Any] = [Dog(name: "Buddy"), Cat(name: "Whiskers"), "String", 42]

for item in items {
    if let dog = item as? Dog {
        dog.bark()
    } else if let cat = item as? Cat {
        cat.meow()
    } else if let string = item as? String {
        print("This is a string: \(string)")
    } else if let number = item as? Int {
        print("This is a number: \(number)")
    }
}
```

#### Hulosa

1. `is` operatori - bu operator ob’ektning turini tekshiradi va `true` yoki `false` qaytaradi.
2. `as?` operatori - bu xavfsiz type casting amalga oshiradi va muvaffaqiyatli bo’lsa optional qiymat qaytaradi, aks holda `nil`.
3. `as!` operatori - bu majburiy `type casting` amalga oshiradi va muvaffaqiyatli bo’lmasa runtime xatosi yuzaga keladi.

`Type casting` yordamida biz ob’ektlarning aniq turini tekshirishimiz va kerakli turga xavfsiz yoki majburiy tarzda aylantirishimiz mumkin. Bu dasturlashda ko’p turdagi ob’ektlar bilan ishlashda muhim.
