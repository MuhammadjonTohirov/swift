# Optional chaining

### Optional Chaining

#### Optional Chaining nima?

Optional chaining — bu optional (?) qiymatlar bilan ishlashda kutilmagan xatolarni oldini olish uchun mo’ljallangan usuldir. Bu usul yordamida optional qiymatlar ichidagi xususiyatlar, metodlar va subskriptlarga xavfsiz va qisqaroq shaklda kirish mumkin. Agar optional qiymat nil bo’lsa, barcha zanjir davomida qiymat nil bo’lib qoladi.

#### Optional Chaining Sintaksisi

Optional chaining ? operatori yordamida amalga oshiriladi. Mana oddiy misol:

```swift
class Person {
    var residence: Residence?
}

class Residence {
    var numberOfRooms = 1
}

let john = Person()
if let roomCount = john.residence?.numberOfRooms {
    print("John's residence has \(roomCount) room(s).")
} else {
    print("Unable to retrieve the number of rooms.")
}
```

Bu misolda john.residence nil bo’lgani uchun john.residence?.numberOfRooms ifodasi nil qiymat qaytaradi va “Unable to retrieve the number of rooms.” deb chop etiladi.

#### Nested Optional Chaining

Optional chaining zanjirli (nested) bo’lishi mumkin, ya’ni bir nechta optional xususiyatlar, metodlar yoki subskriptlar bilan ishlashda foydalanish mumkin:

```swift
class Address {
    var buildingName: String?
    var buildingNumber: String?
    var street: String?
    
    func buildingIdentifier() -> String? {
        if let buildingNumber = buildingNumber, let street = street {
            return "\(buildingNumber) \(street)"
        } else if buildingName != nil {
            return buildingName
        } else {
            return nil
        }
    }
}

class Residence {
    var address: Address?
}

class Person {
    var residence: Residence?
}

let john = Person()
if let buildingIdentifier = john.residence?.address?.buildingIdentifier() {
    print("John's building identifier is \(buildingIdentifier).")
} else {
    print("Unable to retrieve the building identifier.")
}
```

Bu yerda `john.residence?.address?.buildingIdentifier()` zanjirli optional chaining ishlatilgan. `nil` qiymat bo’lgani uchun ifoda nil qiymat qaytaradi va “`Unable to retrieve the building identifier`.” deb chop etiladi.

#### Optional Chaining Bilan Metodlarni Chaqarish

Optional chaining yordamida metodlarni chaqirish ham mumkin. Agar metod optional qiymat orqali chaqirilsa va nil bo’lsa, metod chaqirilmaydi:

```swift
class Person {
    var residence: Residence?
}

class Residence {
    var rooms: [Room] = []
    
    func printNumberOfRooms() {
        print("The number of rooms is \(rooms.count).")
    }
}

class Room {
    let name: String
    
    init(name: String) {
        self.name = name
    }
}

let john = Person()
john.residence?.printNumberOfRooms() // Bu metod chaqirilmaydi, chunki residence nil

john.residence = Residence()
john.residence?.printNumberOfRooms() // Bu metod chaqiriladi, chunki residence endi nil emas
```

Bu misolda `john.residence?.printNumberOfRooms()` birinchi chaqirilganda `nil` qiymat bo’lgani uchun metod chaqirilmaydi. Keyin `john.residence` obyektini Residence ga o’zgartirganda, metod chaqiriladi va “`The number of rooms is 0.`” deb chop etiladi.

#### Optional Chaining Bilan Subskriptlardan Foydalanish

Optional chaining yordamida subskriptlardan ham foydalanish mumkin:

```swift
class Person {
    var residence: Residence?
}

class Residence {
    var rooms: [Room] = []
    
    subscript(i: Int) -> Room? {
        get {
            if i >= 0 && i < rooms.count {
                return rooms[i]
            } else {
                return nil
            }
        }
        set {
            if i >= 0 && i < rooms.count, let newRoom = newValue {
                rooms[i] = newRoom
            }
        }
    }
}

class Room {
    let name: String
    
    init(name: String) {
        self.name = name
    }
}

let john = Person()
if let firstRoomName = john.residence?[0]?.name {
    print("The first room's name is \(firstRoomName).")
} else {
    print("Unable to retrieve the first room's name.")
}
```

Bu misolda `john.residence?[0]?.name` zanjirli subskript optional chaining orqali chaqiriladi. nil bo’lgani uchun “`Unable to retrieve the first room’s name.`” deb chop etiladi.

#### Xulosa

Optional chaining Swift tilida optional qiymatlar bilan xavfsiz ishlashni ta’minlaydi. U orqali optional qiymatlar ichidagi xususiyatlar, metodlar va subskriptlarga kirishda nil qiymatni qaytarib, kutilmagan xatolarning oldini olish mumkin. Bu kodni o’qish va tushunishni ham osonlashtiradi.
