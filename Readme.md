# Main Concept

![App Screenshot](/images/elementAndroid.png)

# Key work Android Kotlin

| Keyword                 | Ánh xạ vật lý             | Ý nghĩa cực ngắn       |
| ----------------------- | ------------------------- | ---------------------- |
| `class`                 | Bản thiết kế              | Template tạo object    |
| `object`                | Nhà thật                  | Instance trong RAM     |
| `interface`             | Chuẩn ổ điện              | Contract hành vi       |
| `abstract`              | Khung xe                  | Base chưa hoàn chỉnh   |
| `inheritance`           | Con thừa hưởng cha        | IS-A                   |
 `enum`                  | Đèn giao thông            | State cố định          |
| `data class`            | Form thông tin            | Chứa state/data        |
| `val`                   | CCCD                      | Không đổi              |
| `var`                   | Số dư ngân hàng           | Thay đổi được          |
| `function`              | Máy bán nước              | Input → Output         |
| `lambda`                | Mini function bỏ túi      | Function ngắn          |
| `higher-order function` | Giao việc                 | Function nhận function |
| `coroutine`             | Nhân viên đi lấy hồ sơ    | Async không block      |
| `suspend`               | Tạm dừng công việc        | Pause không freeze     |
| `flow`                  | Ống nước dữ liệu          | Stream liên tục        |
| `nullable ?`            | Có thể rỗng               | Có thể null            |
| `?.`                    | Gõ cửa trước              | Safe call              |
| `!!`                    | Cam kết chắc chắn         | Null là crash          |
| `object singleton`      | Phòng điều khiển duy nhất | 1 instance             |
| `companion object`      | Văn phòng chung           | Static zone            |
| `constructor`           | Dây chuyền lắp ráp        | Tạo object             |
| `repository`            | Kho trung gian            | Quản lý data           |
| `ViewModel`             | Bộ nhớ sống qua rotate    | State holder           |
| `Activity`              | Màn hình điều phối        | UI container           |
| `Fragment`              | Mảnh UI                   | Disposable UI          |
| `Intent`                | Xe giao hàng              | Chuyển màn hình/data   |
| `Bundle`                | Balo dữ liệu              | Key-value state        |
| `Parcelable`            | Đóng kiện hàng            | Serialize nhanh        |
| `Room`                  | Kho lưu trữ               | Local database         |
| `DAO`                   | Nhân viên kho             | Query DB               |
| `LiveData/Flow`         | Camera realtime           | Observe state          |


# BỨC TRANH TỔNG QUÁT – ANDROID + COROUTINE (END-TO-END)
```
┌────────────────────────────────────────────┐
│                USER ACTION                 │
│        (Click / Open Screen / Event)        │
└────────────────────────────────────────────┘
                    ↓
┌────────────────────────────────────────────┐
│          ANDROID LIFECYCLE OWNER            │
│      Activity / Fragment / ViewModel        │
│                                            │
│  - onCreate / onStart / onResume            │
│  - onDestroy / onCleared                    │
└────────────────────────────────────────────┘
                    ↓
┌────────────────────────────────────────────┐
│                    SCOPE                   │
│  lifecycleScope / viewModelScope            │
│                                            │
│  Quyết định:                               │
│  - Coroutine sống bao lâu?                  │
│  - Khi nào bị cancel?                      │
└────────────────────────────────────────────┘
                    ↓
┌────────────────────────────────────────────┐
│                  COROUTINE                 │
│        (launch / async tạo coroutine)       │
│                                            │
│  - Logic nghiệp vụ                          │
│  - Có thể suspend / resume                 │
│  - KHÔNG phải thread                       │
└────────────────────────────────────────────┘
                    ↓
┌────────────────────────────────────────────┐
│             SUSPEND FUNCTION                │
│                                            │
│  - delay()                                 │
│  - Retrofit suspend                        │
│  - Room suspend                            │
│                                            │
│  → Suspend = tạm dừng LOGIC                │
│  → KHÔNG block thread                     │
└────────────────────────────────────────────┘
                    ↓
┌────────────────────────────────────────────┐
│                 DISPATCHER                 │
│                                            │
│  Dispatchers.Main     → UI Thread           │
│  Dispatchers.IO       → Network / DB        │
│  Dispatchers.Default  → Tính toán           │
│                                            │
│  → Chọn coroutine chạy trên thread nào     │
└────────────────────────────────────────────┘
                    ↓
┌────────────────────────────────────────────┐
│                   THREAD                   │
│           (Main / Background Pool)          │
│                                            │
│  - OS quản lý                              │
│  - Tài nguyên đắt                          │
│  - Có thể chạy nhiều coroutine             │
└────────────────────────────────────────────┘
                    ↓
┌────────────────────────────────────────────┐
│                    CPU                     │
│                                            │
│  - Thực thi instruction                    │
│  - KHÔNG biết coroutine                    │
│  - CHỈ biết thread                         │
└────────────────────────────────────────────┘


```


https://refactoring.guru/design-patterns/observer

https://developer.android.com/topic/architecture/ui-layer

https://developer.android.com/topic/architecture

https://www.geeksforgeeks.org/android/viewmodel-in-android-architecture-components/

# Trả lời câu hỏi Coroutine sống bao lâu ? "withContext = cùng một người, đổi phòng launch = thuê người mới

launch là quyết định kiến trúc
withContext là quyết định điều phối

## 🎯 Liên tưởng thực tế (cực quan trọng)
- 🎬 Ví dụ đời thực: 1 người + đổi phòng làm việc
```
- Người = Coroutine

- Phòng làm việc = Dispatcher / Thread

- Giấy phép làm việc = Job

- Công ty = CoroutineScope
```
## 🧠 Tình huống 1 — withContext (KHÔNG tạo người mới)
### Mục tiêu thực tế
```

Một người đang làm việc ở phòng Main
→ được yêu cầu sang phòng IO xử lý hồ sơ
→ xong việc quay lại phòng Main

⚠️ Vẫn là 1 người
⚠️ Không thuê thêm ai
⚠️ Không có người chạy song song
```
### Code concept
```kt
viewModelScope.launch {
    // 🧍‍♂️ COROUTINE ĐƯỢC TẠO Ở ĐÂY (1 người)

    println("1️⃣ Đang ở phòng MAIN")

    withContext(Dispatchers.IO) {
        // 🔁 KHÔNG tạo coroutine mới
        // 🚪 Chỉ chuyển phòng làm việc sang IO
        // 🧍‍♂️ VẪN là người cũ

        println("2️⃣ Chuyển sang phòng IO")
        Thread.sleep(1000) // giả lập việc nặng
    }

    // 🔙 Quay lại phòng cũ
    println("3️⃣ Quay lại phòng MAIN")
}

👉 Bạn control hoàn toàn:

Không song song

Chạy tuần tự

Kết thúc là xong

```
## 🚀 Tình huống 2 — launch (TẠO NGƯỜI MỚI)

### Mục tiêu thực tế

```
- Thuê thêm 1 người mới
- Người mới sang phòng IO làm việc
- Người cũ không chờ

⚠️ Có song song
⚠️ Khó kiểm soát nếu không quản lý kỹ
```

### Code concept
```kt
viewModelScope.launch {
    // 🧍‍♂️ Người số 1

    println("1️⃣ Người 1 ở MAIN")

    launch(Dispatchers.IO) {
        // 🧍‍♂️ Người số 2 (COROUTINE MỚI)
        println("2️⃣ Người 2 ở IO")
        Thread.sleep(1000)
    }

    // ❗ Người 1 KHÔNG chờ người 2
    println("3️⃣ Người 1 chạy tiếp")
}

1️⃣ Người 1 ở MAIN
3️⃣ Người 1 chạy tiếp
2️⃣ Người 2 ở IO
➡️ Mất kiểm soát thứ tự nếu không cẩn thận

```

## Tình huống 3 — withContext = CONTROL TUYỆT ĐỐI
### Mục tiêu thực tế

“Làm việc nặng → xong rồi mới update UI”


### Code Concept
```kt
viewModelScope.launch {
    // 🧍‍♂️ 1 coroutine duy nhất

    val user = withContext(Dispatchers.IO) {
        // 🔁 Đổi phòng
        // ⏳ CHỜ xong mới đi tiếp
        api.getUser()
    }

    // ✅ CHẮC CHẮN chạy SAU khi IO xong
    showUser(user)
}

```

## 🧪 Tình huống 4 — Khi nào bạn MẤT CONTROL?
### ❌ Dùng launch sai chỗ

```kt

viewModelScope.launch {
    launch(Dispatchers.IO) {
        api.getUser()
    }

    // ❌ UI update quá sớm
    showUser()
}


➡️ Bug ngầm 100%
```
### như này mới đúng

```kt
viewModelScope.launch {
    val data = withContext(Dispatchers.IO) {
        loadData()
    }
    showUI(data)
}

```