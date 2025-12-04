# UCS04-5: Xem đánh giá và bình luận [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đọc các bình luận và điểm đánh giá từ những người dùng khác về một công thức nấu ăn, giúp đưa ra quyết định có nên thử công thức đó hay không.

**Trigger:** Người dùng cuộn xuống phần "Đánh giá và Bình luận" trên trang chi tiết công thức (UC2.5) hoặc nhấn vào tab "Đánh giá".

**Pre-Condition:**
- Công thức tồn tại và có trạng thái "Đã duyệt"
- Công thức có ít nhất một đánh giá từ người dùng khác
- Hệ thống đang hoạt động bình thường

**Post-Condition:**
- Tất cả đánh giá và bình luận của công thức được hiển thị
- Người dùng có thể đọc và tham khảo ý kiến của cộng đồng
- Thống kê đánh giá được cập nhật (nếu có đánh giá mới)

**Basic Flow:**
1. Người dùng đang xem trang chi tiết công thức (UC2.5)
2. Người dùng cuộn xuống phần "Đánh giá và Bình luận" hoặc nhấn tab "Đánh giá"
3. Hệ thống truy vấn cơ sở dữ liệu để lấy tất cả đánh giá của công thức
4. Hệ thống hiển thị phần đánh giá với layout:

   **Tổng quan đánh giá:**
   - a. Điểm đánh giá trung bình (VD: 4.3/5 sao)
   - b. Tổng số đánh giá (VD: 127 đánh giá)
   - c. Biểu đồ phân phối điểm (số lượng đánh giá cho mỗi mức sao)
   - d. Tỷ lệ phần trăm cho mỗi mức sao

   **Bộ lọc và sắp xếp:**
   - e. Bộ lọc theo số sao (Tất cả, 5 sao, 4 sao, 3 sao, 2 sao, 1 sao)
   - f. Sắp xếp theo (Mới nhất, Cũ nhất, Hữu ích nhất, Điểm cao nhất)
   - g. Tìm kiếm trong bình luận

   **Danh sách đánh giá:**
   - h. Mỗi đánh giá hiển thị:
      - Ảnh đại diện người đánh giá
      - Tên người đánh giá (có thể ẩn danh)
      - Số sao đánh giá
      - Nội dung bình luận
      - Thời gian đánh giá
      - Ảnh món ăn đã nấu (nếu có)
      - Số lượt "Hữu ích" và nút "Hữu ích"
      - Nút "Báo cáo" (nếu nội dung không phù hợp)

5. Hệ thống hiển thị danh sách đánh giá với phân trang (10 đánh giá/trang)
6. Người dùng có thể:
   - a. Đọc các bình luận chi tiết
   - b. Xem ảnh món ăn từ người đánh giá
   - c. Nhấn "Hữu ích" cho các đánh giá hữu ích
   - d. Sử dụng bộ lọc để xem đánh giá theo tiêu chí
   - e. Tìm kiếm từ khóa trong bình luận

**Alternative Flow:**
- **Xem đánh giá từ trang chủ:**
  - Người dùng có thể xem đánh giá tổng quan từ trang chủ
  - Hiển thị điểm trung bình và số lượng đánh giá
  - Có thể click để xem chi tiết

- **Xem đánh giá từ danh sách:**
  - Trong danh sách công thức, hiển thị điểm đánh giá trung bình
  - Có thể preview một vài đánh giá nổi bật

- **Xem đánh giá của người dùng cụ thể:**
  - Click vào tên người đánh giá để xem profile
  - Xem các đánh giá khác của người đó

- **Xuất báo cáo đánh giá:**
  - Admin có thể xuất báo cáo tổng hợp đánh giá
  - Bao gồm thống kê và phân tích xu hướng

**Exception Flow:**
- **Chưa có đánh giá nào:**
  - Nếu công thức chưa có đánh giá từ người dùng
  - Hệ thống hiển thị: "Chưa có đánh giá nào cho công thức này."
  - Hiển thị gợi ý: "Hãy là người đầu tiên đánh giá công thức này!"
  - Cung cấp nút "Viết đánh giá" để chuyển đến form đánh giá

- **Lỗi tải dữ liệu:**
  - Nếu có lỗi khi truy vấn đánh giá từ cơ sở dữ liệu
  - Hệ thống hiển thị: "Không thể tải đánh giá. Vui lòng thử lại sau."
  - Cung cấp nút "Làm mới" để thử lại

- **Không tìm thấy kết quả lọc:**
  - Nếu bộ lọc không trả về đánh giá nào
  - Hệ thống hiển thị: "Không có đánh giá nào phù hợp với bộ lọc đã chọn."
  - Cung cấp nút "Xóa bộ lọc" để xem tất cả đánh giá

- **Lỗi hiển thị ảnh:**
  - Nếu ảnh đại diện hoặc ảnh món ăn không tải được
  - Hệ thống hiển thị ảnh placeholder mặc định
  - Không ảnh hưởng đến việc hiển thị nội dung đánh giá

- **Nội dung bị ẩn:**
  - Nếu đánh giá bị ẩn do vi phạm quy định
  - Hệ thống hiển thị: "Đánh giá này đã bị ẩn do vi phạm quy định cộng đồng."
  - Không hiển thị nội dung chi tiết

- **Công thức không tồn tại:**
  - Nếu công thức đã bị xóa hoặc không tồn tại
  - Hệ thống hiển thị: "Công thức không tồn tại hoặc đã bị xóa."
  - Chuyển hướng về trang trước

**Additional Information:**
- **Business Rule:**
  - Chỉ hiển thị đánh giá của công thức có trạng thái "Đã duyệt"
  - Đánh giá được sắp xếp theo thời gian mới nhất theo mặc định
  - Đánh giá bị ẩn do vi phạm không được hiển thị
  - Tên người đánh giá có thể được ẩn danh nếu họ chọn
  - Điểm đánh giá trung bình được làm tròn đến 1 chữ số thập phân
  - Phân trang tối đa 10 đánh giá/trang để đảm bảo hiệu suất
  - Đánh giá được cache trong 5 phút để tăng tốc độ

- **Non-Functional Requirement:**
  - Performance: Thời gian tải danh sách đánh giá phải dưới 2 giây
  - Usability: Giao diện phải rõ ràng, dễ đọc, có thể lọc và tìm kiếm
  - Security: Nội dung đánh giá phải được sanitize để tránh XSS
  - Reliability: Hệ thống phải hoạt động ổn định ngay cả khi có nhiều đánh giá

**Priority:** Low  
**CRUD:** Read


