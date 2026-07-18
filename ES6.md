
# PACER: ES6+ JavaScript

---

# P — Procedural (Thực hành ngay)

Mục tiêu: **gõ code nhiều nhất có thể**.

## 1. let và const

ES5

```javascript
var name = "John";
```

ES6

```javascript
let age = 20;
const PI = 3.14;
```

Quy tắc

* `let` → thay đổi được
* `const` → không gán lại được

Thực hành

```javascript
let count = 0;

count++;

console.log(count);
```

---

## 2. Arrow Function

ES5

```javascript
function add(a, b) {
    return a + b;
}
```

ES6

```javascript
const add = (a, b) => {
    return a + b;
};
```

Viết ngắn

```javascript
const add = (a, b) => a + b;
```

Một tham số

```javascript
const square = x => x * x;
```

Không tham số

```javascript
const hello = () => "Hello";
```

---

## 3. Template Literal

ES5

```javascript
"Hello " + name
```

ES6

```javascript
`Hello ${name}`
```

Ví dụ

```javascript
const name = "Alice";

console.log(`Xin chào ${name}`);
```

---

## 4. Destructuring

### Object

```javascript
const user = {
    name: "An",
    age: 22
};

const { name, age } = user;
```

### Array

```javascript
const colors = ["red", "blue", "green"];

const [first, second] = colors;
```

---

## 5. Spread Operator (...)

Copy

```javascript
const arr1 = [1,2,3];

const arr2 = [...arr1];
```

Ghép

```javascript
const a = [1,2];
const b = [3,4];

const c = [...a, ...b];
```

Object

```javascript
const user = {
    name:"An"
};

const updated = {
    ...user,
    age:22
};
```

---

## 6. Rest Parameter

```javascript
const sum = (...numbers) => {
    return numbers.reduce((a,b)=>a+b);
};

sum(1,2,3,4);
```

---

## 7. Default Parameter

```javascript
const greet = (name="Guest") => {
    console.log(name);
};
```

---

## 8. Enhanced Object Literal

```javascript
const name = "An";
const age = 20;

const user = {
    name,
    age
};
```

---

## 9. Optional Chaining

```javascript
user.address?.city
```

Nếu `address` không tồn tại thì trả về `undefined`.

---

## 10. Nullish Coalescing

```javascript
const value = input ?? "Default";
```

Khác với

```javascript
input || "Default"
```

---

## 11. Promise

```javascript
fetch(url)
    .then(res => res.json())
    .then(data => console.log(data));
```

---

## 12. async / await

```javascript
async function load() {
    const res = await fetch(url);
    const data = await res.json();

    console.log(data);
}
```

---

# C — Conceptual (Hiểu bản chất)

Đây là phần quan trọng nhất.

## ES5

```
Biến
↓

Function

↓

Object

↓

Callback
```

Mọi thứ đều khá độc lập.

---

## ES6

```
let/const
      │
      │
Arrow Function
      │
      │
Template Literal
      │
      │
Destructuring
      │
      │
Spread / Rest
      │
      │
Promise
      │
      │
async/await
```

Tức là các tính năng được thiết kế để **phối hợp với nhau**.

Ví dụ

Arrow

↓

Truyền vào map()

↓

Destructuring kết quả

↓

Spread tạo object mới

↓

async lấy dữ liệu

↓

Template Literal hiển thị

Đây là phong cách viết JavaScript hiện đại.

---

## Tại sao cần let?

ES5

```
var

↓

Function Scope
```

ES6

```
let

↓

Block Scope
```

An toàn hơn vì biến chỉ sống trong `{}`.

---

## Tại sao Arrow Function?

Mục tiêu:

* Viết ít hơn
* Giữ `this`
* Thuận tiện cho callback

---

## Spread và Rest thực ra giống nhau

```
Spread

1
2
3

↓

...

↓

Mở ra
```

```
Rest

1
2
3

↓

...

↓

Gom lại
```

Một dấu `...`

Nhưng hai ý nghĩa.

---

## Promise và async

```
Callback

↓

Promise

↓

async/await
```

Chỉ là ba cách khác nhau để xử lý bất đồng bộ.

---

# A — Analogous (So sánh thực tế)

## Destructuring

Có một hộp

```
User

↓

Tên

Tuổi

Email
```

Thông thường

```
Mở hộp

↓

Lấy từng món
```

Destructuring

```
Đưa tay lấy đúng món cần
```

```javascript
const {name,email}=user;
```

---

## Spread

Giống như

```
Một bộ Lego

↓

Đổ toàn bộ ra bàn
```

```javascript
[...arr]
```

---

## Rest

Ngược lại

```
Các viên Lego

↓

Cho vào một hộp
```

```javascript
(...items)
```

---

## Promise

Giống như đặt đồ ăn.

```
Đặt món

↓

Chờ

↓

Có món
```

Không đứng trước quầy đợi.

---

## async/await

Giống như

```
Đặt món

↓

Ngồi

↓

Đợi

↓

Ăn
```

Code nhìn giống viết tuần tự.

---

# E — Evidence (Ví dụ thực tế)

Ví dụ xử lý danh sách sinh viên.

```javascript
const students = [
    {
        id:1,
        name:"Alice",
        score:9
    },
    {
        id:2,
        name:"Bob",
        score:7
    }
];

const excellent = students
    .filter(({score}) => score >= 8)
    .map(student => ({
        ...student,
        grade:"Excellent"
    }));

console.log(excellent);
```

Trong đoạn code trên sử dụng:

✅ Arrow Function

✅ Destructuring

✅ Spread

✅ Method chaining

✅ Object Literal

---

Ví dụ async

```javascript
const getUsers = async () => {
    const res = await fetch("/users");

    const users = await res.json();

    return users.map(({name}) => name);
};
```

Đã dùng:

* async
* await
* Arrow
* map()
* Destructuring

---

## Lỗi phổ biến

### 1. Nhầm Spread với Rest

Sai

```javascript
const [...a] = arr;
```

(Đây là cú pháp hợp lệ trong destructuring mảng nhưng nhiều người nhầm mục đích của `...`; nó đang **gom phần còn lại**, tức vai trò của **rest**, không phải trải dữ liệu.)

Đúng

```javascript
const copy = [...arr];
```

---

### 2. Dùng `||` thay cho `??`

Sai

```javascript
0 || 10
```

Kết quả

```
10
```

Trong khi

```javascript
0 ?? 10
```

Kết quả

```
0
```

`??` chỉ thay thế khi giá trị là `null` hoặc `undefined`.

---

### 3. Arrow Function và `this`

Sai

```javascript
const person = {
    name:"Tom",
    say: () => console.log(this.name)
};
```

Arrow function **không có `this` riêng**, nên không phù hợp làm phương thức đối tượng khi cần truy cập `this`.

Đúng

```javascript
const person = {
    name:"Tom",
    say() {
        console.log(this.name);
    }
};
```

---

### 4. const không làm object bất biến

```javascript
const user = {
    age:20
};

user.age = 21;
```

Điều này **hợp lệ** vì `const` chỉ ngăn việc gán lại biến `user`, không khóa nội dung của đối tượng.

---

### 5. Copy object bằng Spread chỉ là sao chép nông (shallow copy)

```javascript
const a = {
    profile: {
        age:20
    }
};

const b = {
    ...a
};

b.profile.age = 30;

console.log(a.profile.age);
```

Kết quả

```
30
```

Vì `profile` vẫn tham chiếu đến cùng một đối tượng.

---

# R — Reference (Flashcards)

## Phương thức mảng

| Phương thức    | Mô tả                                             |
| -------------- | ------------------------------------------------- |
| `map()`        | Biến đổi từng phần tử, trả về mảng mới            |
| `filter()`     | Lọc phần tử theo điều kiện                        |
| `find()`       | Trả về phần tử đầu tiên phù hợp                   |
| `findIndex()`  | Trả về chỉ số của phần tử đầu tiên phù hợp        |
| `some()`       | Có ít nhất một phần tử thỏa điều kiện?            |
| `every()`      | Tất cả phần tử đều thỏa điều kiện?                |
| `reduce()`     | Gộp mảng thành một giá trị                        |
| `forEach()`    | Duyệt mảng, không trả về mảng mới                 |
| `includes()`   | Kiểm tra giá trị có trong mảng                    |
| `flat()`       | Làm phẳng mảng lồng nhau                          |
| `flatMap()`    | `map()` rồi `flat()` một cấp                      |
| `sort()`       | Sắp xếp mảng (nên truyền hàm so sánh với số)      |
| `toSorted()`   | Trả về mảng đã sắp xếp mà không thay đổi mảng gốc |
| `toReversed()` | Trả về mảng đảo ngược mới                         |
| `toSpliced()`  | Phiên bản bất biến của `splice()`                 |
| `with()`       | Tạo mảng mới với một phần tử được thay thế        |

## Phương thức Object

| Phương thức            | Mô tả                                                         |
| ---------------------- | ------------------------------------------------------------- |
| `Object.keys()`        | Lấy danh sách khóa                                            |
| `Object.values()`      | Lấy danh sách giá trị                                         |
| `Object.entries()`     | Lấy cặp `[key, value]`                                        |
| `Object.fromEntries()` | Chuyển các cặp `[key, value]` thành object                    |
| `Object.assign()`      | Gộp hoặc sao chép object (shallow)                            |
| `Object.freeze()`      | Đóng băng object, không cho sửa                               |
| `Object.seal()`        | Không cho thêm/xóa thuộc tính, vẫn có thể sửa giá trị hiện có |
| `Object.hasOwn()`      | Kiểm tra object có trực tiếp sở hữu thuộc tính hay không      |
| `Object.groupBy()`     | Nhóm các phần tử theo khóa do hàm callback trả về (mới)       |

---

## Lộ trình ghi nhớ theo PACER

1. **Procedural:** Luyện gõ thành thạo `let/const`, Arrow Function, Template Literal, Destructuring, Spread/Rest, Default Parameter.
2. **Conceptual:** Hiểu sự chuyển dịch từ ES5 sang ES6+, đặc biệt là phạm vi biến, cú pháp hàm, tính bất biến và lập trình bất đồng bộ.
3. **Analogous:** Gắn mỗi tính năng với một hình ảnh quen thuộc (mở hộp, trải Lego, gom đồ, đặt món ăn...) để nhớ nhanh.
4. **Evidence:** Thường xuyên đọc và viết các ví dụ kết hợp nhiều tính năng trong cùng một bài toán thực tế.
5. **Reference:** Tạo flashcards cho các phương thức mảng và đối tượng, ôn theo lặp lại ngắt quãng (spaced repetition) để ghi nhớ lâu dài.
