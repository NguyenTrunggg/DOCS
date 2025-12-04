# UCA05-1: Xem báo cáo, thống kê [HIGH PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xem các báo cáo và biểu đồ thống kê về hoạt động của hệ thống (người dùng, công thức, tương tác), hỗ trợ ra quyết định và theo dõi sức khỏe hệ thống.

**Trigger:** Admin truy cập mục "Dashboard & Thống kê" trong bảng điều khiển quản trị.

**Pre-Condition:**
- Admin đã đăng nhập và có quyền truy cập báo cáo (`Report.Read`)
- Hệ thống có dữ liệu thống kê được tổng hợp định kỳ hoặc real-time

**Post-Condition:**
- Các báo cáo, biểu đồ hiển thị theo phạm vi thời gian và bộ lọc đã chọn
- Admin có thể xuất báo cáo hoặc đào sâu (drill-down) vào dữ liệu chi tiết

**Basic Flow:**
1. Admin mở trang "Dashboard & Thống kê"
2. Hệ thống hiển thị tổng quan (KPIs):
   - a. Tổng số người dùng, người dùng mới (theo ngày/tuần/tháng)
   - b. Tổng số công thức, công thức mới
   - c. Tỷ lệ phê duyệt công thức
   - d. Số lượt xem, yêu thích, bình luận
3. Khu vực biểu đồ tương tác:
   - a. Biểu đồ đường: tăng trưởng người dùng theo thời gian
   - b. Biểu đồ cột: số công thức tạo mới theo danh mục
   - c. Biểu đồ tròn: phân bổ trạng thái công thức (Đã duyệt/Chờ duyệt/Bị từ chối)
   - d. Heatmap: hoạt động theo khung giờ/ngày trong tuần
4. Bộ lọc báo cáo:
   - a. Khoảng thời gian (7 ngày, 30 ngày, tùy chọn)
   - b. Theo danh mục, theo vai trò người dùng, theo trạng thái công thức
5. Admin có thể nhấp vào một KPI/biểu đồ để drill-down xem danh sách chi tiết
6. Admin có thể xuất báo cáo (PDF/CSV) theo bộ lọc hiện tại
7. Hệ thống lưu lựa chọn bộ lọc ưa thích

**Alternative Flow:**
- **Báo cáo tùy chỉnh:**
  - Admin tạo report tùy biến với các trường và biểu đồ chọn lọc
  - Lưu template report để dùng lại

- **Lên lịch gửi báo cáo:**
  - Thiết lập lịch gửi email báo cáo định kỳ (hàng tuần/tháng)

- **Realtime dashboard:**
  - Hiển thị số liệu realtime cho một số chỉ số quan trọng

**Exception Flow:**
- **Không có dữ liệu trong phạm vi:**
  - Hiển thị "Không có dữ liệu cho khoảng thời gian đã chọn"

- **Lỗi tải dữ liệu:**
  - Thông báo: "Không thể tải dữ liệu thống kê. Vui lòng thử lại sau."

- **Thiếu quyền:**
  - Chuyển hướng hoặc thông báo quyền truy cập không hợp lệ

**Additional Information:**
- **Business Rule:**
  - Số liệu phải khớp với nguồn dữ liệu chính (consistency)
  - Báo cáo phải có timestamp dữ liệu

- **Non-Functional Requirement:**
  - Performance: Truy vấn và hiển thị dashboard < 3 giây (với dữ liệu đã được tổng hợp)
  - Usability: Biểu đồ tương tác, hỗ trợ hover để xem chi tiết
  - Security: Phân quyền truy cập báo cáo theo vai trò
  - Reliability: Hệ thống dự phòng khi dịch vụ thống kê lỗi

**Priority:** High  
**CRUD:** Read

