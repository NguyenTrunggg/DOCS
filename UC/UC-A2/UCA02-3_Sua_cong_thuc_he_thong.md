# UCA02-3: Sửa công thức hệ thống [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin chỉnh sửa thông tin của một công thức bất kỳ trên hệ thống, bao gồm nội dung, ảnh, nguyên liệu và bước thực hiện.

**Trigger:** Admin chọn một công thức và nhấn nút "Sửa" trong mục "Quản lý Công thức" (UCA02-1).

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Recipe.Update"
- Công thức tồn tại trong hệ thống

**Post-Condition:**
- Các thay đổi của công thức được lưu lại
- Thời gian cập nhật được ghi nhận

**Basic Flow:**
1. Admin mở chi tiết công thức và nhấn "Sửa"
2. Hệ thống hiển thị form với dữ liệu đã điền sẵn
3. Admin chỉnh sửa các thông tin cần thiết:
   - a. Tên món, mô tả, ảnh/video
   - b. Danh mục, thời gian, độ khó, khẩu phần
   - c. Danh sách nguyên liệu (thêm/bớt/sửa)
   - d. Các bước thực hiện (thêm/bớt/sửa/sắp xếp)
4. Admin nhấn "Lưu"
5. Hệ thống validate và cập nhật vào CSDL
6. Hiển thị thông báo: "Cập nhật công thức thành công"

**Alternative Flow:**
- **Lưu nháp:** Lưu thay đổi ở trạng thái nháp

- **Versioning:** Lưu phiên bản cũ để có thể hoàn nguyên

**Exception Flow:**
- **Không có quyền:** Thông báo "Bạn không có quyền sửa công thức"

- **Dữ liệu không hợp lệ:** Hiển thị danh sách lỗi validation

- **Lỗi CSDL:** Thông báo lỗi, không cập nhật thay đổi

**Additional Information:**
- **Business Rule:**
  - Thay đổi phải đảm bảo tối thiểu 3 nguyên liệu và 3 bước
  - Ảnh/video phải đạt tiêu chuẩn

- **Non-Functional Requirement:**
  - Usability: Giữ nguyên layout và validation giống khi tạo mới
  - Reliability: Lưu lịch sử thay đổi để audit

**Priority:** Medium  
**CRUD:** Update

