# UCA02-7: Từ chối công thức [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin từ chối công thức do người dùng gửi lên khi nội dung không đạt yêu cầu, đồng thời gửi lý do từ chối cho người gửi.

**Trigger:** Admin mở chi tiết công thức ở trạng thái "Chờ duyệt" và nhấn "Từ chối".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Recipe.Moderate"
- Công thức ở trạng thái "Chờ duyệt"

**Post-Condition:**
- Trạng thái công thức chuyển sang "Bị từ chối"
- Lý do từ chối được lưu và gửi cho người dùng
- Công thức không được công khai

**Basic Flow:**
1. Admin mở chi tiết công thức chờ duyệt (UCA02-5)
2. Admin nhấn nút "Từ chối"
3. Hệ thống hiển thị hộp thoại yêu cầu nhập lý do từ chối
4. Admin nhập lý do (bắt buộc) và nhấn "Xác nhận"
5. Hệ thống cập nhật trạng thái công thức thành "Bị từ chối"
6. Hệ thống lưu lý do từ chối và ghi log kiểm duyệt
7. Hệ thống gửi thông báo cho người dùng kèm lý do từ chối
8. Hiển thị thông báo: "Đã từ chối công thức"

**Alternative Flow:**
- **Mẫu lý do nhanh:** Cung cấp các mẫu lý do phổ biến để chọn nhanh (VD: Hình ảnh mờ, Hướng dẫn không rõ ràng)

- **Gợi ý chỉnh sửa:** Đề xuất gợi ý để người dùng chỉnh sửa và gửi lại

**Exception Flow:**
- **Thiếu quyền:** Hiển thị: "Bạn không có quyền từ chối"

- **Trạng thái không hợp lệ:** Công thức không ở trạng thái "Chờ duyệt"

- **Lỗi cập nhật CSDL:** Thông báo lỗi, không đổi trạng thái

**Additional Information:**
- **Business Rule:**
  - Lý do từ chối phải rõ ràng, tôn trọng
  - Có thể đính kèm gợi ý chỉnh sửa

- **Non-Functional Requirement:**
  - Security: Audit đầy đủ hành động kiểm duyệt
  - Reliability: Thông báo gửi đi ngay lập tức

**Priority:** Medium  
**CRUD:** Update

