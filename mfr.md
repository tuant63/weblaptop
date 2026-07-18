# PACER: Học `map`, `filter`, `reduce` trong JavaScript

Ba phương thức `map()`, `filter()`, `reduce()` là các **higher-order functions** (hàm bậc cao) của Array trong JavaScript. Chúng giúp xử lý dữ liệu theo hướng **khai báo (declarative)**: bạn mô tả *muốn lấy kết quả gì* thay vì viết từng bước bằng vòng lặp.

---

# 1. Procedural — Cú pháp, triển khai cơ bản + bài tập

## A. `map()` — Biến đổi từng phần tử

### Cú pháp

```javascript
const newArray = array.map((element, index, array) => {
    return newValue;
});
```

### Cách hoạt động cơ bản

* Duyệt qua từng phần tử.
* Chạy callback cho mỗi phần tử.
* Lấy giá trị `return` tạo thành **mảng mới**.
* Không thay đổi mảng gốc.

### Ví dụ

```javascript
const numbers = [1, 2, 3, 4];

const doubled = numbers.map(number => {
    return number * 2;
});

console.log(doubled);
```

Kết quả:

```javascript
[2, 4, 6, 8]
```

### Viết ngắn hơn

```javascript
const doubled = numbers.map(number => number * 2);
```

---

### Bài tập `map()`

Cho:

```javascript
const prices = [100, 200, 300];
```

Hãy dùng `map()` để tạo mảng mới tăng giá mỗi sản phẩm thêm 10%.

Kết quả mong muốn:

```javascript
[110, 220, 330]
```

---

---

# B. `filter()` — Lọc phần tử theo điều kiện

### Cú pháp

```javascript
const newArray = array.filter((element, index, array) => {
    return condition;
});
```

### Cách hoạt động

* Duyệt từng phần tử.
* Callback phải trả về `true` hoặc `false`.
* Nếu `true` → giữ lại phần tử.
* Nếu `false` → loại bỏ.

### Ví dụ

```javascript
const ages = [12, 18, 25, 10, 30];

const adults = ages.filter(age => {
    return age >= 18;
});

console.log(adults);
```

Kết quả:

```javascript
[18, 25, 30]
```

---

### Bài tập `filter()`

Cho:

```javascript
const products = [
    {name: "Laptop", price: 1000},
    {name: "Mouse", price: 20},
    {name: "Phone", price: 700}
];
```

Dùng `filter()` lấy sản phẩm giá trên 500.

Kết quả:

```javascript
[
 {name: "Laptop", price: 1000},
 {name: "Phone", price: 700}
]
```

---

---

# C. `reduce()` — Gom nhiều giá trị thành một kết quả

### Cú pháp

```javascript
const result = array.reduce((accumulator, currentValue) => {
    return newAccumulator;
}, initialValue);
```

Hai thành phần quan trọng:

* `accumulator`: giá trị đang được tích lũy.
* `currentValue`: phần tử hiện tại.

---

### Ví dụ tính tổng

```javascript
const numbers = [1, 2, 3, 4];

const total = numbers.reduce((sum, number) => {
    return sum + number;
}, 0);

console.log(total);
```

Luồng chạy:

```
Bước 1:
sum = 0
number = 1
=> 0 + 1 = 1

Bước 2:
sum = 1
number = 2
=> 1 + 2 = 3

Bước 3:
sum = 3
number = 3
=> 3 + 3 = 6

Bước 4:
sum = 6
number = 4
=> 6 + 4 = 10
```

Kết quả:

```javascript
10
```

---

### Bài tập `reduce()`

Cho:

```javascript
const cart = [
    {name: "Book", price: 10},
    {name: "Pen", price: 5},
    {name: "Bag", price: 30}
];
```

Dùng `reduce()` tính tổng tiền.

Kết quả:

```javascript
45
```

---

# 2. Conceptual — Logic vận hành và khác biệt bản chất

## Sơ đồ tư duy

```
                ARRAY
                  |
    --------------------------------
    |              |               |
   map          filter          reduce
    |              |               |
Biến đổi       Chọn lọc        Tổng hợp
    |              |               |
N phần tử      N phần tử       N → 1
    |              |               |
Array mới       Array mới      Một giá trị
```

---

## `map()` trả lời câu hỏi:

> "Tôi muốn biến mỗi phần tử thành cái gì?"

Ví dụ:

```
[1,2,3]

map(x => x * 10)

↓

[10,20,30]
```

Số lượng phần tử:

```
Input: 3 phần tử
Output: 3 phần tử
```

---

## `filter()` trả lời:

> "Phần tử nào được phép tồn tại?"

Ví dụ:

```
[1,2,3,4,5]

filter(x => x > 3)

↓

[4,5]
```

Số lượng phần tử:

```
Input: N
Output: 0 → N
```

---

## `reduce()` trả lời:

> "Làm sao gom tất cả thành một kết quả?"

Ví dụ:

```
[1,2,3,4]

reduce(sum)

↓

10
```

Số lượng:

```
Input: N
Output: 1
```

---

# 3. Analogous — Ví dụ đời thực

## `map()` giống như một nhà máy sản xuất

Bạn có:

```
🍎 🍎 🍎 🍎
```

Nhà máy:

> "Mỗi quả táo được rửa sạch"

Kết quả:

```
✨🍎 ✨🍎 ✨🍎 ✨🍎
```

Mọi phần tử đều được xử lý.

---

## `filter()` giống như bảo vệ ở cửa

Danh sách khách:

```
👨 👩 👨 👵 👦
```

Điều kiện:

> "Chỉ người trên 18 tuổi được vào"

Kết quả:

```
👨 👩 👨
```

Một số phần tử bị loại.

---

## `reduce()` giống như thu ngân

Các món hàng:

```
🍔 $5
🍕 $10
🥤 $3
```

Thu ngân cộng:

```
5 + 10 + 3
```

Kết quả:

```
$18
```

Nhiều thứ → một kết quả.

---

# 4. Evidence — Tình huống thực tế kết hợp cả 3 (chaining)

## Bài toán:

Một website bán hàng cần:

1. Lấy các sản phẩm còn hàng.
2. Tính giá sau giảm 20%.
3. Tính tổng doanh thu.

Dữ liệu:

```javascript
const products = [
    {
        name: "Laptop",
        price: 1000,
        stock: true
    },
    {
        name: "Phone",
        price: 800,
        stock: false
    },
    {
        name: "Tablet",
        price: 500,
        stock: true
    }
];
```

---

## Bước 1: `filter()` lấy hàng còn tồn

```javascript
const availableProducts = products.filter(
    product => product.stock
);
```

Kết quả:

```javascript
[
 Laptop,
 Tablet
]
```

---

## Bước 2: `map()` áp dụng giảm giá

```javascript
const discountedProducts = availableProducts.map(
    product => ({
        ...product,
        price: product.price * 0.8
    })
);
```

Kết quả:

```javascript
[
 {name:"Laptop", price:800},
 {name:"Tablet", price:400}
]
```

---

## Bước 3: `reduce()` tính tổng

```javascript
const total = discountedProducts.reduce(
    (sum, product) => sum + product.price,
    0
);

console.log(total);
```

Kết quả:

```
1200
```

---

## Viết chaining gọn:

```javascript
const total = products
    .filter(product => product.stock)
    .map(product => product.price * 0.8)
    .reduce((sum, price) => sum + price, 0);

console.log(total);
```

Luồng tư duy:

```
Danh sách sản phẩm
        |
        ↓
 FILTER
(còn hàng?)
        |
        ↓
 MAP
(giảm giá)
        |
        ↓
 REDUCE
(cộng tiền)
        |
        ↓
 Tổng doanh thu
```

---

# 5. Reference — Bảng flashcard

| Method     | Mục đích              | Callback nhận                               | Callback trả về | Giá trị trả về     |
| ---------- | --------------------- | ------------------------------------------- | --------------- | ------------------ |
| `map()`    | Biến đổi từng phần tử | `(element, index, array)`                   | Giá trị mới     | Array mới          |
| `filter()` | Lọc phần tử           | `(element, index, array)`                   | `true/false`    | Array mới          |
| `reduce()` | Gom dữ liệu           | `(accumulator, currentValue, index, array)` | accumulator mới | Một giá trị bất kỳ |

---

## Bảng tham số chi tiết

| Method         | Parameter        | Ý nghĩa                 |
| -------------- | ---------------- | ----------------------- |
| `element`      | Phần tử hiện tại | Giá trị đang xử lý      |
| `index`        | Vị trí phần tử   | Index hiện tại          |
| `array`        | Mảng gốc         | Toàn bộ array           |
| `accumulator`  | Giá trị tích lũy | Kết quả sau mỗi vòng    |
| `initialValue` | Giá trị ban đầu  | Điểm bắt đầu của reduce |

---

# Quy tắc nhớ nhanh PACER

```
MAP
→ Modify
→ Biến đổi

FILTER
→ Find
→ Tìm cái phù hợp

REDUCE
→ Roll up
→ Gom lại
```

Hoặc:

```
MAP     : 1 → 1
FILTER  : N → 0..N
REDUCE  : N → 1
```

Nếu bạn nắm được câu này, bạn gần như đã nắm bản chất:

> **map thay đổi dữ liệu, filter chọn dữ liệu, reduce kết hợp dữ liệu.**
