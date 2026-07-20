Dưới đây là bài học về **async/await trong JavaScript** theo mô hình **PACER**.

---

# P — Procedural (Thực hành trước)

## 1. Cú pháp cơ bản

```javascript
async function hello() {
  return "Hello";
}

hello().then(console.log);
```

Kết quả:

```
Hello
```

Mặc dù trả về một chuỗi, nhưng `async` luôn khiến hàm trả về một **Promise**.

Tương đương:

```javascript
async function hello() {
  return "Hello";
}

// gần giống

function hello() {
  return Promise.resolve("Hello");
}
```

---

## 2. await

```javascript
function getUser() {
  return Promise.resolve({
    name: "Alice"
  });
}

async function main() {
  const user = await getUser();

  console.log(user.name);
}

main();
```

Kết quả

```
Alice
```

`await` sẽ:

1. Chờ Promise hoàn thành
2. Lấy giá trị trả về
3. Tiếp tục chạy dòng kế tiếp

---

## 3. Ví dụ với API

```javascript
async function getPosts() {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/posts/1"
  );

  const data = await response.json();

  console.log(data);
}

getPosts();
```

Ở đây có hai lần `await`:

* chờ tải dữ liệu
* chờ chuyển JSON thành object

---

## 4. Thực hành ngay

```javascript
function wait(ms) {
  return new Promise(resolve => {
    setTimeout(resolve, ms);
  });
}

async function demo() {
    console.log("Start");

    await wait(2000);

    console.log("After 2 seconds");
}

demo();
```

Bạn sẽ thấy:

```
Start
(đợi 2 giây)
After 2 seconds
```

---

# A — Analogous (Ví dụ so sánh)

Hãy tưởng tượng bạn gọi món ở quán cà phê.

### Không dùng await

Bạn gọi món rồi cứ 2 giây lại chạy đến quầy hỏi:

> "Xong chưa?"

Đó giống callback hoặc liên tục kiểm tra trạng thái.

---

### Dùng Promise

Bạn nhận một **thẻ rung**.

Bạn tiếp tục đọc sách.

Khi thẻ rung thì biết đồ uống đã xong.

---

### Dùng async/await

Bạn gọi món.

Sau đó nói với bạn phục vụ:

> "Khi có cà phê thì gọi tôi."

Bạn vẫn làm việc khác.

Đến lúc cà phê xong, chương trình tiếp tục từ đúng vị trí đang chờ.

Điểm cần lưu ý: `await` **không chặn toàn bộ JavaScript**; nó chỉ tạm dừng **hàm async hiện tại**, còn các tác vụ khác vẫn tiếp tục chạy.

---

# C — Conceptual (Hiểu bản chất)

## JavaScript là Single Thread

```
Main Thread
     │
     ▼
 Chạy từng lệnh
```

Nhưng nhiều tác vụ rất chậm:

* đọc file
* gọi API
* truy vấn database
* timeout

Nếu đợi trực tiếp:

```
Đọc API
↓
Đứng yên 3 giây
↓
Tiếp tục
```

=> giao diện bị treo.

---

### Vì thế Promise xuất hiện

```
API
 │
 │
 ▼
 Promise
 │
 ├── Pending
 ├── Fulfilled
 └── Rejected
```

Promise chỉ là **một lời hứa sẽ có kết quả trong tương lai**.

---

### async/await chỉ là cú pháp đẹp hơn

Thay vì

```javascript
fetch(url)
.then(...)
.then(...)
.catch(...)
```

ta viết

```javascript
const response = await fetch(url);
```

Không thay đổi cơ chế hoạt động.

Chỉ giúp code dễ đọc hơn.

---

### Mối quan hệ

```
Async Function
        │
        ▼
 luôn trả Promise
        │
        ▼
 await chỉ hoạt động với Promise
        │
        ▼
 Promise hoàn thành
        │
        ▼
 tiếp tục chạy
```

Có thể vẽ sơ đồ tư duy như sau:

```
Async/Await
│
├── async
│     └── trả Promise
│
├── await
│     └── đợi Promise
│
├── Promise
│     ├── Pending
│     ├── Fulfilled
│     └── Rejected
│
└── Error
      └── try/catch
```

---

# E — Evidence (Ví dụ thực tế)

```javascript
function getUser(id) {
  return new Promise((resolve, reject) => {

    setTimeout(() => {

      if (id === 1) {
        resolve({
          id: 1,
          name: "Alice"
        });
      } else {
        reject("User not found");
      }

    }, 2000);

  });
}

async function main() {

  console.log("Start");

  try {

    const user = await getUser(1);

    console.log(user);

  } catch (error) {

    console.log("Error:", error);

  }

  console.log("End");

}

main();

console.log("Program continues...");
```

### Luồng chạy

Ngay khi chạy:

```
Start
Program continues...
```

Sau 2 giây:

```
{id:1,name:"Alice"}
End
```

Nếu đổi

```javascript
await getUser(2);
```

thì:

```
Start
Program continues...
Error: User not found
End
```

Điều này cho thấy:

* `await` chỉ tạm dừng bên trong `main()`.
* Mã bên ngoài (`console.log("Program continues...")`) vẫn chạy ngay.
* `try...catch` là cách chuẩn để xử lý Promise bị từ chối (`rejected`) khi dùng `await`.

---

# R — Reference (Flashcard)

### Flashcard 1

**Q:** `async` làm gì?

**A:** Biến hàm thành hàm luôn trả về Promise.

---

### Flashcard 2

**Q:** `await` dùng ở đâu?

**A:** Chỉ bên trong `async function` (hoặc ở mức cao nhất của module ES hỗ trợ *top-level await*).

---

### Flashcard 3

**Q:** `await` chờ cái gì?

**A:** Một Promise (hoặc một giá trị sẽ được tự động bọc thành `Promise.resolve(value)`).

---

### Flashcard 4

**Q:** Nếu Promise bị reject?

**A:** Dùng `try...catch`.

---

### Flashcard 5

**Q:** Có thể dùng nhiều `await` liên tiếp không?

**A:** Có.

```javascript
const user = await getUser();
const posts = await getPosts(user.id);
```

---

### Flashcard 6

**Q:** Muốn chạy nhiều Promise cùng lúc?

**A:** Dùng `Promise.all()`.

```javascript
const [user, posts] = await Promise.all([
  getUser(),
  getPosts()
]);
```

---

### Flashcard 7

**Q:** `await` có chặn toàn bộ chương trình không?

**A:** Không. Nó chỉ tạm dừng hàm `async` hiện tại; các tác vụ khác vẫn tiếp tục được xử lý bởi JavaScript runtime.

---

### Flashcard 8

**Q:** Khi nào nên dùng `await`?

**A:**

* Khi các tác vụ phụ thuộc kết quả của nhau (tuần tự).
* Khi muốn mã dễ đọc và dễ bảo trì.

### Flashcard 9

**Q:** Khi nào nên tránh nhiều `await` liên tiếp?

**A:** Nếu các tác vụ độc lập, hãy chạy song song bằng `Promise.all()` để giảm tổng thời gian chờ.

```javascript
// Chậm hơn
const a = await taskA();
const b = await taskB();

// Nhanh hơn nếu taskA và taskB độc lập
const [a, b] = await Promise.all([taskA(), taskB()]);
```

---

## Tóm tắt trong một câu

* **Procedural:** Dùng `async` để tạo hàm trả về `Promise`, dùng `await` để chờ kết quả và thực hành với các ví dụ trên.
* **Conceptual:** `async/await` là cú pháp giúp làm việc với `Promise` theo phong cách tuần tự, không thay đổi bản chất bất đồng bộ của JavaScript.
* **Analogous:** Giống như gọi món và nhận thẻ rung: bạn không đứng chờ tại quầy, chỉ tiếp tục khi có thông báo.
* **Evidence:** Ví dụ `getUser()` cho thấy cách `await` phối hợp với `try...catch` và cách luồng chương trình vẫn tiếp tục bên ngoài hàm `async`.
* **Reference:** Ghi nhớ các quy tắc: `async` ⇒ trả `Promise`, `await` ⇒ chờ `Promise`, xử lý lỗi bằng `try...catch`, và ưu tiên `Promise.all()` khi có nhiều tác vụ độc lập.
