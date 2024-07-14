# Extensions

Extensions - bu mavjud sinflar(`class`), strukturalar(`struct`), enumeratsiyalar(`enum`) yoki protokollarga(`protocol`) yangi xususiyatlar va metodlar qo’shish imkonini beruvchi vosita. Bu, avvaldan aniqlangan turdagi kodni qayta yozmasdan, ularni kengaytirishga imkon beradi.

#### Extensions qayerda ishlatiladi?

Extensions asosan:

• Yangi hisoblangan xususiyatlar (computed properties) qo’shish\
• Yangi metodlar qo’shish\
• Yangi initsializatorlar qo’shish\
• Yangi subscripts qo’shish\
• Protokollarni amalga oshirish

#### Namuna

```swift
class Person {
    var name: String
    
    init(name: String) {
        self.name = name
    }
}

extension Person {
    func greet() {
        print("Hello, my name is \(name).")
    }
}

let person = Person(name: "Muhammad")
person.greet()  // Output: Hello, my name is Muhammad.
```

`Person` nomli `class` yaratildi va keyinroq `greet` degan method qoshish uchun qaytib `Person` klasiga qaytib uni tahrirlash shart emas (ba'zan shart bo'lishi m-n). Va biz `Person` klasidan `extension` (kengayish) olib unga yetishmayotgan method qo'shsak bo'ladi

Bundan tashqari `swift` tilida avvaldan belgilangan `data type` (ma'lumot turlari) Int, Float, String, Array, Dicitonary va hokazolar uchun ham `extension` yozsa bo'ladi. Sababi bular ham `struct` yoki bazilari `class` ham bo'lishi mumkin.

Masalan

```swift
extension Dictionary {
    func merged(with other: Dictionary) -> Dictionary {
        var mergedDict = self
        for (key, value) in other {
            mergedDict[key] = value
        }
        return mergedDict
    }
}

let dict1 = ["a": 1, "b": 2]
let dict2 = ["b": 3, "c": 4]
let mergedDict = dict1.merged(with: dict2)
print("Merged dictionary: \(mergedDict)")  
// Output: Merged dictionary: ["a": 1, "b": 3, "c": 4]
```

Quida `extension`ning yanada jozibaliroq tomonini ko'rishingiz mumkin

```swift
// student struct yaratamiz
struct Student {
    var id: Int
    var name: String
    var grade: Double
}
```

Endi esa `array` uchun extension yozamiz, faqat bu safar yozilgan extension faqat Student lar massivi uchun ishlaydi.

```swift
extension Array where Element == Student {
    func maxGradedStudent() -> Student? {
        return self.max(by: { $0.grade < $1.grade })
    }
}
// eng yuqori balga ega studentni topib beradi
```

Ko'rib turganingnizdek biz `where Element == T` uslubidan foydalandik.

```swift
// Talabalar ro'yxati
let students = [
    Student(id: 1, name: "Ali", grade: 85.5),
    Student(id: 2, name: "Vali", grade: 90.3),
    Student(id: 3, name: "Sami", grade: 78.4),
    Student(id: 4, name: "Doni", grade: 92.1),
    Student(id: 5, name: "Gani", grade: 88.7)
]

// Maksimal bahoga ega talabani topish
if let topStudent = students.maxGradedStudent() {
    print("Maksimal bahoga ega talaba: \(topStudent.name) (ID: \(topStudent.id)) - Bahosi: \(topStudent.grade)")
} else {
    print("Talabalar ro'yxati bo'sh.")
}
```

#### Nima uchun faqat Student massiv uchun ishlaydi?

Swift’da kengaytmalarni yaratishda biz ularni aniq turdagi ma’lumotlarga cheklashimiz mumkin. Bu orqali biz kengaytmaning faqat aniq turlarga tegishli bo’lishini ta’minlaymiz. Misolda biz Array kengaytmasini faqat Student turidagi elementlarga ega massivlar uchun ishlashini cheklashimiz kerak.

`extension Array where Element == Student`

Ushbu qator kengaytma faqat `Array` turidagi elementlar `Student` bo’lgan hollarda ishlashini bildiradi. Ya’ni, kengaytma faqat `Student` massivlarida ishlaydi, boshqa turlardagi massivlarda emas. Misol uchun, agar siz `Int` yoki `String` massivda `maxGradedStudent` metodini chaqirmoqchi bo’lsangiz, xato yuzaga keladi, chunki bu kengaytma faqat `Student` massivlari uchun mo’ljallangan.

#### Yana bir `Array where` uchun misol

Agar siz Int massiv uchun shunga o’xshash kengaytma yaratmoqchi bo’lsangiz, quyidagi kabi e’lon qilishingiz kerak bo’ladi:

```swift
extension Array where Element == Int {
    func maxElement() -> Int? {
        return self.max()
    }
}

let numbers = [1, 2, 3, 4, 5]
if let maxNumber = numbers.maxElement() {
    print("Maksimal raqam: \(maxNumber)")
}
```

Bunday `where element` uslubini nafaqat `array` balki `collection` tipiga mansub boshqa tiplar uchun ham ishlaydi, masalan `Dictionary`
