# UCS06-1: Tạo kế hoạch bữa ăn [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng tạo một kế hoạch bữa ăn mới (theo ngày/tuần/tháng), thêm các món ăn vào lịch theo ngày và bữa (sáng, trưa, tối), nhằm quản lý thực đơn khoa học và tiện lợi.

**Trigger:** Người dùng nhấn nút "Tạo kế hoạch" từ module "Kế hoạch bữa ăn".

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Hệ thống có dữ liệu công thức để lựa chọn

**Post-Condition:**
- Một kế hoạch bữa ăn mới được lưu với các món đã chọn theo ngày/bữa
- Thời gian tạo kế hoạch được ghi lại
- Người dùng có thể xem/chỉnh sửa/xóa kế hoạch

**Basic Flow:**
1. Người dùng mở module "Kế hoạch bữa ăn"
2. Người dùng nhấn "Tạo kế hoạch"
3. Hệ thống hiển thị form tạo kế hoạch gồm:
   - a. Tên kế hoạch (tùy chọn)
   - b. Khoảng thời gian (từ ngày - đến ngày)
   - c. Chu kỳ hiển thị (Ngày/Tuần/Tháng)
   - d. Số khẩu phần mặc định
4. Người dùng chọn ngày trên lịch và thêm món ăn cho từng bữa:
   - a. Sáng, Trưa, Tối (có thể thêm bữa phụ)
   - b. Tìm và chọn món từ tìm kiếm (UC2.1/UC2.2)
   - c. Tùy chỉnh khẩu phần cho từng món
5. Hệ thống hiển thị preview kế hoạch theo lịch
6. Người dùng nhấn "Lưu kế hoạch"
7. Hệ thống validate dữ liệu (khoảng thời gian hợp lệ, có ít nhất 1 món)
8. Hệ thống lưu kế hoạch, gắn với người dùng
9. Hiển thị thông báo: "Đã tạo kế hoạch bữa ăn thành công"

**Alternative Flow:**
- **Tạo từ template có sẵn:**
  - Người dùng chọn một template thực đơn mẫu (VD: Eat Clean 7 ngày)
  - Hệ thống tự điền các món theo ngày/bữa
  - Người dùng có thể chỉnh sửa trước khi lưu

- **Tạo từ danh sách yêu thích:**
  - Người dùng chọn nhanh các món từ UC4 (Yêu thích)
  - Kéo/thả vào lịch để sắp xếp

- **Tạo bằng AI gợi ý thực đơn:**
  - Người dùng nhập mục tiêu (giảm cân, tăng cơ) và số ngày
  - Hệ thống gọi AI gợi ý thực đơn theo mục tiêu
  - Người dùng xem và chỉnh sửa trước khi lưu

**Exception Flow:**
- **Khoảng thời gian không hợp lệ:**
  - Nếu ngày kết thúc trước ngày bắt đầu
  - Hệ thống hiển thị lỗi và yêu cầu chọn lại

- **Không có món nào trong kế hoạch:**
  - Nếu người dùng chưa thêm món nào
  - Hệ thống yêu cầu thêm ít nhất 1 món

- **Lỗi lưu cơ sở dữ liệu:**
  - Nếu không thể lưu kế hoạch
  - Hiển thị: "Không thể lưu kế hoạch. Vui lòng thử lại sau."

- **Session hết hạn:**
  - Nếu session hết hạn
  - Chuyển hướng đăng nhập và giữ bản nháp kế hoạch

**Additional Information:**
- **Business Rule:**
  - Một kế hoạch phải có ít nhất 1 món
  - Hỗ trợ nhiều bữa/ngày, có thể thêm bữa phụ
  - Khẩu phần có thể tùy chỉnh theo bữa hoặc theo kế hoạch
  - Có thể lưu thành template để tái sử dụng

- **Non-Functional Requirement:**
  - Performance: Lưu kế hoạch < 2 giây
  - Usability: Giao diện kéo/thả, xem theo lịch tuần/tháng
  - Security: Kế hoạch riêng tư theo người dùng
  - Reliability: Tự động lưu nháp định kỳ

**Priority:** Medium  
**CRUD:** Create

