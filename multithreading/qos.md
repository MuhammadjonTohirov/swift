---
description: >-
  Quality of service - bu GCD navbatlarining ustuvorlik darajasini belgilash
  uchun ishlatiladigan mexanizm. Bu ustuvorlik darajasi navbatdagi vazifalarning
  qanchalik tez bajarilishini aniqlaydi. QoS dar
---

# QoS

#### QoS darajalari

1. Background (Fon)\
   • Bu eng past ustuvorlik darajasi.\
   • Uzoq muddatli yoki kam ustuvorlikdagi vazifalar uchun ishlatiladi, masalan, ma’lumotlarni sinxronizatsiya qilish.
2. &#x20;Utility (Yordamchi)\
   • O’rta past ustuvorlik darajasi.\
   • Vaqt talab qiladigan, ammo foydalanuvchi tajribasiga darhol ta’sir qilmaydigan vazifalar uchun ishlatiladi.
3. &#x20;User-Initiated (Foydalanuvchi tomonidan boshlatilgan)\
   • O’rta yuqori ustuvorlik darajasi.\
   • Foydalanuvchi tomonidan boshlatilgan va darhol natija kutayotgan vazifalar uchun ishlatiladi.
4. User-Interactive (Foydalanuvchi bilan muloqot qiluvchi)\
   • Eng yuqori ustuvorlik darajasi.\
   • Foydalanuvchi interfeysi bilan bog’liq va darhol javob qaytarish talab qiladigan vazifalar uchun ishlatiladi.

#### Namuna

```swift
import Foundation

// Background QoS navbati
let backgroundQueue = DispatchQueue.global(qos: .background)
backgroundQueue.async {
    for i in 1...5 {
        print("Background QoS: \(i)")
        sleep(1)
    }
}

// Utility QoS navbati
let utilityQueue = DispatchQueue.global(qos: .utility)
utilityQueue.async {
    for i in 1...5 {
        print("Utility QoS: \(i)")
        sleep(1)
    }
}

// User-Initiated QoS navbati
let userInitiatedQueue = DispatchQueue.global(qos: .userInitiated)
userInitiatedQueue.async {
    for i in 1...5 {
        print("User-Initiated QoS: \(i)")
        sleep(1)
    }
}

// User-Interactive QoS navbati
let userInteractiveQueue = DispatchQueue.global(qos: .userInteractive)
userInteractiveQueue.async {
    for i in 1...5 {
        print("User-Interactive QoS: \(i)")
        sleep(1)
    }
}

// Asosiy oqimni kutish
DispatchQueue.main.async {
    print("Barcha QoS vazifalari bajarildi")
}
```

#### Natija

```
User-Interactive QoS: 1
User-Interactive QoS: 2
User-Interactive QoS: 3
User-Interactive QoS: 4
User-Interactive QoS: 5
User-Initiated QoS: 1
User-Initiated QoS: 2
User-Initiated QoS: 3
User-Initiated QoS: 4
User-Initiated QoS: 5
Utility QoS: 1
Utility QoS: 2
Utility QoS: 3
Utility QoS: 4
Utility QoS: 5
Background QoS: 1
Background QoS: 2
Background QoS: 3
Background QoS: 4
Background QoS: 5
Barcha QoS vazifalari bajarildi
```

1. User-Interactive QoS vazifalari birinchi bajariladi, chunki ular eng yuqori ustuvorlikka ega.
2. User-Initiated QoS vazifalari ikkinchi o’rinda bajariladi.
3. Utility QoS vazifalari uchinchi o’rinda bajariladi.
4. Background QoS vazifalari oxirida bajariladi, chunki ular eng past ustuvorlikka ega.
5. DispatchQueue.main.async - bu xabar asosiy oqimda chiqariladi va barcha QoS vazifalari tugatilganini bildiradi.

QoS darajalaridan foydalangan holda dasturimizda vazifalarni muhimligi va tez bajarilishi kerakligiga qarab ustuvorlik darajasiga ko’ra boshqarishimiz mumkin. Bu foydalanuvchi tajribasini yaxshilash va umumiy dastur samaradorligini oshirish uchun muhimdir.
