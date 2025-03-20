---
description: >-
  QoS (Quality of Service) - bu Grand Central Dispatch (GCD) tizimida
  vazifalarning muhimlik darajasini belgilash mexanizmi. Bu mexanizm orqali
  operatsion tizim vazifalarning muhimligini tushunadi va re
---

# QoS (Quality of Service)

### QoS darajalari

Swift tilida GCD uchun quyidagi QoS darajalari mavjud (yuqori ustuvorlikdan past ustuvorlikka qarab):

1. **`.userInteractive`** - Eng yuqori ustuvorlik
2. **`.userInitiated`** - Yuqori ustuvorlik
3. **`.default`** - O'rtacha ustuvorlik (standart)
4. **`.utility`** - O'rtacha-past ustuvorlik
5. **`.background`** - Eng past ustuvorlik
6. **`.unspecified`** - Aniqlanmagan (tizim o'zi hal qiladi)

### Har bir QoS darajasi tavsifi

#### 1. `.userInteractive`

* **Tavsif:** Foydalanuvchi interfeysi bilan to'g'ridan-to'g'ri bog'liq bo'lgan vazifalar uchun
* **Vaqt ramkasi:** Darhol (millisekundlar)
* **Foydalanish holatlari:** Animatsiyalar, UI yangilanishlari, foydalanuvchi kiritishlari
* **Batareya ta'siri:** Yuqori

```swift
let userInteractiveQueue = DispatchQueue.global(qos: .userInteractive)
userInteractiveQueue.async {
    // UI animatsiyasini yangilash
    self.updateUIAnimation()
}
```

#### 2. `.userInitiated`

* **Tavsif:** Foydalanuvchi tomonidan boshlangan va natijalarni kutadigan vazifalar
* **Vaqt ramkasi:** Bir necha sekund
* **Foydalanish holatlari:** Hujjatni ochish, tugmani bosishga javob berish, faol ma'lumotlarni yuklash
* **Batareya ta'siri:** O'rtacha-yuqori

```swift
let userInitiatedQueue = DispatchQueue.global(qos: .userInitiated)
userInitiatedQueue.async {
    // Foydalanuvchi bosgan hujjatni yuklash
    let document = self.loadDocument(id: documentId)
    
    DispatchQueue.main.async {
        self.displayDocument(document)
    }
}
```

#### 3. `.default`

* **Tavsif:** Standart ustuvorlik, odatiy holat
* **Vaqt ramkasi:** Bir necha sekund
* **Foydalanish holatlari:** Aniq QoS talablariga ega bo'lmagan vazifalar
* **Batareya ta'siri:** O'rtacha

```swift
let defaultQueue = DispatchQueue.global(qos: .default)
defaultQueue.async {
    // Standart vazifa
    self.processData()
}
```

#### 4. `.utility`

* **Tavsif:** Uzoq vaqt davom etadigan, lekin darhol natijalarni talab qilmaydigan vazifalar
* **Vaqt ramkasi:** Bir necha sekund yoki daqiqalar
* **Foydalanish holatlari:** Ma'lumotlarni yuklab olish, import/eksport, progress bar bilan ko'rsatiladigan vazifalar
* **Batareya ta'siri:** O'rtacha-past

```swift
let utilityQueue = DispatchQueue.global(qos: .utility)
utilityQueue.async {
    // Katta rasm faylini yuklab olish
    let imageData = self.downloadLargeImage(url: imageUrl)
    
    DispatchQueue.main.async {
        self.progressBar.isHidden = true
        self.imageView.image = UIImage(data: imageData)
    }
}
```

#### 5. `.background`

* **Tavsif:** Foydalanuvchi ko'rmaydigan va vaqt-sezgirligi past bo'lgan vazifalar
* **Vaqt ramkasi:** Daqiqalar yoki soatlar
* **Foydalanish holatlari:** Zaxira nusxa olish, indekslash, sinxronlash, katta ma'lumotlar to'plamini qayta ishlash
* **Batareya ta'siri:** Past

```swift
let backgroundQueue = DispatchQueue.global(qos: .background)
backgroundQueue.async {
    // Mahalliy ma'lumotlar bazasini sinxronlash
    self.syncDatabase()
    
    // Fayllarni zaxiralash
    self.backupDocuments()
}
```

#### 6. `.unspecified`

* **Tavsif:** Tizim o'zi ustuvorlikni aniqlaydi
* **Foydalanish holatlari:** Legacy kodlar bilan ishlash yoki maxsus vaziyatlarda

```swift
let unspecifiedQueue = DispatchQueue.global(qos: .unspecified)
unspecifiedQueue.async {
    // Tizim ustuvorlikni o'zi belgilaydi
    self.doSomeWork()
}
```

### QoS'ni qo'llash usullari

QoS'ni GCD'da quyidagi yo'llar bilan qo'llash mumkin:

#### 1. Global navbat yaratishda

```swift
let backgroundQueue = DispatchQueue.global(qos: .background)
```

#### 2. Maxsus navbat yaratishda

```swift
let customQueue = DispatchQueue(label: "com.example.customQueue", qos: .utility)
```

#### 3. Vazifa yaratishda (`async` metodi orqali)

```swift
DispatchQueue.global().async(qos: .userInitiated) {
    // Kod...
}
```

#### 4. `DispatchWorkItem` yaratishda

```swift
let workItem = DispatchWorkItem(qos: .utility) {
    // Kod...
}
DispatchQueue.global().async(execute: workItem)
```

### QoS'ning amaliy tavsiyalari

1. **UI-ga aloqador vazifalarni yuqori ustuvorlikda bajaring**: `.userInteractive` yoki `.userInitiated`ustuvorliklarini foydalanuvchi interfeysi bilan bog'liq bo'lgan vazifalar uchun ishlating.
2. **Uzoq muddatli vazifalarni past ustuvorlikda bajaring**: `.utility` yoki `.background` ustuvorliklarini uzoq vaqt davom etadigan operatsiyalar uchun ishlating. Malumotlar omboriga narsa saqlash yoki olish uchun `.utility`  dan foydalanish yaxshiroq. Sababi u backgroundan ko'ra darajasi bir muncha yuqoriroq
3. **QoS darajasini vazifaning muhimligiga qarab tanlang**: Har bir vazifaning foydalanuvchi tajribasiga qanday ta'sir qilishini o'ylab ko'ring.
4. **QoS invertsiyasidan qoching**: Yuqori ustuvorlikdagi vazifalar past ustuvorlikdagi vazifalar tomonidan bloklansmasligiga e'tibor bering.
5. **QoS-ni haddan tashqari ishlatmang**: Barcha vazifalar uchun `.userInteractive` dan foydalanish umumiy tizim ishlashini pasaytiradi.

### Xulosalar

`QoS` GCD'da vazifalarning ustuvorligini boshqarish uchun kuchli mexanizmdir. To'g'ri `QoS` darajalarini tanlash orqali, tizim resurslarining samarali taqsimlanishini ta'minlash va foydalanuvchi tajribasini yaxshilash mumkin. Har bir vazifa uchun to'g'ri `QoS` darajasini tanlash, batareya quvvatini tejash va dastur samaradorligini oshirish uchun muhimdir.

Vazifalarning muhimligini va ularning foydalanuvchi interfeysi bilan bog'liqligini hisobga olgan holda `QoS` darajasini tanlab, `Swift` dasturlaringizni yanada samarali, tezkor va batareya-samarali qilishingiz mumkin.
