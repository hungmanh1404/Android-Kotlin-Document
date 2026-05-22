http://proandroiddev.com/android-interview-series-2024-part-1-android-basics-23a713f4a648

https://itviec.com/blog/cau-hoi-phong-van-kotlin/


# 📘 Android Core Concepts - Tổng Hợp Kiến Thức Cơ Bản

## Mục lục
1. [Activity và Fragment giống và khác nhau gì trong Android Kotlin?](#activity-vs-fragment)
1. [Override vs Overload](#override-vs-overload)
2. [Từ khóa `super` trong kế thừa](#từ-khóa-super-trong-kế-thừa)
3. [Val và Var trong LiveData](#val-và-var-trong-livedata)
4. [Class vs Data Class](#class-vs-data-class)
5. [Mức độ truy cập (Access Modifier)](#mức-độ-truy-cập-access-modifier)
6. [Class vs Object](#class-vs-object)
7. [Đa kế thừa trong Kotlin](#đa-kế-thừa-trong-kotlin)
8. [ListView vs RecyclerView](#listview-vs-recyclerview)
9. [Adapter: Select 1 hoặc nhiều item](#adapter-select-1-hoặc-nhiều-item)
10. [Kiểm tra chuỗi có thể chuyển sang Int hay không](#kiểm-tra-chuỗi-có-thể-chuyển-sang-int-hay-không)
11. [LiveData và cập nhật UI](#livedata-và-cập-nhật-ui)
12. [Tính đóng gói: `internal val a private set`](#tính-đóng-gói-internal-val-a-private-set)
13. [Lateinit vs Lazy](#lateinit-vs-lazy)
14. [Coroutine Dispatcher: IO, Main, Default](#coroutine-dispatcher-io-main-default)
15. [ViewGroup: FrameLayout, RelativeLayout, ConstraintLayout](#viewgroup-framelayout-relativelayout-constraintlayout)

---
(#override-vs-overload)

## Activity vs Fragment
**Điểm giống nhau**

- Activity và Fragment đều là thành phần dùng để xây dựng UI trong Android.

***Cả hai đều có thể:***

- Hiển thị giao diện lên màn hình thông qua layout XML hoặc ViewBinding.
- Có lifecycle riêng.
- Có các callback lifecycle như onCreate(), onStart(), onResume(), onPause(), onStop(), onDestroy().
- Xử lý logic UI, sự kiện click, observe data từ ViewModel.
- Nhận/truyền dữ liệu thông qua Intent, Bundle, arguments, Navigation Component.
- Có thể tương tác với ViewModel, Repository, API, Database...

Ví dụ đơn giản:
```kt
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }
}
class HomeFragment : Fragment() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }
}
```
**Điểm khác nhau cốt lõi**
| Tiêu chí          | Activity                                         | Fragment                                                                                   |
| ----------------- | ------------------------------------------------ | ------------------------------------------------------------------------------------------ |
| Bản chất          | Là một màn hình độc lập của app                  | Là một phần UI nằm bên trong Activity                                                      |
| Phụ thuộc         | Có thể tồn tại độc lập                           | Bắt buộc phải gắn vào Activity                                                             |
| Window            | Có window riêng                                  | Không có window riêng                                                                      |
| Khai báo Manifest | Thường phải khai báo trong `AndroidManifest.xml` | Không cần khai báo trong Manifest                                                          |
| Reuse UI          | Khó reuse hơn                                    | Dễ reuse hơn                                                                               |
| Navigation        | Điều hướng giữa các Activity bằng `Intent`       | Điều hướng giữa Fragment bằng `FragmentManager` hoặc Navigation Component                  |
| Context           | Activity là một `Context`                        | Fragment không phải là `Context`, phải lấy qua `requireContext()` hoặc `requireActivity()` |
| Permission        | Activity có thể trực tiếp request permission     | Fragment phải thông qua API của Fragment hoặc Activity                                     |
| Lifecycle UI      | Activity có lifecycle gắn với toàn màn hình      | Fragment có 2 lifecycle: Fragment lifecycle và View lifecycle                              |
| Back stack        | Activity nằm trong task/back stack của hệ thống  | Fragment có back stack riêng trong Activity                                                |

**Giải thích dễ hiểu bằng ánh xạ vật lý**

- Hãy tưởng tượng app Android là một ngôi nhà.

- Activity = căn phòng

- Activity giống như một căn phòng hoàn chỉnh.

```
Nó có:
Cửa ra vào riêng.
Không gian riêng.
Có thể tự tồn tại.
Có thể chứa nhiều đồ vật bên trong.

Ví dụ: LoginActivity, MainActivity, PaymentActivity.

Fragment = đồ nội thất / khu vực trong căn phòng

Fragment giống như một khu vực nhỏ hoặc món đồ nội thất trong căn phòng.

Ví dụ:

Khu vực danh sách sản phẩm.
Khu vực chi tiết sản phẩm.
Khu vực profile.
Khu vực setting.

Fragment không thể tự đứng một mình ngoài căn phòng. Nó phải được đặt trong một Activity.
```

## Override vs Overload
**Override**: Ghi đè lại hàm của lớp cha trong lớp con.  
**Overload**: Cùng tên hàm nhưng khác tham số (số lượng hoặc kiểu dữ liệu).

```kotlin
// Override
open class Animal {
    open fun speak() { println("Animal sound") }
}
class Dog : Animal() {
    override fun speak() { println("Woof!") }
}

// Overload
fun show(value: String) {}
fun show(value: Int) {}
```

---

## Từ khóa `super` trong kế thừa
`super` dùng để gọi đến hàm hoặc constructor của lớp cha.

```kotlin
open class Parent {
    open fun hello() = println("Hello from Parent")
}

class Child : Parent() {
    override fun hello() {
        super.hello() // Gọi hàm từ Parent
        println("Hello from Child")
    }
}
```

Nếu **không dùng `super.hello()`**, nghĩa là **bỏ qua logic của lớp cha**, chỉ chạy logic lớp con.

---

## Val và Var trong LiveData
- `val` dùng để **chỉ đọc (immutable)** dữ liệu LiveData từ View (Fragment/Activity).
- `var` hoặc `MutableLiveData` được dùng trong ViewModel để **thay đổi giá trị**.

```kotlin
private val _user = MutableLiveData<String>()
val user: LiveData<String> get() = _user

_user.value = "Mạnh" // chỉ thay đổi được từ ViewModel
```

Khác với `val/var` thông thường, LiveData còn **quan sát (observe)** và **tự động cập nhật UI**.

---

## Class vs Data Class
- `class`: Dùng để định nghĩa đối tượng có hành vi (behavior).
- `data class`: Dùng để lưu trữ dữ liệu (data holder).

```kotlin
data class User(val name: String, val age: Int)
val user1 = User("Mạnh", 25)
println(user1.toString()) // Tự sinh ra toString(), equals(), copy()
```

---

## Mức độ truy cập (Access Modifier)
| Modifier | Phạm vi truy cập | Ghi chú |
|-----------|------------------|---------|
| `public` | Mọi nơi | Mặc định nếu không khai báo |
| `private` | Trong cùng file hoặc class | Bảo vệ dữ liệu nội bộ |
| `protected` | Trong class và subclass | Dùng khi kế thừa |
| `internal` | Trong cùng module | Giới hạn trong module app |

---

## Class vs Object
| Đặc điểm | Class | Object |
|-----------|--------|---------|
| Tạo nhiều instance được | ✅ | ❌ |
| Có constructor | ✅ | ❌ |
| Dùng khi cần đối tượng duy nhất | ❌ | ✅ |

```kotlin
object AppConfig {
    const val BASE_URL = "https://api.example.com"
}
```

---

## Đa kế thừa trong Kotlin
Kotlin **không hỗ trợ đa kế thừa class**, nhưng **có thể implement nhiều interface**.

```kotlin
interface Flyable { fun fly() }
interface Walkable { fun walk() }

class Bird : Flyable, Walkable {
    override fun fly() = println("Flying")
    override fun walk() = println("Walking")
}
```

---

## ListView vs RecyclerView
| So sánh | ListView | RecyclerView |
|----------|-----------|--------------|
| Tái sử dụng View | ❌ | ✅ |
| Hiệu suất | Thấp | Cao |
| Layout tùy chỉnh | Giới hạn | Linh hoạt (Grid, Staggered, Linear) |
| Animation | Khó | Dễ thêm |

=> **RecyclerView tối ưu và linh hoạt hơn**.

---

## Adapter: Select 1 hoặc nhiều item
- **Select 1 item:** Lưu index hoặc ID được chọn.
- **Select nhiều item:** Lưu danh sách các item được chọn (List<Int> hoặc MutableSet<Int>).

```kotlin
val selectedItems = mutableSetOf<Int>()

fun onItemClicked(position: Int) {
    if (selectedItems.contains(position))
        selectedItems.remove(position)
    else
        selectedItems.add(position)
}
```

---

## Kiểm tra chuỗi có thể chuyển sang Int hay không

```kotlin
val text = "123a"
val number = text.toIntOrNull()
if (number != null) println("Là số") else println("Không phải số")
```

---

## LiveData và cập nhật UI
LiveData cho phép **quan sát dữ liệu**. Khi giá trị thay đổi, **UI tự cập nhật**.

```kotlin
viewModel.name.observe(viewLifecycleOwner) { newName ->
    binding.textView.text = newName
}
```

---

## Tính đóng gói: `internal val a private set`
- `internal`: Biến chỉ được truy cập trong module.
- `private set`: Chỉ class hiện tại được phép **thay đổi giá trị**.

```kotlin
internal val count = 0
    private set
```

=> Code ngoài class chỉ có thể đọc, không thể gán lại.

---

## Lateinit vs Lazy

| So sánh | `lateinit` | `lazy` |
|----------|-------------|---------|
| Dùng cho | var | val |
| Cho phép null? | Không | Có |
| Thích hợp cho | biến được khởi tạo sau (VD: ViewBinding) | khởi tạo lười, chỉ khi dùng lần đầu |
| Dùng được cho kiểu nguyên thủy? | ❌ | ✅ |

```kotlin
lateinit var name: String
val user by lazy { loadUser() }
```

---

## Coroutine Dispatcher: IO, Main, Default
- `Dispatchers.IO`: Dùng cho đọc/ghi file, gọi API.
- `Dispatchers.Main`: Dùng để cập nhật UI.
- `Dispatchers.Default`: Xử lý logic nặng (CPU-bound).

```kotlin
viewModelScope.launch(Dispatchers.IO) {
    val data = fetchData()
    withContext(Dispatchers.Main) {
        updateUI(data)
    }
}
```

---

## ViewGroup: FrameLayout, RelativeLayout, ConstraintLayout
| Layout | Đặc điểm |
|---------|-----------|
| **FrameLayout** | Các view con xếp chồng lên nhau. |
| **RelativeLayout** | Sắp xếp dựa trên vị trí tương đối giữa các view. |
| **ConstraintLayout** | Linh hoạt, tối ưu hiệu suất hơn RelativeLayout. |

```xml
<FrameLayout>
    <ImageView />
    <TextView />
</FrameLayout>
```

---

**Tổng kết:**  
File này giúp nắm vững nền tảng Android: kế thừa, đóng gói, layout, LiveData, Coroutine, Adapter và ViewModel.

## Vì sao lại dùng enum để khai báo các type của input?
- Khi khai báo kiểu truyền vào thì nó thuộc các type đó, không thể truyền text free vào đc. Nó sẽ báo lỗi và nghiêm ngặt code hơn.
