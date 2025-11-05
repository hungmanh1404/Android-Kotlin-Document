# Giải thích và ví dụ về AndroidManifest.xml

## 1. `<manifest>`

Là phần tử gốc của file `AndroidManifest.xml`. Nó mô tả toàn bộ ứng
dụng.

**Ví dụ:**

``` xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp">
</manifest>
```

**Tác dụng:**\
- Khai báo namespace Android.\
- Đặt tên package duy nhất cho ứng dụng.\
- Có thể chứa các quyền (permissions) và cấu hình chung.

------------------------------------------------------------------------

## 2. `<application>`

Định nghĩa toàn bộ ứng dụng -- nơi chứa tất cả Activity, Service,
Receiver, Provider.

**Ví dụ:**

``` xml
<application
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:theme="@style/Theme.MyApp">
</application>
```

**Tác dụng:**\
- Cấu hình biểu tượng, tên, theme.\
- Chỉ định class kế thừa `Application` nếu cần khởi tạo toàn cục.\
- Chứa tất cả các thành phần con: `<activity>`, `<service>`,
`<receiver>`, `<provider>`.

------------------------------------------------------------------------

## 3. `<activity>`

Đại diện cho một màn hình giao diện trong app.

**Ví dụ:**

``` xml
<activity android:name=".MainActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

**Tác dụng:**\
- Khai báo một Activity (màn hình).\
- Gắn `intent-filter` để chỉ định đây là Activity khởi đầu của app.

------------------------------------------------------------------------

## 4. `<intent-filter>`

Xác định cách mà Activity, Service, hoặc Receiver phản ứng với các
Intent.

**Ví dụ:**

``` xml
<intent-filter>
    <action android:name="android.intent.action.SEND" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:mimeType="text/plain" />
</intent-filter>
```

**Tác dụng:**\
- Cho phép thành phần nhận các Intent cụ thể.\
- Ví dụ: Activity nhận Intent chia sẻ văn bản (`SEND`).

------------------------------------------------------------------------

## 5. `<service>`

Định nghĩa một tác vụ chạy ngầm, không có giao diện người dùng.

**Ví dụ:**

``` xml
<service android:name=".MyBackgroundService" />
```

**Tác dụng:**\
- Dùng cho tác vụ nền như tải dữ liệu, phát nhạc, xử lý API.

------------------------------------------------------------------------

## 6. `<receiver>`

Khai báo `BroadcastReceiver` để nhận sự kiện hệ thống hoặc ứng dụng
khác.

**Ví dụ:**

``` xml
<receiver android:name=".NetworkChangeReceiver">
    <intent-filter>
        <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
    </intent-filter>
</receiver>
```

**Tác dụng:**\
- Lắng nghe sự kiện (ví dụ: mất mạng, pin yếu).

------------------------------------------------------------------------

## 7. `<provider>`

Khai báo `ContentProvider` để chia sẻ dữ liệu giữa các ứng dụng.

**Ví dụ:**

``` xml
<provider
    android:name=".MyContentProvider"
    android:authorities="com.example.myapp.provider" />
```

**Tác dụng:**\
- Cho phép các app khác truy cập dữ liệu có kiểm soát.

------------------------------------------------------------------------

## 8. `<uses-permission>`

Yêu cầu quyền đặc biệt từ người dùng.

**Ví dụ:**

``` xml
<uses-permission android:name="android.permission.CAMERA" />
```

**Tác dụng:**\
- Khai báo quyền mà ứng dụng cần như CAMERA, INTERNET, LOCATION, v.v.

------------------------------------------------------------------------

## 9. `<uses-feature>`

Khai báo phần cứng hoặc tính năng mà ứng dụng yêu cầu.

**Ví dụ:**

``` xml
<uses-feature android:name="android.hardware.camera" android:required="true" />
```

**Tác dụng:**\
- Giúp Google Play xác định thiết bị nào có thể cài đặt app.

------------------------------------------------------------------------

## 10. `<meta-data>`

Cung cấp thông tin bổ sung cho hệ thống hoặc thư viện.

**Ví dụ:**

``` xml
<meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="AIza..." />
```

**Tác dụng:**\
- Dùng để truyền dữ liệu cấu hình cho SDK (Google Maps, Firebase, v.v.).

------------------------------------------------------------------------

## Tổng kết

  Element               Chức năng chính                     Ví dụ thường gặp
  --------------------- ----------------------------------- -----------------------
  `<manifest>`          Khai báo tổng thể app               package, permissions
  `<application>`       Cấu hình app, chứa các thành phần   icon, theme
  `<activity>`          Màn hình giao diện                  MainActivity
  `<service>`           Chạy nền                            phát nhạc, tải file
  `<receiver>`          Nhận broadcast                      NetworkChangeReceiver
  `<provider>`          Chia sẻ dữ liệu                     ContentProvider
  `<uses-permission>`   Yêu cầu quyền                       CAMERA, INTERNET
  `<meta-data>`         Cấu hình SDK                        Google Maps key
