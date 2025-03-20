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

<details>

<summary><code>.barrier</code> bayrog'i</summary>

`DispatchWorkItem` yaratishda, `.barrier` (to'siq) bayrog'i ham uning xulq-atvorini sozlash uchun ishlatilishi mumkin:

```swift
let workItem = DispatchWorkItem(flags: .barrier) {
    // Vazifa kodi shu yerda
}
```

### U nima qiladi

`.barrier` bayrog'i navbatdagi vazifalarni ikki guruhga bo'ladi - to'siqdan oldingi va to'siqdan keyingi. Bu bayroq bilan yaratilgan ish elementi:

* Navbatdagi undan oldin yuborilgan barcha vazifalar tugatilguncha kutadi
* Bajariladigan vaqtda, navbatdagi boshqa hech qanday vazifa bajarilmaydi
* Tugaganidan keyin, navbatdagi keyingi vazifalar bajarilishni boshlaydi

Bu bayroq konkurent (concurrent) navbatlarda resursga xavfsiz kirish uchun juda foydali, chunki u read-write yoki write-write konfliktlarining oldini olishga yordam beradi.

### Hayotiy misol bilan amaliy muammo

Tasavvur qiling, siz matnli hujjatlar ustida ishlaydigan dastur yaratyapsiz. Bu dastur konkurent (concurrent) tarzda ishlab, bir vaqtning o'zida ko'plab foydalanuvchilar qayta o'qishi mumkin, lekin faqat bir foydalanuvchi bir vaqtda hujjatni o'zgartirishi mumkin.

#### Muammo stsenariyi

Siz hujjat boshqarish tizimini ishlab chiqyapsiz, u quyidagi vazifalarni bajaradi:

* Bir vaqtning o'zida ko'plab foydalanuvchilar hujjatni o'qiy oladi
* Faqat bir foydalanuvchi bir vaqtda hujjatni tahrirlashi mumkin
* Tahrirlash tugagach, o'qish operatsiyalari yana davom etishi kerak

Bu muammo "ko'plab o'quvchilar, bitta yozuvchi" muammosi sifatida tanilgan. Oddiy mutexlar yoki qulflar bilan bu muammoni hal qilish mumkin, lekin GCD'ning `.barrier` bayrog'i bilan yanada samarali yechim taqdim etiladi.

#### `.barrier` bilan yechim

```swift
class DocumentManager {
    // Konkurent navbat yaratish
    private let documentQueue = DispatchQueue(label: "com.docapp.document", attributes: .concurrent)
    
    // Hujjat ma'lumotlari
    private var documentContent: String = ""
    
    // O'qish metodi - bir vaqtning o'zida ko'plab o'qish operatsiyalari mumkin
    func readDocument(completion: @escaping (String) -> Void) {
        documentQueue.async {
            // Ma'lumotlarni o'qish
            let content = self.documentContent
            completion(content)
        }
    }
    
    // Yozish metodi - faqat bir vaqtda bitta yozish operatsiyasi
    func writeDocument(newContent: String, completion: @escaping () -> Void) {
        // To'siq ish elementi yaratish
        let writeOperation = DispatchWorkItem(flags: .barrier) {
            // Hujjatni yangilash
            self.documentContent = newContent
            completion()
        }
        
        // To'siq ish elementini navbatda bajarish
        documentQueue.async(execute: writeOperation)
    }
    
    // Hujjatni yangilash metodi - mavjud matnni yangi matn bilan almashtiradi
    func updateDocument(replaceText: String, withText: String, completion: @escaping () -> Void) {
        // To'siq ish elementi yaratish
        let updateOperation = DispatchWorkItem(flags: .barrier) {
            // Hujjatni yangilash
            self.documentContent = self.documentContent.replacingOccurrences(of: replaceText, with: withText)
            completion()
        }
        
        // To'siq ish elementini navbatda bajarish
        documentQueue.async(execute: updateOperation)
    }
}
```

Ushbu misolda, `DocumentManager` sinfi hujjat operatsiyalarini boshqaradi:

1. `readDocument` metodi oddiy asinxron vazifa sifatida ishlaydi va bir vaqtning o'zida ko'plab o'qish operatsiyalari amalga oshirilishi mumkin.
2. `writeDocument` va `updateDocument` metodlari `.barrier` bayrog'i bilan ish elementlarini yaratadi:
   * Barcha oldingi vazifalar (o'qish yoki yozish) tugatilgunga qadar kutadi
   * Bajarilayotgan vaqtda, boshqa hech qanday vazifa bajarilmaydi
   * Tugagandan so'ng, keyingi vazifalar (o'qish yoki yozish) bajarilishni boshlaydi

#### Bu yondashuvning afzalliklari

1. **Thread xavfsizligi**: To'siq mexanizmi konkurent navbatda resursga xavfsiz kirishni ta'minlaydi, bu esa data race muammolari yoki ma'lumotlar buzilishini oldini oladi.
2. **Samaradorlik**: Bu yondashuv oddiy qulflardan ko'ra samaraliroq, chunki u o'qish operatsiyalarini bir vaqtning o'zida bajarilishiga imkon beradi, faqat yozish operatsiyalari to'siqni hosil qiladi.
3. **Moslashuvchanlik**: Siz endi konkurent va to'siq operatsiyalarni bir navbatda kombinatsiya qila olasiz, bu esa kod tuzilishini soddalashtirishga yordam beradi.

#### Qachon ishlatmaslik kerak

Quyidagi hollarda `.barrier` dan foydalanishdan qochish kerak:

* Serial (ketma-ket) navbatlarda - bunday navbatlarda har doim bitta vazifa bir vaqtda bajariladi, shuning uchun to'siqlar kerak emas
* Vazifalar bir-biriga bog'liq bo'lmagan resurslar bilan ishlashda - bu holda to'siqlar keraksiz kechikishlarga olib keladi
* Oddiy mutex yoki semafor yetarli bo'lgan holatlarda

### Muhim eslatma

`.barrier` bayrog'i faqat konkurent navbatlardagina to'liq ishlaydi va custom yaratilgan navbatlarda to'g'ri ishlaydi. Global navbatlar (masalan, `DispatchQueue.global()`) bilan to'siqlar kutiladigan tarzda ishlamasligi mumkin, chunki global navbatlar butun tizim uchun ulashilgan.

Amalda, to'g'ri natija olish uchun, to'siqlarni custom yaratilgan konkurent navbat bilan ishlatish tavsiya etiladi:

```swift
// To'g'ri:
let customQueue = DispatchQueue(label: "com.example.queue", attributes: .concurrent)
let barrierItem = DispatchWorkItem(flags: .barrier) { /* kod */ }
customQueue.async(execute: barrierItem)

// Noto'g'ri (kutiladigan natijani bermasligi mumkin):
let globalQueue = DispatchQueue.global()
let barrierItem = DispatchWorkItem(flags: .barrier) { /* kod */ }
globalQueue.async(execute: barrierItem)
```

`.barrier` bayrog'i juda kuchli vosita bo'lib, to'g'ri ishlatilganda konkurent dasturlashdagi muammolarni samarali hal qilishga yordam beradi.

</details>

<details>

<summary><code>.detached</code> bayrog'i</summary>

`DispatchWorkItem` yaratishda, `.detached` bayrog'i ham uning xulq-atvorini sozlash uchun ishlatilishi mumkin:

```swift
let workItem = DispatchWorkItem(flags: .detached) {
    // Vazifa kodi shu yerda
}
```

### U nima qiladi

`.detached` bayrog'i ish elementini tashqi kontekstlardan ajratadi. Bu bayroq bilan yaratilgan ish elementi:

* Uni yaratgan threaddan QoS (Xizmat sifati) ustuvorligini meros olmaydi
* Uni bajaradigan navbatdan QoS ustuvorligini meros olmaydi
* Umuman o'z "ajratilgan" xolatida ishlaydi

Bu bayroq vazifalar bilan bog'liq bo'lgan joriy thread va navbat kontekstlaridan butunlay mustaqil bo'lishini ta'minlash uchun ishlatiladi.

### Hayotiy misol bilan amaliy muammo

Tasavvur qiling, siz ishlab chiqayotgan dastur turli xil operatsiyalarni amalga oshirishi kerak, lekin ba'zi vazifalar hech qanday ustuvorlik bilan bog'liq bo'lmasligi kerak. Bu, ayniqsa, yuqori ustuvorlikdagi UI thread'dan ishga tushiriladigan ammo past ustuvorlikda ishlashi kerak bo'lgan orqa fon vazifasi uchun muhimdir.

#### Muammo stsenariyi

Mobil dasturda uzundan-uzun analitika ma'lumotlarini yig'ish va yuborish vazifasi mavjud. Bu operatsiya:

* Foydalanuvchining amallari to'g'risida ma'lumotlarni yig'adi
* Ilovada qanday xatoliklar yuz bergani to'g'risida ma'lumotlarni to'playdi
* Bu ma'lumotlarni saqlaydi va ularni server bilan sinxronlashadi

Bu vazifalar o'z tabiati jihatidan past ustuvorlikka ega bo'lishi kerak, chunki ular foydalanuvchi tajribasiga bevosita ta'sir qilmaydi. Ammo, mavjud QoS kontekstlarini meros qilish mexanizmi tufayli:

* Agar foydalanuvchi UI harakatlarining natijasi o'laroq analitika vazifasi ishga tushirilsa, u yuqori ustuvorlikni meros qilib olishi va tizim resurslarini noto'g'ri taqsimlashi mumkin
* Bu esa batareya sarfini oshirishi va boshqa muhim operatsiyalar uchun resurslarni cheklashi mumkin

#### `.detached` bilan yechim

```swift
class AnalyticsManager {
    // Past ustuvorlikdagi navbat
    private let analyticsQueue = DispatchQueue(label: "com.app.analytics", qos: .background)
    
    // Ma'lumotlar to'plami
    private var analyticsData: [String: Any] = [:]
    
    func trackEvent(_ eventName: String, parameters: [String: Any]? = nil) {
        // Bu vazifa har qanday threaddan, shu jumladan asosiy UI threaddan chaqirilishi mumkin
        
        // Ajratilgan ish elementi yaratish - bu ustuvorlikdan mustaqil ishlaydi
        let trackingWork = DispatchWorkItem(flags: .detached) {
            // Analitika ma'lumotlarini yig'ish
            self.collectEventData(eventName, parameters: parameters)
            
            // Agar kerak bo'lsa, ma'lumotlarni saqlash
            self.saveDataIfNeeded()
        }
        
        // Ish elementini past ustuvorlikli analitika navbatida bajarish
        analyticsQueue.async(execute: trackingWork)
    }
    
    func syncWithServer() {
        // Ajratilgan ish elementi yaratish
        let syncWork = DispatchWorkItem(flags: .detached) {
            // Ma'lumotlarni serverga yuborish
            self.uploadData()
            
            // Yuborilgan ma'lumotlarni tozalash
            self.clearSentData()
        }
        
        // Ish elementini past ustuvorlikli analitika navbatida bajarish
        analyticsQueue.async(execute: syncWork)
    }
    
    private func collectEventData(_ eventName: String, parameters: [String: Any]?) {
        // Ma'lumotlarni yig'ish logikasi...
    }
    
    private func saveDataIfNeeded() {
        // Ma'lumotlarni saqlash logikasi...
    }
    
    private func uploadData() {
        // Ma'lumotlarni yuborish logikasi...
    }
    
    private func clearSentData() {
        // Yuborilgan ma'lumotlarni tozalash...
    }
}
```

Ushbu misolda, foydalanuvchi asosiy thread (`.userInteractive` QoS bilan) orqali event qayd qilish funksiyasini chaqirishi mumkin. `.detached` bayrog'i ishlatilmasa, ish elementi `.userInteractive` ustuvorligini meros qilib olishi va keragidan ortiqcha resurs olishi mumkin. `.detached` bayrog'i bilan esa, ish elementi barcha tashqi kontekstlardan ajratiladi va navbat ustuvorligini (`.background`) ishlatadi.

#### Bu yondashuvning afzalliklari

1. **Resurs samaradorligi**: Analitika vazifalari har doim past ustuvorlikda ishlaydi, bu esa muhim UI operatsiyalari uchun ko'proq resurs qoldiradi.
2. **Batareya quvvatini tejash**: Past ustuvorlikdagi operatsiyalar kamroq protsessor quvvatini talab qiladi va batareyadan quvvat kamroq sarflanadi.
3. **Izchil xulq-atvor**: Analitika vazifalari har doim bir xil ustuvorlikda ishlaydi, bu esa ular qayerdan chaqirilishidan qat'i nazar dasturning oldindan aytib bo'ladigan xulq-atvorini ta'minlaydi.

#### Qachon ishlatmaslik kerak

Quyidagi hollarda `.detached` dan foydalanishdan qochish kerak:

* Vazifalar kontekst ustuvorligini saqlashi kerak bo'lganda (masalan, foydalanuvchi harakatlariga javob beruvchi muhim operatsiyalar)
* Vazifalar o'rtasida ustuvorlik munosabatlarini saqlash kerak bo'lganda
* Vazifalar bir-biriga bog'liq bo'lganda va bir xil ustuvorlikda ishlashi kerak bo'lganda

</details>

<details>

<summary><code>.inheritQoS</code> bayrog'i</summary>

`DispatchWorkItem` yaratishda, `.inheritQoS` bayrog'i ham uning xulq-atvorini sozlash uchun ishlatilishi mumkin bo'lgan muhim bayroqlardan biridir:

```swift
let workItem = DispatchWorkItem(flags: .inheritQoS) {
    // Vazifa kodi shu yerda
}
```

### U nima qiladi

`.inheritQoS` bayrog'i ish elementiga o'zini bajarayotgan navbatning QoS (Xizmat sifati) darajasini meros qilib olishga imkon beradi. Bu, vazifa ish qaysi navbatda bajarilsa, o'sha navbatning ustuvorlik darajasida bajarilishini ta'minlaydi.

### Hayotiy misol bilan amaliy muammo

Videoni qayta ishlash va eksport qilish ilovasini tasavvur qiling. Bu ilova turli xil QoS darajalariga ega bo'lgan bir nechta navbatlardan foydalanadi:

* Yuqori ustuvorlikdagi UI navbati
* O'rta ustuvorlikdagi qayta ishlash navbati
* Past ustuvorlikdagi eksport qilish navbati

#### Muammo stsenariyi

Tasavvur qiling, foydalanuvchilar bir necha video loyihalarni bir vaqtning o'zida qayta ishlashi va eksport qilishi mumkin. Har bir loyiha uchun vazifalarning ustuvorligi loyihaning ahamiyatiga qarab o'zgarishi kerak. Foydalanuvchi hozirda ishlaydigan loyiha eng muhim hisoblanadi, lekin orqa fondagi loyihalar ham ma'lum bir darajada ishlashda davom etishi kerak.

Bu hollarda, agar vazifalar o'z ustuvorliklarini saqlasalar, tizim resurslarini samarasiz taqsimlash va foydalanuvchi tajribasini yomonlashtirishi mumkin. Masalan:

* Faol loyiha bilan bir xil ustuvorlikda ishlaydigan orqa fondagi loyihalar foydalanuvchi interfeysi sezuvchanligini pasaytirishi mumkin
* Past ustuvorlikdagi eksport navbatida ishlayotgan faol loyiha sekinlashishi mumkin

#### `.inheritQoS` bilan yechim

```swift
class VideoProcessor {
    // Turli navbatlar
    private let uiQueue = DispatchQueue.main
    private let processingQueue = DispatchQueue(label: "com.videoapp.processing", qos: .userInitiated)
    private let exportQueue = DispatchQueue(label: "com.videoapp.export", qos: .utility)
    
    // Loyiha holati
    private var activeProject: VideoProject?
    private var backgroundProjects: [VideoProject] = []
    
    func processActiveProject(_ project: VideoProject) {
        activeProject = project
        
        // Asosiy qayta ishlash vazifasi
        let processingWork = DispatchWorkItem {
            // Qayta ishlash kodi...
            self.finalizeProcessing(project)
        }
        
        // Eksport vazifasi - ustuvorlikni meros qilib oladi
        // Ya'ni bu vazifa qaysi navbatda bajarilsa, o'sha navbatning ustuvorligida ishlaydi
        let exportWork = DispatchWorkItem(flags: .inheritQoS) {
            self.exportVideo(project)
        }
        
        // Qayta ishlash vazifasi userInitiated QoS bilan ishlaydi
        processingQueue.async(execute: processingWork)
        
        // Qayta ishlash tugatilgandan so'ng, eksport vazifasi utiliy QoS bilan ishlaydi
        exportQueue.async(execute: exportWork)
        
        // UI ni yangilash
        updateUI(for: project, status: "Processing...")
    }
    
    func processBackgroundProject(_ project: VideoProject) {
        backgroundProjects.append(project)
        
        // Orqa fon loyihasini past ustuvorlikda qayta ishlash
        let processingWork = DispatchWorkItem {
            // Qayta ishlash kodi...
            self.finalizeProcessing(project)
        }
        
        // Eksport vazifasi - ustuvorlikni meros qilib oladi
        let exportWork = DispatchWorkItem(flags: .inheritQoS) {
            self.exportVideo(project)
        }
        
        // Bu yerda, utility navbati ichida background navbati yaratiladi
        let backgroundQueue = DispatchQueue(label: "com.videoapp.background", qos: .background)
        
        // Orqa fon loyihasi background QoS bilan qayta ishlanadi
        backgroundQueue.async(execute: processingWork)
        
        // Eksport vazifasi background QoS bilan ishlaydi
        backgroundQueue.async(execute: exportWork)
        
        // UI ni yangilash
        updateUI(for: project, status: "Queued for background processing...")
    }
    
    private func finalizeProcessing(_ project: VideoProject) {
        // Qayta ishlashni tugatish...
    }
    
    private func exportVideo(_ project: VideoProject) {
        // Video eksport qilish...
    }
    
    private func updateUI(for project: VideoProject, status: String) {
        uiQueue.async {
            // UI ni yangilash kodi...
        }
    }
}

struct VideoProject {
    let name: String
    // Boshqa xususiyatlar...
}
```

#### Bu yondashuvning afzalliklari

1. **Navbat-ma'lum ustuvorlik**: `.inheritQoS` bayrog'i bilan, ish elementlari o'zlarini bajarayotgan navbatning ustuvorligini meros qilib oladi. Bu, faol loyiha yuqori ustuvorlikda ishlashini, orqa fon loyihalari esa tizim resurslarini ortiqcha ishlatmasdan orqa fonda ishlashini ta'minlaydi.
2. **Moslashuvchan resurs taqsimoti**: Tizim bir vaqtning o'zida bir nechta loyihalar ustida ishlaganda resurslarni to'g'ri taqsimlaydi, bunda faol loyihalar yuqori, orqa fon loyihalari esa past ustuvorlikda ishlaydi.
3. **Yaxshi foydalanuvchi tajribasi**: Foydalanuvchilar faol loyihalari tezroq qayta ishlanishini ko'radilar, orqa fon loyihalari esa UI sekinlashmasdan orqa fonda ishlaydi.

#### Qachon ishlatmaslik kerak

Quyidagi hollarda `.inheritQoS` dan foydalanishdan qochish kerak:

* Vazifa bajarilayotgan navbatdan qat'i nazar, muayyan ustuvorlikda ishlashi kerak bo'lganda
* Vazifalar boshqa kontekstlardan (masalan, turli xil foydalanuvchi harakatlaridan) kelib chiqqanda va o'z ustuvorliklarini saqlab qolishlari kerak bo'lganda (bu holda `.assignCurrentContext` yaxshiroq tanlov bo'lishi mumkin)

</details>

<details>

<summary><code>assignCurrentContext</code> bayrog'i</summary>

`.assignCurrentContext` bayrog'i ish elementiga navbatning o'z `QoS` sozlamasidan emas, balki ish elementini navbatga topshirgan threaddan kontekstni (`QoS` sinfi bilan birga) meros qilib olishga majbur qiladi.

### Hayotiy misol bilan amaliy muammo

Orqa fonda rasmlarni yuklashi kerak bo'lgan va shu bilan birga sezgir UI saqlaydigan foto almashish ilovasini ko'rib chiqaylik. Bu yerda `.assignCurrentContext` hal qilishga yordam beradigan muammo:

#### Muammo stsenariyi

Tasavvur qiling, ish elementlari ilovangizning bir qismida yaratiladi, lekin boshqasida bajariladi. Tegishli kontekst boshqaruvisiz:

* Yuqori ustuvorlikdagi UI vazifasi past ustuvorlikda bajariladigan ishni yaratishi mumkin
* Tizim turli navbatlar bo'ylab bog'liq operatsiyalarni to'g'ri kuzatib bora olmaydi
* Ilovangizning sezuvchanligi izchilsiz va oldindan aytib bo'lmaydigan bo'ladi

```swift
class PhotoUploader {
    private let uploadQueue = DispatchQueue(label: "com.photoapp.upload", qos: .utility)
    
    func uploadPhoto(_ photo: UIImage) {
        // Bu kod asosiy threadda ishlaydi (userInteractive QoS bilan)
        
        // Rasmni tayyorlash
        let compressedImage = compressImage(photo)
        
        // Joriy threadning kontekstini meros qilib oladigan ish elementi yaratish
        // (bu holda userInteractive) uploadQueue'ning utility QoS o'rniga
        let uploadWorkItem = DispatchWorkItem(flags: .assignCurrentContext) {
            self.performUpload(compressedImage)
        }
        
        // Vazifa utility navbatiga topshirilgan bo'lsa ham, 
        // userInteractive QoS bilan ishlaydi
        uploadQueue.async(execute: uploadWorkItem)
        
        // UI ni darhol yangilash
        updateUIForUploadInProgress()
    }
    
    private func performUpload(_ image: UIImage) {
        // Tarmoq orqali yuklash kodi
    }
    
    private func compressImage(_ image: UIImage) -> UIImage {
        // Rasmni siqish kodi
        return image
    }
    
    private func updateUIForUploadInProgress() {
        // UI ni yangilash kodi
    }
}
```

#### Bu yondashuvning afzalliklari

1. **Ustuvorlikni saqlash**: Yuklash operatsiyasi orqa fon navbatida bajarilsa ham, u asosiy threadning yuqori QoS darajasini saqlab qoladi, bu foydalanuvchi boshlaydigan yuklamalarning tegishli ustuvorlikka ega bo'lishini ta'minlaydi.
2. **Yaxshiroq foydalanuvchi tajribasi**: Muhim yuklamalar (to'g'ridan-to'g'ri foydalanuvchi harakati orqali boshlangan) avtomatlashtirilgan orqa fon yuklamalariga qaraganda tezroq yakunlanadi, bu esa ilovani yanada sezgir qiladi.
3. **Soddalashtirilgan ustuvorlik boshqaruvi**: Ilovangizning turli qismlari bo'ylab QoS darajalarini qo'lda kuzatib va tarqatib yurishingiz shart emas.

#### Qachon ishlatmaslik kerak

Quyidagi hollarda `.assignCurrentContext` dan foydalanishdan qochish kerak:

* Vazifalarning qaerda yaratilganidan qat'i nazar, navbatning QoS darajasida ishlashini aniq xohlaysiz
* Tizim resurslari uchun UI operatsiyalari bilan raqobat qilmasligi kerak bo'lgan uzoq muddatli orqa fon vazifalarini bajaryapsiz

Bu bayroq, ayniqsa, ish bir nechta kontekstlar orqali o'tadigan, lekin o'zining asl ustuvorligi va ahamiyatini saqlab qolishi kerak bo'lgan holatlarda foydalidir.

</details>

## DispatchWorkItem bayroqlari qiyosiy tahlilihoz

### Bayroqlar qisqacha tavsifi

1. **`.assignCurrentContext`** - Ish elementiga uni yaratgan thread kontekstini (QoS) meros qilib olishga majbur qiladi
2. **`.inheritQoS`** - Ish elementiga uni bajaradigan navbatning QoS darajasini meros qilib olishga imkon beradi
3. **`.detached`** - Ish elementini barcha tashqi kontekstlardan ajratadi, ustuvorlikni meros olmaydi
4. **`.barrier`** - Ish elementini to'siq vazifasi sifatida bajaradi, konkurent navbatlarda read-write operatsiyalarni boshqaradi

### Qiyosiy tahlil



| Bayroq                      | Asosiy maqsadi                              | Qachon ishlatish kerak                                                                                                                     | Qachon ishlatmaslik kerak                                              |
| --------------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------- |
| **`.barrier`**              | Resurslarga xavfsiz kirish                  | Konkurent navbatlarda resursga xavfsiz kirish kerak bo'lganda, ayniqsa "ko'plab o'quvchilar, bitta yozuvchi" senariylarda                  | Serial navbatlarda yoki global navbatlarda                             |
| **`.detached`**             | Hamma kontekstlardan mustaqil ishlash       | Vazifalar past ustuvorlikda ishlashi kerak bo'lganda, qayerdan chaqirilishidan qat'i nazar                                                 | Vazifalar kontekst yoki navbat ustuvorligini saqlashi kerak bo'lganda  |
| **`.inheritQoS`**           | Navbat ustuvorligini qabul qilish           | Vazifalar bajarilayotgan navbatning ustuvorligida ishlashi kerak bo'lganda                                                                 | Vazifalar o'z ustuvorligini saqlashi kerak bo'lganda                   |
| **`.assignCurrentContext`** | Yaratuvchi threadning ustuvorligini saqlash | Ustuvorlik vazifalarning yaratilish kontekstiga bog'liq bo'lishi kerak bo'lganda (masalan, foydalanuvchi harakati bilan bog'liq vazifalar) | Vazifalar bajarilayotgan navbat ustuvorligida ishlashi kerak bo'lganda |

Har bir bayroq o'ziga xos muammolarni hal qiladi va GCD'da multithreading bilan ishlashda qaysi vaziyat uchun qaysi bayroqni to'g'ri tanlashni bilish muhimdir. To'g'ri bayroqni to'g'ri vaziyatda qo'llash dasturingizning ishlashini va xavfsizligini sezilarli darajada yaxshilashi mumkin.
