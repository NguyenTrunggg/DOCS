# UCA02-2: Thêm công thức hệ thống [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin tạo một công thức mới với tư cách là nguồn chính thức của hệ thống. Công thức do Admin tạo mặc định ở trạng thái "Đã duyệt".

**Trigger:** Admin nhấn nút "Thêm công thức mới" trong mục "Quản lý Công thức".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Recipe.Create"
- Có danh mục/đơn vị/nguyên liệu chuẩn hoá để lựa chọn

**Post-Condition:**
- Công thức mới được lưu vào CSDL với trạng thái "Đã duyệt"
- Công thức xuất hiện trong danh sách công khai

**Basic Flow:**
1. Admin nhấn "Thêm công thức mới"
2. Hệ thống hiển thị form nhập liệu chi tiết:
   - a. Tên món, mô tả ngắn
   - b. Ảnh/video đại diện
   - c. Danh mục món ăn
   - d. Thời gian chuẩn bị/nấu, độ khó, khẩu phần
   - e. Danh sách nguyên liệu (tên, định lượng, đơn vị)
   - f. Các bước thực hiện (có thể kèm ảnh)
3. Admin nhập đầy đủ thông tin và nhấn "Lưu/Xuất bản"
4. Hệ thống validate dữ liệu, upload media nếu có
5. Hệ thống lưu công thức với trạng thái "Đã duyệt" và gắn Admin là người tạo
6. Hiển thị thông báo: "Đã tạo công thức thành công"

**Alternative Flow:**
- **Lưu nháp:** Lưu ở trạng thái "Nháp" để chỉnh sửa sau

- **Nhập từ file:** Import từ file JSON/CSV theo format chuẩn

- **Sao chép từ công thức có sẵn:** Copy và chỉnh sửa nội dung

**Exception Flow:**
- **Thiếu thông tin bắt buộc:** Hiển thị danh sách trường thiếu và yêu cầu bổ sung

- **Ảnh/video không hợp lệ:** Báo lỗi kích thước/định dạng

- **Lỗi lưu CSDL:** Thông báo lỗi và không tạo công thức

**Additional Information:**
- **Business Rule:**
  - Tên món phải duy nhất trong phạm vi công thức hệ thống
  - Phải có tối thiểu 3 nguyên liệu và 3 bước
  - Media phải đạt chuẩn kích thước/định dạng

- **Non-Functional Requirement:**
  - Performance: Lưu < 3 giây (không tính upload lớn)
  - Security: Chỉ Admin/Super Admin được phép
  - Reliability: Lưu nháp tự động định kỳ

**Priority:** Medium  
**CRUD:** Create

