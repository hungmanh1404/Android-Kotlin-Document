# SharedPreferences vs Room Database => là 2 công nghệ  để lưu trữ dữ liệu trên máy => nhưng mục đích, cấu trúc, và sức mạnh hoàn toàn khác nhau.

| Tiêu chí                  | **SharedPreferences**                                | **Room Database**                                                         |
| ------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------------- |
| 💡 Mục đích               | Lưu **dữ liệu đơn giản**, kiểu key–value             | Lưu **dữ liệu phức tạp**, có cấu trúc (giống SQL)                         |
| 📄 Cấu trúc               | Dạng **Key – Value**, không quan hệ                  | **Bảng (Entity)**, có **DAO**, có quan hệ giữa các bảng                   |
| 🧠 Loại dữ liệu           | Chỉ lưu: `String`, `Int`, `Boolean`, `Float`, `Long` | Lưu được **object**, **list**, **quan hệ**, thậm chí **@Embedded** object |
| 🧰 Công nghệ nền          | File XML nội bộ                                      | SQLite (ORM wrapper)                                                      |
| 📦 Dung lượng phù hợp     | Dưới 1 MB                                            | Có thể hàng chục MB (database thực sự)                                    |
| ⚙️ Cách truy xuất         | Đơn giản, trực tiếp (`getString`, `putInt`, …)       | Phải dùng DAO (`@Dao`, `@Query`, …)                                       |
| 🚀 Hiệu năng              | Rất nhanh cho dữ liệu nhỏ                            | Tốt cho dữ liệu lớn, có chỉ mục và truy vấn                               |
| 🔄 Đồng bộ hóa            | Không có truy vấn nâng cao, chỉ ghi/đọc toàn bộ      | Có thể truy vấn, lọc, sort, limit, join,…                                 |
| 🧩 Tích hợp LiveData/Flow | ❌ Không                                              | ✅ Có (Room hỗ trợ Flow và LiveData)                                       |
| 🧰 Phạm vi sử dụng        | Dữ liệu nhẹ: token, theme, login, cài đặt,…          | Dữ liệu lớn: user list, story, order, message,…                           |


### ⚠️ Nếu mà dữ liệu qúa lớn mà bạn ko sử dụng đúng loại thì sẽ gây ra Memory Issue 

### 🧩  1. Khi dùng SharedPreferences để lưu dữ liệu quá lớn

- SharedPreferences thực chất là một file XML được load toàn bộ vào RAM mỗi khi đọc.

👉 Nghĩa là:

- Khi bạn gọi getSharedPreferences("AppPrefs"),
→ Android đọc toàn bộ file XML vào bộ nhớ.

- Nếu file đó chứa hàng ngàn dòng JSON, list dài,
→ bạn đang ép RAM phải giữ cả khối dữ liệu khổng lồ mỗi lần đọc.

📍 Kết quả:

- Memory tăng nhanh (vì file XML quá lớn)

- App lag, chậm, thậm chí OutOfMemoryError trên máy yếu

- File XML dễ hỏng (corrupt) nếu app bị tắt giữa lúc ghi

```kt

// ❌ Không nên làm như thế này!
val json = Gson().toJson(bigUserList) // 10.000 users
prefs.edit().putString("users", json).apply()

// ✅ Dùng Room Database — vì Room chỉ load những gì bạn query, không load toàn bộ file.
val users = userDao.getUsers(limit = 50)
```
### 2. Khi dùng Room nhưng query hoặc xử lý sai cách

- Ngay cả Room, nếu bạn:

- SELECT * FROM table (load toàn bộ hàng trăm ngàn dòng)

- hoặc giữ List kết quả lớn trong memory mà không phân trang (Paging)

👉 Vẫn có thể bị OutOfMemoryError.

✅ Cách khắc phục:

- Dùng LIMIT trong query hoặc Paging 3 library

- Dùng Flow để stream dữ liệu dần dần

- Không giữ dữ liệu lớn trong LiveData hoặc ViewModel quá lâu

| Tình huống                                         | Dễ gây lỗi Memory? | Giải pháp                                    |
| -------------------------------------------------- | ------------------ | -------------------------------------------- |
| Lưu list lớn (JSON) trong SharedPreferences        | ⚠️ Có              | Dùng Room                                    |
| Ghi/đọc SharedPreferences liên tục                 | ⚠️ Có              | Gộp và batch lại, dùng apply() thay commit() |
| Dùng Room nhưng query `SELECT *` trên bảng cực lớn | ⚠️ Có              | Dùng LIMIT, Paging, Flow                     |
| Load ảnh/video từ DB thay vì file                  | ⚠️ Có              | Lưu path thay vì blob                        |
