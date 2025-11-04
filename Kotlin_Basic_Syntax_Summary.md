# 📘 Kotlin Basic Syntax Summary

Tổng hợp toàn bộ kiến thức cơ bản trong [Kotlin Basic Syntax](https://kotlinlang.org/docs/basic-syntax.html) — kèm ví dụ dễ hiểu.

---

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
