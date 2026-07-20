# Fetch API trong JavaScript theo hệ thống PACER

## C — Conceptual (Bản chất và vị trí trong hệ sinh thái JavaScript)

### Fetch API là gì?

**Fetch API** là API chuẩn của trình duyệt (và hiện cũng được hỗ trợ trong Node.js hiện đại) dùng để **gửi các HTTP request** tới server và nhận dữ liệu trả về.

Nó được thiết kế để thay thế **XMLHttpRequest (XHR)** với cú pháp hiện đại hơn, dựa trên **Promise**.

Luồng hoạt động:

```
JavaScript
      │
      ▼
 Fetch API
      │
HTTP Request
      │
      ▼
   Web Server
      │
HTTP Response
      │
      ▼
 Response Object
      │
      ▼
JSON / Text / Blob...
```

### Fetch nằm ở đâu?

```
JavaScript Runtime
│
├── DOM API
├── BOM API
├── Fetch API      ← Giao tiếp mạng
├── Web Storage
├── WebSocket
└── IndexedDB
```

Nó **không phải là một phần của ngôn ngữ JavaScript (ECMAScript)** mà là **Web API** do trình duyệt (và Node.js) cung cấp.

---

### Đặc điểm quan trọng

* Bất đồng bộ (Asynchronous)
* Dựa trên Promise
* Hỗ trợ async/await
* Có thể gửi:

  * GET
  * POST
  * PUT
  * PATCH
  * DELETE
* Có thể gửi:

  * JSON
  * FormData
  * Blob
  * File
  * Text

---

## P — Procedural (Quy trình fetch dữ liệu)

### Bước 1. Gửi request

```javascript
fetch(url)
```

Ví dụ

```javascript
fetch("https://jsonplaceholder.typicode.com/users");
```

---

### Bước 2. Chờ server phản hồi

Fetch trả về Promise

```javascript
const response = await fetch(url);
```

Lúc này

```
response
```

chưa phải dữ liệu JSON.

Nó chỉ là

```
Response object
```

---

### Bước 3. Kiểm tra trạng thái

```javascript
if (!response.ok) {
    throw new Error("Request failed");
}
```

`response.ok`

```
true
```

khi status từ

```
200 → 299
```

---

### Bước 4. Đọc dữ liệu

Nếu server trả JSON

```javascript
const data = await response.json();
```

Nếu text

```javascript
await response.text();
```

Nếu file

```javascript
await response.blob();
```

---

### Bước 5. Xử lý dữ liệu

```javascript
console.log(data);
```

---

### Ví dụ đầy đủ

```javascript
async function loadUsers() {
    try {
        const response = await fetch(
            "https://jsonplaceholder.typicode.com/users"
        );

        if (!response.ok) {
            throw new Error("HTTP Error");
        }

        const users = await response.json();

        console.log(users);
    } catch (error) {
        console.error(error);
    }
}

loadUsers();
```

---

### Gửi POST

```javascript
fetch(url, {
    method: "POST",
    headers: {
        "Content-Type": "application/json"
    },
    body: JSON.stringify({
        name: "Alice",
        age: 20
    })
});
```

---

### Dùng async/await

```javascript
const response = await fetch(url);

const data = await response.json();
```

---

### Dùng Promise

```javascript
fetch(url)
    .then(res => res.json())
    .then(data => console.log(data))
    .catch(err => console.error(err));
```

---

## A — Analogous (So sánh thực tế)

Hãy tưởng tượng **đặt món ăn qua ứng dụng giao đồ ăn**.

| Thực tế           | Fetch API         |
| ----------------- | ----------------- |
| Mở ứng dụng       | `fetch()`         |
| Gửi đơn hàng      | HTTP Request      |
| Nhà hàng nhận đơn | Server            |
| Chế biến món      | Server xử lý      |
| Shipper giao món  | HTTP Response     |
| Mở hộp đồ ăn      | `response.json()` |
| Ăn món            | Sử dụng dữ liệu   |

Điểm quan trọng:

Khi gọi

```javascript
fetch(...)
```

bạn **chưa nhận được món ăn**.

Bạn chỉ mới gửi đơn.

Sau đó phải:

```
đợi
```

↓

```
response
```

↓

```
mở hộp
```

↓

```
ăn
```

Tương tự:

```
fetch()

↓

Response

↓

json()

↓

Data
```

---

## E — Evidence & Reference (Ví dụ thực tế và các tham số/mã lỗi quan trọng)

### Ví dụ 1: Lấy danh sách người dùng

```javascript
const response = await fetch(
    "https://jsonplaceholder.typicode.com/users"
);

const users = await response.json();

console.log(users);
```

---

### Ví dụ 2: Gửi dữ liệu

```javascript
await fetch(
    "https://jsonplaceholder.typicode.com/posts",
    {
        method: "POST",
        headers: {
            "Content-Type": "application/json"
        },
        body: JSON.stringify({
            title: "Hello",
            body: "World"
        })
    }
);
```

---

### Ví dụ 3: Upload file

```javascript
const form = new FormData();

form.append("image", file);

fetch("/upload", {
    method: "POST",
    body: form
});
```

---

## Các tham số quan trọng của `fetch()`

Cú pháp:

```javascript
fetch(url, options)
```

### `url`

Địa chỉ API.

```javascript
fetch("/api/users")
```

---

### `method`

Phương thức HTTP.

```javascript
method: "GET"
method: "POST"
method: "PUT"
method: "PATCH"
method: "DELETE"
```

---

### `headers`

Khai báo header.

```javascript
headers: {
    "Content-Type": "application/json",
    "Authorization": "Bearer TOKEN"
}
```

---

### `body`

Dữ liệu gửi lên server.

```javascript
body: JSON.stringify(data)
```

---

### `mode`

```javascript
mode: "cors"
```

Liên quan tới chính sách CORS.

---

### `credentials`

```javascript
credentials: "include"
```

Cho phép gửi cookie.

Các giá trị:

```
omit
same-origin
include
```

---

### `cache`

```javascript
cache: "no-cache"
```

---

### `signal`

Dùng với `AbortController`.

```javascript
const controller = new AbortController();

fetch(url, {
    signal: controller.signal
});
```

---

## Các thuộc tính quan trọng của `Response`

```javascript
response.status
```

```javascript
response.ok
```

```javascript
response.headers
```

```javascript
response.url
```

```javascript
response.type
```

---

## Các phương thức đọc dữ liệu

```javascript
response.json()
```

JSON

---

```javascript
response.text()
```

Text

---

```javascript
response.blob()
```

File

---

```javascript
response.arrayBuffer()
```

Binary

---

```javascript
response.formData()
```

FormData

---

## Các mã trạng thái HTTP thường gặp

| Mã  | Ý nghĩa               | Gợi ý xử lý                                                 |
| --- | --------------------- | ----------------------------------------------------------- |
| 200 | OK                    | Thành công                                                  |
| 201 | Created               | Tạo mới thành công                                          |
| 204 | No Content            | Thành công nhưng không có dữ liệu trả về                    |
| 301 | Moved Permanently     | Chuyển hướng vĩnh viễn                                      |
| 302 | Found                 | Chuyển hướng tạm thời                                       |
| 304 | Not Modified          | Dùng dữ liệu trong bộ nhớ đệm                               |
| 400 | Bad Request           | Kiểm tra dữ liệu gửi lên                                    |
| 401 | Unauthorized          | Đăng nhập hoặc gửi token hợp lệ                             |
| 403 | Forbidden             | Không có quyền truy cập                                     |
| 404 | Not Found             | Kiểm tra URL hoặc tài nguyên                                |
| 405 | Method Not Allowed    | Sai phương thức HTTP (GET/POST...)                          |
| 409 | Conflict              | Xung đột dữ liệu (ví dụ trùng tài nguyên)                   |
| 422 | Unprocessable Content | Dữ liệu hợp lệ về cú pháp nhưng không đạt yêu cầu nghiệp vụ |
| 429 | Too Many Requests     | Giới hạn tần suất, cần chờ hoặc retry                       |
| 500 | Internal Server Error | Lỗi phía server                                             |
| 502 | Bad Gateway           | Gateway nhận phản hồi không hợp lệ từ server phía sau       |
| 503 | Service Unavailable   | Dịch vụ tạm thời không khả dụng                             |
| 504 | Gateway Timeout       | Gateway hết thời gian chờ                                   |

---

## Lưu ý quan trọng khi sử dụng Fetch API

* `fetch()` **chỉ từ chối (reject) Promise khi có lỗi mạng hoặc request bị hủy**, không tự động reject với các mã HTTP như `404` hay `500`. Vì vậy cần kiểm tra `response.ok` hoặc `response.status`.
* `response.json()` chỉ dùng khi phản hồi thực sự là JSON; nếu không sẽ phát sinh lỗi khi phân tích dữ liệu.
* Mỗi `Response` chỉ có thể được đọc nội dung một lần. Nếu cần đọc nhiều lần, hãy dùng `response.clone()`.
* Nên kết hợp `AbortController` để hủy các request không còn cần thiết (ví dụ khi người dùng rời khỏi trang hoặc nhập tìm kiếm mới).
* Với các request cần xác thực bằng cookie giữa các miền, hãy cấu hình `credentials` phù hợp và đảm bảo server hỗ trợ CORS đúng cách.

### Tóm tắt PACER

| Thành phần               | Nội dung chính                                                                                                                                              |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Conceptual**           | Fetch API là Web API hiện đại để giao tiếp HTTP, hoạt động dựa trên Promise và hỗ trợ `async/await`.                                                        |
| **Procedural**           | `fetch()` → nhận `Response` → kiểm tra `response.ok` → đọc dữ liệu (`json()`, `text()`,...) → xử lý kết quả hoặc lỗi.                                       |
| **Analogous**            | Giống như đặt đồ ăn: gửi đơn (`fetch`) → nhận gói hàng (`Response`) → mở hộp (`json()`) → sử dụng món ăn (dữ liệu).                                         |
| **Evidence & Reference** | Áp dụng cho các thao tác GET/POST/upload file, nắm các tùy chọn (`method`, `headers`, `body`, `signal`,...) và các mã HTTP phổ biến để xử lý lỗi đúng cách. |
