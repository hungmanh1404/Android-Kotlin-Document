# Thead Concept Android.

## Main Thead - Only 1 on app
### Cú pháp:
```kt
 lifecycleScope.launch(Dispatchers.Main) {
    //excute on main thread
 }
```
### Bản chất 
- do Android framework tạo ra
- Gắn với UI + Looper
-  Xử lý: Vẽ UI, Touch, Click, Lifecycle

### Quy tắc cốt lõi sống còn phân biệt.

```
 Main thread = chỉ render UI
 Không nhân bản luôn chỉ 1
 Không phải pool
```
### Ánh xạ đời thưc dễ hiểu.
- Công nhân này chỉ một, làm 1 phồng ban duy nhất, làm một nhiệm vụ duy nhất.

## Defaut Thead Pool - CPU pool
### Cú pháp:
```kt
 lifecycleScope.launch(Dispatchers.Defaut) {
    //excute on Defaut thread
 }
```
### Bản chất 
- Pool thread cho CPU-bound
- Số thread = số core CPU
- Giới hạn chặt để không tranh CPU

### Quy tắc cốt lõi sống còn phân biệt.
- Dùng khi tính toán nặng
- Sort/Map
- DiffUtil
- JSON parse lớn.

### Ánh xạ đời thực dễ hiêu
 - Nhóm công nhân làm việc nặng.
 - Không tuyển thêm nhân viên chỉ = số slot(core) để tránh loạn hệ thống.

## IO Thread Pool - I/0 Pool
### Bản chất:
-  Tạo Pool cho Wating/Blocking khi thực thi
- Có thể tạo rất nhiều theard.
- Giới hạn trần threard = 64 threads

### Cú pháp
```kt
withContext(Dispatchers.IO) {
    callApi()
}
```
###  Quy tắc cốt lõi sống còn phân biệt.
- Dispatchers.IO =

- Coroutine Dispatcher dùng chung một thread pool chuyên xử lý IO-bound tasks.

### Bản chất kỹ thuật

- Dựa trên shared coroutine scheduler

- Dùng chung với Dispatchers.Default

- Có cơ chế mở rộng số thread khi cần

- Giới hạn mặc định:

- Tối đa:

- 64 threads
- hoặc
- Số core CPU
(tùy cái nào lớn hơn)

- Ví dụ:

- CPU 8 cores → IO pool có thể lên tới 64 threads

### Quan hệ kích thước (Hierarchy)
```kt
Process
 └── Thread Pool (Dispatchers.IO)
      ├── Thread 1
      ├── Thread 2
      ├── Thread 3

➡️ Nhỏ nhất: Thread
➡️ Lớn hơn: Thread Pool
➡️ Lớn hơn nữa: Process
```

### Ánh xạ đời thực dễ hiểu
- Nhiều việc cần nhiều công nhân để xử lý.