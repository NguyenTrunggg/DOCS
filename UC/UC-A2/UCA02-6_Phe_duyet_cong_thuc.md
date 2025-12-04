# UCA02-6: Phê duyệt công thức [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin duyệt và cho phép công thức do người dùng gửi lên được hiển thị công khai trên hệ thống sau khi kiểm tra chất lượng nội dung.

**Trigger:** Admin mở chi tiết một công thức đang ở trạng thái "Chờ duyệt" và nhấn nút "Phê duyệt".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Recipe.Moderate"
- Công thức đang ở trạng thái "Chờ duyệt"

**Post-Condition:**
- Trạng thái công thức chuyển sang "Đã duyệt"
- Công thức hiển thị công khai trong hệ thống
- Người dùng gửi công thức nhận thông báo đã được duyệt

**Basic Flow:**
1. Admin mở chi tiết công thức chờ duyệt (UCA02-5)
2. Hệ thống hiển thị đầy đủ nội dung: tên, mô tả, ảnh/video, nguyên liệu, bước thực hiện
3. Admin kiểm tra chất lượng nội dung (độ rõ ràng, phù hợp, không vi phạm)
4. Admin nhấn "Phê duyệt"
5. Hệ thống cập nhật trạng thái công thức thành "Đã duyệt"
6. Hệ thống ghi lại log kiểm duyệt (ai duyệt, thời gian)
7. Hệ thống gửi thông báo cho người dùng gửi công thức
8. Hiển thị thông báo: "Đã phê duyệt công thức"

**Alternative Flow:**
- **Chỉnh sửa nhẹ trước khi duyệt:** Admin chỉnh sửa lỗi chính tả/định dạng nhỏ rồi duyệt

- **Phân công kiểm duyệt:** Gán cho kiểm duyệt viên khác nếu cần chuyên môn

**Exception Flow:**
- **Thiếu quyền:** Thông báo "Bạn không có quyền phê duyệt"

- **Trạng thái không hợp lệ:** Công thức không ở "Chờ duyệt" -> thông báo lỗi

- **Lỗi cập nhật CSDL:** Thông báo lỗi, không thay đổi trạng thái

**Additional Information:**
- **Business Rule:**
  - Nội dung phải tuân thủ quy định cộng đồng và pháp luật
  - Ảnh/video phải phù hợp, không phản cảm

- **Non-Functional Requirement:**
  - Security: Audit đầy đủ các hành động kiểm duyệt
  - Reliability: Trạng thái đồng bộ tức thời

**Priority:** Medium  
**CRUD:** Update

