# Services Android
1. Khái niệm:
-  Service là một trong 4 thành phần chính (Application Components) của Android (cùng với Activity, BroadcastReceiver, ContentProvider).

- Nhiệm vụ: Thực hiện các tác vụ chạy lâu dài (long-running) ở chế độ nền (background).

- Đặc điểm: Không có giao diện người dùng (UI). Nó vẫn chạy kể cả khi người dùng chuyển sang ứng dụng khác.

- Ví dụ dễ hiểu: Activity giống như nhân viên phục vụ bàn (giao tiếp với khách), còn Service là đầu bếp trong bếp (làm việc âm thầm, khách không thấy nhưng việc vẫn phải chạy). Ví dụ thực tế: Ứng dụng nghe nhạc (tắt màn hình nhạc vẫn hát), ứng dụng chạy GPS khi lái xe.