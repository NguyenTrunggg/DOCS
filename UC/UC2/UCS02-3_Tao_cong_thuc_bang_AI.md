# UCS02-3: Tạo công thức bằng AI [HIGH PRIORITY]

**Use Case Description:** Chức năng này cho phép hệ thống sử dụng trí tuệ nhân tạo (AI) để tạo ra một công thức nấu ăn mới hoàn toàn từ danh sách nguyên liệu mà người dùng cung cấp, khi không tìm thấy công thức phù hợp trong cơ sở dữ liệu hiện có.

**Trigger:** Người dùng nhấn nút "Sử dụng AI để tạo công thức mới" sau khi tìm kiếm theo nguyên liệu (UC2.1) không trả về kết quả.

**Pre-Condition:**
- Việc tìm kiếm theo nguyên liệu (UC2.1) đã được thực hiện nhưng không trả về kết quả
- Hệ thống có kết nối đến API của mô hình AI (GPT hoặc tương tự)
- Người dùng có ít nhất 2-3 nguyên liệu chính để tạo công thức
- API AI đang hoạt động và có thể xử lý yêu cầu

**Post-Condition:**
- Một công thức mới được tạo ra bởi AI và hiển thị cho người dùng
- Công thức bao gồm: tên món, danh sách nguyên liệu chi tiết, các bước thực hiện, thời gian nấu, độ khó
- Công thức được lưu tạm thời trong session của người dùng
- Người dùng có thể lưu công thức vào yêu thích hoặc báo cáo nếu không hài lòng

**Basic Flow:**
1. Sau khi tìm kiếm theo nguyên liệu không có kết quả, hệ thống hiển thị thông báo: "Không tìm thấy công thức phù hợp. Bạn có muốn sử dụng AI để tạo một công thức mới từ những nguyên liệu này không?"
2. Người dùng nhấn nút "Đồng ý" hoặc "Sử dụng AI"
3. Hệ thống hiển thị màn hình loading với thông báo: "AI đang tạo công thức cho bạn..."
4. Hệ thống chuẩn bị prompt cho AI với thông tin:
   - a. Danh sách nguyên liệu chính người dùng có
   - b. Yêu cầu tạo công thức nấu ăn ngon, thực tế
   - c. Định dạng output mong muốn (JSON structure)
5. Hệ thống gửi yêu cầu đến API AI với prompt đã chuẩn bị
6. Hệ thống nhận phản hồi từ AI và xử lý dữ liệu:
   - a. Parse JSON response từ AI
   - b. Validate dữ liệu nhận được
   - c. Định dạng lại văn bản cho phù hợp với giao diện
7. Hệ thống hiển thị công thức vừa được tạo với các thông tin:
   - a. Tên món ăn (do AI đặt)
   - b. Mô tả ngắn về món ăn
   - c. Danh sách nguyên liệu chi tiết với định lượng
   - d. Các bước thực hiện từng bước
   - e. Thời gian nấu ước tính
   - f. Độ khó (Dễ/Trung bình/Khó)
   - g. Khẩu phần ăn
8. Hệ thống cung cấp các tùy chọn cho người dùng:
   - a. "Lưu vào Yêu thích"
   - b. "Chia sẻ công thức"
   - c. "Báo cáo không phù hợp"
   - d. "Tạo công thức khác"

**Alternative Flow:**
- **Tạo công thức từ "Tủ lạnh ảo":**
  - Người dùng có thể chọn "Tạo công thức từ Tủ lạnh ảo"
  - Hệ thống lấy danh sách nguyên liệu từ UC5 (Tủ lạnh ảo)
  - Tiếp tục luồng từ bước 3 của Basic Flow

- **Tạo công thức với yêu cầu đặc biệt:**
  - Người dùng có thể thêm yêu cầu: "Món chay", "Ít calo", "Nhanh chóng"
  - Hệ thống thêm yêu cầu này vào prompt cho AI
  - Tiếp tục luồng từ bước 4 của Basic Flow

- **Tạo nhiều công thức:**
  - Sau khi xem công thức đầu tiên, người dùng có thể nhấn "Tạo công thức khác"
  - Hệ thống gửi yêu cầu mới đến AI với cùng nguyên liệu
  - Tiếp tục luồng từ bước 5 của Basic Flow

**Exception Flow:**
- **API AI bị lỗi hoặc quá tải:**
  - Nếu API AI trả về lỗi hoặc timeout
  - Hệ thống hiển thị thông báo: "AI hiện đang quá tải. Vui lòng thử lại sau ít phút."
  - Cung cấp nút "Thử lại" và nút "Quay lại tìm kiếm"

- **Kết quả từ AI không đúng định dạng:**
  - Nếu AI trả về dữ liệu không đúng cấu trúc JSON mong muốn
  - Hệ thống hiển thị thông báo: "Không thể xử lý kết quả từ AI. Vui lòng thử lại."
  - Cung cấp nút "Thử lại với prompt khác"

- **Kết quả từ AI không hợp lý:**
  - Nếu AI tạo ra công thức có nguyên liệu không phù hợp hoặc hướng dẫn sai
  - Hệ thống hiển thị cảnh báo: "Công thức này có thể cần điều chỉnh. Vui lòng kiểm tra kỹ trước khi nấu."
  - Cung cấp nút "Báo cáo không phù hợp"

- **Mất kết nối mạng:**
  - Nếu mất kết nối trong quá trình gọi API AI
  - Hệ thống hiển thị: "Mất kết nối mạng. Vui lòng kiểm tra kết nối và thử lại."
  - Cung cấp nút "Thử lại" khi kết nối được khôi phục

- **Nguyên liệu quá ít:**
  - Nếu người dùng chỉ có 1 nguyên liệu
  - Hệ thống hiển thị: "Với 1 nguyên liệu, AI khó tạo ra công thức hoàn chỉnh. Bạn có muốn thêm nguyên liệu khác không?"
  - Cung cấp gợi ý các nguyên liệu phổ biến để thêm vào

- **Nguyên liệu không phù hợp:**
  - Nếu nguyên liệu quá kỳ lạ hoặc không phù hợp để nấu ăn
  - Hệ thống hiển thị: "Nguyên liệu này có thể không phù hợp để tạo công thức. Bạn có muốn thử nguyên liệu khác không?"
  - Cung cấp gợi ý các nguyên liệu thay thế

**Additional Information:**
- **Business Rule:**
  - AI chỉ được kích hoạt khi tìm kiếm theo nguyên liệu không có kết quả
  - Mỗi người dùng chỉ được sử dụng AI tối đa 5 lần/ngày để tránh lạm dụng
  - Prompt cho AI phải được chuẩn hóa để đảm bảo chất lượng output
  - Công thức do AI tạo không được lưu vào cơ sở dữ liệu chính
  - Công thức AI chỉ được lưu tạm thời trong session (24 giờ)
  - Người dùng có thể báo cáo công thức không phù hợp để cải thiện AI

- **Non-Functional Requirement:**
  - Performance: Thời gian AI tạo công thức phải dưới 15 giây
  - Usability: Giao diện phải hiển thị tiến trình loading rõ ràng
  - Security: API key của AI phải được bảo mật, không expose ra client
  - Reliability: Hệ thống phải có fallback khi AI không khả dụng

**Priority:** High  
**CRUD:** Create


