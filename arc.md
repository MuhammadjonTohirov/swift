---
description: Automatic reference counting
---

# ARC

### ARC (Automatic Reference Counting) nima?

ARC — bu Swift dasturlash tilida xotirani avtomatik boshqarish usuli. ARC yordamida xotira boshqaruvi avtomatik amalga oshiriladi. U class turidagi obyektlarning xotirada qanchalik ishlatilayotganligini kuzatadi va kerak bo‘lmasa, ularni xotiradan o‘chirib tashlaydi. Bu dasturchilarga xotira boshqarish bilan bog‘liq muammolarni hal qilishni soddalashtiradi.

#### Qanday ishlaydi?

ARC har bir class turidagi obyekt yaratganda, uning xotiraga joylashishini boshqaradi va unga “`reference count`” (havolalar soni) beradi. Har safar obyektdan foydalanilganda, bu son oshiriladi, va agar obyektdan foydalanish tugasa, u kamaytiriladi. “`Reference count`” nolga teng bo‘lganda, obyekt xotiradan o‘chiriladi.

#### ARC foydalanish turlari

1\. Kuchli (`strong`) havola (`reference`):

* Obyektga kuchli havola o‘rnatilganda, uning “`reference count`”i `1` ga oshadi.
* Agar har ikkala obyekt bir-biriga kuchli havola qilsalar, “siklik havola” (`retain cycle`) yuzaga kelishi mumkin va xotira hech qachon ozod bo‘lmaydi.

Siklik Havola Muammosi (`Cyclic reference`)

```swift
class Ota {
    var ism: String
    var bola: Bola? // Kuchli havola

    init(ism: String) {
        self.ism = ism
        print("\(ism) yaratilgan")
    }

    deinit {
        print("\(ism) o'chirildi")
    }
}

class Bola {
    var ism: String
    var ota: Ota? // Kuchli havola

    init(ism: String) {
        self.ism = ism
        print("\(ism) yaratilgan")
    }

    deinit {
        print("\(ism) o'chirildi")
    }
}

var ota: Ota? = Ota(ism: "Akbar")
var bola: Bola? = Bola(ism: "Aziz")

ota?.bola = bola
bola?.ota = ota

ota = nil
bola = nil

// "deinit" funksiyasi hech qachon chaqirilmaydi va obyektlar xotirada qolib ketadi.
```

> Yuqoridagi kodda ota va bola obyektlari bir-birini kuchli havola (`strong reference`) qilib turadi. Natijada, ikkala obyekt ham “`reference count`”ga ega bo‘lib qoladi va hech qachon xotiradan o‘chirilmaydi. Bu esa `memory leak` (xotira sizib chiqishi) muammosiga olib keladi.

### Yechim: weak yoki unowned dan foydalanish

Bu muammoni hal qilish uchun weak yoki unowned havolalaridan foydalanish kerak.

* `weak reference` (zaif havola) : “`reference count`”ni oshirmaydi va obyekt yo‘q bo‘lganda `nil`ga o‘rnini almashtiradi. `weak` havola har doim `Optional` bo‘ladi.
* `unowned reference` (o‘zaro bo‘lmagan havola): “`reference count`”ni oshirmaydi, lekin obyekt mavjud bo‘lishi aniq bo‘lgan holatlarda ishlatiladi. `unowned` havola `nil` bo‘lmaydi!\
  Shunga etibor berish kerakki agar `unowned object` allaqachon `deinit` bo'lgan bo'lsa, anushu `object` dan foydalanish chog'ida `fatalError` yuzaga keladi

#### `weak` bilan siklik havolani to‘xtatish

```swift
class Ota {
    var ism: String
    var bola: Bola? // Kuchli havola

    init(ism: String) {
        self.ism = ism
        print("\(ism) yaratilgan")
    }

    deinit {
        print("\(ism) o'chirildi")
    }
}

class Bola {
    var ism: String
    weak var ota: Ota? // Zaif havola

    init(ism: String) {
        self.ism = ism
        print("\(ism) yaratilgan")
    }

    deinit {
        print("\(ism) o'chirildi")
    }
}

var ota: Ota? = Ota(ism: "Akbar")
var bola: Bola? = Bola(ism: "Aziz")

ota?.bola = bola
bola?.ota = ota

ota = nil
bola = nil

// Endi "deinit" funksiyasi chaqiriladi va obyektlar xotiradan o'chiriladi.
```

> Yuqoridagi kodda, Bola sinfida ota zaif havola (weak) qilib belgilanadi. Endi obyektlar bir-birini kuchli ushlab turmaydi va nilga o‘tganda “deinit” funksiyasi chaqiriladi. Shu tarzda obyektlar xotiradan to‘g‘ri o‘chiriladi.

Swift’da `ARC` xotira boshqarishni avtomatlashtiradi, ammo kuchli, zaif (`weak`), va o‘zaro bo‘lmagan (`unowned`) havolalarni to‘g‘ri ishlatishni talab qiladi. Noto‘g‘ri va to‘g‘ri misollar yordamida `ARC` ishlashini to‘liq tushunishingiz mumkin, shuningdek, siklik havola (`retain cycle`) kabi muammolardan qochishingiz mumkin.
