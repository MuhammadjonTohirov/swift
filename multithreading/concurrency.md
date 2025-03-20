# Concurrency

### Concurrency nima?

#### Concurrency

Bir vaqtning o’zida bir nechta vazifalarni boshqarish tushunchasidir. Bu vazifalar bir vaqtda bajarilishini anglatmaydi (bu parallelizm bo’lardi), aksincha, vazifalar boshlanishi, ishlashi va tugallanishi mumkin bo’lgan vaqt davrlarida bir-biriga o’xshash tarzda bajariladi.

#### Hayotiy misol

Band oshxonada oshpazlar samarali ishlashlari kerak. Bu yerda concurrency paydo bo’ladi. Bu bir nechta oshpazlar (thread’lar) vazifalarni o’z vaqtida va resurslarni optimal tarzda bajarishini ta’minlaydi, oshxona samarali va tez ishlashi uchun.

#### GCD yordamida concurrency’ni amalga oshirish

Quyida GCD yordamida concurrency’ni amalga oshirishga oid kod namunasi keltirilgan. Ushbu misolda biz bir nechta oshpazning turli vazifalarni bajarishini simulyatsiya qilamiz.

```swift
import Foundation

// Oshxona uchun seriyali navbat yaratish
let kitchenQueue = DispatchQueue(label: "com.example.kitchenQueue", attributes: .concurrent)
let group = DispatchGroup()

// Oshpazning vazifasini ta'riflash funksiyasi
func cookDish(dishName: String, duration: Int) {
    group.enter()
    kitchenQueue.async {
        print("Oshpaz \(dishName) ni pishirishni boshladi")
        sleep(UInt32(duration))
        print("Oshpaz \(dishName) ni tugatdi")
        group.leave()
    }
}

// Turli xil taomlar va ularni pishirish vaqtlari
let dishes = [("Salat", 2), ("Sho'rva", 3), ("Shashlik", 5), ("Palov", 4)]

// Har bir taom uchun oshpaz vazifasini bajarish
for dish in dishes {
    cookDish(dishName: dish.0, duration: dish.1)
}

// Asosiy oqimni kutish
group.notify(queue: .main) {
    print("Barcha vazifalar bajarildi")
}
```

```
Oshpaz Salat ni pishirishni boshladi 
Oshpaz Sho'rva ni pishirishni boshladi
Oshpaz Shashlik ni pishirishni boshladi
Oshpaz Palov ni pishirishni boshladi
Oshpaz Salat ni tugatdi
Oshpaz Sho'rva ni tugatdi
Oshpaz Palov ni tugatdi // palov shashlikdan oldin tugaydi sababi 4 sekund bo'ladi
Oshpaz Shashlik ni tugatdi
Barcha vazifalar bajarildi
```

### Izoh

Yuqoridagi Swift kod namunasida Grand Central Dispatch (GCD) yordamida concurrency amalga oshirilgan. Bu kod bir necha oshpazning bir vaqtning o'zida turli taomlarni tayyorlashini simulyatsiya qiladi. Kodda `kitchenQueue` deb nomlangan parallel navbat va `DispatchGroup` ishlatiladi. `cookDish` funksiyasi orqali har bir oshpazning vazifasi ta'riflanadi, u boshlanishi, ma'lum vaqt kutishi va tugallanishidan iborat. Dastur turli vaqtlarda pishiriladigan taomlar ro'yxatini oladi va har bir vazifa baxillanganida `group.notify` yordamida barcha vazifalar bitishi natijasi konsolda chiqariladi. Bu yondashuv bir vaqtda bajariladigan vazifalarni samarali ravishda boshqarish va sinxronlashtirishni ta'minlaydi.
