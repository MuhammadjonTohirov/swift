---
description: Some methods about Int, double and float
---

# Extended Int, Float, Double

#### Int

<details>

<summary><code>distance(to:)</code> </summary>

2 ta integerni orasidagi farqni ko'rsatadi

```swift
// Masalan
let x = 5
let y = 10
let distance = x.distance(to: y) // farq 5
```

</details>

<details>

<summary><code>truncatingRemainder(dividingBy:)</code> </summary>

2 ta integerni bir biriga bo'lgandagi qoldiqni topadi

```swift
// Masalan
let x = 7
let y = 3
let remainder = x.truncatingRemainder(dividingBy: y) // qoldiq 1

// yoki
// x % y ham qoldiqni topadi

```

</details>

<details>

<summary><code>signum()</code> </summary>

Integerni 0 dan kichik teng yoki kattaligiga qarab -1, 0 yoki 1 ni qaytaradi

```swift
// Masalan
let x = 10
let sign = x.signum() // natija 1
```

</details>

<details>

<summary><code>isMultiple(of:)</code></summary>

&#x20;Bu orqali integer boshqa bir integerga qoldiqsiz bo'linishi yoki yoqligini bilsa bo'ladi.

```swift
// Masalan

let x = 10
let y = 5
let isMultiple = x.isMultiple(of: y) // natija true, ya'ni 10, 5 ga bo'linadi
// yoki 10, 5 ni ko'paytmalaridan iborat

```

</details>

#### Float va double

<details>

<summary><code>round()</code></summary>

Float you double sonini eng yaqin integerga yahlitlaydi

```
// Masalan
let i = 2.4
print(i.round())
// natija 2

let j = 2.7
print(j.round())
// natija 3
```

</details>

<details>

<summary><code>abs()</code></summary>

Biror sonni absolut qiymatini topish uchun foydalaniladi

```
// Masalan
let t = -4.1
print(abs(t))
// natija 4.1

```

</details>

#### Casting
