# GCD

### GCD (Grand Central Dispatch) nima?

**GCD** iOS’da bir nechta vazifalarni samarali boshqarishga yordam beruvchi texnologiyadir. Bu thread’lar havzasini taqdim etadi va ushbu thread’lar bo’ylab vazifalarni rejalashtirishni boshqaradi, bu esa fon operatsiyalarini bosh thread’ni qulflamasdan bajarishni osonlashtiradi.

• **Queue**’lar: GCD vazifalarni boshqarish uchun queue’lardan foydalanadi. Ikkita asosiy tur mavjud:\
• **Serial Queue**: Vazifalarni bir vaqtda, tartib bilan bajaradi.\
• **Concurrent Queue**: Bir vaqtning o’zida bir nechta vazifalarni bajarishi mumkin.

#### Hayotiy misol

GCD’ni vazifalarni oshpazlarga tayinlaydigan oshxona menejeri deb tasavvur qiling:

• Menejer sabzavotlarni to’g’ralash, taomni pishirish yoki taomni bezash uchun qaysi oshpaz (thread)ni tanlashini hal qiladi.\
• Ikkita turdagi vazifalarni tayinlash (queue’lar) mavjud:\
• Serial Queue: Menejer vazifalarni bir oshpazga ketma-ketlikda beradi.\
• Concurrent Queue: Menejer vazifalarni bir nechta oshpazga bir vaqtda beradi, ularga parallel ishlashga imkon beradi.\


```swift
let queue = DispatchQueue.global(qos: .background)
queue.async {
    // Oshpaz 1 sabzavotlarni fon rejimida to'g'ralayapti
    print("Sabzavotlarni orqa fon rejimida to'g'ralayapti")
}
```

### QoS (Quality of Service) nima?

**QoS** xizmat sifati degan ma’noni anglatadi va u vazifalarning ustuvorligini belgilash imkonini beradi. Bu GCD’ga vazifalarning muhimligini aniqlash va resurslarni shunga mos ravishda boshqarishga yordam beradi.

• **Background**: Eng past ustuvorlik, sinxronizatsiya yoki zaxira kabi vazifalar uchun ishlatiladi.\
• **Utility**: O’rtacha ustuvorlik, tarmoqga kirish yoki fayl kirishi kabi vazifalar uchun ishlatiladi.\
• **User-Initiated**: Yuqori ustuvorlik, foydalanuvchi to’g’ridan-to’g’ri kutayotgan vazifalar uchun ishlatiladi.\
• **User-Interactive**: Eng yuqori ustuvorlik, foydalanuvchi interfeysiga ta’sir qiladigan vazifalar uchun ishlatiladi.

#### Hayotiy misol

Oshxonada menejer qaysi vazifalar eng muhim ekanini aniqlaydi. Mijoz kutayotgan taomni pishirish (User-Interactive) oshxonani tozalashdan (Background) ko’ra muhimroqdir.\
