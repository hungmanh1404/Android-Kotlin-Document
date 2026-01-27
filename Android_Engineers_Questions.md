# Android Engineer Document: https://www.androidengineers.in/questions/

## 1. Higher-Order Functions (HOF) trong Kotlin là gì ? Ví dụ?
### Định nghĩa CHUẨN
```
Higher-Order Function là hàm:

Nhận hàm khác làm tham số

HOẶC trả về một hàm
```
### Example:
- Nhận hàm làm tham số (phổ biến nhất)
```kt
🔹 Không dùng HOF (cứng, khó mở rộng)
fun printIfEven(n: Int) {
    if (n % 2 == 0) println(n)
}
```
```kt
🔹 Dùng HOF (linh hoạt)
fun printIf(n: Int, condition: (Int) -> Boolean) {
    if (condition(n)) {
        println(n)
    }
}

printIf(4) { it % 2 == 0 }
printIf(7) { it > 5 }
👉 Logic điều kiện được truyền vào, không hard-code.
```
- Ví dụ 2 — Trả về một hàm
```kt
fun multiplier(factor: Int): (Int) -> Int {
    return { value -> value * factor }
}

val double = multiplier(2)
val triple = multiplier(3)

double(10) // 20
triple(10) // 30
👉 Hàm tạo ra hàm khác (function factory).
```

- Kotlin standard library dùng HOF KHẮP NƠI
```kt
val users = listOf("An", "Binh", "Chi")

val result = users
    .filter { it.length > 2 }
    .map { it.uppercase() }


filter nhận (T) -> Boolean

map nhận (T) -> R

👉 Không có HOF → không có Kotlin collections hiện đại
```

## 2. What happens if you rotate the device?
- Rotation is a configuration change. Android assumes the current UI may no longer be valid, so it destroys and recreates the UI to reload correct resources (layout, qualifiers, dimensions).
```
Rotation ≠ process death
Process stays alive; UI is rebuilt.

Rotation destroys UI, not data.

```

##

Coroutine không tồn tại để chạy nhanh hơn thread
mà để sử dụng thread THÔNG MINH HƠN
```
CPU
 ↓
Thread (OS quản lý, nặng)
 ↓
Coroutine (logic nhẹ, suspend)
 ↓
Dispatcher (chọn thread phù hợp)
 ↓
Scope (quản lý sống/chết)
 ↓
Android Lifecycle (quyết định scope)

```
| Thành phần | Trả lời câu hỏi  |
| ---------- | ---------------- |
| CPU        | Ai thực thi?     |
| Thread     | Thực thi ở đâu?  |
| Coroutine  | Làm việc gì?     |
| Dispatcher | Chọn thread nào? |
| Scope      | Sống bao lâu?    |
| Lifecycle  | Khi nào chết?    |

- Ví dụ thực tế: gọi API trong Fragment
```kt
viewLifecycleOwner.lifecycleScope.launch {
    val data = withContext(Dispatchers.IO) {
        api.getData()
    }
    showUI(data)
}
-> Chuỗi thực sự xảy ra:
Lifecycle Fragment sống
 → lifecycleScope tồn tại
 → launch coroutine
 → Dispatcher.IO chọn thread nền
 → Coroutine chạy
 → Suspend (chờ network)
 → Resume
 → Dispatcher.Main quay về UI thread
```
## 3. Mô tả + sơ đồ UDF (Unidirectional Data Flow) cho Android XML và Jetpack Compose ? 

```
UI không tự thay đổi dữ liệu

UI chỉ gửi Event

ViewModel xử lý → cập nhật State

UI render lại từ State
//
UI bắn event
ViewModel đổi state
UI vẽ lại


```
- SƠ ĐỒ UDF CHO ANDROID XML (Activity / Fragment)
```
┌──────────────┐
│   XML View   │
│ (Button,Text)│
└──────┬───────┘
       │  Event (click, input)
       ▼
┌──────────────┐
│ ViewModel    │
│ (handleEvent)│
└──────┬───────┘
       │  Update State
       ▼
┌──────────────┐
│  State       │
│ (LiveData / │
│  StateFlow)  │
└──────┬───────┘
       │  Observe
       ▼
┌──────────────┐
│   XML View   │
│ (render UI)  │
└──────────────┘

```

- SƠ ĐỒ UDF CHO JETPACK COMPOSE
```
┌──────────────────┐
│   Composable UI  │
│  (Stateless)     │
└─────────┬────────┘
          │ Event (onClick)
          ▼
┌──────────────────┐
│   ViewModel      │
│   handleIntent   │
└─────────┬────────┘
          │ update
          ▼
┌──────────────────┐
│   UI State       │
│ (StateFlow)      │
└─────────┬────────┘
          │ collectAsState()
          ▼
┌──────────────────┐
│   Composable UI  │
│   Recompose      │
└──────────────────┘

```

- SO SÁNH XML vs COMPOSE (UDF)

| Tiêu chí | XML              | Compose      |
| -------- | ---------------- | ------------ |
| Event    | Listener         | Lambda       |
| State    | LiveData / Flow  | State / Flow |
| Render   | Thủ công         | Tự động      |
| UDF      | Có, nhưng dễ phá | Thuần UDF    |
| Bug UI   | Dễ sync sai      | Ít hơn       |
