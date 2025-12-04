# UCA02-5: Xem danh sách công thức chờ duyệt [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xem danh sách các công thức do người dùng gửi lên đang chờ duyệt, kèm tìm kiếm, lọc và phân trang.

**Trigger:** Admin truy cập mục "Kiểm duyệt" hoặc bộ lọc trạng thái = "Chờ duyệt" trong "Quản lý Công thức".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Recipe.Moderate"

**Post-Condition:**
- Danh sách công thức chờ duyệt được hiển thị
- Admin có thể mở chi tiết để phê duyệt/từ chối

**Basic Flow:**
1. Admin mở trang "Công thức chờ duyệt"
2. Hệ thống truy vấn danh sách công thức có trạng thái "Chờ duyệt"
3. Hệ thống hiển thị các cột:
   - a. ID, Tên công thức
   - b. Người gửi (User)
   - c. Ngày gửi
   - d. Trạng thái hiện tại
4. Công cụ hỗ trợ:
   - a. Tìm kiếm theo tên
   - b. Lọc theo người gửi
   - c. Sắp xếp theo ngày gửi
5. Admin nhấp vào một công thức để xem chi tiết và thực hiện phê duyệt (UCA02-6) hoặc từ chối (UCA02-7)

**Alternative Flow:**
- **Phân công kiểm duyệt viên:** Gán công thức cho kiểm duyệt viên cụ thể

- **Xuất danh sách chờ duyệt:** Xuất CSV để tổng hợp

**Exception Flow:**
- **Không có công thức chờ duyệt:** Hiển thị: "Không có công thức nào trong hàng đợi"

- **Lỗi tải dữ liệu:** Thông báo lỗi và gợi ý thử lại

**Additional Information:**
- **Business Rule:**
  - Chỉ công thức do người dùng gửi mới có trạng thái "Chờ duyệt"
  - Mỗi công thức chỉ được duyệt bởi 1 admin tại một thời điểm (lock)

- **Non-Functional Requirement:**
  - Performance: Tải trang < 2 giây
  - Security: Chỉ người có quyền kiểm duyệt mới truy cập được

**Priority:** Low  
**CRUD:** Read

