# UCS05-2: Xem danh sách nguyên liệu trong tủ [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng xem toàn bộ các nguyên liệu hiện có trong "Tủ lạnh ảo" của mình, bao gồm tên, số lượng, đơn vị, ngày hết hạn và ghi chú.

**Trigger:** Người dùng đã đăng nhập truy cập trang "Tủ lạnh ảo" từ menu hoặc trang chủ.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Trong tủ có thể có hoặc chưa có nguyên liệu

**Post-Condition:**
- Danh sách nguyên liệu được hiển thị đầy đủ và cập nhật
- Người dùng có thể lọc, sắp xếp, tìm kiếm nguyên liệu
- Thống kê tổng quan được hiển thị (tổng số mục, sắp hết hạn, đã hết hạn)

**Basic Flow:**
1. Người dùng truy cập trang "Tủ lạnh ảo"
2. Hệ thống truy vấn kho nguyên liệu của người dùng
3. Hệ thống hiển thị danh sách theo bảng hoặc thẻ (cards) với các cột:
   - a. Tên nguyên liệu
   - b. Số lượng
   - c. Đơn vị
   - d. Ngày hết hạn (nếu có)
   - e. Trạng thái (Còn dùng/ Sắp hết hạn/ Đã hết hạn)
   - f. Ghi chú (nếu có)
   - g. Hành động (Sửa, Xóa)
4. Thanh công cụ cung cấp:
   - a. Tìm kiếm theo tên nguyên liệu (autocomplete)
   - b. Lọc theo trạng thái (Tất cả/ Sắp hết hạn/ Đã hết hạn)
   - c. Lọc theo danh mục (Rau củ, Thịt, Gia vị... nếu có)
   - d. Sắp xếp theo tên, ngày hết hạn, ngày thêm, số lượng
5. Hệ thống hiển thị màu cảnh báo cho nguyên liệu sắp hết hạn/đã hết hạn
6. Người dùng có thể nhấn "Thêm nguyên liệu" (UC5.1) hoặc "Cập nhật" (UC5.3) hoặc "Xóa" (UC5.4)

**Alternative Flow:**
- **Xem dạng lưới (grid) hoặc danh sách (list):**
  - Người dùng chuyển đổi view giữa grid và list
  - Hệ thống ghi nhớ lựa chọn cho lần sau

- **Xuất danh sách:**
  - Người dùng xuất danh sách ra PDF/Excel để in hoặc chia sẻ

- **Đồng bộ từ thiết bị khác:**
  - Dữ liệu được đồng bộ real-time nếu người dùng mở trên nhiều thiết bị

**Exception Flow:**
- **Chưa có nguyên liệu:**
  - Hệ thống hiển thị trang trống với thông báo: "Tủ lạnh ảo của bạn đang trống."
  - Gợi ý: "Nhấn 'Thêm nguyên liệu' để bắt đầu" và link đến UC5.1

- **Lỗi tải dữ liệu:**
  - Nếu xảy ra lỗi khi truy vấn dữ liệu
  - Hiển thị: "Không thể tải danh sách. Vui lòng thử lại sau."
  - Cung cấp nút "Làm mới"

- **Session hết hạn:**
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."

**Additional Information:**
- **Business Rule:**
  - Chỉ hiển thị nguyên liệu của chính người dùng
  - Trạng thái hết hạn tính theo ngày hiện tại (FEFO)
  - Lưu lựa chọn sắp xếp/lọc của người dùng cho lần truy cập sau
  - Phân trang 50 mục/trang để tối ưu hiệu suất

- **Non-Functional Requirement:**
  - Performance: Thời gian tải danh sách < 2 giây cho 500 mục
  - Usability: Hỗ trợ bàn phím, truy cập nhanh, hiển thị cảnh báo rõ ràng
  - Security: Dữ liệu tách biệt theo người dùng
  - Reliability: Đồng bộ real-time không xung đột

**Priority:** Low  
**CRUD:** Read

