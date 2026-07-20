# JSON trong JavaScript theo hệ thống PACER

---

# C — Conceptual (Bản chất của JSON và sự khác biệt với JavaScript Object)

## JSON là gì?

**JSON (JavaScript Object Notation)** là **một định dạng văn bản (text format)** dùng để **lưu trữ và trao đổi dữ liệu** giữa các hệ thống.

Mặc dù cú pháp của JSON rất giống Object trong JavaScript, nhưng **JSON không phải là Object**.

Ví dụ:

```json
{
  "name": "Alice",
  "age": 20
}
```

Đây là **dữ liệu JSON**, thực chất là **một chuỗi văn bản** nếu được truyền qua mạng hoặc lưu trong tệp.

---

## Vai trò của JSON

JSON đóng vai trò là **ngôn ngữ chung để trao đổi dữ liệu** giữa:

```text
Browser
     │
     │ HTTP
     ▼
 JSON
     ▲
     │
Server
```

Ví dụ:

* Browser gửi dữ liệu đăng nhập
* Server trả về thông tin người dùng
* API trả về danh sách sản phẩm
* File cấu hình (`config.json`)

Đều thường sử dụng JSON.

---

## JSON khác gì JavaScript Object?

Đây là điểm dễ gây nhầm lẫn nhất.

| JSON                          | JavaScript Object                                                |
| ----------------------------- | ---------------------------------------------------------------- |
| Là **chuỗi văn bản** (text)   | Là **đối tượng trong bộ nhớ**                                    |
| Dùng để trao đổi dữ liệu      | Dùng để thao tác trong chương trình                              |
| Key **bắt buộc** dùng dấu `"` | Key có thể không cần dấu `"`                                     |
| Không chứa hàm (function)     | Có thể chứa function                                             |
| Không chứa `undefined`        | Có thể chứa `undefined`                                          |
| Không có comment              | Có thể có comment trong mã JavaScript (không phải trong literal) |

Ví dụ Object:

```javascript
const user = {
    name: "Alice",
    age: 20,
    hello() {
        console.log("Hi");
    }
};
```

Ví dụ JSON hợp lệ:

```json
{
    "name": "Alice",
    "age": 20
}
```

---

## Mối quan hệ giữa JSON và Object

Có thể hình dung:

```text
Server
      │
      ▼
JSON String
      │
JSON.parse()
      ▼
JavaScript Object
      │
Xử lý dữ liệu
      │
JSON.stringify()
      ▼
JSON String
      │
      ▼
Server
```

**Kết luận:**

* JSON dùng để **truyền và lưu trữ**.
* Object dùng để **lập trình và xử lý dữ liệu**.

---

# P — Procedural (Parse và Stringify)

Có hai thao tác quan trọng.

---

## 1. Parse (JSON → Object)

Giả sử server trả về:

```text
'{"name":"Alice","age":20}'
```

Đây vẫn là **chuỗi**.

Muốn sử dụng phải chuyển thành Object.

### Bước 1

Có JSON String

```javascript
const json = '{"name":"Alice","age":20}';
```

---

### Bước 2

Parse

```javascript
const user = JSON.parse(json);
```

---

### Bước 3

Sử dụng như Object

```javascript
console.log(user.name);
console.log(user.age);
```

Kết quả

```text
Alice
20
```

---

## 2. Stringify (Object → JSON)

Giả sử có Object

```javascript
const user = {
    name: "Alice",
    age: 20
};
```

---

### Bước 1

Stringify

```javascript
const json = JSON.stringify(user);
```

---

### Bước 2

Kết quả

```text
'{"name":"Alice","age":20}'
```

Đây là chuỗi JSON để:

* gửi API
* lưu file
* lưu LocalStorage

---

## Quy trình đầy đủ

```text
Server
      │
      ▼
JSON String
      │
JSON.parse()
      ▼
Object
      │
Chỉnh sửa
      ▼
Object mới
      │
JSON.stringify()
      ▼
JSON String
      │
      ▼
Server
```

---

## Ví dụ với Fetch API

```javascript
const response = await fetch("/users");

const users = await response.json();
```

Thực chất:

```javascript
const text = await response.text();

const users = JSON.parse(text);
```

Phương thức `response.json()` là cách tiện lợi để đọc và phân tích JSON thành Object.

---

# A — Analogous (Ẩn dụ thực tế)

Hãy tưởng tượng **gửi hàng qua bưu điện**.

### JavaScript Object

Là **đồ vật thật** trong nhà bạn.

Ví dụ:

```text
Laptop
Điện thoại
Sách
```

---

### JSON

Là **đóng gói hàng vào một chiếc hộp tiêu chuẩn**.

Bưu điện không quan tâm bên trong là gì.

Chỉ cần:

* đúng quy cách
* dễ vận chuyển
* mọi nơi đều đọc được

---

Quy trình:

```text
Đồ vật thật
(Object)

↓

Đóng hộp

(JSON.stringify)

↓

Hộp hàng

(JSON)

↓

Vận chuyển

↓

Mở hộp

(JSON.parse)

↓

Đồ vật thật

(Object)
```

JSON chính là **chiếc hộp tiêu chuẩn** giúp các hệ thống trao đổi dữ liệu với nhau.

---

# E — Evidence & Reference (Quy tắc cú pháp và mẫu JSON)

## Các kiểu dữ liệu được phép trong JSON

| Kiểu    | Hợp lệ |
| ------- | ------ |
| String  | ✅      |
| Number  | ✅      |
| Boolean | ✅      |
| Null    | ✅      |
| Array   | ✅      |
| Object  | ✅      |

Ví dụ:

```json
{
    "name": "Alice",
    "age": 20,
    "isStudent": true,
    "address": null,
    "scores": [8, 9, 10]
}
```

---

## Những gì JSON **không** hỗ trợ

### Function

```javascript
{
    hello() {}
}
```

❌ Không hợp lệ.

---

### Undefined

```javascript
{
    "age": undefined
}
```

❌ Không hợp lệ.

---

### Comment

```json
{
    // tên
    "name": "Alice"
}
```

❌ Không hợp lệ.

---

### Dấu phẩy cuối

```json
{
    "name":"Alice",
}
```

❌ Không hợp lệ.

---

## Quy tắc cú pháp bắt buộc

### 1. Key phải dùng dấu ngoặc kép

Đúng:

```json
{
    "name": "Alice"
}
```

Sai:

```json
{
    name: "Alice"
}
```

---

### 2. Chuỗi phải dùng dấu ngoặc kép

Đúng:

```json
{
    "city":"Ha Noi"
}
```

Sai:

```json
{
    "city":'Ha Noi'
}
```

---

### 3. Dữ liệu phải hợp lệ

Ví dụ:

```json
{
    "age":20,
    "student":true,
    "address":null
}
```

---

### 4. Object dùng `{}`

```json
{
    "name":"Alice"
}
```

---

### 5. Array dùng `[]`

```json
[
    1,
    2,
    3
]
```

---

## Mẫu JSON chuẩn

```json
{
    "id": 1,
    "name": "Nguyen Van A",
    "email": "vana@example.com",
    "age": 25,
    "isStudent": false,
    "address": {
        "city": "Ha Noi",
        "district": "Ba Dinh"
    },
    "skills": [
        "JavaScript",
        "HTML",
        "CSS"
    ],
    "scores": [
        8,
        9,
        10
    ],
    "phone": null
}
```

Đây là một JSON hợp lệ vì:

* Tất cả key đều dùng dấu `"`
* Chuỗi đều dùng dấu `"`
* Chỉ sử dụng các kiểu dữ liệu được phép
* Không có `function`, `undefined` hoặc comment
* Không có dấu phẩy thừa ở cuối

---

## Các lỗi thường gặp khi làm việc với JSON

| Lỗi                                  | Nguyên nhân                                      | Cách khắc phục                                                       |
| ------------------------------------ | ------------------------------------------------ | -------------------------------------------------------------------- |
| `SyntaxError: Unexpected token`      | Chuỗi JSON sai cú pháp                           | Kiểm tra dấu `{}`, `[]`, `"` và dấu phẩy                             |
| `Unexpected end of JSON input`       | JSON bị cắt hoặc thiếu dấu đóng                  | Đảm bảo chuỗi JSON đầy đủ                                            |
| `JSON.parse()` lỗi                   | Dữ liệu không phải JSON hợp lệ                   | Xác thực hoặc bọc trong `try...catch`                                |
| `JSON.stringify()` bỏ qua thuộc tính | Giá trị là `undefined`, `function` hoặc `Symbol` | Chuyển sang kiểu dữ liệu JSON hỗ trợ hoặc loại bỏ các thuộc tính này |

---

## Tóm tắt PACER

| Thành phần               | Nội dung chính                                                                                                                                                             |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Conceptual**           | JSON là định dạng văn bản chuẩn để trao đổi dữ liệu; JavaScript Object là cấu trúc dữ liệu trong bộ nhớ. Hai khái niệm có cú pháp giống nhau nhưng mục đích khác nhau.     |
| **Procedural**           | Dùng `JSON.parse()` để chuyển JSON thành Object và `JSON.stringify()` để chuyển Object thành chuỗi JSON.                                                                   |
| **Analogous**            | JSON giống như một chiếc hộp đóng gói tiêu chuẩn giúp vận chuyển dữ liệu giữa các hệ thống; Object là món đồ thật được lấy ra để sử dụng.                                  |
| **Evidence & Reference** | JSON chỉ hỗ trợ `string`, `number`, `boolean`, `null`, `object`, `array`; key và chuỗi phải dùng dấu `"`, không hỗ trợ `function`, `undefined`, comment hay dấu phẩy cuối. |
