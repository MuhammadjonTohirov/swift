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
