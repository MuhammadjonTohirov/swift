---
description: To‘liq qo‘llanma
---

# DispatchWorkItemFlags

## Kirish

DispatchWorkItemFlags — bu Swiftda Grand Central Dispatch (GCD) orqali ish elementlari (WorkItem) ustida ishlashda qo‘llaniladigan maxsus flaglar to‘plami. Bu bayroqlar (flags) yordamida:

• Kodni optimallashtirish

• Xavfsizlikni oshirish

• Ishlash tezligini oshirish mumkin bo‘ladi.

Ushbu maqolada `.barrier`, `.detached`, `.inheritQoS` va `.assignCurrentContext` bayroqlari haqida batafsil ma’lumot beriladi.

### `.barrier` — Xavfsiz yozish operatsiyasi`\`

Agar yagona manbani bir vaqtning o‘zida o‘qish va yozish kerak bo‘lsa, sinxronizatsiya qilinmagan holda bu kod `race conditionga` olib kelishi mumkin. Misol tariqasida:

```swift
var sharedResource = [String]()
let queue = DispatchQueue(label: "com.example.queue", attributes: .concurrent)

let writerWorkItem = DispatchWorkItem {
    for i in 1...5 {
        sharedResource.append("Item \(i)")
        print("Yozilmoqda: Item \(i)")
    }
}

let readerWorkItem = DispatchWorkItem {
    for _ in 1...5 {
        print("O'quvchi ma'lumotlarni ko'rmoqda: \(sharedResource)")
    }
}

// Bir vaqtning o'zida queue ichida yozish va o'qish sodir bo'ladi
queue.async(execute: readerWorkItem)
queue.async(execute: writerWorkItem)
queue.async(execute: readerWorkItem)
```

• readerWorkItem massivdagi ma’lumotlarni o‘qiyotgan paytida writerWorkItem massivga ma’lumot qo‘shishi mumkin.\
• Natijada sharedResource bir vaqtning o‘zida o‘qib/yozib bo‘linib ketishi ehtimoli bor — bu undefined behavior yoki crashga olib kelishi mumkin.

#### `.barrier` bilan to‘g‘ri usul

```swift
import Foundation

var sharedResource = [String]()
let queue = DispatchQueue(label: "com.example.queue", attributes: .concurrent)

let writerWorkItem = DispatchWorkItem {
    // .barrier flag bilan yozish paytida boshqa vazifalar kutib turadi
    queue.async(flags: .barrier) {
        for i in 1...5 {
            sharedResource.append("Item \(i)")
            print("Yozilmoqda: Item \(i)")
        }
    }
}

let readerWorkItem = DispatchWorkItem {
    // O‘qishni, masalan, queue.sync orqali sinxron bajarish mumkin
    queue.sync {
        for _ in 1...5 {
            print("O'quvchi ma'lumotlarni ko'rmoqda: \(sharedResource)")
        }
    }
}

queue.async(execute: readerWorkItem)
queue.async(execute: writerWorkItem)
queue.async(execute: readerWorkItem)
```

#### `.barrier`ning afzalliklari:

• Yozish vaqtida boshqa hamma ishlar to‘xtatiladi yoki navbatda kutib turadi.

• O‘qish paytida massivni hech kim o‘zgartirmaydi, shu bois xavfsiz va barqaror ishlaydi.

• Race condition yo‘q qilinadi.

### .detached — Mustaqil bajarish

`.detached` bayrog‘i ish elementiga mustaqillik beradi, ya’ni u asosiy ish navbatidan ajralib ishga tushishi mumkin. Bu ba’zan ma’lum ishni asosiy oqimga bog‘lamasdan, alohida resurslar bilan odam xohlagan vaqtda bajarish uchun foydali.

#### `.detached` ishlatilmaganda muammo

```swift
let workItem = DispatchWorkItem {
    print("Bu ish elementi asosiy oqim bilan bog‘langan.")
}

DispatchQueue.global().async(execute: workItem)
```

• Ish elementi `DispatchQueue.global()` bilan bog‘langan, lekin u haligacha joriy boshqaruv kontekstiga qisman bog‘liqbo‘lishi mumkin.\
• Ba’zi hollarda ish to‘liq mustaqil bo‘lishi kerak bo‘ladi, masalan, ishni raqamli signallarni qayta ishlashda, operatsion tizim darajasidagi bevosita chaqiruvlarda yoki resurslarni alohida boshqarishda.

#### `.detached` bilan to‘g‘ri usul

```swift
let detachedWorkItem = DispatchWorkItem(flags: .detached) {
    print("Bu mustaqil ish bajarilmoqda — asosiy oqimga bevosita bog‘liq emas.")
}

DispatchQueue.global().async(execute: detachedWorkItem)
```

#### Foyda:

• Ish to‘liq mustaqil bajariladi, asosiy navbatga ortiqcha yuk tushmaydi.\
• Kerak bo‘lganda yopiq (`standalone`) jarayon sifatida ishlatish mumkin.

### .inheritQoS — Sifat darajasini meros qilib olish

QoS (Quality of Service) — GCDda ishlarning ustuvorlik (`priority`) darajasini belgilaydi. `.inheritQoS` bayrog‘i ish elementiga chaqirgan navbatning `QoS` darajasini meros qilib olish imkonini beradi.

#### `.inheritQoS` ishlatilmaganda muammo

```swift
import Foundation

// .background QoS darajasidagi navbat
let backgroundQueue = DispatchQueue.global(qos: .background)

let workItem = DispatchWorkItem {
    // Bu ish elementi default QoS bilan bajarilishi mumkin
    print("QoS ni meros qilib olmadik.")
}

backgroundQueue.async(execute: workItem)
```

**Natija**: `workItem` `.background` darajasida emas, balki `default` yoki boshqa `QoS` darajasida bajarilib ketishi mumkin. Bu asinxron kodda kutmagan natijalaringizga sabab bo‘ladi.

#### `.inheritQoS` bilan to‘g‘ri usul

```swift
import Foundation

let backgroundQueue = DispatchQueue.global(qos: .background)

let workItem = DispatchWorkItem(flags: .inheritQoS) {
    // Bu ish endi backgroundQueue'dan QoS darajasini meros qilib oldi
    print("QoS ni meros qilib oldik — .background darajasida ishlayapman.")
}

backgroundQueue.async(execute: workItem)
```

**Foyda:**\
• Ish elementi chaqirgan navbatning QoS darajasida bajarilib, kutilgan ustuvorlik saqlanadi.\
• Resurslarni bir xilda boshqarish, parallel jarayonni izchil rejalashtirish osonlashadi.

### .assignCurrentContext — Joriy kontekstni biriktirish

`.assignCurrentContext` bayrog‘i ish elementini joriy kontekstga biriktiradi. Bu asosan ichki kontekstlarda (masalan, `GUI`, `audio`, `videoni` qayta ishlash, yoki spesifik `callback` jarayonlarida) resurslarni to‘g‘ri boshqarish uchun ishlatiladi.

