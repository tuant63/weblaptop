# Callback trong JavaScript (PACER)

---

# P — Procedural (Học bằng cách làm)

## Callback là gì?

Callback là **một hàm được truyền vào làm tham số của một hàm khác**, và sẽ được gọi (call back) vào một thời điểm nào đó.

### Cú pháp cơ bản

```javascript
function greet(name, callback) {
    console.log("Xin chào " + name);
    callback();
}

function sayBye() {
    console.log("Tạm biệt!");
}

greet("An", sayBye);
```

**Kết quả**

```text
Xin chào An
Tạm biệt!
```

Ở đây:

- `sayBye` là callback.
- Chúng ta truyền **tên hàm**, không phải `sayBye()`.

---

## Callback có tham số

```javascript
function calculate(a, b, callback) {
    const result = a + b;
    callback(result);
}

calculate(3, 5, function(result) {
    console.log(result);
});
```

**Kết quả**

```text
8
```

---

## Callback bất đồng bộ

```javascript
console.log("Bắt đầu");

setTimeout(function () {
    console.log("Đã đợi 2 giây");
}, 2000);

console.log("Kết thúc");
```

**Kết quả**

```text
Bắt đầu
Kết thúc
Đã đợi 2 giây
```

---

## Bài tập thực hành

Hãy tự gõ đoạn code sau:

```javascript
function welcome(name, callback) {
    console.log("Welcome " + name);
    callback();
}

function finish() {
    console.log("Done!");
}

welcome("Nam", finish);
```

Sau đó thử:

1. Đổi callback để in `"Have a nice day!"`
2. Truyền callback bằng arrow function.
3. Cho callback nhận tham số `name`.

---

# C — Conceptual (Hiểu bản chất)

## Callback thực chất là gì?

Trong JavaScript:

> Hàm là **first-class value**.

Điều đó có nghĩa là hàm có thể:

- Gán vào biến.
- Truyền làm tham số.
- Trả về từ một hàm khác.

Ví dụ:

```javascript
function hello() {}

const x = hello;
```

`x` bây giờ cũng là một hàm.

---

## Callback chỉ đơn giản là

```text
Function
    │
    ▼
Được truyền vào
    │
    ▼
Hàm khác
    │
    ▼
Được gọi sau
```

Không có "ma thuật" nào cả.

---

## Callback giúp xử lý bất đồng bộ như thế nào?

### Không dùng callback

```text
Gửi request
      │
      ▼
Đứng chờ server
      │
      ▼
Nhận dữ liệu
      │
      ▼
Tiếp tục chương trình
```

➡ CPU bị chờ, lãng phí thời gian.

---

### Có callback

```text
Gửi request
      │
      ▼
Tiếp tục làm việc khác
      │
      ▼
Server trả dữ liệu
      │
      ▼
Runtime gọi callback
```

Nhờ đó JavaScript vẫn có thể phản hồi trong khi chờ tác vụ hoàn thành.

---

## Sơ đồ tư duy

```text
Callback
│
├── Function
│
├── Passed as argument
│
├── Executed later
│
├── Synchronous
│     └── chạy ngay
│
└── Asynchronous
      ├── setTimeout()
      ├── Event Listener
      └── Network Request
```

---

# A — Analogous (So sánh)

Hãy tưởng tượng bạn gọi món ở quán cà phê.

Bạn nói với nhân viên:

> "Khi pha xong thì gọi tôi."

Bạn không đứng cạnh máy pha.

Bạn chỉ **đưa trước một hành động cần thực hiện sau**.

Quy trình:

```text
Đặt món
    │
    ▼
Pha cà phê
    │
    ▼
Gọi khách
```

Trong JavaScript cũng tương tự:

```text
Gửi request
    │
    ▼
Đợi server
    │
    ▼
Callback được gọi
```

### Điểm giống

- Không biết chính xác khi nào công việc hoàn thành.
- Chỉ định trước việc cần thực hiện sau khi hoàn thành.

### Điểm khác

- Ngoài đời: nhân viên gọi bạn.
- JavaScript: Runtime hoặc Event Loop gọi callback.

---

# E — Evidence (Ví dụ thực tế)

## Lỗi phổ biến nhất

Sai:

```javascript
greet("Nam", sayBye());
```

Đúng:

```javascript
greet("Nam", sayBye);
```

---

### Điều gì xảy ra?

Đoạn này:

```javascript
sayBye()
```

nghĩa là:

> Thực thi hàm ngay lập tức.

Sau đó giá trị trả về của `sayBye()` (thường là `undefined`) mới được truyền vào.

Ví dụ:

```javascript
function greet(name, callback) {
    callback();
}

function sayBye() {
    console.log("Bye");
}

greet("Nam", sayBye());
```

**Kết quả**

```text
Bye
TypeError: callback is not a function
```

Quá trình diễn ra:

1. `sayBye()` chạy.
2. In ra `"Bye"`.
3. Trả về `undefined`.
4. Thực tế chương trình trở thành:

```javascript
greet("Nam", undefined);
```

5. Khi thực hiện:

```javascript
callback();
```

thì tương đương:

```javascript
undefined();
```

➡ Gây lỗi `TypeError`.

---

# R — Reference (Flashcard)

## Callback

> Hàm được truyền vào một hàm khác để được gọi sau.

---

## Truyền callback

✅ Đúng

```javascript
doSomething(callback);
```

❌ Sai

```javascript
doSomething(callback());
```

---

## Callback có thể là

```javascript
function a() {}
```

Hoặc

```javascript
const a = function() {};
```

Hoặc

```javascript
const a = () => {};
```

---

## Callback nhận tham số

```javascript
callback(result);
```

---

## Hàm nhận callback

```javascript
function run(callback) {
    callback();
}
```

---

## Một số API sử dụng callback

```javascript
setTimeout()
```

```javascript
setInterval()
```

```javascript
addEventListener()
```

```javascript
Array.prototype.forEach()
```

---

## Callback đồng bộ

```javascript
const numbers = [1, 2, 3];

numbers.forEach(function(number) {
    console.log(number);
});
```

---

## Callback bất đồng bộ

```javascript
setTimeout(function () {
    console.log("Hello");
}, 1000);
```

---

## Ghi nhớ

- Function là **first-class value** trong JavaScript.
- Callback chỉ là một function được truyền vào function khác.
- Callback có thể **đồng bộ** hoặc **bất đồng bộ**.
- Callback thường được dùng trong:
  - Event Handling
  - Timer
  - Network Request
  - File System (Node.js)
- Nhiều callback lồng nhau có thể dẫn đến **Callback Hell**.
- `Promise` và `async/await` được giới thiệu để giúp quản lý luồng bất đồng bộ dễ đọc và dễ bảo trì hơn.
