# Adapter Android

| Hàm                                         | Tác dụng                          | Khi nào dùng                                                             |
| ------------------------------------------- | --------------------------------- | ------------------------------------------------------------------------ |
| `notifyDataSetChanged()`                    | Refresh **toàn bộ danh sách**     | Khi thay đổi toàn bộ dữ liệu hoặc không biết chính xác item nào thay đổi |
| `notifyItemInserted(position)`              | Thêm 1 item tại vị trí `position` | Khi bạn thêm mới 1 phần tử vào danh sách                                 |
| `notifyItemRemoved(position)`               | Xóa 1 item tại vị trí `position`  | Khi bạn xóa phần tử khỏi danh sách                                       |
| `notifyItemChanged(position)`               | Cập nhật lại 1 item cụ thể        | Khi bạn sửa nội dung item đó                                             |
| `notifyItemMoved(fromPosition, toPosition)` | Di chuyển vị trí item             | Khi bạn thay đổi thứ tự item (drag & drop)                               |
| `notifyItemRangeInserted(start, count)`     | Thêm nhiều item liên tiếp         | Khi bạn thêm 1 danh sách nhỏ vào giữa                                    |
| `notifyItemRangeRemoved(start, count)`      | Xóa nhiều item liên tiếp          | Khi bạn xóa 1 dải item                                                   |
| `notifyItemRangeChanged(start, count)`      | Cập nhật nhiều item liên tiếp     | Khi bạn thay đổi dữ liệu của nhiều phần tử liền nhau 

- Data Model
```kt
data class User(val id: Int, var name: String)
```

- Adapter cơ bản
```kt
class UserAdapter(private val users: MutableList<User>) :
    RecyclerView.Adapter<UserAdapter.UserViewHolder>() {

    inner class UserViewHolder(val view: View) : RecyclerView.ViewHolder(view) {
        val tvName = view.findViewById<TextView>(R.id.tvName)
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): UserViewHolder {
        val view = LayoutInflater.from(parent.context)
            .inflate(R.layout.item_user, parent, false)
        return UserViewHolder(view)
    }

    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.tvName.text = users[position].name
    }

    override fun getItemCount() = users.size

    // ---- CRUD ----
    fun addUser(user: User) {
        users.add(user)
        notifyItemInserted(users.size - 1)
    }

    fun updateUser(position: Int, newName: String) {
        users[position].name = newName
        notifyItemChanged(position)
    }

    fun deleteUser(position: Int) {
        users.removeAt(position)
        notifyItemRemoved(position)
    }

    fun refreshAll(newList: List<User>) {
        users.clear()
        users.addAll(newList)
        notifyDataSetChanged()
    }
}

```
- Sử dụng Adapter trong Activity
```kt
class MainActivity : AppCompatActivity() {

    private lateinit var adapter: UserAdapter
    private val users = mutableListOf(
        User(1, "Mạnh"),
        User(2, "Hằng"),
        User(3, "Trinh")
    )

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        adapter = UserAdapter(users)
        val recyclerView = findViewById<RecyclerView>(R.id.recyclerView)
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = adapter

        // Thêm
        adapter.addUser(User(4, "Mới thêm"))

        // Sửa
        adapter.updateUser(1, "Hằng sửa")

        // Xóa
        adapter.deleteUser(2)

        // Làm mới toàn bộ
        adapter.refreshAll(listOf(User(1, "A"), User(2, "B"), User(3, "C")))
    }
}

``` 