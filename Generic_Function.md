# ⚡️ PHẦN NÂNG CAO: GENERIC FUNCTION TRONG KOTLIN
🧩 1. Tổng quan lại cho chắc

Generic nghĩa là “tổng quát hóa” —
Tức là  chưa cần biết kiểu dữ liệu cụ thể mà vẫn có thể viết code hoạt động tốt với mọi loại kiểu.

Ví dụ cơ bản:
```kt
fun <T> printItem(item: T) {
    println("Item: $item")
}

fun main() {
    printItem("Mạnh")    // String
    printItem(25)        // Int
    printItem(true)      // Boolean
}

```
2. Ví dụ hay: Hoán đổi vị trí 2 giá trị bất kỳ

```kt
fun <T> swap(a: T, b: T): Pair<T, T> {
    return Pair(b, a)
}

fun main() {
    val result = swap("Hello", "World")
    println(result)  // 👉 (World, Hello)
}

```
💼 3. Ví dụ thực tế: Lấy phần tử đầu tiên trong danh sách bất kỳ
```kt
fun <T> firstElement(list: List<T>): T? {
    return if (list.isNotEmpty()) list[0] else null
}

fun main() {
    println(firstElement(listOf(1, 2, 3)))          // 👉 1
    println(firstElement(listOf("A", "B", "C")))    // 👉 A
}

```