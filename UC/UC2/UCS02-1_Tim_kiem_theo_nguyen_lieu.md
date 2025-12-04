# UCS02-1: Tìm kiếm theo Nguyên liệu [HIGH PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng nhập danh sách các nguyên liệu mình có, hệ thống sẽ thực hiện thuật toán tìm kiếm thông minh để trả về các công thức phù hợp nhất, sắp xếp theo độ ưu tiên từ cao đến thấp.

**Trigger:** Người dùng truy cập trang tìm kiếm và chọn tab "Tìm theo nguyên liệu" hoặc nhấn nút "Tìm kiếm theo nguyên liệu".

**Pre-Condition:**
- Hệ thống đang hoạt động bình thường
- Cơ sở dữ liệu công thức có sẵn các công thức với thông tin nguyên liệu
- Người dùng có ít nhất một nguyên liệu để tìm kiếm

**Post-Condition:**
- Danh sách các công thức phù hợp được hiển thị, sắp xếp theo độ ưu tiên
- Kết quả tìm kiếm được lưu vào session để có thể lọc/sắp xếp tiếp
- Thống kê tìm kiếm được ghi lại (số lượng kết quả, thời gian tìm kiếm)
- Nếu không có kết quả, hệ thống đề xuất sử dụng AI để tạo công thức mới

**Basic Flow:**
1. Người dùng truy cập trang tìm kiếm từ menu chính hoặc trang chủ
2. Người dùng chọn tab "Tìm theo nguyên liệu" hoặc nhấn nút tương ứng
3. Người dùng nhập danh sách nguyên liệu mình có bằng một trong các cách:
   - a. Gõ tên nguyên liệu và chọn từ danh sách gợi ý (autocomplete)
   - b. Chọn từ danh sách nguyên liệu phổ biến
   - c. Nhập nhiều nguyên liệu cách nhau bằng dấu phẩy
4. Hệ thống hiển thị danh sách nguyên liệu đã chọn với khả năng xóa từng nguyên liệu
5. Người dùng có thể thêm/bớt nguyên liệu và nhấn nút "Tìm kiếm"
6. Hệ thống thực hiện thuật toán tìm kiếm thông minh với 3 mức ưu tiên:
   - a. Ưu tiên 1: Tìm công thức chứa TẤT CẢ nguyên liệu người dùng nhập
   - b. Ưu tiên 2: Tìm công thức chứa NHIỀU NHẤT nguyên liệu có thể (N-1 nguyên liệu)
   - c. Ưu tiên 3: Gợi ý công thức chỉ cần thêm 1-2 nguyên liệu phổ biến
7. Hệ thống sắp xếp kết quả theo độ ưu tiên và hiển thị danh sách công thức
8. Mỗi công thức hiển thị: ảnh đại diện, tên món, thời gian nấu, độ khó, số sao đánh giá
9. Người dùng có thể xem chi tiết công thức bằng cách nhấp vào từng món

**Alternative Flow:**
- **Tìm kiếm từ "Tủ lạnh ảo":**
  - Người dùng có thể import nguyên liệu từ danh sách "Tủ lạnh ảo" đã lưu
  - Hệ thống tự động điền các nguyên liệu vào form tìm kiếm
  - Tiếp tục luồng từ bước 5 của Basic Flow

- **Tìm kiếm nâng cao:**
  - Người dùng chọn "Tìm kiếm nâng cao"
  - Có thể thêm bộ lọc: loại món ăn, thời gian nấu, độ khó, khẩu phần
  - Hệ thống áp dụng thêm các bộ lọc vào thuật toán tìm kiếm

- **Lưu tìm kiếm:**
  - Nếu kết quả tốt, người dùng có thể lưu tìm kiếm này
  - Hệ thống lưu danh sách nguyên liệu và kết quả vào "Tìm kiếm đã lưu"

**Exception Flow:**
- **Không tìm thấy công thức nào:**
  - Nếu không có công thức nào phù hợp với nguyên liệu đã nhập
  - Hệ thống hiển thị thông báo: "Không tìm thấy công thức phù hợp với nguyên liệu này."
  - Hiển thị nút "Sử dụng AI để tạo công thức mới" (kích hoạt UC2.3)
  - Gợi ý một số nguyên liệu phổ biến khác để thêm vào

- **Nguyên liệu không hợp lệ:**
  - Nếu người dùng nhập nguyên liệu không có trong hệ thống
  - Hệ thống hiển thị gợi ý: "Không tìm thấy '[tên nguyên liệu]'. Bạn có thể tham khảo các nguyên liệu gần giống: [danh sách gợi ý]"
  - Người dùng có thể chọn từ gợi ý hoặc nhập lại

- **Quá ít nguyên liệu:**
  - Nếu người dùng chỉ nhập 1 nguyên liệu
  - Hệ thống hiển thị cảnh báo: "Với 1 nguyên liệu, kết quả có thể rất nhiều. Bạn có muốn thêm nguyên liệu khác không?"
  - Vẫn thực hiện tìm kiếm nhưng hiển thị cảnh báo

- **Lỗi thuật toán tìm kiếm:**
  - Nếu có lỗi trong quá trình xử lý thuật toán
  - Hệ thống hiển thị thông báo: "Đã xảy ra lỗi trong quá trình tìm kiếm. Vui lòng thử lại."
  - Cung cấp nút "Thử lại" để thực hiện tìm kiếm lại

- **Timeout tìm kiếm:**
  - Nếu tìm kiếm mất quá 10 giây
  - Hệ thống hiển thị: "Tìm kiếm đang mất quá nhiều thời gian. Bạn có muốn thử với ít nguyên liệu hơn không?"
  - Cung cấp tùy chọn hủy hoặc tiếp tục chờ

**Additional Information:**
- **Business Rule:**
  - Thuật toán tìm kiếm ưu tiên công thức có tỷ lệ nguyên liệu khớp cao nhất
  - Công thức có tất cả nguyên liệu được đánh giá 100% phù hợp
  - Công thức thiếu 1 nguyên liệu được đánh giá 80-90% phù hợp
  - Công thức thiếu 2 nguyên liệu được đánh giá 60-80% phù hợp
  - Nguyên liệu phổ biến (gia vị, dầu ăn, muối) được ưu tiên thấp hơn
  - Kết quả tìm kiếm được cache trong 30 phút để tăng tốc độ
  - Tối đa hiển thị 50 kết quả mỗi trang, có phân trang

- **Non-Functional Requirement:**
  - Performance: Thời gian tìm kiếm phải dưới 5 giây cho tối đa 20 nguyên liệu
  - Usability: Giao diện tìm kiếm phải trực quan, có autocomplete và gợi ý
  - Security: Truy vấn tìm kiếm phải được sanitize để tránh SQL injection
  - Reliability: Hệ thống phải hoạt động ổn định ngay cả khi có nhiều người dùng tìm kiếm đồng thời

**Priority:** High  
**CRUD:** Read

