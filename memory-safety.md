---
description: Xotira xavfsizligi
---

# Memory Safety

Swift dasturlash tili xotira xavfsizligini ta'minlash uchun bir qator mexanizmlarni taqdim etadi. Xotira xavfsizligi dasturlashda eng muhim aspektlardan biri bo'lib, noto'g'ri xotira boshqaruvi jiddiy xatoliklar va xavfsizlik zaifliklariga olib kelishi mumkin.

### Xotira xavfsizligi nima?

Xotira xavfsizligi - bu dastur xotiraga kirish va undan foydalanishda xatosiz ishlashini ta'minlash. Swift ushbu muammolarni avtomatik yechish uchun bir nechta mexanizmlarni taqdim etadi:

1. **Automatic Reference Counting (ARC)** - xotirani avtomat boshqarish uchun
2. **O'zgaruvchilar kirishini nazorat qilish** - bir vaqtning o'zida xotiraning bir qismiga bir nechta kirish holatlarini oldini olish
3. **Qiymat turlari va havola turlari** - ma'lumotlar o'zgarishini boshqarish

### 1. Xotiraga kirish nizolari

Swift'da ko'pincha uchraydigan xotira xavfsizligi muammolardan biri - bu xotiraga kirish nizolari. Bu quyidagi hollarda sodir bo'ladi:

```swift
func add() {
    var son = 10
    
    // Bu kod kompilyatsiya vaqtida xato beradi
    withUnsafePointer(to: &son) { ptr in
        son += 5 // ❌ son o'zgaruvchisiga bir vaqtning o'zida kirish
        print(ptr.pointee)
    }
}
```

Yuqoridagi misolda, `son` o'zgaruvchisi `withUnsafePointer` funksiyasi ichida ishlatilayotgan vaqtda uni o'zgartirishga urinish xotira xavfsizligi muammosini keltirib chiqaradi.

#### To'g'ri variant:

```swift
func correct() {
    var son = 10
    
    // Avval o'zgaruvchini o'zgartiring
    son += 5
    
    // Keyin xavfsiz ko'rsatkich bilan foydalaning
    withUnsafePointer(to: &son) { ptr in
        print(ptr.pointee)
    }
}
```

### 2. O'zgaruvchilarni o'zlashtirish va o'zgartirishning nizolari

Swift in-out parametrlar bilan ishlaganda xotira xavfsizligiga alohida e'tibor qaratadi.

```swift
func swap(x: inout Int, y: inout Int) {
    let vaqtinchalik = x
    x = y
    y = vaqtinchalik
}

var son = 10
swap(x: &son, y: &son) // X Kompilyatsiya vaqtidagi xato
```

Bu kod kompilyatsiya vaqtida xato beradi, chunki `son` o'zgaruvchisi `nizoli` funksiyaga ikki marta reference sifatida berilmoqda.

#### To'g'ri variant:

```swift
func swap<T>(x: inout T, y: inout T) {
    let vaqtinchalik = x
    x = y
    y = vaqtinchalik
}

var a = 10
var b = 20
swap(x: &a, y: &b) // ✅ Turli o'zgaruvchilar
```

### 3. Swift ARC (Automatic Reference Counting)

{% content-ref url="arc.md" %}
[arc.md](arc.md)
{% endcontent-ref %}

### 4. Value Types va Reference Types

Swift'da qiymat turlari (struct, enum) va havola turlari (class) o'rtasidagi farqni tushunish xotira xavfsizligi uchun muhim:

```swift
// Qiymat turi - nusxa ko'chiriladi
struct StructMisol {
    var qiymat: Int
}

// Havola turi - havolalar almashtiriladi
class ClassMisol {
    var qiymat: Int
    
    init(qiymat: Int) {
        self.qiymat = qiymat
    }
}

func valueVsReference() {
    var struct1 = StructMisol(qiymat: 10)
    var struct2 = struct1 // Nusxa ko'chiriladi
    struct2.qiymat = 20
    
    print(struct1.qiymat) // 10 - o'zgarmadi
    print(struct2.qiymat) // 20
    
    var class1 = ClassMisol(qiymat: 10)
    var class2 = class1 // Havola ko'chiriladi
    class2.qiymat = 20
    
    print(class1.qiymat) // 20 - o'zgardi
    print(class2.qiymat) // 20
}
```

### 5. Xavfsiz kod yozish uchun tavsiyalar

1. **`weak` va `unowned` havolalarni to'g'ri ishlating** - siklik havolalarni (retain cycles) oldini olish uchun
2. **Imkon qadar `struct` ishlating** - ular qiymat turlari bo'lib, havolalar muammolarini kamaytiradi
3. **O'zgaruvchilarni iloji boricha local qilib ishlating** - global o'zgaruvchilar nizolarga olib kelishi mumkin
4. **Xotiraga kirish nizolarini bartaraf eting** - bir vaqtda xotiraning bir qismiga bir nechta kirish holatlarini oldini oling
5. **`inout` parametrlarni ehtiyotkorlik bilan ishlating** - kerak bo'lmaganda ishlatmang

### Xulosa

Swift'da xotira xavfsizligi dasturning ishonchliligi va xavfsizligi uchun juda muhim. Swift dasturlash tili xotira bilan ishlashda yuzaga keladigan ko'plab muammolarni automatik hal qiladi, ammo dasturchilar ham xotira bilan ishlaydigan kodlarni yaxshi tushunish va to'g'ri yozish muhimdir.

* ARC mexanizmini to'g'ri tushunish
* Siklik havolalar (retain cycles) uchun `weak` va `unowned` havolalardan foydalanish
* Bir vaqtning o'zida bir xotira joyiga kirishning oldini olish
* Value types va reference types o'rtasidagi farqlarni bilish

Bu prinsiplarga amal qilish orqali, Swift'da ishonchli va xotira jihatdan xavfsiz dasturlar yaratish mumkin.
