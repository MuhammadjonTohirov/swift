---
description: >-
  Swift'da open va public orasidagi farqlar open va public kirish nazorati
  darajalari Swift'ning kirish nazorati spektrida eng yuqori (eng ochiq)
  darajalar hisoblanadi. Ular bir-biriga o'xshash bo'lsa-d
---

# Open vs Public

### Umumiy o'xshashliklar

* Ikkalasi ham **boshqa modullardan ko'rinadi**
* Ikkalasi ham **public API** ni ta'minlash uchun ishlatiladi
* Ikkalasi ham elementlarga **external kirish imkonini** beradi

### Asosiy farqlar

#### `open` xususiyatlari

1. **Faqat klasslarga va ularning a'zolariga qo'llaniladi**
2. **Boshqa modullardan vorislik (subclassing) imkoniyati** - `open` class boshqa modullarda subclass qilib olinishi mumkin
3. **Boshqa modullardan override qilish imkoniyati** - `open` metodlar boshqa modullarda override qilinishi mumkin

#### `public` xususiyatlari

1. **Barcha turlarga qo'llanilishi mumkin** (class, struct, enum, protokol, va hokazo)
2. **Faqat o'z modulida vorislik olish** - `public` classdan faqat o'z modulida subclass yaratish mumkin, boshqa modulda emas
3. **Faqat o'z modulida override qilish** - `public` metodlarni faqat o'z modulida override qilish mumkin, boshqa modulda emas

### Amaliy misol

```swift
// MyFramework.swift modulida
open class Animal {
    open func makeSound() {
        print("Generic sound")
    }
    
    public func eat() {
        print("Eating")
    }
}

public class Vehicle {
    public func move() {
        print("Moving")
    }
}

// Foydalanuvchi dasturida (boshqa modul)
import MyFramework

// Animal - open class
class Dog: Animal {  // ✅ Ruxsat etilgan - chunki Animal open
    override func makeSound() {  // ✅ Ruxsat etilgan - chunki makeSound() open
        print("Woof!")
    }
    
    // Override qilib bo'lmaydi - chunki eat() metodi public, open emas
    // override func eat() { } // ❌ Ruxsat etilmagan
}

// Vehicle - public class
// class Car: Vehicle { } // ❌ Ruxsat etilmagan - chunki Vehicle open emas, faqat public
```

### Open va public'ni qachon ishlatish kerak?

#### `open`ni qo'llash:

1. **Kengaytirish uchun mo'ljallangan klasslar** - boshqalar tomonidan vorislik olinadigan va o'zgartirilishi kutilgan klasslar uchun
2. **Framework'lardagi asosiy klasslar** - boshqa dasturchilar tomonidan moslashtirilishi kerak bo'lgan klasslar
3. **Barqaror va yaxshi hujjatlashtirilgan API'lar** - chunki ular keng miqyosda ishlatiladi va o'zgartiriladi

#### `public`ni qo'llash:

1. **O'zgartirilmasdan foydalanish uchun mo'ljallangan tiplar** - struct, enum va class'lar uchun
2. **Vorislik olish kerak bo'lmagan API'lar uchun** - kengaytirish emas, balki faqat foydalanish uchun mo'ljallangan
3. **Protokollar, strukturalar va enum'lar uchun** - chunki ular uchun `open` mavjud emas

### Amaliy tavsiyalar

1. **Framework yozayotganda, standart sifatida `public`ni tanlang**, faqat kerak bo'lganda `open`ga o'ting
2. **`open`ni o'ylab ishlatish** - bu klassdan vorislik olish va uni o'zgartirish uchun ruxsat berish demakdir
3. **`open` va `public` metodlarini yaxshilab hujjatlash** - bu boshqa dasturchilar uchun juda muhim
4. **API dizayn evolyutsiyasini hisobga olish** - `open` qilingan API'ni kelajakda o'zgartirish qiyinroq

### Framework dizayni namunasi

```swift
// NetworkingKit framework
// Open - kengaytirish uchun mo'ljallangan
open class NetworkClient {
    // Open - o'zgartirilishi kerak bo'lishi mumkin
    open func sendRequest(_ request: URLRequest) {
        // Default implementatsiya
    }
    
    // Public - o'zgartirilmasligi kerak
    public func cancel() {
        // Requestni bekor qilish
    }
    
    // Public initializer
    public init() { }
}

// Public - faqat foydalanish uchun mo'ljallangan, vorislik olish emas
public struct NetworkResponse {
    public let data: Data?
    public let statusCode: Int
    
    public init(data: Data?, statusCode: Int) {
        self.data = data
        self.statusCode = statusCode
    }
}

// Public enum - faqat foydalanish uchun
public enum NetworkError: Error {
    case invalidURL
    case serverError(Int)
    case noConnection
}
```

### Qiyosiy jadval

| Xususiyat                             | open                                  | public                     |
| ------------------------------------- | ------------------------------------- | -------------------------- |
| Boshqa modullardan ko'rinadi          | ✅ Ha                                  | ✅ Ha                       |
| Qo'llanilishi                         | Faqat `class`lar va ularning a'zolari | Har qanday tur va a'zolari |
| Boshqa moduldan turib vorislik olish  | ✅ Ha                                  | ❌ Yo'q                     |
| Boshqa moduldan turib override qilish | ✅ Ha                                  | ❌ Yo'q                     |
| O'z modulidan turib vorislik olish    | ✅ Ha                                  | ✅ Ha                       |
| O'z modulidan turib override qilish   | ✅ Ha                                  | ✅ Ha                       |

### Xulosa

* **`open`** - bu Swift'dagi eng ochiq kirish darajasi bo'lib, u boshqa modullardan vorislik olish va override qilish imkonini beradi. Bu framework yaratishda muhim, chunki u boshqa dasturchilar sizning kodingizni kengaytirishi uchun yo'l ochadi.
* **`public`** - bu ham ochiq daraja, lekin u faqat o'z modulida vorislik olish va override qilish imkonini beradi. Bu ko'proq "foydalanish uchun", lekin "kengaytirish uchun emas" yondashuvini ta'minlaydi.

Kirish nazoratini to'g'ri tanlash API dizayni uchun juda muhim va u sizning kodingiz qanday foydalanilishi va kengaytirilishini boshqaradi.
