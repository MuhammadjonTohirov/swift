# Init and Deinit

### Init

`init` metodi `class` yoki `struct`da yaratilganda avtomatik ravishda chaqiriladi va klass propertylari uchun boshlang'ich qiymatlarni belgilaydi. `init` metodi yordamida, klass propertylarini belgilash va ularga qiymat berish mumkin.

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

### Deinit

`deinit` metodi `class` yoki `actor` obyekti xotiradan o'chirilganida avtomatik ravishda chaqiriladi. `deinit`&#x20;

Quyidagi misolda `Person` nomli `class` yaratilgan, `deinit` funksiyasi avtomatik ravishda ishga tushadi:

```swift
class Person {
    var name: String
    
    init(name: String) {
        self.name = name
        print("\(name) is born.")
    }
    
    deinit {
        print("\(name) is gone.")
    }
}

var person1: Person? = Person(name: "John")
person1 = nil

// John is born.
// John is gone.
```

`Person` classidan `person1` nomli obyekt yaratilgan. Dastur o'tishini davom ettirganda esa `person1` ning qiymati `nil` ga tenglandi. Bu esa `person1` ni xotirasidan o'chirishiga olib keladi. `person1` ni qiymati `nil` ga tenglanishidan keyin avtomatik ravishda `deinit` funksiyasi chaqiriladi. Shu sababli, quyidagi chiqish bo'ladi:

> `struct` lar uchun esa, obyektlar avtomatik ravishda o'chirilishlari sababli `deinit` funksiyasini yaratishga to'g'ri kelmaydi, chunki struct obyektlari qiymatlarining qoldiq hisobidan to'xtamasligi sababli o'zgarishsiz yaratilishadi.
