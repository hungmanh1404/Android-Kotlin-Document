![App Screenshot](/images/elementAndroid.png)

1. viewModelScope vs lifecycleScope: 
Nếu bạn chỉ nhớ “cái này cho ViewModel, cái kia cho Fragment” → chưa đủ
Phải hiểu scope gắn với cái gì và chết khi nào
```kt
Logic / data / API → viewModelScope
Render / collect / animation → lifecycleScope
```
2. BỨC TRANH TỔNG QUÁT – ANDROID + COROUTINE (END-TO-END)
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