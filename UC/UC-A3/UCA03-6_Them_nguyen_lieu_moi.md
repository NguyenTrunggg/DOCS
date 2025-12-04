# UCA03-6: Thêm nguyên liệu mới [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin thêm một nguyên liệu mới vào CSDL chuẩn hoá để hệ thống nhận diện và người dùng có thể sử dụng khi tìm kiếm hoặc thêm công thức.

**Trigger:** Admin nhấn "Thêm nguyên liệu" trong trang "Quản lý Nguyên liệu".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Ingredient.Create"

**Post-Condition:**
- Nguyên liệu mới được lưu vào CSDL và xuất hiện trong danh sách

**Basic Flow:**
1. Admin nhấn "Thêm nguyên liệu"
2. Hệ thống hiển thị form nhập liệu:
   - a. Tên nguyên liệu (bắt buộc, chuẩn hoá, không dấu)
   - b. Đơn vị mặc định (vd: gram, ml, cái)
   - c. Danh mục (Rau củ, Thịt, Gia vị,...)
   - d. Tên đồng nghĩa (tùy chọn)
3. Admin nhập thông tin và nhấn "Lưu"
4. Hệ thống validate (tên duy nhất, độ dài hợp lệ)
5. Hệ thống lưu nguyên liệu và hiển thị thông báo "Đã thêm nguyên liệu"

**Alternative Flow:**
- **Nhập hàng loạt:** Import danh sách nguyên liệu từ CSV

- **Gợi ý từ dữ liệu người dùng:** Đề xuất nguyên liệu mới dựa trên tần suất nhập custom

**Exception Flow:**
- **Tên trùng:** Hiển thị "Nguyên liệu đã tồn tại"

- **Lỗi lưu CSDL:** Thông báo lỗi và không tạo mới

**Additional Information:**
- **Business Rule:**
  - Tên nguyên liệu duy nhất (case-insensitive, không dấu)
  - Có thể lưu alias để hỗ trợ tìm kiếm

- **Non-Functional Requirement:**
  - Performance: Lưu < 1 giây
  - Security: Chỉ Admin/Super Admin được phép

**Priority:** Medium  
**CRUD:** Create

