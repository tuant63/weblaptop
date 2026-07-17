
# Biến dùng để tham chiếu đến một giá trị trong bộ nhớ

## 1. Primitive (lưu trực tiếp trong Stack)

```javascript
let a = 5;
let b = a;
```

### Stack

```
┌───────────────────────┐
│       STACK           │
├─────────┬─────────────┤
│   a     │      5      │
├─────────┼─────────────┤
│   b     │      5      │
└─────────┴─────────────┘
```

* `b = a` ⇒ **copy giá trị** từ `a`.
* Sau đó `a` và `b` là hai biến độc lập.

---

## 2. Object (lưu trên Heap)

```javascript
let a = { num: 5 };
let b = a;
```

### Stack + Heap

```
                STACK
┌─────────┬────────────────┐
│   a     │   0x1A2B3C ─────┐
├─────────┼────────────────┤│
│   b     │   0x1A2B3C ─────┘
└─────────┴────────────────┘
                    │
                    │
                    ▼
                HEAP
         ┌─────────────────┐
         │   { num: 5 }    │
         └─────────────────┘
```

* `a` chứa địa chỉ `0x1A2B3C`.
* `b = a` ⇒ **copy địa chỉ**, không copy object.
* Vì vậy cả `a` và `b` đều trỏ tới **cùng một object**.

---

## 3. Khi thay đổi object

```javascript
b.num = 10;
```

```
                STACK
┌─────────┬────────────────┐
│   a     │   0x1A2B3C ─────┐
├─────────┼────────────────┤│
│   b     │   0x1A2B3C ─────┘
└─────────┴────────────────┘
                    │
                    ▼
                HEAP
         ┌─────────────────┐
         │   { num: 10 }   │
         └─────────────────┘
```

Kết quả:

```javascript
console.log(a.num); // 10
console.log(b.num); // 10
```

Vì `a` và `b` cùng tham chiếu đến một object.

---

## 4. Khi gán object mới

```javascript
b = { num: 20 };
```

```
                STACK
┌─────────┬────────────────┐
│   a     │   0x1A2B3C ───┐ │
├─────────┼────────────────┤ │
│   b     │   0x8F7D91 ───┼─┘
└─────────┴────────────────┘
              │            │
              ▼            ▼
          HEAP          HEAP
   ┌──────────────┐  ┌──────────────┐
   │ { num: 10 }  │  │ { num: 20 }  │
   └──────────────┘  └──────────────┘
```

Lúc này:

* `a.num` → `10`
* `b.num` → `20`

Hai biến đã trỏ đến **hai object khác nhau**.

Đây là phiên bản **hệ thống hóa tinh gọn** về **Scope trong JavaScript** mà mình thường dùng khi học hoặc đi phỏng vấn.

---

# Scope trong JavaScript

> **Scope** = phạm vi mà một biến có thể được truy cập.

Có **3 loại Scope**.

```
Scope
│
├── Global Scope
├── Function Scope
└── Block Scope
```

---

# 1. Global Scope

Biến khai báo bên ngoài mọi function hoặc block.

```javascript
const a = 10;

function test() {
    console.log(a);
}

test(); // 10
```

```
Global
┌──────────────────────┐
│ a = 10               │
│ function test()      │
└──────────────────────┘
```

✅ Truy cập được ở mọi nơi (nếu không bị shadow).

---

# 2. Function Scope

Biến khai báo trong function.

```javascript
function test() {
    const a = 10;
}

console.log(a); // Error
```

```
Global
┌──────────────────┐
│ function test()  │
│  ┌────────────┐  │
│  │ a = 10     │  │
│  └────────────┘  │
└──────────────────┘
```

* Chỉ dùng được trong function.
* `var`, `let`, `const` đều tạo Function Scope khi khai báo trong function.

---

# 3. Block Scope

Block là cặp `{}`

```javascript
{
    let a = 10;
}

console.log(a); // Error
```

```
{
   let a = 10;
}
```

Chỉ **let** và **const** có Block Scope.

`var` thì **không**.

```javascript
{
    var a = 10;
}

console.log(a); // 10
```
# Hoisting trong JavaScript

Khi bạn chạy code, JavaScript sẽ tự động "cẩu" tất cả các khai báo (biến, hàm) lên trên cùng của file (hoặc trên cùng của hàm) trước khi nó thực sự đọc và chạy từng dòng code từ trên xuống dưới

# Ví dụ

## 1. Function Declaration

Function Declaration được hoisting **toàn bộ**, vì vậy có thể gọi trước khi khai báo.

```javascript
xinChao();

function xinChao() {
    console.log("Hello!");
}
```

**Kết quả**

```text
Hello!
```

### Điều gì thực sự xảy ra?

Trong quá trình hoisting, JavaScript đã đăng ký toàn bộ hàm trước khi thực thi.

```javascript
function xinChao() {
    console.log("Hello!");
}

xinChao();
```

---

## 2. Biến khai báo bằng `var`

Với `var`, JavaScript chỉ hoisting **tên biến** và khởi tạo giá trị mặc định là `undefined`.

```javascript
console.log(ten);

var ten = "JavaScript";
```

**Kết quả**

```text
undefined
```

### Điều gì thực sự xảy ra?

```javascript
var ten;          // undefined

console.log(ten);

ten = "JavaScript";
```

> `var` được hoisting cùng giá trị mặc định là `undefined`.

---

## 3. Biến khai báo bằng `let` và `const`

`let` và `const` cũng được hoisting, nhưng **không được khởi tạo ngay**.

```javascript
console.log(tuoi);

let tuoi = 20;
```

**Kết quả**

```text
ReferenceError
```

### Điều gì thực sự xảy ra?

JavaScript đã tạo biến `tuoi`, nhưng biến này đang nằm trong **Temporal Dead Zone (TDZ)**.

```text
Hoisted
    │
    ▼
───────────────
 Temporal Dead Zone
───────────────
    │
let tuoi = 20;
    │
    ▼
Có thể sử dụng
```

Trong khoảng thời gian này:

- Biến đã tồn tại.
- Nhưng chưa được khởi tạo.
- Mọi thao tác truy cập đều gây ra `ReferenceError`.
