
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


# Phần 1: Câu lệnh điều kiện

## 1. Procedural (Thực hành ngay)

### `if`
```javascript
if (condition) {
  // code chạy khi condition là true
}
```

### `if...else`
```javascript
if (condition) {
  // true
} else {
  // false
}
```

### `if...else if...else`
```javascript
if (condition1) {
  // ...
} else if (condition2) {
  // ...
} else {
  // ...
}
```

### Toán tử ba ngôi (Ternary)
```javascript
const result = condition ? valueIfTrue : valueIfFalse;
```

### `switch`
```javascript
switch (value) {
  case "A":
    // ...
    break;
  case "B":
    // ...
    break;
  default:
    // ...
}
```

---

## 2. Conceptual (Hiểu bản chất)

**Ý tưởng cốt lõi:**

> Chương trình đánh giá một **biểu thức điều kiện** → nhận kết quả **true** hoặc **false** → quyết định nhánh nào sẽ được thực thi.

### Mối quan hệ giữa các lệnh rẽ nhánh đơn giản:
```
Điều kiện
     │
     ▼
 true / false
     │
 ┌───┴────┐
 ▼        ▼
if      else
```

### Khi có nhiều khả năng xảy ra (chuỗi điều kiện):
```
if
 │
 ├── true → chạy và thoát
 │
 └── false
      │
  else if
      │
  true/false
      │
     ...
      │
    else
```

### Tóm tắt vai trò:
* **`if`**: Kiểm tra một điều kiện duy nhất.
* **`else`**: Xử lý mọi trường hợp còn lại khi các điều kiện trước đó đều sai.
* **`else if`**: Kiểm tra thêm một điều kiện mới khi điều kiện trước đó sai.
* **`switch`**: So sánh chính xác **một giá trị** với nhiều trường hợp cụ thể (`case`) một cách trực quan.
* **Ternary (`? :`)**: Phiên bản viết tắt ngắn gọn của `if...else` khi cần gán hoặc trả về giá trị nhanh.

---

## 3. Analogous (Ví dụ tương tự)

### Ví dụ: Cổng kiểm soát vé ở rạp chiếu phim
* Nếu có vé → được vào xem.
* Nếu không có vé → bị từ chối truy cập.

```javascript
if (hasTicket) {
    enterCinema();
} else {
    denyAccess();
}
```

### Phân chia nhiều mức giá vé theo độ tuổi:
* Dưới 13 tuổi → Vé trẻ em
* Từ 13 đến 59 tuổi → Vé người lớn
* Từ 60 tuổi trở lên → Vé người cao tuổi

> Luồng tư duy này tương ứng hoàn toàn với chuỗi câu lệnh `if...else if...else`.

---

## 4. Evidence (Lỗi logic thường gặp)

### ❌ Trường hợp viết sai logic
```javascript
let score = 85;

if (score >= 50) {
    console.log("Đậu");
} else if (score >= 80) {
    console.log("Giỏi");
}
```
* **Kết quả in ra:** `Đậu`
* **Vấn đề:** Do điều kiện rộng hơn (`score >= 50`) được đặt lên trước, nó đã thỏa mãn ngay lập tức khiến JavaScript bỏ qua toàn bộ nhánh `else if` phía sau, dẫn đến việc đánh giá sai danh hiệu.

### ✅ Trường hợp viết đúng logic
```javascript
if (score >= 80) {
    console.log("Giỏi");
} else if (score >= 50) {
    console.log("Đậu");
}
```
* **Nguyên tắc cốt lõi:** Trong chuỗi `if...else if`, hãy đặt **điều kiện cụ thể hơn hoặc phạm vi hẹp hơn lên trước**, sau đó mới đến các điều kiện tổng quát/rộng hơn.

---

## 5. Reference (Tham khảo nhanh)

### So sánh các loại câu lệnh điều kiện
| Câu lệnh | Khi nào dùng |
| --- | --- |
| `if` | Kiểm tra một điều kiện đơn lẻ |
| `if...else` | Chọn thực thi một trong hai nhánh duy nhất |
| `if...else if...else` | Nhiều điều kiện phức tạp loại trừ lẫn nhau |
| `switch` | So sánh một giá trị cụ thể với nhiều trường hợp bằng nhau (`===`) |
| **Ternary `? :`** | Gán hoặc trả về giá trị nhanh dựa trên điều kiện đúng/sai |

### Các lưu ý quan trọng
| Vấn đề | Cách xử lý |
| --- | --- |
| **Thứ tự điều kiện sai** | Đặt điều kiện hẹp/cụ thể lên trước, điều kiện rộng/tổng quát ra sau. |
| **Quên `break` trong `switch`** | Dẫn đến hiện tượng "rơi tự do" xuống các case tiếp theo. Cần kiểm tra kỹ từ khóa `break` ở cuối mỗi case. |
| **Truthy / Falsy** | Ngoài kiểu dữ liệu `true/false` thuần túy, JavaScript coi một số giá trị là **Truthy** (ép kiểu thành true như `"hello"`, `1`) hoặc **Falsy** (ép kiểu thành false như `""`, `0`, `null`, `undefined`, `NaN`). |

---

# Phần 2: Vòng lặp

## 1. Procedural (Thực hành ngay)

### Vòng lặp `for`
*Dùng khi biết trước số lần lặp cụ thể.*
```javascript
for (khởi_tạo; điều_kiện; cập_nhật) {
  // Khối code thực thi
}

// Ví dụ:
for (let i = 0; i < 3; i++) {
  console.log(i); // Kết quả: 0, 1, 2
}
```

### Vòng lặp `while`
*Dùng khi không biết trước số lần lặp, chỉ biết vòng lặp sẽ chạy tiếp khi điều kiện còn đúng.*
```javascript
while (điều_kiện) {
  // Khối code thực thi
}

// Ví dụ:
let j = 0;
while (j < 3) {
  console.log(j); // Kết quả: 0, 1, 2
  j++;
}
```

### Vòng lặp `for...of`
*Dùng để duyệt qua lần lượt từng giá trị của một đối tượng có thể lặp (như mảng - Array).*
```javascript
for (const phần_tử of mảng) {
  // Khối code thực thi
}

// Ví dụ:
const colors = ["Đỏ", "Xanh", "Vàng"];
for (const color of colors) {
  console.log(color); // Kết quả: Đỏ, Xanh, Vàng
}
```

---

## 2. Conceptual (Hiểu bản chất)

**Ý tưởng cốt lõi:**
> Vòng lặp giúp thực thi một khối code lặp đi lặp lại **nhiều lần** dựa trên một **điều kiện**. Khi điều kiện trả về giá trị sai (false), vòng lặp sẽ dừng lại ngay lập tức.

### Sơ đồ vận hành của vòng lặp `for`:
```
[Khởi tạo]
     │
     ▼
┌───[Kiểm tra Điều kiện]─── false ──→ Thoát vòng lặp
│         │ true
│         ▼
│   [Thực thi Code]
│         │
│         ▼
│   [Cập nhật biến đếm]
│         │
└─────────┘
```

### Ba "trạm kiểm soát" cốt lõi:
1.  **Khởi tạo:** Thiết lập biến đếm ban đầu, chỉ chạy **duy nhất một lần** khi bắt đầu vòng lặp.
2.  **Kiểm tra điều kiện:** Thực hiện trước mỗi vòng lặp. Nếu **true** → chạy tiếp; nếu **false** → dừng lại và thoát.
3.  **Cập nhật:** Chạy ngay sau khi khối code trong vòng lặp hoàn thành để chuẩn bị cho lần kiểm tra điều kiện tiếp theo.

> ⚠️ **Cảnh báo cực kỳ quan trọng:** Nếu khâu **Cập nhật** bị lỗi hoặc bị quên (ví dụ: quên tăng biến đếm), điều kiện kiểm tra sẽ luôn luôn đúng. Điều này dẫn tới hiện tượng **Vòng lặp vô hạn (Infinite Loop)**, gây tràn bộ nhớ và treo ứng dụng.

### Sự khác biệt cốt lõi:
*   **`for`**: Gộp cả 3 thành phần (khởi tạo, điều kiện, cập nhật) nằm gọn trên một dòng. Thích hợp cho số lần lặp *xác định rõ ràng*.
*   **`while`**: Chỉ chứa duy nhất **điều kiện**. Việc khởi tạo phải làm bên ngoài và cập nhật phải viết bên trong thân vòng lặp. Thích hợp cho số lần lặp *không xác định trước*.
*   **`for...of`**: Tự động duyệt qua từng phần tử từ đầu đến cuối mà không cần quản lý biến đếm hay chỉ số index, giúp code sạch và an toàn hơn.

---

## 3. Analogous (Ví dụ tương tự)

### Ví dụ 1: Người kiểm kê hàng trong kho (`for` loop)
*   **Nhiệm vụ:** Kiểm tra lần lượt từng thùng hàng trên một kệ có cố định **10 thùng**.
*   **Khởi tạo:** Bạn đứng trước thùng số **1**.
*   **Điều kiện:** Còn đứng ở vị trí thùng nhỏ hơn hoặc bằng **10** thì còn làm việc.
*   **Cập nhật:** Sau khi kiểm xong một thùng, bạn **bước sang** vị trí thùng kế tiếp liền kề.
> Quy trình này giống như vòng lặp `for`, bạn biết chính xác điểm đầu, điểm cuối và số bước đi.

### Ví dụ 2: Xúc cát ra khỏi hố (`while` loop)
*   Bạn không thể biết trước cái hố đó sâu bao nhiêu xẻng cát. Quy tắc duy nhất là:
*   **Điều kiện:** "Khi nào **trong hố vẫn còn cát** thì vẫn tiếp tục xúc."
*   Cứ sau mỗi xẻng cát, bạn lại nhìn vào hố để kiểm tra điều kiện. Khi hố sạch cát, bạn dừng lại.
> Quy trình này giống như vòng lặp `while`, hành động lặp lại dựa hoàn toàn vào tình trạng thực tế chứ không theo số lần định sẵn.

---

## 4. Evidence (Ví dụ thực tế)

**Bài toán:** Bạn có một giỏ hàng (mảng gồm các đối tượng), cần duyệt qua để tính tổng tiền và in ra hóa đơn chi tiết cho khách hàng.

```javascript
const cart = [
  { name: "Áo thun", price: 150000 },
  { name: "Quần jeans", price: 500000 },
  { name: "Mũ lưỡi trai", price: 100000 }
];

let total = 0;

console.log("--- HÓA ĐƠN ---");
for (const item of cart) {
  console.log(`- ${item.name}: ${item.price.toLocaleString()}đ`);
  total += item.price; // Cộng dồn giá trị vào biến tổng tiền
}
console.log("----------------");
console.log(`Tổng cộng: ${total.toLocaleString()}đ`);
```

**Kết quả hiển thị trên Console:**
```text
--- HÓA ĐƠN ---
- Áo thun: 150.000đ
- Quần jeans: 500.000đ
- Mũ lưỡi trai: 100.000đ
----------------
Tổng cộng: 750.000đ
```
> 💡 **Mẹo nhỏ:** Hãy cẩn thận khi sử dụng từ khóa `return` ở bên trong vòng lặp. Nếu vòng lặp đó nằm trong một hàm, `return` sẽ ngay lập tức kết thúc và thoát ra khỏi **toàn bộ hàm chứa nó**, chứ không chỉ dừng lại riêng vòng lặp đó.

---

# Phần 3: Hàm

## 1. Procedural (Thực hành ngay)



### Khai báo hàm (Function Declaration)
*Cách khai báo truyền thống. Có thể gọi hàm trước khi khai báo nhờ cơ chế Hoisting.*
```javascript
// Khai báo
function tinhTong(a, b) {
    return a + b;
}

// Gọi hàm
let ket_qua = tinhTong(5, 3);
console.log(ket_qua); // Output: 8
```

### Biểu thức hàm (Function Expression)
*Gán hàm cho một biến. Không thể gọi hàm trước khi khai báo.*
```javascript
// Khai báo
const tinhHieu = function(a, b) {
    return a - b;
};

// Gọi hàm
console.log(tinhHieu(10, 4)); // Output: 6
```

### Hàm mũi tên (Arrow Function)
*Cú pháp ngắn gọn, không có `this` riêng.*
```javascript
// Khai báo
const tinhTich = (a, b) => {
    return a * b;
};

// Dạng rút gọn khi chỉ có 1 dòng return
const tinhThuong = (a, b) => a / b;

// Gọi hàm
console.log(tinhTich(4, 5));   // Output: 20
console.log(tinhThuong(8, 2)); // Output: 4
```

---

## 2. Conceptual (Hiểu bản chất)

### Logic vận hành: Input → Process → Output
Hàm giống như một cỗ máy. Bạn đưa nguyên liệu vào (Input/Tham số), máy xử lý (Process/Khối lệnh) và trả ra thành phẩm (Output/`return`). Nếu không có `return`, hàm mặc định trả về `undefined`.

```
Input (Tham số)
     │
     ▼
Process (Khối lệnh trong thân hàm)
     │
     ▼
Output (return / undefined)
```

### Tham số (Parameter) vs Đối số (Argument)
- **Tham số:** Là biến được liệt kê trong dấu ngoặc tròn khi *định nghĩa* hàm.
  - Ví dụ: `a`, `b` trong `function(a, b)`
- **Đối số:** Là giá trị thực tế bạn truyền vào khi *gọi* hàm.
  - Ví dụ: `5`, `3` trong `tinhTong(5, 3)`

### Phạm vi (Scope)
| Loại biến | Nơi khai báo | Phạm vi truy cập |
|:---|:---|:---|
| **Biến toàn cục** | Ngoài hàm | Dùng được ở mọi nơi |
| **Biến cục bộ** | Trong hàm | Chỉ dùng được bên trong hàm đó |

> **Quy tắc:** Hàm nhìn thấy biến bên ngoài nó, nhưng bên ngoài không nhìn thấy biến bên trong hàm.

---

## 3. Analogous (Ví dụ tương tự)

**Hàm giống như một chiếc máy pha cà phê tự động.**

| Máy pha cà phê | Hàm trong JavaScript |
|:---|:---|
| Máy pha cà phê | Hàm |
| Nguyên liệu đầu vào (Cà phê bột, Nước) | Đối số bạn cung cấp |
| Khe chứa nguyên liệu | Tham số định nghĩa sẵn |
| Quá trình xay và đun | Logic xử lý bên trong hàm |
| Ly cà phê thành phẩm | Giá trị được `return` |

**Phê bình ví dụ:**
Nếu máy pha cà phê không có `return` (không có vòi chảy ra), bạn vẫn nghe máy hoạt động (hàm vẫn chạy), nhưng bạn chẳng nhận được gì (`undefined`). Một lập trình viên giỏi luôn đảm bảo hàm có `return` hữu ích (trừ trường hợp đặc biệt chỉ cần thực thi lệnh như thay đổi giao diện).

---

## 4. Evidence (Ví dụ thực tế & Lỗi phổ biến)

### Đoạn mã thực tế: Kiểm tra tuổi truy cập
```javascript
function kiemTraTuoi(tuoi) {
    const TUOI_TOI_THIEU = 18;
    if (tuoi >= TUOI_TOI_THIEU) {
        return "Được phép truy cập";
    } else {
        return `Bạn còn ${TUOI_TOI_THIEU - tuoi} năm nữa mới đủ tuổi.`;
    }
}

console.log(kiemTraTuoi(20)); // Output: Được phép truy cập
console.log(kiemTraTuoi(15)); // Output: Bạn còn 3 năm nữa mới đủ tuổi.
```

### Lỗi phổ biến: Nhầm lẫn giữa `return` và `console.log`

#### ❌ Sai
```javascript
function cong(a, b) {
    console.log(a + b); // In ra màn hình, nhưng không trả về giá trị
}
let ketQua = cong(2, 3);
console.log(ketQua * 2); // Output: NaN (Vì ketQua là undefined)
```
**Vấn đề:** `console.log` chỉ hiển thị giá trị ra console, không trả về dữ liệu để tái sử dụng. Biến `ketQua` nhận giá trị `undefined` vì hàm không có `return`.

#### ✅ Đúng
```javascript
function cong(a, b) {
    return a + b; // Trả về giá trị
}
let ketQua = cong(2, 3);
console.log(ketQua * 2); // Output: 10
```
**Nguyên tắc:** Khi cần lấy giá trị ra khỏi hàm để sử dụng tiếp, luôn dùng `return`.

---

## 5. Reference (Tham khảo nhanh)

### Quy tắc đặt tên hàm
| Quy tắc | Ví dụ |
|:---|:---|
| Dùng động từ mô tả hành động | `getValue`, `calculateTotal`, `createUser` |
| Dùng camelCase | `kiemTraSoNguyenTo`, `layThongTinNguoiDung` |
| Tên ngắn gọn nhưng đủ nghĩa | `tinhTong` thay vì `tinhTongCuaHaiSo` |

### Các hàm có sẵn phổ biến (Built-in Functions)
| Hàm | Chức năng |
|:---|:---|
| `parseInt()` | Chuyển chuỗi thành số nguyên |
| `parseFloat()` | Chuyển chuỗi thành số thực |
| `String()` | Chuyển giá trị thành chuỗi |
| `Array.isArray()` | Kiểm tra một biến có phải là mảng không |
| `console.log()` | In ra console |
| `setTimeout(fn, ms)` | Thực thi hàm sau một khoảng thời gian |
| `setInterval(fn, ms)` | Thực thi hàm lặp đi lặp lại |

### So sánh các loại hàm
| Loại hàm | Cú pháp | Hoisting | Có `this` riêng |
|:---|:---|:---|:---|
| Function Declaration | `function tên() {}` | ✅ Có | ✅ Có |
| Function Expression | `const tên = function() {}` | ❌ Không | ✅ Có |
| Arrow Function | `const tên = () => {}` | ❌ Không | ❌ Không |
