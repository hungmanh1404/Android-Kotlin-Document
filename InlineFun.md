🎯 BÀI TOÁN THỰC TẾ

- App gọi API lấy user

+ Nếu thành công → xử lý data

+ Nếu lỗi → xử lý error

+ Code chạy RẤT NHIỀU LẦN (hot path)

1. CÁCH 1 — fun THƯỜNG (KHÔNG inline)
```kt
fun <T> callApi(
    request: () -> T,
    onSuccess: (T) -> Unit,
    onError: (Throwable) -> Unit
) {
    try {
        val result = request()
        onSuccess(result)
    } catch (e: Throwable) {
        onError(e)
    }
}

Gọi API
callApi(
    request = {
        // giả lập call API
        "User data"
    },
    onSuccess = {
        println("Success: $it")
    },
    onError = {
        println("Error: ${it.message}")
    }
)

🧠 JVM THỰC SỰ LÀM GÌ (FUN THƯỜNG)
main()
 ├─ tạo lambda request (heap)
 ├─ tạo lambda onSuccess (heap)
 ├─ tạo lambda onError (heap)
 └─ call callApi()
      ├─ request.invoke()
      ├─ onSuccess.invoke()
      └─ onError.invoke()

📦 VÙNG NHỚ

❌ 3 lambda object

❌ 1 function call

❌ 3 lần invoke()

➡️ Overhead lớn nếu gọi API nhiều

```

2. CÁCH 2 — inline fun (CHUẨN THƯ VIỆN KOTLIN)
```kt
inline fun <T> callApi(
    request: () -> T,
    onSuccess: (T) -> Unit,
    onError: (Throwable) -> Unit
) {
    try {
        val result = request()
        onSuccess(result)
    } catch (e: Throwable) {
        onError(e)
    }
}

Gọi API (KHÔNG ĐỔI)
callApi(
    request = {
        "User data"
    },
    onSuccess = {
        println("Success: $it")
    },
    onError = {
        println("Error: ${it.message}")
    }
)

🧠 COMPILER BIẾN INLINE THÀNH GÌ?
fun main() {
    try {
        val result = "User data"
        println("Success: $result")
    } catch (e: Throwable) {
        println("Error: ${e.message}")
    }
}

📦 VÙNG NHỚ

✅ Không lambda object

✅ Không function call

✅ Không invoke()

➡️ Rất nhanh – rất ít GC
```
