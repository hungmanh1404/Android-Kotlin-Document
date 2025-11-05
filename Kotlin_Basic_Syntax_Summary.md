# 📘 Kotlin Basic Syntax Summary

Tổng hợp toàn bộ kiến thức cơ bản trong [Kotlin Basic Syntax](https://kotlinlang.org/docs/basic-syntax.html) — kèm ví dụ dễ hiểu.

---


🧩 1. Kotlin cơ bản (làm nền tảng ngôn ngữ)

- Biến và kiểu dữ liệu: val, var, Int, String, List, Map, Boolean

```kt
val scoreMap = mutableMapOf(
    "A" to 90,
    "B" to 85
)

// Thêm phần tử mới
scoreMap["C"] = 70

// Sửa giá trị
scoreMap["B"] = 95

```

- Cấu trúc điều khiển: if-else, when, for, while, do while

- Hàm (function): cách khai báo, truyền tham số, trả về giá trị

- Lớp & đối tượng: class, object, constructor, inheritance, interface

```kt
class UserViewModel(private val user: User)
          ↑
          |__ Constructor (cổng nhận dữ liệu)
          
Khi gọi:
val viewModel = UserViewModel(User("Mạnh", 25))
           ↑
           |__ Kotlin gọi constructor, truyền dữ liệu vào


// INTERFACE
interface UserRepository {
    fun getUserName(): String
}

// Triển khai từ API
class ApiUserRepository : UserRepository {
    override fun getUserName() = "Từ API: Mạnh"
}

// Triển khai từ local DB
class LocalUserRepository : UserRepository {
    override fun getUserName() = "Từ Room DB: Mạnh"
}

// ViewModel chỉ cần biết interface, không quan tâm nguồn dữ liệu
class UserViewModel(private val repo: UserRepository) {
    fun showUser() {
        println(repo.getUserName())
    }
}

```

- Null Safety: dấu ?, !!, ?:, let, run

- Extension function: mở rộng chức năng cho class có sẵn
```kt
fun View.hide() {
    this.visibility = View.GONE
}

fun View.show() {
    this.visibility = View.VISIBLE
}

fun String.truncate(maxLength: Int): String {
    return if (this.length <= maxLength) this else take(maxLength - 3) + "..."
}

fun main() {
    val shortUsername = "KotlinFan42"
    val longUsername = "JetBrainsLoverForever"

    println("Short username: ${shortUsername.truncate(15)}") 
    // KotlinFan42
    println("Long username:  ${longUsername.truncate(15)}")
    // JetBrainsLov...
}
```

- Lambda, higher-order function: dùng cho callback, adapter
```kt
Lambda = một hàm ẩn danh (anonymous function) — tức là hàm không có tên riêng, có thể được gán vào biến, truyền như tham số, hoặc trả về từ hàm khác.
val sum: (Int, Int) -> Int = { x: Int, y: Int -> x + y }

Trong đó { x: Int, y: Int -> x + y } là lambda. 
Kotlin

//Higher-order
Một hàm được gọi là higher-order nếu nó nhận một hoặc nhiều hàm làm tham số, hoặc trả về một hàm. 
Kotlin

Nói cách khác: thay vì chỉ nhận dữ liệu như Int, String… thì nó nhận “hàm” như một tham số.

fun operate(a: Int, b: Int, operation: (Int, Int) -> Int): Int {
    return operation(a, b)
}

fun main() {
    val sum = operate(4, 5) { x, y -> x + y }
    println(sum)  // 👉 9
}

operate là higher-order function vì tham số operation là một hàm (Int, Int) -> Int.

Khi gọi operate(4, 5) { x, y -> x + y }, ta truyền lambda làm operation.

Kết quả: 9.

```

📱 2. Android cơ bản (nắm vững cấu trúc app)

**Hiểu rõ 3 thành phần chính:**

- Application: Chạy đầu tiên khởi tạo những thứ cần thiết

- Activity: màn hình chính của app

- Fragment: phần nhỏ tái sử dụng trong Activity

- View: các thành phần UI (TextView, Button, ImageView, RecyclerView, v.v.)

**Các phần quan trọng:**

- Lifecycle của Activity & Fragment

- Intent và Bundle (chuyển dữ liệu giữa các màn hình)

```kt
//Intent
val intent = Intent(this, SecondActivity::class.java)
startActivity(intent)

val intent = Intent(this, DetailActivity::class.java)
intent.putExtra("userName", "Mạnh Nguyễn")
intent.putExtra("age", 25)
startActivity(intent)

val userName = intent.getStringExtra("userName")
val age = intent.getIntExtra("age", 0)

binding.textView.text = "Tên: $userName - Tuổi: $age"

//Bundle

val bundle = Bundle()
bundle.putString("email", "manh@gmail.com")
bundle.putInt("score", 99)

val intent = Intent(this, ResultActivity::class.java)
intent.putExtras(bundle)
startActivity(intent)

val bundle = intent.extras
val email = bundle?.getString("email")
val score = bundle?.getInt("score")

binding.textView.text = "Email: $email - Điểm: $score"



```

- RecyclerView (hiển thị danh sách dữ liệu)

- ViewBinding / DataBinding (liên kết layout với code Kotlin)

- Context và Application (hiểu cách Android quản lý tài nguyên)


## 🧱 1. Khai báo biến (Variables)

```kotlin
val name = "Manh"   // immutable (không thể thay đổi)
var age = 25        // mutable (có thể thay đổi)
```

**Giải thích:**
- `val` = value → hằng số, không đổi được.
- `var` = variable → có thể gán lại giá trị mới.

```kotlin
val PI = 3.14
var count = 0
count += 1
// PI = 3.1415 ❌ lỗi, vì val không thể gán lại
```

---

## 🧮 2. Kiểu dữ liệu cơ bản (Basic Types)

```kotlin
val a: Int = 10
val b: Double = 3.14
val c: Boolean = true
val d: String = "Hello"
```

**Chuỗi có thể nối:**
```kotlin
val name = "Manh"
println("Hello, $name!")
println("Length: ${name.length}")
```

---

## 🔁 3. Cấu trúc điều khiển (Control Flow)

**If expression:**
```kotlin
val max = if (a > b) a else b
```

**When (thay cho switch):**
```kotlin
val x = 3
when (x) {
    1 -> println("x == 1")
    2 -> println("x == 2")
    in 3..5 -> println("x nằm trong 3 đến 5")
    else -> println("x khác các giá trị trên")
}
```

---

## 🔄 4. Vòng lặp (Loops)

```kotlin
for (i in 1..5) println(i)
for (i in 1 until 5) println(i)
for (i in 10 downTo 1 step 2) println(i)
```

```kotlin
var i = 0
while (i < 3) {
    println(i)
    i++
}
```

---

## 🧩 5. Hàm (Functions)

```kotlin
fun sum(a: Int, b: Int): Int {
    return a + b
}
```

**Rút gọn:**
```kotlin
fun sum(a: Int, b: Int) = a + b
```

**Không trả về gì (Unit):**
```kotlin
fun printSum(a: Int, b: Int): Unit {
    println("Sum = ${a + b}")
}
```

---

## 🧱 6. Classes & Objects

```kotlin
class Person(val name: String, var age: Int)

val p = Person("Manh", 25)
println(p.name)
p.age = 26
```

**Có property tính toán:**
```kotlin
class Rectangle(val width: Int, val height: Int) {
    val area: Int
        get() = width * height
}
```

---

## 🧩 7. Nullable và Null Safety

```kotlin
var name: String? = null
println(name?.length)
println(name!!.length)
```

**Elvis operator (`?:`):**
```kotlin
val length = name?.length ?: 0
```

---

## 🧰 8. Collections

```kotlin
val items = listOf("apple", "banana", "kiwi")
for (item in items) println(item)
```

```kotlin
val nums = mutableListOf(1, 2, 3)
nums.add(4)
```

```kotlin
val fruits = listOf("apple", "banana", "kiwi")
val result = fruits.filter { it.startsWith("a") }.map { it.uppercase() }
println(result)
```

---

## 🧠 9. String Templates

```kotlin
val name = "Manh"
val age = 25
println("My name is $name, I'm $age years old")
```

---

## ⚙️ 10. Smart Casts

```kotlin
fun demo(x: Any) {
    if (x is String) {
        println(x.length)
    }
}
```

---

## ⚡ 11. Ranges

```kotlin
for (i in 1..5) print(i)
for (i in 5 downTo 1) print(i)
for (i in 1..10 step 2) print(i)
```

---

## 🧑‍💻 Tổng kết

| Chủ đề | Từ khóa | Ghi nhớ nhanh |
|--------|----------|----------------|
| Biến | `val`, `var` | `val` = không đổi, `var` = đổi được |
| Kiểu dữ liệu | `Int`, `String`, `Boolean` | Kotlin tự đoán type |
| Cấu trúc điều khiển | `if`, `when`, `for`, `while` | Linh hoạt, rút gọn hơn Java |
| Hàm | `fun` | Có thể rút gọn 1 dòng |
| Lớp | `class` | Có constructor rút gọn |
| Null safety | `?`, `!!`, `?:` | Tránh lỗi NullPointerException |
| Collection | `listOf`, `mutableListOf` | Dễ filter/map |
| Smart cast | `is` | Kotlin tự ép kiểu khi hợp lệ |

---

✨ Nếu muốn, bạn có thể tạo mini project nhỏ để luyện tất cả phần này (ví dụ: app quản lý danh sách học viên hoặc tính điểm trung bình).
