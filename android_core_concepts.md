# 🧠 Tổng Hợp Các Khái Niệm Quan Trọng Trong Android (Thực Chiến)

## ⚙️ 1. Nền tảng cốt lõi (Core Android)

| Khái niệm | Dễ hiểu | Dùng khi nào |
|------------|----------|--------------|
| **Context** | Môi trường hệ thống của app | Mọi nơi (load resource, inflate layout, mở Activity) |
| **Application class** | Toàn cục, chạy đầu tiên | Dùng để khởi tạo SDK, Koin/Hilt, cấu hình log, theme |
| **Activity** | Màn hình giao diện | Giao tiếp với ViewModel, điều hướng người dùng |
| **Fragment** | Mảnh giao diện tái sử dụng | Tab, bottom nav, view pager |
| **Intent** | Gửi dữ liệu, mở màn hình | Chuyển Activity hoặc gửi broadcast |
| **Bundle** | Gói dữ liệu key–value | Truyền dữ liệu tạm thời giữa component |
| **Service** | Chạy nền (background) | Phát nhạc, định vị, tải file ngầm |
| **BroadcastReceiver** | Nghe sự kiện hệ thống | Battery low, network change |
| **ContentProvider** | Chia sẻ dữ liệu giữa app | Gallery, danh bạ, file provider |

---

## 💾 2. Lưu trữ dữ liệu

| Khái niệm | Dễ hiểu | Dùng khi nào |
|------------|----------|--------------|
| **SharedPreferences / DataStore** | Lưu dữ liệu nhỏ, key–value | Token, setting, theme |
| **Room Database** | ORM quản lý SQLite | Lưu dữ liệu lớn: story, user, cache |
| **Repository pattern** | Lớp trung gian giữa data & UI | Gom API + DB + logic caching |
| **ViewModel + LiveData/StateFlow** | Giữ dữ liệu & phản ứng UI | Dữ liệu không mất khi xoay màn hình |

---

## 🌐 3. Giao tiếp mạng (Networking)

| Khái niệm | Dễ hiểu | Dùng khi nào |
|------------|----------|--------------|
| **Retrofit** | Gọi REST API | Kết nối server lấy dữ liệu |
| **OkHttp Interceptor** | Chèn log, header, retry | Theo dõi và xử lý request |
| **GraphQL (Apollo, Ktor)** | API dạng truy vấn | Khi backend dùng GraphQL |
| **Coroutine / Flow** | Xử lý bất đồng bộ | Chạy API, DB không chặn UI |
| **Result Wrapper (sealed class)** | Quản lý thành công/lỗi | Tách biệt UI state: Loading, Success, Error |

---

## 🧩 4. UI & UX

| Khái niệm | Dễ hiểu | Dùng khi nào |
|------------|----------|--------------|
| **RecyclerView + Adapter** | Danh sách có thể cuộn | Hiển thị danh sách sản phẩm, user |
| **DiffUtil** | So sánh dữ liệu hiệu quả | Cập nhật danh sách mượt hơn |
| **ConstraintLayout / Compose UI** | Tạo giao diện | Compose cho UI hiện đại |
| **Custom View** | Tạo view riêng | Thanh progress, button tùy biến |
| **MotionLayout / Animation** | Hiệu ứng chuyển động | Splash screen, scroll animation |

---

## 🧠 5. Kiến trúc (Architecture)

| Khái niệm | Dễ hiểu | Dùng khi nào |
|------------|----------|--------------|
| **MVVM (Model–View–ViewModel)** | Cấu trúc phổ biến hiện nay | Tách biệt UI và logic |
| **Clean Architecture** | Phân tầng rõ ràng | Khi dự án lớn, cần mở rộng |
| **Dependency Injection (Hilt, Koin, Dagger)** | Tiêm phụ thuộc | Quản lý lifecycle & dependency tự động |
| **UseCase / Interactor** | Chứa logic nghiệp vụ | Code dễ test, dễ mở rộng |

---

## 🔔 6. Thành phần hệ thống & tiện ích

| Khái niệm | Dễ hiểu | Dùng khi nào |
|------------|----------|--------------|
| **WorkManager** | Chạy tác vụ nền lâu dài | Sync dữ liệu, upload file, backup |
| **AlarmManager** | Hẹn giờ chạy task | Nhắc nhở, thông báo định kỳ |
| **Notification** | Hiển thị thông báo | Push notification, message |
| **Foreground Service** | Dịch vụ có thông báo | Chạy nhạc, định vị |
| **Deeplink / Navigation Component** | Điều hướng màn hình | Mở app từ link ngoài |
| **Permissions** | Quyền truy cập hệ thống | Camera, Location, Storage |

---

## 🧰 7. Công cụ nâng cao

| Khái niệm | Dễ hiểu | Dùng khi nào |
|------------|----------|--------------|
| **ViewBinding / DataBinding** | Kết nối XML ↔ Kotlin | Giảm findViewById, code sạch |
| **Jetpack Compose** | UI hiện đại viết bằng code | Thay thế XML UI cổ điển |
| **Lifecycle-aware Components** | Quan sát vòng đời Activity | Tối ưu resource, tránh leak |
| **Crashlytics / Firebase** | Theo dõi lỗi runtime | Phân tích crash, log |
| **Proguard / R8** | Nén, tối ưu, bảo mật code | Khi build release |

---

## 🧭 Ghi nhớ nhanh
> **Android app = 7 nhóm lớn**  
> 1️⃣ Core (Activity, Context, Intent...)  
> 2️⃣ Data (Room, Prefs, Repo)  
> 3️⃣ Network (Retrofit, Flow, API)  
> 4️⃣ UI (RecyclerView, Compose, MotionLayout)  
> 5️⃣ Architecture (MVVM, Hilt, Us
