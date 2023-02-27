# Class and struct

## Class

A class is a blueprint for creating objects in programming. It defines the properties and methods that an object will have, and objects created from a class are stored in the computer's memory and can be shared between different parts of a program.

`Class` - bu dasturlashda ob'ektlarni yaratish rejasi. Class property va methodlardan iborat nomga ega bo'lgan kod bloki. Va `class` car yaratilinayotgan dasturning turli qisimlarida ishatishga mo'ljallangan.

{% hint style="info" %}
`property` - class yoki struct ga tegishli bo'lgan o'zgaruvchi yoki o'zgarmas kattalik

`method` - class yoki struct ga tegishli funksia
{% endhint %}

Quida `Person` deb nomlangan class yaratish tartibini ko'rishingiz mumkin

```swift
class Person {
    var name: String = "Abbos" // property -> name = Abbos
    var age: Int = 27 // property -> age = 27
    
    func sayHello() { // method sayHello
        print("Hello, my name is \(name)")
    }
}
```

`Person` degan class yaratilindi va unga `name`, `age` deb nomlangan propertylar va `sayHello` deb nomlangan method lar yozildi.

`init` - bu yerda `construct` (ya'ni quruvchi) vazifasidagi class yoki struct ichida keladigan `swift` dasturlash tiliga xos xususiyatidir.

{% hint style="info" %}
`constructor` - quruvchi, class yoki struct ning birorta obyekti yaratish jarayonida ishga tushadigan `method`.
{% endhint %}

```swift
// Person degan class dan foydalanish jarayoni

let person1 = Person()
print(person1)

// natija: __lldb_expr_1.Person
```

Yuqorida biz `person1` nomli `Person` clasiga tegishli obyekt yaratdik. `print` orqali `person1` ni konsolga chiqarmoqchi bo'lganimizda noaniq ma'lumot paydo bo'ldi. Bu qo'rqinchli emas va bu oddiygina classni swift uchun tushunarli ko'rinishi.

Xo'sh unda biz qanday qilib class (Person) ichidagi ma'lumot larni olib chiqamiz.

```swift
// Juda ham oddiy
print(person1.name)
// Abbos
```

Ko'rib turganingizdek, `class` obyektidan keyin `.` ni qo'yish orqali unga tegishli methodlarni yoki propertylarni ko'rish mumkin.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Get class properties and methods</p></figcaption></figure>

### init

`init` metodi klass yaratilganda avtomatik ravishda chaqiriladi va klass xususiyatlari uchun boshlang'ich qiymatlarni belgilaydi. `init` metodi yordamida, klass xususiyatlarini belgilash va ularga qiymat berish mumkin.

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

> Bu kodda, `Person` nomli klassda `name` va `age` xususiyatlari mavjud va ular `init` funksiyasi orqali belgilanadi. `init` funksiyasi, `name` va `age` xususiyatlariga qiymatlar beradi. `self` kaliti, klass xususiyatlariga murojat qilish uchun ishlatiladi.

```
// Some code
let person = Person(name: "John", age: 30)

```
