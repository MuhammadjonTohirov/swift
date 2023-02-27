# Init

`init` metodi klass yaratilganda avtomatik ravishda chaqiriladi va klass propertylari uchun boshlang'ich qiymatlarni belgilaydi. `init` metodi yordamida, klass propertylarini belgilash va ularga qiymat berish mumkin.

Quyidagi misol `Person` nomli bir klassda `init` funksiyasi mavjud:

```swift
// Person klassi

class Person {
  var name: String
  var age: Int
  
  init(name: String, age: Int) {
    self.name = name
    self.age = age
  }
}
```

> Bu kodda, `Person` nomli klassda `name` va `age` xususiyatlari mavjud va ular `init` funksiyasi orqali belgilanadi. `init` funksiyasi, `name` va `age` xususiyatlariga qiymatlar beradi. `self` kaliti, klass propertylariga murojat qilish uchun ishlatiladi.

Quyidagi kodda, `Person` klassidan bir obyekt yaratiladi va `init` funksiyasi chaqiriladi:

```swift
// Some code
let person = Person(name: "John", age: 30)

```

> Bu kodda, `name`ga "John" qiymati, `age` ga 30 qiymati berilgan va `Person` klassidan yangi bir obyekt yaratiladi. Obyekt yaratilganda, avtomatik ravishda `init` funksiyasi chaqiriladi va `name` va `age` xususiyatlariga belgilangan qiymatlar beriladi.
