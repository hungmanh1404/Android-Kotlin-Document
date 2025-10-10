# What is the Intent ?

👉 Intent trong Android có thể hiểu đơn giản là:
- Một thông điệp (message object) được gửi đi để yêu cầu một hành động nào đó từ hệ thống hoặc từ một component khác trong ứng dụng.
- Nó giống như lá thư hoặc mệnh lệnh mà bạn gửi đi để nói với Android:

    + "Mở một màn hình mới (Activity)."

    + "Chạy một công việc nền (Service)."

    + "Báo cho tôi biết khi có sự kiện (BroadcastReceiver)."

## Các loại Intent:
1. Explicit Intent (Intent tường minh)

    + Chỉ định rõ tên component mà mình muốn mở/chạy.

    + Thường dùng để gọi Activity hoặc Service trong cùng ứng dụng.

📌 Ví dụ: mở DetailActivity từ MainActivity:
```kt
    val intent = Intent(this, DetailActivity::class.java)
    startActivity(intent)
```
2. Implicit Intent (Intent ngầm định)

    + Không chỉ định rõ component nào.

    + Thay vào đó, bạn nói: "Tôi muốn thực hiện hành động này, ai có khả năng thì hãy xử lý."

    + Android sẽ tìm ứng dụng phù hợp để xử lý.

📌 Ví dụ: mở trình duyệt web:
```kt
    val intent = Intent(Intent.ACTION_VIEW)
    intent.data = Uri.parse("https://www.google.com")
    startActivity(intent)

```
📌 Ví dụ: Gọi điện thoại:
```kt
    val intent = Intent(Intent.ACTION_DIAL)
    intent.data = Uri.parse("tel:0123456789")
    startActivity(intent)

```
📌 Ví dụ: Gửi tin nhắn SMS:

```kt
    val intent = Intent(Intent.ACTION_SENDTO)
    intent.data = Uri.parse("smsto:0123456789")
    intent.putExtra("sms_body", "Xin chào, đây là nội dung tin nhắn")
    startActivity(intent)

```

<details>
  <summary>👉 Xem code SmsFragment</summary>

 ```kt
 class SmsFragment : Fragment() {

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        val view = inflater.inflate(R.layout.fragment_sms, container, false)

        val btnSendSms: Button = view.findViewById(R.id.btnSendSms)
        btnSendSms.setOnClickListener {
            sendSms()
        }

        return view
    }

    private fun sendSms() {
        val phoneNumber = "0123456789"
        val message = "Xin chào, đây là nội dung tin nhắn"

        val smsUri = Uri.parse("smsto:$phoneNumber")
        val intent = Intent(Intent.ACTION_SENDTO, smsUri)
        intent.putExtra("sms_body", message)

        // Đảm bảo có app xử lý SMS trước khi gọi
        if (intent.resolveActivity(requireActivity().packageManager) != null) {
            startActivity(intent)
        } else {
            Toast.makeText(requireContext(), "Không tìm thấy ứng dụng SMS", Toast.LENGTH_SHORT).show()
        }
    }
}
///ragment_sms.xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center">

    <Button
        android:id="@+id/btnSendSms"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Gửi SMS" />

</FrameLayout>


 ```


</details>



## Thành phần của Intent

| Thành phần | Mô tả ngắn |
|------------|------------|
| **Component name (tên component)** | Tên của Activity/Service/BroadcastReceiver cụ thể (nếu dùng explicit intent). Nếu không chỉ định, Intent là ngầm định. *(Nguồn: Android Developers)* |
| **Action** | Hành động muốn làm (ví dụ `ACTION_VIEW`, `ACTION_SEND`, `ACTION_DIAL`…) *(Nguồn: Android Developers)* |
| **Data** | URI dữ liệu + kiểu MIME để chỉ rõ dữ liệu nào đang được xử lý (ví dụ ảnh, trang web, file) *(Nguồn: Android Developers)* |
| **Category** | Danh mục bổ sung để giúp hệ thống chọn component phù hợp hơn (ví dụ `CATEGORY_DEFAULT`, `CATEGORY_BROWSABLE`) *(Nguồn: Android Developers)* |
| **Extras** | Các dữ liệu bổ sung theo cặp key-value, để gửi thông tin thêm (ví dụ nội dung tin nhắn, subject email…) *(Nguồn: Android Developers)* |
| **Flags** | Các cờ (flags) để hướng dẫn hệ thống cách xử lý Intent (ví dụ: tạo task mới, clear history…) *(Nguồn: Android Developers)* |

- Ví dụ bạn muốn chia sẻ một văn bản bằng cách sử dụng implicit Intent:

```kt
val textToShare = "Xin chào từ ứng dụng của tôi!"
val intent = Intent().apply {
    action = Intent.ACTION_SEND
    putExtra(Intent.EXTRA_TEXT, textToShare)
    type = "text/plain"
}
startActivity(Intent.createChooser(intent, "Chia sẻ với"))

```
+ Action = ACTION_SEND → muốn gửi/chia sẻ.

+ Extras = EXTRA_TEXT → nội dung văn bản để chia sẻ.

+ Type = "text/plain" → loại dữ liệu là văn bản.

+ Không có component name ⇒ là implicit Intent.

## Sử dụng postValue setValue

| Phương thức   | Dùng ở luồng nào            | Hoạt động                                                                       | Thường dùng khi                                                            |
| ------------- | --------------------------- | ------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| `setValue()`  | **Main thread (UI thread)** | Cập nhật giá trị **ngay lập tức** và thông báo observer                         | Bạn đang ở **UI thread** (ví dụ trong ViewModel hoặc khi click button)     |
| `postValue()` | **Background thread**       | Gửi giá trị để cập nhật **sau đó** (chuyển qua main thread nội bộ của LiveData) | Bạn đang ở **background thread** (ví dụ trong coroutine hoặc callback API) |



