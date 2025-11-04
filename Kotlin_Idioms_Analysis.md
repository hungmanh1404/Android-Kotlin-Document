# 📘 Kotlin Idioms — Phân tích chi tiết kèm ví dụ

Tài liệu này phân tích các *idioms* (thói quen/kiểu viết mã hay dùng) trong Kotlin dựa trên trang chính thức **Kotlin Idioms** và mở rộng với ví dụ giải thích rõ ràng. Nguồn gốc: *Kotlin documentation — Idioms*

---

## Mục lục
1. Create DTOs — `data class` (tóm tắt chức năng & ví dụ)
2. Default function parameters
3. Filtering / collection idioms
4. String interpolation
5. Null-safety idioms: `?.`, `?:`, `let`, `run`, `throw`
6. Scoping functions: `with`, `apply`, `also`, `let`, `run` — so sánh + ví dụ
7. Lazy property (`by lazy`)
8. Extension functions
9. Singletons (`object`) và companion objects
10. Inline / reified generics
11. Builder-style & single-expression functions
12. Using `use` for resource management (try-with-resources)
13. Common small idioms (swap, TODO, firstOrNull, etc.)
14. Kết luận ngắn

---

## 1. Create DTOs — `data class` (tại sao dùng)
**Ý chính:** `data class` tự tạo các phương thức thường cần ở các lớp dữ liệu: `equals`, `hashCode`, `toString`, `copy`, và các `componentN()` để destructure. 

**Ví dụ:**
```kotlin
data class Customer(val name: String, val email: String)

fun main() {
    val c1 = Customer("An", "an@example.com")
    val c2 = c1.copy(email = "an2@example.com")
    val (name, email) = c2  // destructuring via component1/component2
    println(c1)  // Customer(name=An, email=an@example.com)
    println("name=$name, email=$email")
}
```

**Phân tích:** Dùng `data` khi class chủ yếu chứa dữ liệu — tiết kiệm boilerplate so với Java.

---

## 2. Default function parameters
**Ý chính:** Thay vì overload nhiều phương thức, đặt giá trị mặc định cho tham số.

```kotlin
fun greet(name: String = "Bạn", excited: Boolean = false) {
    val suffix = if (excited) "!" else "."
    println("Xin chào $name$suffix")
}

fun main() {
    greet()                 // Xin chào Bạn.
    greet("Mạnh")           // Xin chào Mạnh.
    greet("Mạnh", true)     // Xin chào Mạnh!
}
```

**Phân tích:** Giảm số lượng overload, code rõ ràng hơn.

---

## 3. Filter một collection (lambda ngắn gọn)
```kotlin
val list = listOf(-2, 0, 3, 5, -1)
val positives = list.filter { it > 0 }   // it là từng phần tử
println(positives) // [3, 5]
```

**Mẹo:** Dùng `filterNot`, `map`, `flatMap`, `associate`, `groupBy` để xử lý collection theo phong cách hàm.

---

## 4. String interpolation
```kotlin
val name = "Mạnh"
println("Xin chào, $name")           // dễ đọc
println("Length: ${name.length}")    // chèn biểu thức
```

---

## 5. Null-safety idioms: `?.`, `?:`, `let`, `run`, `throw`
**Cốt lõi:** Kotlin khuyến khích xử lý `null` rõ ràng. Các idiom phổ biến gồm:

- `?.` — safe call  
- `?:` — Elvis (fallback)  
- `let` — thực thi block khi không null  
- `run` — chạy block và trả về kết quả (dùng với `?:` nếu fallback cần logic)  
- `?: throw` — ném ngoại lệ nếu null

**Ví dụ:**
```kotlin
fun emailLengthOrDefault(user: User?): Int {
    // trả về độ dài email nếu user và email khác null, ngược lại trả 0
    return user?.email?.length ?: 0
}

// Nếu muốn làm fallback phức tạp:
val files = File("test").listFiles()
val size = files?.size ?: run {
    // tính toán phức tạp nếu files == null
    println("files null - tính fallback")
    42
}

// Exec if not null
val maybe: String? = "ok"
maybe?.let {
    println("Not null: $it")
}
```

**Phân tích:** Những idiom này làm code ngắn, an toàn hơn so với null checks thủ công.

*Citation:* null-safety patterns are recommended in the Idioms doc. citeturn0view0

---

## 6. Scoping functions: `with`, `apply`, `also`, `let`, `run` — khi dùng cái nào?
Kotlin có 5 scoping functions thường dùng. Phân biệt theo hai trục: 
- `this` vs `it` (how receiver is referenced)
- Return value: original object vs lambda result

| Function | Receiver inside | Returns |
|----------|------------------|---------|
| `let`    | `it`             | lambda result |
| `run`    | `this`           | lambda result |
| `also`   | `it`             | original object |
| `apply`  | `this`           | original object |
| `with(obj)`| `this`         | lambda result |

**Ví dụ & khi dùng:**

- `apply` — cấu hình object và trả về object (thường dùng builder-like):
```kotlin
val rect = Rectangle().apply {
    width = 100
    height = 50
}
```

- `also` — làm side-effect (log, validate) rồi trả về object:
```kotlin
val list = mutableListOf<Int>()
list.also { println("Before add: $it") }.add(1)
```

- `let` — xử lý giá trị không-null, hoặc để chain các thao tác, trả về kết quả:
```kotlin
val result = "123".toIntOrNull()?.let { it * 2 } ?: 0
```

- `run` — khi muốn thực thi block và trả kết quả, và cần `this` làm receiver:
```kotlin
val area = Rectangle(3,4).run {
    length * breadth
}
```

- `with(obj)` — gọi nhiều hàm trên `obj` và trả về kết quả cuối cùng:
```kotlin
val myTurtle = Turtle()
with(myTurtle) {
    penDown()
    for (i in 1..4) { forward(100.0); turn(90.0) }
    penUp()
}
```

**Phân tích:** Chọn dựa trên: bạn có muốn `this` hay `it`, và bạn cần trả về object gốc hay kết quả tính toán.

(Scoping examples are taken from the idioms page and expanded.) citeturn0view0

---

## 7. Lazy property (`by lazy`)
**Ý chính:** Giá trị chỉ tính khi lần đầu truy cập; useful for expensive init.

```kotlin
val config: String by lazy {
    println("Init config")
    // load config from file or compute
    "config-value"
}

fun main() {
    println("Before")
    println(config) // đây mới in "Init config"
    println(config) // dùng lại, không tính lại
}
```

---

## 8. Extension functions
**Ý chính:** Mở rộng behavior cho class mà không cần kế thừa.

```kotlin
fun String.spaceToCamelCase(): String {
    return this.split(" ").joinToString("") { it.capitalize() }
}

fun main() {
    println("convert this to camel case".spaceToCamelCase()) // ConvertThisToCamelCase
}
```

**Lưu ý:** extension chỉ là syntactic sugar — không thay đổi lớp gốc ở runtime.

---

## 9. Singleton (`object`) & companion object
```kotlin
object Resource {
    val name = "Name"
}

class MyClass {
    companion object {
        fun create(): MyClass = MyClass()
    }
}

fun main() {
    println(Resource.name)
    val m = MyClass.create()
}
```

**Phân tích:** `object` tạo singleton thread-safe; `companion object` dùng cho static-like members.

---

## 10. Inline + Reified generics
**Vấn đề:** Thông thường generic types bị xóa (type erasure) trên JVM. `inline` + `reified` cho phép truy cập `T::class` bên trong.

```kotlin
inline fun <reified T: Any> Gson.fromJson(json: JsonElement): T =
    this.fromJson(json, T::class.java)
```

**Phân tích:** Dùng khi cần type information ở runtime cho generic.

(Idiom appears on the doc: example with Gson.)

---

## 11. Builder-style & single-expression functions
**Ví dụ single-expression:**
```kotlin
fun theAnswer() = 42
fun transform(color: String): Int = when (color) {
    "Red" -> 0
    "Green" -> 1
    else -> throw IllegalArgumentException("Invalid color")
}
```

**Builder-style (methods return `Unit` but used with `apply`):**
```kotlin
fun arrayOfMinusOnes(size: Int): IntArray {
    return IntArray(size).apply { fill(-1) }
}
```

---

## 12. `use` — resource management (try-with-resources)
```kotlin
Files.newInputStream(Paths.get("/some/file.txt")).buffered().reader().use { reader ->
    println(reader.readText())
}
```
`use` tự đóng resource khi block kết thúc.

---

## 13. Các idiom nhỏ khác (tổng hợp)
- Swap: `a = b.also { b = a }`  
- TODO marker: `TODO("reason")` (trong IDE sẽ hiển thị) — trả `Nothing`.  
- `firstOrNull()` dùng để lấy phần tử đầu hoặc `null`.  
- `try` là biểu thức: `val result = try { ... } catch (e: Exception) { ... }`

---

## 14. Kết luận ngắn
Trang Idioms gom các pattern ngắn gọn, an toàn và dễ đọc cho Kotlin. Dùng các idioms này giúp code idiomatic — tức là theo “văn phong” Kotlin, dễ bảo trì và ít boilerplate hơn. Để hiểu sâu, hãy thực hành từng idiom với ví dụ thực tế (DTOs, xử lý null, scoping functions, collections).

---
