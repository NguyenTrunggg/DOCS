# Template Đặc Tả USE CASE (UC)

## Overview
Template này được thiết kế để tạo ra các Use Case đặc tả chi tiết, có cấu trúc và tính nhất quán cao. Template tuân thủ chuẩn quốc tế và phù hợp với hệ thống iLMS.

---

## Cấu trúc Use Case

### [UC_ID]: [Tên Use Case] [[PRIORITY LEVEL]]

**Use Case Description:** [Mô tả ngắn gọn về chức năng chính của Use Case này. Nên bắt đầu bằng "Cho phép [Actor]..." hoặc "Use case này cho phép [Actor]..."]

**Trigger:** [Sự kiện hoặc hành động khởi tạo Use Case]

**Pre-Condition:**
- [Điều kiện tiên quyết 1]
- [Điều kiện tiên quyết 2]
- [Điều kiện tiên quyết 3]

**Post-Condition:**
- [Kết quả mong đợi 1]
- [Kết quả mong đợi 2]
- [Kết quả mong đợi 3]

**Basic Flow:**
1. [Bước 1 - Hành động đầu tiên]
2. [Bước 2 - Hành động tiếp theo]
3. [Bước 3 - Hành động thứ ba]
   - a. [Chi tiết con của bước 3]
   - b. [Chi tiết con khác của bước 3]
4. [Bước 4 - Tiếp tục luồng chính]
5. [Bước 5 - Kết thúc luồng chính]

**Alternative Flow:**
- **[Tên luồng thay thế:**
  - [Mô tả điều kiện để kích hoạt luồng này]
  - [Các bước thực hiện trong luồng thay thế]
  - [Kết quả của luồng thay thế]

**Exception Flow:**
- **[Tên luồng ngoại lệ:**
  - [Điều kiện gây ra ngoại lệ]
  - [Cách xử lý ngoại lệ]
  - [Thông báo lỗi hoặc hành động khắc phục]

**Additional Information:**
- **Business Rule:**
  - [Quy tắc nghiệp vụ 1]
  - [Quy tắc nghiệp vụ 2]
  - [Quy tắc nghiệp vụ 3]
- **Non-Functional Requirement:**
  - Performance: [Yêu cầu về hiệu suất]
  - Usability: [Yêu cầu về khả năng sử dụng]
  - Security: [Yêu cầu về bảo mật]
  - Reliability: [Yêu cầu về độ tin cậy]

**Priority:** [High/Medium/Low]  
**CRUD:** [Create/Read/Update/Delete]

---

## Hướng dẫn sử dụng Template

### 1. Thông tin cơ bản

#### UC_ID
- Format: `[UCS/UCI/UCA/UCAI]-[Số thứ tự]-[Số chi tiết]`
- Ví dụ: `UCS01-1`, `UCI02-3`, `UCA03-2`

#### Tên Use Case
- Sử dụng tiếng Việt, ngắn gọn, dễ hiểu
- Bắt đầu bằng động từ hành động
- Ví dụ: "Xem danh sách môn học", "Làm bài tập coding"

#### Priority Level
- **HIGH PRIORITY:** Chức năng cốt lõi, bắt buộc phải có
- **MEDIUM PRIORITY:** Chức năng quan trọng, nên có
- **LOW PRIORITY:** Chức năng bổ sung, có thể có sau

### 2. Chi tiết các thành phần

#### Use Case Description
- Mô tả ngắn gọn mục đích chính
- Xác định rõ Actor chính
- Giải thích giá trị mang lại

#### Trigger
- Sự kiện khởi tạo Use Case
- Có thể là hành động của user hoặc sự kiện hệ thống

#### Pre-Condition
- Điều kiện tiên quyết phải được thỏa mãn
- Bao gồm trạng thái hệ thống, quyền truy cập, dữ liệu cần thiết

#### Post-Condition
- Trạng thái hệ thống sau khi hoàn thành Use Case
- Dữ liệu được tạo/cập nhật
- Thông báo được gửi

#### Basic Flow
- Luồng chính thành công
- Đánh số thứ tự rõ ràng
- Sử dụng bullet points cho chi tiết con
- Mỗi bước phải cụ thể và có thể thực hiện được

#### Alternative Flow
- Các luồng thay thế hợp lệ
- Điều kiện kích hoạt rõ ràng
- Không phải là lỗi mà là các cách thực hiện khác

#### Exception Flow
- Các tình huống lỗi và cách xử lý
- Thông báo lỗi cụ thể
- Hành động khắc phục

#### Additional Information

**Business Rule:**
- Quy tắc nghiệp vụ cụ thể
- Ràng buộc về dữ liệu
- Chính sách của tổ chức

**Non-Functional Requirement:**
- **Performance:** Thời gian phản hồi, throughput
- **Usability:** Giao diện thân thiện, dễ sử dụng
- **Security:** Bảo mật, xác thực, phân quyền
- **Reliability:** Độ tin cậy, khả năng phục hồi

### 3. Ví dụ mẫu

```markdown
### UCS01-1: Xem danh sách môn học đã đăng ký [HIGH PRIORITY]

**Use Case Description:** Chức năng này cho phép Sinh viên (SV) xem một danh sách tổng quan tất cả các môn học mà họ đã đăng ký trong học kỳ hiện tại hoặc các học kỳ trước. Đây là trang chính sau khi SV đăng nhập.

**Trigger:** Sinh viên đăng nhập thành công vào hệ thống iLMS.

**Pre-Condition:** SV đã có tài khoản và đã được ghi danh vào ít nhất một môn học.

**Post-Condition:** Danh sách các môn học SV đã đăng ký được hiển thị trên màn hình.

**Basic Flow:**
1. SV truy cập trang đăng nhập của iLMS và nhập thông tin tài khoản
2. Hệ thống xác thực thông tin và đăng nhập thành công
3. Hệ thống tự động chuyển hướng SV đến trang "Dashboard" hoặc "My Courses"
4. Trên trang này, hệ thống truy vấn cơ sở dữ liệu để lấy danh sách các môn học mà tài khoản SV này đã đăng ký
5. Hệ thống hiển thị danh sách các môn học dưới dạng các thẻ (cards) hoặc một danh sách. Mỗi mục hiển thị thông tin cơ bản như:
   - a. Tên môn học
   - b. Mã môn học
   - c. Tên giảng viên phụ trách
   - d. Một thanh tiến độ tổng quan (nếu có)

**Alternative Flow:**
- **Lọc theo học kỳ:**
  - Trên trang danh sách môn học, có một bộ lọc (dropdown) để chọn học kỳ
  - Mặc định, hệ thống hiển thị học kỳ hiện tại
  - SV chọn một học kỳ khác từ bộ lọc
  - Hệ thống gửi yêu cầu mới và hiển thị danh sách các môn học tương ứng với học kỳ đã chọn

**Exception Flow:**
- **Sinh viên chưa đăng ký môn nào:**
  - Nếu tài khoản SV chưa được ghi danh vào bất kỳ môn học nào
  - Hệ thống sẽ hiển thị một thông báo thân thiện: "Bạn hiện chưa được ghi danh vào môn học nào. Vui lòng liên hệ phòng đào tạo để biết thêm chi tiết."

**Additional Information:**
- **Business Rule:**
  - Chỉ những môn học có trạng thái "Đang diễn ra" (Active) hoặc "Đã kết thúc" (Completed) mới được hiển thị
- **Non-Functional Requirement:**
  - Performance: Thời gian tải trang danh sách môn học phải dưới 2 giây
  - Usability: Giao diện phải sạch sẽ, dễ nhìn, giúp sinh viên nhanh chóng tìm thấy môn học họ cần

**Priority:** High  
**CRUD:** Read
```

### 4. Lưu ý quan trọng

#### Về ngôn ngữ
- Sử dụng tiếng Việt cho mô tả
- Thuật ngữ kỹ thuật có thể dùng tiếng Anh
- Đảm bảo tính nhất quán trong toàn bộ tài liệu

#### Về cấu trúc
- Tuân thủ đúng thứ tự các thành phần
- Mỗi bước trong Basic Flow phải có thể thực hiện được
- Alternative Flow và Exception Flow phải có điều kiện kích hoạt rõ ràng

#### Về nội dung
- Mô tả chi tiết nhưng không quá dài dòng
- Tập trung vào hành vi của hệ thống, không phải implementation
- Đảm bảo tính đầy đủ và chính xác

#### Về traceability
- Liên kết rõ ràng với các UC khác
- Đảm bảo tính nhất quán với requirements
- Có thể trace ngược về business objectives

### 5. Checklist hoàn thiện UC

- [ ] UC_ID tuân thủ format chuẩn
- [ ] Tên UC ngắn gọn, rõ ràng
- [ ] Priority level được xác định đúng
- [ ] Use Case Description mô tả đầy đủ mục đích
- [ ] Trigger xác định rõ sự kiện khởi tạo
- [ ] Pre-Condition liệt kê đầy đủ điều kiện tiên quyết
- [ ] Post-Condition mô tả rõ kết quả mong đợi
- [ ] Basic Flow có đủ các bước chính
- [ ] Alternative Flow được xác định (nếu có)
- [ ] Exception Flow được xử lý đầy đủ
- [ ] Business Rules được liệt kê rõ ràng
- [ ] Non-Functional Requirements được xác định
- [ ] CRUD operation được ghi nhận đúng
- [ ] Nội dung được review về tính chính xác
- [ ] Format và cấu trúc tuân thủ template

---

## Kết luận

Template này cung cấp framework đầy đủ để tạo ra các Use Case chất lượng cao, đảm bảo tính nhất quán và tính đầy đủ trong toàn bộ dự án iLMS. Việc tuân thủ template sẽ giúp:

1. **Tăng tính nhất quán** trong toàn bộ tài liệu
2. **Đảm bảo tính đầy đủ** của thông tin
3. **Dễ dàng review và maintain** 
4. **Hỗ trợ traceability** với các artifacts khác
5. **Tạo foundation** cho việc thiết kế và phát triển hệ thống

Hãy sử dụng template này một cách linh hoạt, điều chỉnh theo nhu cầu cụ thể của từng Use Case nhưng vẫn đảm bảo tuân thủ cấu trúc cơ bản.
