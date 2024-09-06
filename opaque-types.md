---
description: >-
  Opaque Types (aniq bo’lmagan turlar) Swift tilida funksiyaning qaytish
  qiymatini yashirishga yordam beradi, lekin shu bilan birga, bu qiymatning
  ma’lum bir protokolga mos kelishini ta’minlaydi. Bu jud
---

# Opaque types

Ba’zida biz funksiyalar yoki metodlar tomonidan qaytarilgan qiymatlarning aniq turini yashirishni xohlaymiz. Buning sababi ko’pincha kodni yanada soddalashtirish va moslashuvchan qilishdir. `Opaque Types` yordamida biz funksiyamiz qaytarayotgan qiymat aniq bir protokolga mos kelishini aytishimiz mumkin, lekin bu aniq qanday tur ekanligini ko‘rsatmaymiz.

`Opaque type` sintaksisi

```swift
// funksia_nomi() -> some ProtocolNomi
func functionName() -> some ProtocolName {
    // Funksiya tanasi
}
```

***

Keling, oddiy misolni ko‘rib chiqamiz. Bizda Animal nomli protokol bor va u hayvonning tovush chiqarish qobiliyatini belgilaydi. Dog va Cat tuzilmalari ushbu protokolga mos keladi:

```swift
protocol Animal {
    func sound() -> String
}

struct Dog: Animal {
    func sound() -> String {
        return "Woof!"
    }
}

struct Cat: Animal {
    func sound() -> String {
        return "Meow!"
    }
}

func createAnimal() -> some Animal { // Bu yerda opaqeu type dan foydalanyapmiz
    return Dog()
}
```

Yuqoridagi `createAnimal()` funksiyasi `some Animal` turini qaytaradi. Bu degani, u `Animal` protokoliga mos keladigan biror obyektni qaytaradi, lekin aniq qaytadigan tur (Dog) ekanligini yashiradi.

> Shuni yodda tutingki, bu funksiya faqat bitta aniq turdagi qiymatni qaytarishi mumkin (`Dog` yoki `Cart` yoki biror boshqa `Animal` `protocol` ini qabul qilgan `struct` yoki `class`)

#### Noto’g’ri Misol

```swift
func createRandomAnimal() -> some Animal {
    if Bool.random() {
        return Dog()  // Xato: 'Dog' va 'Cat' turli turlar
    } else {
        return Cat()
    }
}
```

Bu yerda `createRandomAnimal()` funksiyasi turli turdagi qiymatlarni qaytarishga urinadi (`Dog` yoki `Cat`), bu esa `some Animal` uchun to‘g‘ri kelmaydi. `Opaque Types` bir xil aniq turdagi qiymatni qaytarishni talab qiladi.

#### Opaque Types quyidagi holatlarda juda foydali bo’lishi mumkin:

**1**. **Ma’lumotlarni yashirish**: Agar siz qaytuvchi qiymatning aniq turini yashirishni xohlasangiz, lekin qiymatning protokolga mos kelishini ta’minlamoqchi bo’lsangiz. Bu inkapsulyatsiyani yaxshilaydi va kodni himoyalaydi.\
**2**. **Interfeysni soddalashtirish**: Opaque types kod interfeyslarini soddalashtiradi. Funksiyangiz faqat bitta protokolga mos keladigan qiymatni qaytarishini aytib, foydalanuvchilar uchun API’ni tushunarliroq qilishingiz mumkin.\
**3**. **Qo’shimcha ma’lumot bermaslik**: Foydalanuvchilar tur haqidagi qo’shimcha ma’lumotlarni bilishini xohlamaganingizda. Masalan, siz funksiyadan biror obyektni qaytarishingiz mumkin, lekin foydalanuvchi uning qanday turda ekanligini bilishi shart emas.\
**4**. **Tur xavfsizligini ta’minlash**: Opaque Types foydalanish aniq bir tur bilan ishlashda xatoliklar sonini kamaytiradi va kodda aniq bir protokolga mos keladigan tur ishlatilishini ta’minlaydi.

Xo'sh `-> some ProtocolName` bilan `-> ProtocolName` nima farqi bor.

