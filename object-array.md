# Học Object và Array trong JavaScript theo hệ thống PACER

Mục tiêu sau bài này:

* Biết tạo và thao tác với Object, Array.
* Hiểu bản chất của **kiểu dữ liệu tham chiếu (Reference Type)**.
* Dễ ghi nhớ bằng ví dụ thực tế.
* Tránh lỗi sao chép dữ liệu thường gặp.
* Có bảng tổng hợp để ôn tập nhanh.

---

# P — Procedural (Thực hành)

## 1. Object

### Tạo Object

```javascript
const user = {
  name: "Nam",
  age: 20,
  isStudent: true
};
```

Object gồm nhiều **thuộc tính (property)** dưới dạng:

```
key: value
```

---

### Truy cập dữ liệu

Có hai cách.

**Cách 1: Dấu chấm**

```javascript
console.log(user.name);
```

```
Nam
```

---

**Cách 2: Dấu ngoặc vuông**

```javascript
console.log(user["age"]);
```

```
20
```

Khi tên thuộc tính nằm trong biến:

```javascript
const key = "name";

console.log(user[key]);
```

---

### Thêm thuộc tính

```javascript
user.city = "Hà Nội";
```

Kết quả

```javascript
{
  name: "Nam",
  age:20,
  isStudent:true,
  city:"Hà Nội"
}
```

---

### Cập nhật

```javascript
user.age = 21;
```

---

### Xóa

```javascript
delete user.isStudent;
```

---

## 2. Array

### Tạo

```javascript
const fruits = ["Apple", "Banana", "Orange"];
```

---

### Truy cập

```javascript
console.log(fruits[0]);
```

```
Apple
```

```javascript
console.log(fruits[2]);
```

```
Orange
```

---

### Thêm cuối

```javascript
fruits.push("Mango");
```

```
["Apple","Banana","Orange","Mango"]
```

---

### Xóa cuối

```javascript
fruits.pop();
```

---

### Thêm đầu

```javascript
fruits.unshift("Grape");
```

---

### Xóa đầu

```javascript
fruits.shift();
```

---

### Sửa phần tử

```javascript
fruits[1] = "Kiwi";
```

---

## 3. Object chứa Array

```javascript
const student = {
    name: "Lan",
    scores: [8,9,10]
};

student.age = 20;

student.scores.push(7);

console.log(student);
```

Kết quả

```javascript
{
    name:"Lan",
    age:20,
    scores:[8,9,10,7]
}
```

---

## Quy tắc cần nhớ

Object

* dùng `{ }`
* lưu theo `key:value`
* truy cập bằng `.` hoặc `[]`

Array

* dùng `[ ]`
* lưu theo index
* index bắt đầu từ **0**

---

# A — Conceptual (Hiểu bản chất)

## JavaScript chia dữ liệu thành hai nhóm

### Primitive (kiểu nguyên thủy)

Ví dụ

```javascript
Number
String
Boolean
Null
Undefined
BigInt
Symbol
```

Primitive lưu **giá trị trực tiếp**.

Ví dụ

```javascript
let a = 10;
let b = a;

b = 20;
```

```
a = 10
b = 20
```

Hai biến hoàn toàn độc lập.

---

### Reference Type (kiểu tham chiếu)

Bao gồm

* Object
* Array
* Function

Chúng **không lưu dữ liệu trực tiếp**.

Biến chỉ lưu **địa chỉ trong bộ nhớ (reference)**.

Ví dụ

```javascript
const person = {
    name:"An"
};
```

Bộ nhớ

```
person
   │
   ▼
+----------------+
| name : "An"    |
+----------------+
```

`person` chỉ là một "mũi tên" trỏ tới vùng nhớ chứa object.

---

## Khi gán

```javascript
const a = {
    name:"Nam"
};

const b = a;
```

Sơ đồ

```
a ─────┐
       ▼
    +---------+
    | name    |
    | Nam     |
    +---------+
       ▲
b ─────┘
```

Không hề tạo object mới.

Hai biến cùng trỏ tới một object.

---

Nếu sửa

```javascript
b.name = "Lan";
```

thì

```javascript
console.log(a.name);
```

cũng thành

```
Lan
```

Đó là bản chất của **Reference Type**.

---

## Array cũng giống Object

```javascript
const arr1 = [1,2,3];

const arr2 = arr1;
```

```
arr1 ───┐
        ▼
   [1,2,3]
        ▲
arr2 ───┘
```

Thêm

```javascript
arr2.push(4);
```

Kết quả

```
arr1
```

cũng thành

```
[1,2,3,4]
```

---

## Sơ đồ tư duy

```
JavaScript

├── Primitive
│     ├── Number
│     ├── String
│     ├── Boolean
│     └── ...
│
└── Reference
      ├── Object
      ├── Array
      └── Function
```

Primitive

```
Variable
   │
   ▼
Value
```

Reference

```
Variable
   │
   ▼
Address
   │
   ▼
Object
```

Đây là điểm khác biệt quan trọng nhất của Object và Array.

---

# A — Analogous (So sánh thực tế)

## Object giống hồ sơ nhân viên

```
Nhân viên

Tên
Tuổi
Địa chỉ
SĐT
```

Trong JavaScript

```javascript
const employee = {
    name:"Nam",
    age:25,
    phone:"0123..."
};
```

Mỗi thuộc tính có một **tên**.

Giống từng ô trong biểu mẫu.

---

## Array giống hàng ghế trong rạp

```
Ghế

0
1
2
3
4
```

Không quan tâm tên ghế.

Chỉ quan tâm vị trí.

```javascript
const seats = [
    "Nam",
    "Lan",
    "Hoa"
];
```

Muốn lấy Hoa

```
seats[2]
```

---

## Object giống từ điển

```
"apple"
↓

"quả táo"
```

Tra bằng từ khóa.

```javascript
dictionary["apple"]
```

---

## Array giống danh sách mua sắm

```
1. Sữa

2. Trứng

3. Bánh
```

Muốn lấy món thứ ba

```
shoppingList[2]
```

---

## Reference giống chìa khóa nhà

Biến

```
person
```

không phải là căn nhà.

Nó chỉ là **chìa khóa**.

Có hai chìa khóa

```
a

b
```

đều mở cùng một căn nhà.

Sửa đồ trong nhà bằng chìa khóa nào thì căn nhà vẫn thay đổi.

Đó chính là

```
Reference
```

---

# E — Evidence (Ví dụ thực tế và lỗi thường gặp)

## Ví dụ 1: Danh sách sinh viên

```javascript
const classroom = [
    {
        id:1,
        name:"An",
        score:9
    },
    {
        id:2,
        name:"Lan",
        score:8
    }
];
```

Đây là cấu trúc rất phổ biến khi làm việc với API hoặc cơ sở dữ liệu.

Có thể hình dung:

```
Array

↓

Student

↓

Properties
```

```
Classroom

├── Student 1

│      ├── id

│      ├── name

│      └── score

│

└── Student 2
```

---

## Ví dụ 2: Giỏ hàng

```javascript
const cart = [
    {
        name:"Laptop",
        price:1500
    },
    {
        name:"Mouse",
        price:20
    }
];
```

Website thương mại điện tử thường lưu dữ liệu như vậy.

---

## Lỗi logic phổ biến nhất

Nhiều người nghĩ

```javascript
const user1 = {
    name:"Nam"
};

const user2 = user1;
```

là tạo bản sao.

Không đúng.

Hai biến đang dùng chung object.

Nếu

```javascript
user2.name = "Lan";
```

thì

```javascript
console.log(user1.name);
```

kết quả

```
Lan
```

---

### Cách sao chép đúng

#### Object

```javascript
const user2 = {
    ...user1
};
```

---

#### Hoặc

```javascript
const user2 = Object.assign({}, user1);
```

---

#### Array

```javascript
const arr2 = [...arr1];
```

---

Ví dụ

```javascript
const a = [1,2,3];

const b = [...a];

b.push(4);

console.log(a);
```

```
[1,2,3]
```

Không bị thay đổi.

---

### Lưu ý về sao chép nông (Shallow Copy)

Toán tử `...` và `Object.assign()` chỉ sao chép **một cấp**. Nếu Object hoặc Array chứa các Object/Array lồng nhau, chúng vẫn dùng chung tham chiếu ở các cấp bên trong.

```javascript
const original = {
  user: {
    name: "Nam"
  }
};

const copy = { ...original };

copy.user.name = "Lan";

console.log(original.user.name); // "Lan"
```

Khi cần bản sao hoàn toàn độc lập của cấu trúc lồng nhau, hãy dùng các kỹ thuật sao chép sâu (deep copy), chẳng hạn `structuredClone()` trong các môi trường JavaScript hiện đại.

---

# R — Reference (Flashcard)

| Phương thức   | Chức năng                              | Có thay đổi mảng gốc? |
| ------------- | -------------------------------------- | --------------------- |
| `push()`      | Thêm cuối                              | ✅ Có                  |
| `pop()`       | Xóa cuối                               | ✅ Có                  |
| `shift()`     | Xóa đầu                                | ✅ Có                  |
| `unshift()`   | Thêm đầu                               | ✅ Có                  |
| `splice()`    | Thêm/Xóa/Sửa tại vị trí bất kỳ         | ✅ Có                  |
| `slice()`     | Cắt và tạo mảng mới                    | ❌ Không               |
| `concat()`    | Ghép mảng                              | ❌ Không               |
| `join()`      | Chuyển mảng thành chuỗi                | ❌ Không               |
| `includes()`  | Kiểm tra phần tử tồn tại               | ❌ Không               |
| `indexOf()`   | Tìm vị trí đầu tiên                    | ❌ Không               |
| `find()`      | Trả về phần tử đầu tiên thỏa điều kiện | ❌ Không               |
| `findIndex()` | Trả về chỉ số đầu tiên thỏa điều kiện  | ❌ Không               |
| `filter()`    | Lọc phần tử                            | ❌ Không               |
| `map()`       | Biến đổi từng phần tử                  | ❌ Không               |
| `forEach()`   | Duyệt mảng                             | ❌ Không               |
| `reduce()`    | Gộp mảng thành một giá trị             | ❌ Không               |
| `some()`      | Có ít nhất một phần tử thỏa điều kiện? | ❌ Không               |
| `every()`     | Tất cả phần tử đều thỏa điều kiện?     | ❌ Không               |
| `sort()`      | Sắp xếp                                | ✅ Có                  |
| `reverse()`   | Đảo ngược                              | ✅ Có                  |
| `flat()`      | Làm phẳng mảng lồng nhau               | ❌ Không               |
| `flatMap()`   | `map()` rồi `flat()` một cấp           | ❌ Không               |

## Tóm tắt ghi nhớ

```
Object
├─ Dùng {}
├─ Dữ liệu theo key:value
├─ Truy cập bằng . hoặc []
└─ Thuộc kiểu Reference

Array
├─ Dùng []
├─ Dữ liệu theo index
├─ Index bắt đầu từ 0
├─ Thường dùng push(), pop(), map(), filter()
└─ Thuộc kiểu Reference

Reference Type
├─ Biến lưu địa chỉ bộ nhớ
├─ Gán = là sao chép tham chiếu
├─ Hai biến có thể cùng trỏ một Object/Array
└─ Dùng ... hoặc Object.assign() chỉ tạo shallow copy
```

> **Mấu chốt cần nhớ:** Object và Array đều là **Reference Type**. Khác biệt lớn nhất giữa chúng là **Object tổ chức dữ liệu theo cặp `key: value`**, còn **Array tổ chức dữ liệu theo chỉ số (index)**. Việc hiểu cách chúng lưu trữ theo tham chiếu sẽ giúp bạn tránh nhiều lỗi khi gán, truyền dữ liệu giữa các hàm và sao chép dữ liệu trong JavaScript.
