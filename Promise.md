Dưới đây là bài học về **Promise trong JavaScript** được sắp xếp theo hệ thống **PACER** để vừa hiểu bản chất vừa có thể thực hành ngay.

---

# P — Procedural (Quy trình thực hành)

## 1. Tạo một Promise

```javascript
const promise = new Promise((resolve, reject) => {
  // Công việc bất đồng bộ

  const success = true;

  if (success) {
    resolve("Thành công!");
  } else {
    reject("Thất bại!");
  }
});
```

* `resolve(value)` → báo thành công.
* `reject(error)` → báo thất bại.

---

## 2. Sử dụng Promise

```javascript
promise
  .then((result) => {
    console.log(result);
  })
  .catch((error) => {
    console.log(error);
  })
  .finally(() => {
    console.log("Luôn chạy.");
  });
```

Thứ tự:

```
Promise
   ↓
then()      // nếu resolve
   ↓
catch()     // nếu reject hoặc có lỗi
   ↓
finally()   // luôn chạy
```

---

## 3. Ví dụ với setTimeout

```javascript
const downloadFile = new Promise((resolve, reject) => {

  setTimeout(() => {
    resolve("Download hoàn tất");
  }, 2000);

});

downloadFile
  .then(console.log)
  .catch(console.error);
```

Sau 2 giây:

```
Download hoàn tất
```

---

## 4. Thực hành ngay

Viết Promise sinh số ngẫu nhiên.

Nếu > 5 thì thành công.

Nếu ≤ 5 thì thất bại.

```javascript
const randomPromise = new Promise((resolve, reject) => {

  const number = Math.floor(Math.random() * 10);

  if (number > 5) {
    resolve(number);
  } else {
    reject(number);
  }

});

randomPromise
  .then(n => console.log("Success:", n))
  .catch(n => console.log("Failed:", n));
```

---

# A — Analogous (Ví dụ so sánh)

Hãy tưởng tượng bạn **đặt đồ ăn trên ứng dụng**.

Bạn bấm **Đặt món**.

Lúc này nhà hàng chưa giao ngay.

Có ba khả năng:

```
Bạn đặt món
      │
      ▼
Đầu bếp đang nấu
(Pending)
      │
 ┌────┴────┐
 │         │
 ▼         ▼
Giao thành công
(Fulfilled)

Hoặc

Hủy đơn
(Rejected)
```

Khi đồ ăn giao:

```
then()
```

Khi nhà hàng hủy:

```
catch()
```

Sau cùng ứng dụng đều đóng màn hình loading:

```
finally()
```

Đây chính là Promise.

---

# C — Conceptual (Hiểu bản chất)

Promise là **một đối tượng đại diện cho kết quả của một tác vụ bất đồng bộ trong tương lai**.

Thay vì trả kết quả ngay, Promise hứa rằng:

> "Sau này tôi sẽ trả kết quả hoặc trả lỗi."

## Ba trạng thái

```
            Promise
               │
        Pending
      (đang chờ)
        /      \
       /        \
Fulfilled    Rejected
(thành công) (thất bại)
```

### 1. Pending

* vừa được tạo
* chưa có kết quả

Ví dụ:

```
Đang tải dữ liệu...
```

---

### 2. Fulfilled

Công việc hoàn thành.

```
resolve(data)
```

sẽ kích hoạt

```
.then(...)
```

---

### 3. Rejected

Có lỗi hoặc thất bại.

```
reject(error)
```

sẽ kích hoạt

```
.catch(...)
```

---

## Logic vận hành

```
new Promise()

      │

executor chạy ngay

      │

Có resolve ?

      │
     YES
      │
then()

-------------------

Có reject ?

      │
     YES
      │
catch()

-------------------

Cuối cùng

finally()
```

Một Promise chỉ có thể chuyển trạng thái **một lần**:

```
Pending

↓

Fulfilled

hoặc

Rejected
```

Sau khi đã hoàn thành hoặc thất bại thì **không thể quay lại Pending hoặc đổi sang trạng thái còn lại**.

---

# E — Evidence (Ví dụ thực tế)

## Ví dụ gọi API mô phỏng

```javascript
function login(username, password) {

  return new Promise((resolve, reject) => {

    setTimeout(() => {

      if (username === "admin" && password === "123") {
        resolve({
          name: "Admin",
          role: "Administrator"
        });
      } else {
        reject(new Error("Sai tài khoản hoặc mật khẩu"));
      }

    }, 1500);

  });

}

login("admin", "123")
  .then(user => {
    console.log("Đăng nhập thành công");
    console.log(user);
  })
  .catch(err => {
    console.log(err.message);
  })
  .finally(() => {
    console.log("Ẩn loading");
  });
```

Kết quả:

```
Đăng nhập thành công

{
  name: "Admin",
  role: "Administrator"
}

Ẩn loading
```

---

## Thất bại

```javascript
login("abc", "111")
  .then(user => console.log(user))
  .catch(err => console.log(err.message));
```

Kết quả:

```
Sai tài khoản hoặc mật khẩu
```

---

## Lỗi thường gặp 1

Quên `return` Promise.

Sai:

```javascript
function fetchData() {
  new Promise((resolve) => {
    resolve("Done");
  });
}

fetchData().then(console.log);
```

Lỗi:

```
Cannot read properties of undefined
```

Đúng:

```javascript
function fetchData() {
  return new Promise((resolve) => {
    resolve("Done");
  });
}
```

---

## Lỗi thường gặp 2

Quên `catch()`

```javascript
Promise.reject("Error");
```

Có thể gây cảnh báo:

```
Unhandled Promise Rejection
```

---

## Lỗi thường gặp 3

Gọi cả `resolve()` và `reject()`

```javascript
new Promise((resolve, reject) => {

  resolve("OK");
  reject("Error");

});
```

Kết quả:

Chỉ lời gọi đầu tiên (`resolve("OK")`) có hiệu lực; các lần thay đổi trạng thái sau sẽ bị bỏ qua.

---

# R — Reference (Flashcard)

| Phương thức                    | Mô tả ngắn                                                                                                                                                                  |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Promise.resolve(value)`       | Tạo Promise đã thành công với `value`.                                                                                                                                      |
| `Promise.reject(reason)`       | Tạo Promise đã thất bại với `reason`.                                                                                                                                       |
| `Promise.all(iterable)`        | Thành công khi **tất cả** Promise thành công; thất bại ngay khi có một Promise thất bại.                                                                                    |
