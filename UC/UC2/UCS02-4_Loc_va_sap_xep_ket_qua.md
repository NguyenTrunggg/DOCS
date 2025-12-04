# UCS02-4: Lọc và Sắp xếp kết quả [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng lọc và sắp xếp kết quả tìm kiếm công thức theo các tiêu chí khác nhau như loại món ăn, thời gian nấu, độ khó, điểm đánh giá để tìm được công thức phù hợp nhất với nhu cầu.

**Trigger:** Người dùng nhấn nút "Lọc" hoặc "Sắp xếp" sau khi có kết quả tìm kiếm từ UC2.1 hoặc UC2.2.

**Pre-Condition:**
- Người dùng đã thực hiện tìm kiếm và có kết quả hiển thị
- Kết quả tìm kiếm được lưu trong session của người dùng
- Hệ thống có đủ dữ liệu để thực hiện lọc và sắp xếp

**Post-Condition:**
- Kết quả tìm kiếm được lọc và sắp xếp theo tiêu chí người dùng chọn
- Giao diện hiển thị kết quả mới với thông tin về số lượng kết quả sau khi lọc
- Các bộ lọc được lưu để có thể áp dụng cho các tìm kiếm tiếp theo
- Trạng thái lọc/sắp xếp được lưu trong session

**Basic Flow:**
1. Người dùng đã có kết quả tìm kiếm từ trang tìm kiếm (UC2.1 hoặc UC2.2)
2. Người dùng nhấn nút "Lọc & Sắp xếp" hoặc biểu tượng bộ lọc
3. Hệ thống hiển thị panel lọc và sắp xếp với các tùy chọn:

   **Bộ lọc:**
   - a. Loại món ăn (Món chính, Món tráng miệng, Món chay, Món Hàn, Món Nhật, v.v.)
   - b. Thời gian nấu (Dưới 15 phút, 15-30 phút, 30-60 phút, Trên 60 phút)
   - c. Độ khó (Dễ, Trung bình, Khó)
   - d. Khẩu phần (1-2 người, 3-4 người, 5-6 người, Trên 6 người)
   - e. Điểm đánh giá (Từ 4 sao trở lên, Từ 3 sao trở lên)
   - f. Người tạo (Hệ thống, Người dùng)

   **Sắp xếp:**
   - a. Độ liên quan (mặc định)
   - b. Thời gian nấu (tăng dần/giảm dần)
   - c. Điểm đánh giá (cao đến thấp)
   - d. Ngày tạo (mới nhất/cũ nhất)
   - e. Tên món ăn (A-Z, Z-A)

4. Người dùng chọn các tiêu chí lọc và sắp xếp mong muốn
5. Người dùng nhấn nút "Áp dụng" hoặc "Lọc"
6. Hệ thống thực hiện lọc và sắp xếp kết quả theo tiêu chí đã chọn
7. Hệ thống hiển thị kết quả mới với thông tin:
   - a. Số lượng kết quả sau khi lọc (ví dụ: "Hiển thị 12 trong 45 kết quả")
   - b. Danh sách công thức đã được lọc và sắp xếp
   - c. Các bộ lọc đang được áp dụng
8. Người dùng có thể xem kết quả và thực hiện các hành động khác

**Alternative Flow:**
- **Lọc nhanh:**
  - Hệ thống hiển thị các nút lọc nhanh phổ biến (VD: "Nhanh", "Dễ làm", "Nổi tiếng")
  - Người dùng nhấp vào một trong các nút này
  - Hệ thống tự động áp dụng bộ lọc tương ứng

- **Lưu bộ lọc:**
  - Sau khi áp dụng bộ lọc thành công, người dùng có thể nhấn "Lưu bộ lọc này"
  - Hệ thống lưu bộ lọc với tên do người dùng đặt
  - Người dùng có thể sử dụng lại bộ lọc đã lưu cho các lần tìm kiếm sau

- **Xóa tất cả bộ lọc:**
  - Người dùng có thể nhấn "Xóa tất cả bộ lọc" để quay về kết quả ban đầu
  - Hệ thống hiển thị lại tất cả kết quả tìm kiếm gốc

- **Lọc theo nguyên liệu:**
  - Người dùng có thể thêm bộ lọc "Có nguyên liệu" hoặc "Không có nguyên liệu"
  - Chọn nguyên liệu cụ thể để lọc công thức có/không có nguyên liệu đó

**Exception Flow:**
- **Không có kết quả sau khi lọc:**
  - Nếu tất cả kết quả bị loại bỏ sau khi áp dụng bộ lọc
  - Hệ thống hiển thị thông báo: "Không có kết quả nào phù hợp với bộ lọc đã chọn."
  - Cung cấp nút "Xóa bộ lọc" để quay về kết quả ban đầu
  - Gợi ý các bộ lọc khác để thử

- **Bộ lọc không hợp lệ:**
  - Nếu người dùng chọn bộ lọc mâu thuẫn (VD: "Dưới 15 phút" và "Trên 60 phút")
  - Hệ thống hiển thị cảnh báo: "Bộ lọc đã chọn không hợp lệ. Vui lòng kiểm tra lại."
  - Highlight các bộ lọc mâu thuẫn và yêu cầu người dùng sửa

- **Lỗi xử lý bộ lọc:**
  - Nếu có lỗi server khi xử lý bộ lọc
  - Hệ thống hiển thị: "Đã xảy ra lỗi khi áp dụng bộ lọc. Vui lòng thử lại."
  - Cung cấp nút "Thử lại" để áp dụng bộ lọc lại

- **Session hết hạn:**
  - Nếu session hết hạn trong quá trình lọc
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."

- **Quá nhiều kết quả sau khi lọc:**
  - Nếu vẫn còn > 100 kết quả sau khi lọc
  - Hệ thống hiển thị: "Vẫn còn [số] kết quả. Bạn có thể thêm bộ lọc để thu hẹp kết quả."
  - Gợi ý các bộ lọc bổ sung

**Additional Information:**
- **Business Rule:**
  - Bộ lọc có thể được kết hợp nhiều tiêu chí cùng lúc
  - Kết quả lọc phải được sắp xếp theo thứ tự đã chọn
  - Bộ lọc được lưu trong session tối đa 2 giờ
  - Người dùng có thể lưu tối đa 5 bộ lọc tùy chỉnh
  - Bộ lọc nhanh được cập nhật dựa trên xu hướng tìm kiếm
  - Kết quả lọc phải giữ nguyên thông tin gốc của công thức

- **Non-Functional Requirement:**
  - Performance: Thời gian lọc và sắp xếp phải dưới 1 giây
  - Usability: Giao diện bộ lọc phải trực quan, dễ sử dụng
  - Security: Bộ lọc phải được validate để tránh SQL injection
  - Reliability: Hệ thống phải hoạt động ổn định với nhiều bộ lọc phức tạp

**Priority:** Medium  
**CRUD:** Read


