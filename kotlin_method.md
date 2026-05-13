 # Kotlin Cheat Sheet

## 1. Chuỗi (String)
| Phương thức | Mô tả | Ví dụ |
|------------|-------|-------|
| `length` | Độ dài chuỗi | `"Hello".length` → 5 |
| `toUpperCase()/toLowerCase()` | Chuyển chữ hoa/thường | `"Hello".toUpperCase()` → `"HELLO"` |
| `trim()` | Loại bỏ khoảng trắng đầu/cuối | `"  Hi  ".trim()` → `"Hi"` |
| `substring(start,end)` | Lấy chuỗi con | `"Hello".substring(1,4)` → `"ell"` |
| `contains("sub")` | Kiểm tra chứa substring | `"Hello".contains("ell")` → true |
| `replace(old,new)` | Thay thế ký tự/chuỗi | `"Hello".replace("l","x")` → `"Hexxo"` |
| `split(delimiter)` | Tách chuỗi thành List | `"a,b,c".split(",")` → `["a","b","c"]` |
| `startsWith()/endsWith()` | Kiểm tra đầu/cuối | `"Hello".startsWith("H")` → true |
| `isEmpty()/isBlank()` | Kiểm tra rỗng/giá trị trắng | `"  ".isBlank()` → true |

**Ví dụ:**
```kotlin
val email = "   user@example.com   "
val cleanEmail = email.trim().toLowerCase()
println(cleanEmail) // "user@example.com"
```

## 2. Số (Number: Int, Double, Float, Long)
| Phương thức / Toán tử | Mô tả | Ví dụ |
|---------------------|-------|-------|
| `+ - * / %` | Toán tử cơ bản | `5 % 2` → 1 |
| `toInt()/toDouble()/toFloat()/toLong()` | Chuyển kiểu | `"123".toInt()` → 123 |
| `range` | Tạo dải số | `(1..5).toList()` → `[1,2,3,4,5]` |
| `coerceIn(min,max)` | Giới hạn giá trị | `10.coerceIn(0,5)` → 5 |
| `plus()/minus()/times()/div()` | Toán tử phương thức | `5.plus(3)` → 8 |
| `random()` | Lấy số ngẫu nhiên | `(1..10).random()` → 1..10 |

**Ví dụ:**
```kotlin
val score = "95".toInt()
val limitedScore = score.coerceIn(0, 100)
println(limitedScore) // 95
```

## 3. Mảng & List (Array, List, MutableList)
| Phương thức | Mô tả | Ví dụ |
|------------|-------|-------|
| `size` | Độ dài | `arrayOf(1,2,3).size` → 3 |
| `get(index)` / `[index]` | Lấy giá trị | `arr[1]` → 2 |
| `set(index,value)` / `[index] = value` | Gán giá trị | `arr[0] = 10` |
| `first()/last()` | Phần tử đầu/cuối | `listOf(1,2,3).first()` → 1 |
| `indexOf(value)` | Vị trí phần tử | `list.indexOf(3)` → 2 |
| `contains(value)` | Kiểm tra tồn tại | `list.contains(2)` → true |
| `add(value)` | Thêm phần tử (MutableList) | `mutableList.add(4)` |
| `remove(value)` | Xóa phần tử (MutableList) | `mutableList.remove(2)` |
| `sort()/sorted()` | Sắp xếp | `listOf(3,1,2).sorted()` → `[1,2,3]` |
| `filter { condition }` | Lọc | `list.filter { it > 2 }` → `[3]` |
| `map { transformation }` | Biến đổi | `list.map { it*2 }` → `[2,4,6]` |
| `forEach { action }` | Duyệt | `list.forEach { println(it) }` |

**Ví dụ:**
```kotlin
val numbers = mutableListOf(1,2,3)
numbers.add(4)
val doubled = numbers.map { it*2 }  // [2,4,6,8]
val filtered = numbers.filter { it > 2 } // [3,4]
```

---

**Mẹo nhớ nhanh:**
- **String** → length, trim, split, substring, replace
- **Number** → toán học & chuyển đổi, coerceIn, random
- **Array/List** → size, [i], add/remove, map/filter/sort