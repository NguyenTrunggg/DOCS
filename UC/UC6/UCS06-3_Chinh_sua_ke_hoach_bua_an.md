# UCS06-3: Chỉnh sửa kế hoạch bữa ăn [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng thay đổi, di chuyển hoặc thay thế các món ăn trong kế hoạch bữa ăn đã tạo, áp dụng cho một ngày/bữa cụ thể hoặc cho cả phạm vi ngày.

**Trigger:** Người dùng nhấn nút "Chỉnh sửa" khi đang xem kế hoạch (UC6.2) hoặc từ danh sách kế hoạch.

**Pre-Condition:**
- Người dùng đã đăng nhập
- Kế hoạch bữa ăn tồn tại và thuộc về người dùng

**Post-Condition:**
- Kế hoạch được cập nhật theo các thay đổi
- Lịch sử chỉnh sửa được ghi lại (nếu có)

**Basic Flow:**
1. Người dùng mở kế hoạch bữa ăn (UC6.2) và nhấn "Chỉnh sửa"
2. Hệ thống chuyển sang chế độ chỉnh sửa (edit mode)
3. Người dùng thực hiện các thao tác:
   - a. Kéo/thả món giữa các bữa hoặc ngày
   - b. Thay thế món: tìm món khác và thay
   - c. Thêm món mới vào bữa
   - d. Xóa món khỏi bữa
   - e. Đổi khẩu phần cho món/bữa
4. Hệ thống hiển thị preview thay đổi ngay trên lịch
5. Người dùng nhấn "Lưu thay đổi"
6. Hệ thống validate dữ liệu và cập nhật kế hoạch
7. Hiển thị thông báo: "Cập nhật kế hoạch thành công"

**Alternative Flow:**
- **Chỉnh sửa theo batch:**
  - Áp dụng thay đổi cho nhiều ngày cùng lúc (VD: thay tất cả bữa trưa bằng món X)

- **Hoàn nguyên (Undo/Redo):**
  - Hỗ trợ hoàn nguyên thao tác gần nhất

- **Sao chép kế hoạch:**
  - Tạo một bản sao kế hoạch để chỉnh sửa mà không ảnh hưởng bản gốc

**Exception Flow:**
- **Xung đột dữ liệu:**
  - Nếu kế hoạch được chỉnh sửa đồng thời trên thiết bị khác
  - Hệ thống cảnh báo và yêu cầu tải lại kế hoạch

- **Lỗi lưu cơ sở dữ liệu:**
  - Nếu cập nhật thất bại
  - Hiển thị: "Không thể cập nhật. Vui lòng thử lại sau."

- **Session hết hạn:**
  - Tự động lưu nháp và yêu cầu đăng nhập lại

**Additional Information:**
- **Business Rule:**
  - Sau chỉnh sửa vẫn phải đảm bảo cấu trúc hợp lệ (ngày/bữa)
  - Khẩu phần có thể được kế thừa từ thiết lập kế hoạch

- **Non-Functional Requirement:**
  - Performance: Lưu thay đổi < 1 giây
  - Usability: Kéo/thả mượt, có Undo/Redo
  - Reliability: Tự động lưu nháp định kỳ

**Priority:** Medium  
**CRUD:** Update

