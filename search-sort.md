# Search, Sort

### Search

Search array yoki istalgan collection tipidagi to'plam ichidan biror bir ma'lumotni qidirish

```swift
// Masalan quidagi joy nomlari ichidan Sam bilan boshlanadiganini 
// toping desa.

let names: [String] = [
    "Artel",
    "Humo Arena",
    "Mega Planet",
    "Samsung",
    "Apple"
]

let result = names.filter { name in
    name.starts(with: "Sam")
}

print(result)

// ["Samsung"]
```

```
bizda natija sifatida array qaytib olamiz va arrayda faqat Sam 
bilan boshlandaigan malumotlar bo'ladi
```

Etibor bergan bo'lsangiz. Biz yuqorida avval o'tmagan narsalardan ham foydalandik.\
Lekin buni shu paytga qadar o'rganganlarimiz yordamida ham qilsak bo'ladi

```swift
// Masalan

let names: [String] = [
    "Artel",
    "Humo Arena",
    "Mega Planet",
    "Samsung",
    "Apple"
]

var searchResult: [String] = []

for name in names {
    if name.starts(with: "Sam") {
        searchResult.append(name)
    }
}

print(searchResult)

// ["Samsung"]
```

Bir xil natija 2 xil yondashuv.

### Sort

Sort bu tartiblash bo'lib array yoki boshqa bir to'plamlarni ma'lumotlarini tartiblash demakdir

```swift
// Masalan
let array = [4, 5, 2, 1, 0, 3]
print(array.sorted())

// [0, 1, 2, 3, 4, 5] o'sish tartibida tartiblandi

print(array.sorted(by: >))

// [5, 4, 3, 2, 1, 0] kamayish tartibida tartiblandi
```

Boshqacha uslublari ham bor ulardan biri&#x20;

#### Selection sort

```swift
// Boshqacha uslubda sort qilish, masalan selection sort

func sort(array: [Int]) -> [Int] {
    var arr = array

    for i in 0..<arr.count {
        var x = i

        while x > 0 && arr[x] < arr[x - 1] {
            let t = arr[x - 1]
            arr[x - 1] = arr[x]
            arr[x] = t

            x -= 1
        }
    }
    
    return arr
}

print(sort(array: [4, 5, 2, 1, 0, 3]))

// [0, 1, 2, 3, 4, 5]
```

Bu yerda bo'layotgan xodisa quidagicha

```textile
for loop ni har bir qadamini tekshirganda
0chi va 1chi qadamlarda o'zgarishsiz, chunki boshida o'zgarishshart emas

0 [4, 5, 2, 1, 0, 3]
1 [4, 5, 2, 1, 0, 3]
2 [2, 4, 5, 1, 0, 3] - 2chi qadamga kelganda 2 raqami, 4 va 5 dan kichiklig uchun birinchi qatorga tushib qoldi.
3 [1, 2, 4, 5, 0, 3] - 3chi qadamda qadamda esa bir eng kichikligi uchun birinchi qatorga o'tib qoldi
4 [0, 1, 2, 4, 5, 3] - 4chi qadamda ham shunga o'xshash xolat faqat 0 bilan
5 [0, 1, 2, 3, 4, 5] - oxirgi qadamda 3 o'rtaga ya`ni ikkidan keyinga tushdi
```

Keling endi while loop ni ham ichini tekshirib koramiz

```textile
Bu yerda biz ketmaketlikni to'liqroq ko'rishimiz mumkin.

0 [4, 5, 2, 1, 0, 3]
1 [4, 5, 2, 1, 0, 3]
2 [4, 5, 2, 1, 0, 3]
    2 [4, 2, 5, 1, 0, 3] - 2 5 bilan o'rin almashdi
    2 [2, 4, 5, 1, 0, 3] - 2 4 bilan o'rin almashdi
3 [2, 4, 5, 1, 0, 3]
    3 [2, 4, 1, 5, 0, 3] - 1 5 bilan o'rin almashdi
    3 [2, 1, 4, 5, 0, 3] - 1 4 bilan o'rin almashdi
    3 [1, 2, 4, 5, 0, 3] - 1 2 bilan o'rin almashdi
4 [1, 2, 4, 5, 0, 3]
    4 [1, 2, 4, 0, 5, 3] - 0 5 bilan o'rin almashdi
    4 [1, 2, 0, 4, 5, 3] - 0 4 bilan o'rin almashdi
    4 [1, 0, 2, 4, 5, 3] - 0 2 bilan o'rin almashdi
    4 [0, 1, 2, 4, 5, 3] - 0 1 bilan o'rin almashdi
5 [0, 1, 2, 4, 5, 3]
    5 [0, 1, 2, 4, 3, 5] - 3 5 bilan o'rin almashdi
    5 [0, 1, 2, 3, 4, 5] - 3 4 bilan o'rin almashdi
    
va shu bilan tartiblash jarayoni tugatildi.

```
