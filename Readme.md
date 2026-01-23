![App Screenshot](/images/elementAndroid.png)

1. viewModelScope vs lifecycleScope: 
Nếu bạn chỉ nhớ “cái này cho ViewModel, cái kia cho Fragment” → chưa đủ
Phải hiểu scope gắn với cái gì và chết khi nào
```kt
Logic / data / API → viewModelScope
Render / collect / animation → lifecycleScope
```
2. 

