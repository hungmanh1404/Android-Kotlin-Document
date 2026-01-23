# Từ khoá & câu kích hoạt giúp AI viết prompt hiệu quả hơn 

## Dưới đây là một bộ từ/ cụm từ — theo nhóm — bạn có thể chèn vào prompt để khiến AI trả lời sâu, có cấu trúc, thực tế và dễ dùng lại. Mình cũng kèm mẫu prompt cải tiến để bạn copy — paste ngay được.

1. Vai trò (Role)

- “Bạn là một chuyên gia …” — (ví dụ: chuyên gia product, dev senior, giáo viên tiếng Anh, copywriter quảng cáo)

2. “Act as a …” / “Hãy đóng vai…”

- Quy trình & phương pháp (Process / Method)

3. “Step by step” / “Từng bước một”

4. Walk me through” / “Hướng dẫn chi tiết”

- “Show the steps and reasoning” (dùng cẩn thận — yêu cầu giải thích; AI nên đưa các bước thay vì nội tâm)

5. “Algorithm” / “Thuật toán”

6. “Checklist” / “Danh sách kiểm tra”

7. Độ sâu & phân tích (Depth / Analysis)

8. “In-depth” / “Phân tích sâu”

9. “Compare and contrast” / “So sánh — đối chiếu”

10. “Root cause” / “Nguyên nhân gốc rễ”

11. “Edge cases” / “Các trường hợp biên”

12. Đầu ra & định dạng (Output / Format)

13. “Provide output as:” (bullet list, table, JSON, markdown, step-by-step plan)

14. “Give examples” / “Kèm ví dụ”

15. “Include sample code” / “Kèm đoạn mã mẫu”

16. “Summarize in X words” / “Tóm tắt ≤ X từ”

17. “Provide templates” / “Cho mẫu”

18. Ràng buộc & giả định (Constraints / Assumptions)

19. “Assume …” / “Giả sử …”

20. “Limit to …” / “Giới hạn ở …”

21. “Use only … (libraries, standards)”

22. “No jargon” / “Không dùng thuật ngữ chuyên sâu”

23. Phong cách & giọng điệu (Tone / Style)

- “Professional”, “Casual”, “Persuasive”, “Concise”, “Humorous”

- “Write for [audience]” — (junior devs, product managers, general public, kids)

24. Kiểm chứng & nguồn (Verification / Sources)

25. “Cite sources” / “Trích dẫn nguồn”

26. “Provide references and links” (nếu cần tìm trên web)

27. “Give confidence level” / “Đánh giá độ chắc chắn (high/medium/low)”

28. Lặp & tối ưu hoá (Iteration)

29. “Refine” / “Tối ưu”

30. “Provide 3 alternatives” / “Cho 3 phương án”

31. “Explain trade-offs” / “Giải thích đánh đổi”

## Prompt Framework 4 Lớp để AI làm đúng

```xml
Bạn là Senior [Lĩnh vực].

Từ giờ trở đi, mỗi khi tôi gửi file, tôi sẽ dùng:
- "FILE START <tên file>"
- "FILE END"

Bạn phải lưu lại nội dung mỗi file và dùng đúng 100% khi sinh code hoặc sửa code.

==========================================
CONTEXT (bối cảnh dự án):
[Điền kiến trúc, ngôn ngữ, pattern, constraint]

==========================================
BUSINESS RULE:
[Điền logic nghiệp vụ cần tuân thủ]

==========================================
GOAL (mục tiêu):
[Điền rõ bạn muốn AI tạo file mới, sửa file nào, logic cụ thể]

==========================================
RESPONSE RULES (quy tắc bắt buộc):
1. Không tự đổi tên class/field/function.
2. Không giả định thông tin nếu tôi chưa cung cấp.
3. Nếu thông tin thiếu → hỏi lại tôi.
4. Output 100% dạng code block.
5. Không giải thích dài dòng.
6. Luôn dựa trên file tôi đã gửi.

```