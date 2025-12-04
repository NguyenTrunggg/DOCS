# UCS02-2: Tìm kiếm theo Tên món ăn [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng nhập tên món ăn vào ô tìm kiếm, hệ thống sẽ tìm và hiển thị các công thức có tên khớp hoặc liên quan nhất đến từ khóa, hỗ trợ tìm kiếm bằng tiếng Việt có dấu và không dấu.

**Trigger:** Người dùng nhập tên món ăn vào ô tìm kiếm và nhấn nút "Tìm kiếm" hoặc nhấn phím Enter.

**Pre-Condition:**
- Hệ thống đang hoạt động bình thường
- Cơ sở dữ liệu công thức có sẵn các công thức với tên món ăn
- Người dùng có ít nhất một từ khóa để tìm kiếm

**Post-Condition:**
- Danh sách các công thức có tên khớp hoặc liên quan được hiển thị
- Kết quả được sắp xếp theo độ liên quan (khớp chính xác → khớp một phần → liên quan)
- Lịch sử tìm kiếm được lưu để gợi ý cho lần sau
- Thống kê tìm kiếm được ghi lại (số lượng kết quả, từ khóa tìm kiếm)

**Basic Flow:**
1. Người dùng truy cập trang tìm kiếm từ menu chính hoặc trang chủ
2. Người dùng nhập tên món ăn vào ô tìm kiếm (ví dụ: "thịt kho tàu", "bánh mì", "phở bò")
3. Hệ thống hiển thị gợi ý tìm kiếm real-time khi người dùng gõ (autocomplete)
4. Người dùng có thể:
   - a. Chọn từ gợi ý hiển thị
   - b. Tiếp tục gõ và nhấn "Tìm kiếm"
   - c. Nhấn phím Enter để tìm kiếm
5. Hệ thống thực hiện tìm kiếm với thuật toán:
   - a. Tìm kiếm chính xác (exact match)
   - b. Tìm kiếm một phần (partial match)
   - c. Tìm kiếm liên quan (fuzzy search)
   - d. Tìm kiếm không dấu (normalize Vietnamese text)
6. Hệ thống sắp xếp kết quả theo độ liên quan và hiển thị danh sách
7. Mỗi công thức hiển thị: ảnh đại diện, tên món, mô tả ngắn, thời gian nấu, độ khó, số sao
8. Người dùng có thể nhấp vào công thức để xem chi tiết (UC2.5)
9. Người dùng có thể lọc/sắp xếp kết quả (UC2.4)

**Alternative Flow:**
- **Tìm kiếm từ lịch sử:**
  - Người dùng nhấp vào biểu tượng lịch sử bên cạnh ô tìm kiếm
  - Hệ thống hiển thị danh sách các từ khóa đã tìm kiếm trước đó
  - Người dùng chọn từ khóa để tìm kiếm lại

- **Tìm kiếm từ gợi ý phổ biến:**
  - Hệ thống hiển thị danh sách "Tìm kiếm phổ biến" trên trang chủ
  - Người dùng nhấp vào một trong các gợi ý
  - Hệ thống tự động thực hiện tìm kiếm với từ khóa đó

- **Tìm kiếm nâng cao:**
  - Người dùng chọn "Tìm kiếm nâng cao"
  - Có thể thêm bộ lọc: loại món ăn, độ khó, thời gian nấu
  - Hệ thống kết hợp tìm kiếm tên với các bộ lọc

- **Tìm kiếm bằng giọng nói:**
  - Người dùng nhấn biểu tượng micro
  - Hệ thống chuyển đổi giọng nói thành text
  - Tự động thực hiện tìm kiếm với text đã chuyển đổi

**Exception Flow:**
- **Không tìm thấy kết quả:**
  - Nếu không có công thức nào khớp với từ khóa
  - Hệ thống hiển thị thông báo: "Không tìm thấy kết quả cho '[từ khóa]'"
  - Gợi ý: "Bạn có thể thử với từ khóa khác hoặc kiểm tra chính tả"
  - Hiển thị danh sách "Tìm kiếm phổ biến" để gợi ý

- **Từ khóa quá ngắn:**
  - Nếu từ khóa chỉ có 1-2 ký tự
  - Hệ thống hiển thị cảnh báo: "Từ khóa quá ngắn. Vui lòng nhập ít nhất 3 ký tự."
  - Không thực hiện tìm kiếm

- **Từ khóa chứa ký tự đặc biệt:**
  - Nếu từ khóa chứa ký tự không hợp lệ
  - Hệ thống tự động loại bỏ ký tự đặc biệt và thông báo
  - Tiếp tục tìm kiếm với từ khóa đã làm sạch

- **Quá nhiều kết quả:**
  - Nếu tìm thấy > 100 kết quả
  - Hệ thống hiển thị: "Tìm thấy [số] kết quả. Hãy thử từ khóa cụ thể hơn để thu hẹp kết quả."
  - Hiển thị kết quả đầu tiên và có phân trang

- **Lỗi hệ thống:**
  - Nếu có lỗi server khi tìm kiếm
  - Hệ thống hiển thị: "Đã xảy ra lỗi hệ thống. Vui lòng thử lại sau."
  - Cung cấp nút "Thử lại" để tìm kiếm lại

- **Timeout tìm kiếm:**
  - Nếu tìm kiếm mất quá 5 giây
  - Hệ thống hiển thị: "Tìm kiếm đang mất quá nhiều thời gian. Vui lòng thử lại."
  - Cung cấp tùy chọn hủy hoặc tiếp tục chờ

**Additional Information:**
- **Business Rule:**
  - Tìm kiếm hỗ trợ tiếng Việt có dấu và không dấu
  - Tìm kiếm không phân biệt hoa thường
  - Ưu tiên hiển thị công thức có tên khớp chính xác trước
  - Sau đó là công thức có tên chứa từ khóa
  - Cuối cùng là công thức có tên liên quan (fuzzy search)
  - Kết quả tìm kiếm được cache trong 15 phút
  - Tối đa hiển thị 20 kết quả mỗi trang, có phân trang
  - Lịch sử tìm kiếm được lưu tối đa 50 từ khóa gần nhất

- **Non-Functional Requirement:**
  - Performance: Thời gian tìm kiếm phải dưới 2 giây
  - Usability: Autocomplete phải phản hồi trong vòng 300ms
  - Security: Từ khóa tìm kiếm phải được sanitize để tránh XSS
  - Reliability: Hệ thống phải hoạt động ổn định với nhiều yêu cầu tìm kiếm đồng thời

**Priority:** Medium  
**CRUD:** Read


