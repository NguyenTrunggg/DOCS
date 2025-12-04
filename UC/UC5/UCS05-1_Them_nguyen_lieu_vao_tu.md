# UCS05-1: Thêm nguyên liệu vào tủ [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng thêm một loại nguyên liệu mới vào danh sách nguyên liệu hiện có trong "Tủ lạnh ảo" của mình để quản lý tồn kho cá nhân.

**Trigger:** Người dùng đã đăng nhập nhấn nút "Thêm nguyên liệu" trên trang "Tủ lạnh ảo".

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Hệ thống có danh mục nguyên liệu chuẩn hóa để gợi ý/autocomplete

**Post-Condition:**
- Nguyên liệu mới được thêm vào danh sách trong "Tủ lạnh ảo" của người dùng
- Số lượng và đơn vị được lưu lại chính xác
- Thời gian thêm được ghi lại để audit/sắp xếp
- Danh sách hiển thị được cập nhật real-time

**Basic Flow:**
1. Người dùng truy cập trang "Tủ lạnh ảo" (UC5)
2. Người dùng nhấn nút "Thêm nguyên liệu"
3. Hệ thống hiển thị form thêm nguyên liệu với các trường:
   - a. Tên nguyên liệu (autocomplete từ danh mục chuẩn hóa)
   - b. Số lượng (số thập phân, >= 0)
   - c. Đơn vị (vd: gram, kg, ml, lít, trái, bó)
   - d. Ngày hết hạn (tùy chọn)
   - e. Ghi chú (tùy chọn)
4. Người dùng nhập thông tin và nhấn "Lưu"
5. Hệ thống validate dữ liệu đầu vào (tên hợp lệ, số lượng > 0, đơn vị hợp lệ)
6. Hệ thống thêm bản ghi nguyên liệu vào kho cá nhân của người dùng
7. Hệ thống cập nhật giao diện danh sách nguyên liệu
8. Hệ thống hiển thị thông báo: "Đã thêm nguyên liệu thành công"

**Alternative Flow:**
- **Thêm nhanh từ gợi ý phổ biến:**
  - Hệ thống hiển thị danh sách nguyên liệu phổ biến
  - Người dùng chọn nhanh và nhập số lượng/đơn vị
  - Tiếp tục từ bước 6 của Basic Flow

- **Quét mã vạch (barcode):**
  - Người dùng sử dụng camera/thiết bị quét mã vạch
  - Hệ thống nhận diện sản phẩm và gợi ý nguyên liệu, đơn vị
  - Người dùng xác nhận và nhập số lượng

- **Nhập hàng loạt:**
  - Người dùng mở modal nhập nhiều dòng (CSV đơn giản)
  - Hệ thống parse và hiển thị preview
  - Người dùng xác nhận để thêm hàng loạt

**Exception Flow:**
- **Nguyên liệu không hợp lệ/không tồn tại trong danh mục:**
  - Hệ thống gợi ý nguyên liệu gần đúng hoặc cho phép tạo nguyên liệu custom (đánh dấu)
  - Hiển thị cảnh báo: "Nguyên liệu này chưa được chuẩn hóa"

- **Trùng nguyên liệu:**
  - Nếu nguyên liệu đã tồn tại trong danh sách
  - Hệ thống hỏi: "Gộp số lượng vào mục có sẵn?"
  - Nếu đồng ý, cộng dồn số lượng và cập nhật ngày hết hạn theo quy tắc

- **Giá trị số lượng không hợp lệ:**
  - Nếu số lượng <= 0 hoặc không phải số
  - Hệ thống hiển thị lỗi và yêu cầu nhập lại

- **Lỗi lưu cơ sở dữ liệu:**
  - Nếu không thể lưu do lỗi hệ thống
  - Hiển thị: "Không thể thêm nguyên liệu. Vui lòng thử lại sau."

**Additional Information:**
- **Business Rule:**
  - Tên nguyên liệu ưu tiên chọn từ danh mục chuẩn hóa để đồng nhất tìm kiếm
  - Hỗ trợ nhiều đơn vị, có quy tắc quy đổi nội bộ (vd: 1000g = 1kg)
  - Cho phép tạo nguyên liệu custom nhưng cần review sau
  - Nếu gộp, ngày hết hạn lấy ngày gần nhất (FEFO) hoặc cho người dùng chọn
  - Lưu lịch sử thay đổi (audit trail) cho từng nguyên liệu

- **Non-Functional Requirement:**
  - Performance: Thêm nguyên liệu < 1 giây
  - Usability: Autocomplete mượt, hỗ trợ nhập nhanh bàn phím
  - Security: Chỉ chủ sở hữu được phép thao tác tủ cá nhân
  - Reliability: Dữ liệu được lưu nhất quán, không trùng lặp

**Priority:** Low  
**CRUD:** Create

