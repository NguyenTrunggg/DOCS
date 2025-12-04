# UCS06-2: Xem kế hoạch bữa ăn [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng xem lại kế hoạch bữa ăn đã tạo theo dạng lịch (ngày/tuần/tháng), với đầy đủ thông tin món ăn theo từng bữa và các thao tác nhanh.

**Trigger:** Người dùng truy cập module "Kế hoạch bữa ăn" và chọn một kế hoạch trong danh sách.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công
- Đã có ít nhất một kế hoạch bữa ăn được tạo (hoặc hiển thị trang trống nếu chưa có)

**Post-Condition:**
- Kế hoạch được hiển thị đầy đủ theo thời gian đã chọn
- Người dùng có thể thực hiện các thao tác nhanh: xem chi tiết món, đổi bữa, xóa món

**Basic Flow:**
1. Người dùng mở module "Kế hoạch bữa ăn"
2. Hệ thống hiển thị danh sách các kế hoạch gần đây
3. Người dùng chọn một kế hoạch để xem
4. Hệ thống hiển thị kế hoạch theo dạng lịch:
   - a. Chọn chế độ xem: Ngày/Tuần/Tháng
   - b. Mỗi ô hiển thị các bữa: Sáng/Trưa/Tối/Bữa phụ
   - c. Mỗi món hiển thị tên, ảnh thumbnail, thời gian nấu, độ khó
5. Người dùng có thể click vào món để xem chi tiết (UC2.5)
6. Người dùng có thể dùng thao tác kéo/thả để đổi vị trí món giữa các bữa (không lưu ngay)
7. Có các thao tác nhanh:
   - a. Đổi bữa (move)
   - b. Xóa món (remove)
   - c. Sao chép món sang ngày khác (copy)
8. Có bộ lọc hiển thị:
   - a. Theo bữa (chỉ hiển thị bữa Trưa, v.v.)
   - b. Theo danh mục món
   - c. Theo độ khó
9. Nút "Chỉnh sửa" để chuyển sang UC6.3

**Alternative Flow:**
- **Xem nhiều kế hoạch:**
  - Người dùng có thể chọn so sánh 2 kế hoạch song song

- **In kế hoạch:**
  - Xuất ra PDF phiên bản tối ưu để in

- **Chia sẻ kế hoạch:**
  - Tạo link chia sẻ để gia đình cùng xem

**Exception Flow:**
- **Chưa có kế hoạch:**
  - Hiển thị: "Bạn chưa có kế hoạch bữa ăn nào"
  - Nút "Tạo kế hoạch" (UC6.1)

- **Lỗi tải dữ liệu:**
  - Hiển thị: "Không thể tải kế hoạch. Vui lòng thử lại sau."

- **Session hết hạn:**
  - Chuyển hướng đăng nhập, giữ ngữ cảnh kế hoạch

**Additional Information:**
- **Business Rule:**
  - Chỉ hiển thị kế hoạch của chính người dùng
  - Dữ liệu món ăn được lấy snapshot tại thời điểm lưu kế hoạch để đảm bảo nhất quán

- **Non-Functional Requirement:**
  - Performance: Tải kế hoạch < 2 giây
  - Usability: Lịch tương tác mượt, hỗ trợ kéo/thả
  - Security: Kế hoạch là dữ liệu riêng tư

**Priority:** Low  
**CRUD:** Read

