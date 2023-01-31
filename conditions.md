---
description: (if va switch)
---

# Conditions

> Xolatni tekshirish uchun `if` va `switch` dan foydalaning

<details>

<summary>If, else</summary>

`if` agar xolat qanotlantirilsa ya'ni `true` bo'lsa scope ya'ni `{}` orasidagi amallarni bajaring `else` aks xolda `{}` orasidagi amallarni bajaring.

```swift
// Example

let grade = 75

if grade >= 75 { 
    print("You did pass with grade \(grade))
} else {
    print("You failed")
}

// output
// You did pass with grade 75

// Note: agar grade 75 dan kam bo'lganda edi natija
// You failed, bo'lar edi.
```

</details>

<details>

<summary>Switch, case</summary>

`Switch` `if` dan farqli o'laroq bir necha xolatni tekshirish qobiliyatiga ega.\
Boshqacha qlib aytganda `switch` bu ingliz tilidan almashtirish, o'zgartirish degani bo'lib bir necha almashinuvchi xolatlarga ya'ni `case`larga ega bo'ladi.&#x20;

Quidagi switch uchun namuna keltirilgan

```swift
// Example

let state = "two"

switch state {
case "one":
    print("You have just started the game")
case "two":
    print("Congrats now you can play in the second stage")
case "three":
    print("You almost there, keep up")
case "four":
    print("You won the game")
default:
    print("This is wrong input")
}
```

Switch orqali bir necha xolat bir xil natija keltirsa quidagi namuna orqali ko'rishingiz mumkin

```swift
// Example

let state = "one"

switch state {
case "zero", "one":
    print("You have just started the game")
case "two":
    print("Congrats now you can play in the second stage")
case "three":
    print("You almost there, keep up")
case "four":
    print("You won the game")
default:
    print("This is wrong input")
}

//Bu yerda agar state ning qiymati zero yoki one bo'lsa bir xil natija hosil bo'ladi 
```

`Switch` ning yana bir qulayliklardan dasturchi bir necha mantiqiy xolatni tekshira olishida&#x20;

```
// Example

let grade = 100

switch grade {
case let g where g <= 65:
    print("You failed")
case let g where g > 65 && g < 75 :
    print("Your degree is 3")
case let g where g >= 75 && g < 90:
    print("You got 4")
case let g where g >= 90:
    print("You got 5 for this exam")
default:
    print("Not valid grade")
}

// natija 
// You got 5 for this exam
```



</details>



