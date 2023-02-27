# Property

`property` klasslarda o'zining o'ziga xos tushunchadir. `property`, klass obyektining xususiyatlari va qiymatlari sifatida ishlatiladi. Klassda, `var` va `let` kalitlari orqali `property` yaratish mumkin.

`var` kaliti yaratilgan `property` ni o'zgartirishga imkon beradi. Biroq, `let` kaliti yaratilgan `property` ni o'zgartirish mumkin emas, ya'ni u o'zgartirilmaydi.

Quyidagi misol `Person` klassida `name` va `age` nomli xususiyatlar (property) mavjud:

```swift
class Person {
  var name: String
  let age: Int
  
  init(name: String, age: Int) {
    self.name = name
    self.age = age
  }
}
```

> Bu kodda, `Person` nomli klassda `name` va `age` xususiyatlari (property) mavjud. `name` xususiyati `var` kaliti bilan aniqlangan va bu xususiyat obyekt yaratilgandan so'ng o'zgartirilishi mumkin. `age` xususiyati `let` kaliti bilan aniqlangan va bu xususiyat obyekt yaratilgandan so'ng o'zgartirilishi mumkin emas.

Quyidagi kodda, `Person` klassidan yangi bir obyekt yaratiladi va `name` va `age` xususiyatlari belgilanadi. So'ngra biz `name` va `age` tashqaridan qiymat berishga harakat qilamiz:

```swift
// Person obyektini yaratish
let person = Person(name: "John", age: 30)

// name ni o'zgartirish
person.name = "Mike"

// age ni o'zgartirish: iloji yo'q, chunki age let kaliti 
// bilan aniqlangan va o'zgartirish mumkin emas.
```

> Bu kodda, `person` obyekti yaratiladi va `name` va `age` xususiyatlari belgilanadi. `name` xususiyatiga qiymat o'zgartiriladi, shunday qilib `person` obyekti `Mike` nomi bilan yangilandi. Lekin `age` xususiyati `let` kaliti bilan aniqlangan va o'zgartirish mumkin emas.
>
> \
> `let` kaliti bilan aniqlangan xususiyatlarni o'zgartirish mumkin emas, chunki ular o'zgartirilmaydi. `age` o'zgaruvchan emas, chunki bu insonning yoshini aks ettiradi va yosh o'zgartirilmaydi.

### Setters and Getters

`get` va `set` methodlari, Swift dasturlash tili yordamida xususiyatlarga (`property`) yig'indisiga tegishli funksiyalar hisoblanadi.

> `get` methodi, xususiyatning o'qish (get) operatsiyasini bajaradi, ya'ni xususiyatning qiymatini qaytaradi. `set` methodi esa xususiyatning yozish (set) operatsiyasini bajaradi, ya'ni xususiyatning qiymatini o'zgartiradi.

Bunday qilingan sabab, bir nechta o'qish va yozish operatsiyalari bajarilgan bo'lishi mumkin va bu operatsiyalar o'zaro aloqali bo'lishi kerak.

Misol uchun quyidagi `Location` klassi `latitude` va `longitude` xususiyatlari va `coordinate` xususiyati bilan birlashtirilgan:

```swift
class Location {
    var latitude: Double
    var longitude: Double
    
    var coordinate: (Double, Double) {
        get {
            print("Get coordinate")
            return (latitude, longitude)
        }
        set(newCoordinate) {
            latitude = newCoordinate.0
            longitude = newCoordinate.1
        }
    }
    
    init(latitude: Double, longitude: Double) {
        self.latitude = latitude
        self.longitude = longitude
    }
}
```

`coordinate` xususiyati `get` va `set` methodlari bilan birlikda yaratilgan. `get` bloki `latitude` va `longitude` qiymatlaridan bir tuple qaytaradi, va `set` bloki tupleni qabul qiladi va uning birinchi va ikkinchi elementlarini mos ravishda `latitude` va `longitude`ga beradi.

Shunday qilib, quyidagi kodda `Location` obyektining `latitude` va `longitude` xususiyatlari orqali `coordinate` xususiyatiga murojaat qilish mumkin:

```swift
let location = Location(latitude: 40.7128, longitude: -74.0060)
print(location.coordinate) // Output: (40.7128, -74.006)
location.coordinate = (37.7749, -122.4194)
print(location.latitude) // Output: 37.7749
print(location.longitude) // Output: -122.4194
```
