# let, apply, run, also, và with => Scope Functions

| Đặc điểm            | `run`                    | `let`                     | `apply`             |
| ------------------- | ------------------------ | ------------------------- | ------------------- |
| Từ khóa trong block | `this`                   | `it`                      | `this`              |
| Trả về              | Kết quả trong block      | Kết quả trong block       | Chính object        |
| Dùng khi            | Cần xử lý và trả giá trị | Xử lý với object nullable | Cấu hình object     |
| Null safety         | Có (dùng `?.run`)        | Có (dùng `?.let`)         | Có (dùng `?.apply`) |


## ***let*** : Nên dùng khi tránh NullPointerException (NPE) và xử lý logic gọn gàng.

### 1. Cú pháp cơ bản của let

```kt
object?.let {
    // code xử lý với 'it'
}

user?.let { u ->
    println("User name: ${u.name}")
}
```

- Nếu object ≠ null, khối let sẽ được thực thi.

- Nếu object = null, khối let bị bỏ qua, không lỗi NPE.

- Bên trong khối let, đối tượng được gọi là it (bạn có thể đổi tên nếu muốn).

### 2. Ví dụ cơ bản: tránh NullPointerException

```kt
val name: String? = "Manh"

name?.let {
    println("Tên là $it")
}

```
- ✅ Kết quả: Tên là Manh
- ❌ Nếu name = null, sẽ không in gì cả, không lỗi.

### 3. Dùng let để return giá trị khác

```kt
val length = name?.let {
    it.length
} ?: 0

println("Độ dài tên: $length")

<!-- Giải thích: -->

// name?.let { it.length } trả về độ dài nếu name ≠ null

// Nếu name = null, dùng ?: để trả về 0

// ✅ Cách này gọn và tránh NPE mà không cần if (name != null).

```
### 4. Ví dụ thực tế: trong Android
```kt
val user: User? = getUserFromDatabase()

user?.let {
    textViewName.text = it.name
    textViewEmail.text = it.email
}

✅ Gọn hơn nhiều so với:

if (user != null) {
    textViewName.text = user.name
    textViewEmail.text = user.email
}

// Khi cần return giá trị khác
val result = inputText?.let {
    it.trim().uppercase()
} ?: "Không có dữ liệu"

✅ Nếu inputText = " hello ", kết quả "HELLO"
✅ Nếu inputText = null, kết quả "Không có dữ liệu"

```

## ***run*** : Khi bạn muốn xử lý trên object và trả về một giá trị mới.
### 1. Cú pháp cơ bản của run
```kt
object.run {
    // khối lệnh sử dụng 'this'
}

```
- this là đối tượng hiện tại trong block (giống apply).

- run trả về giá trị cuối cùng trong block (giống let).

👉 Vì vậy run là sự kết hợp giữa let và apply.


### 2. Ví dụ cơ bản
```kt
val name = "Mạnh"

val result = name.run {
    println("Xin chào $this")
    length  // Giá trị cuối cùng được return
}

println(result) // 👉 4

// Giải thích:

this = "Mạnh"

run trả về this.length (giá trị cuối trong block)

Biến result = 4

```

### 3. Dùng run với object có thể null

```kt
val user: User? = getUserOrNull()

val age = user?.run {
    println("Tên: $name")
    age
} ?: 0

//Giải thích:

✅ Nếu user ≠ null → in ra tên, lấy age
✅ Nếu user = null → trả về 0, không lỗi NullPointerException

```

### 4. Ví dụ thực tế trong Android: cấu hình Dialog
```kt
//Cách thông thường:
val dialog = AlertDialog.Builder(context)
dialog.setTitle("Xác nhận")
dialog.setMessage("Bạn có chắc muốn xóa?")
dialog.setPositiveButton("Đồng ý", null)
dialog.setNegativeButton("Hủy", null)
val alertDialog = dialog.create()
alertDialog.show()

// Dùng run

AlertDialog.Builder(context).run {
    setTitle("Xác nhận")
    setMessage("Bạn có chắc muốn xóa?")
    setPositiveButton("Đồng ý", null)
    setNegativeButton("Hủy", null)
    create()
}.show()

// Giải thích
- run trả về kết quả cuối cùng trong block → ở đây là create()

- Bạn có thể gọi .show() ngay sau đó
→ Gọn gàng, dễ đọc, không cần giữ biến trung gian

```

### 5. Ví dụ với cấu hình object trả về giá trị
```kt
val result = StringBuilder().run {
    append("Xin chào ")
    append("Mạnh")
    toString()
}
println(result) // 👉 "Xin chào Mạnh"

// sau khi gắn text vào thì thực thi toString() luôn.

```




## ***apply***: khởi tạo và cấu hình một object trong một khối, sau đó trả lại chính object đó để dùng tiếp.

### 1. Cú pháp của apply

```kt
object.apply {
    // this Gọi thuộc tính hoặc phương thức của object
}

```
- Bên trong apply, từ khóa mặc định là this (ẩn, không cần viết).

- apply trả về chính đối tượng (object) sau khi khối lệnh được thực hiện.
- 👉 Rất thích hợp khi bạn muốn cấu hình một object rồi return lại chính nó.

### Ví dụ 1 — Khởi tạo View trong Android

```kt
// ❌ Cách thông thường:
val textView = TextView(context)
textView.text = "Xin chào"
textView.textSize = 18f
textView.setTextColor(Color.BLUE)
textView.setPadding(16, 16, 16, 16)

✅ Dùng apply:
val textView = TextView(context).apply {
    text = "Xin chào"
    textSize = 18f
    setTextColor(Color.BLUE)
    setPadding(16, 16, 16, 16)
}


| Lý do          | Giải thích                                                       |
| -------------- | ---------------------------------------------------------------- |
| ✅ Gọn gàng hơn | Không cần lặp lại tên biến `textView` nhiều lần                  |
| ✅ Dễ đọc hơn   | Các thuộc tính của View được gom nhóm rõ ràng                    |
| ✅ Linh hoạt    | `apply` trả về chính `textView`, có thể truyền tiếp vào chỗ khác |

ví dụ: 

layout.addView(
    TextView(context).apply {
        text = "Welcome!"
        setTextColor(Color.RED)
        textSize = 20f
    }
)

→ Khởi tạo + add view trong một dòng.
```
### Ví dụ 2 — Tạo object data class

```kt
// Giả sử bạn có:

data class User(var name: String = "", var age: Int = 0)

// Dùng apply:

val user = User().apply {
    name = "Mạnh"
    age = 25
}


```

### Ví dụ 3 — SharedPreferences Editor

```kt
// Trước đây:
val editor = sharedPreferences.edit()
editor.putString("username", "Manh")
editor.putBoolean("isLogin", true)
editor.apply()

// apply:
sharedPreferences.edit().apply {
    putString("username", "Manh")
    putBoolean("isLogin", true)
    apply()
}

```

## ***also*** : thực hiện hành động phụ trên object (log, kiểm tra, thao tác phụ) mà vẫn trả lại chính object để dùng tiếp.

### Khác biệt chính giữa also và apply

| Đặc điểm         | `apply`                                   | `also`                                                     |
| ---------------- | ----------------------------------------- | ---------------------------------------------------------- |
| Trong block dùng | `this`                                    | `it`                                                       |
| Return           | Chính object                              | Chính object                                               |
| Mục đích         | Cấu hình, thiết lập thuộc tính cho object | Thực hiện hành động phụ (log, kiểm tra, debug, validation) |
| Dùng khi         | Khởi tạo và set giá trị                   | Ghi log, debug, hoặc xử lý phụ                             |


### 1. Cú pháp cơ bản của also

```kt
object.also {
    // thao tác với 'it'
}

```

### 2 Ví dụ kết hợp also với apply
```kt
val textView = TextView(context).apply {
    text = "Xin chào"
    textSize = 18f
}.also {
    Log.d("UI", "TextView đã được khởi tạo: ${it.text}")
}

```