# PHÂN TÍCH YÊU CẦU TỐI THIỂU TRONG UC3

**Ngày phân tích:** 2025-01-27  
**Mục đích:** Kiểm tra và giải thích lý do yêu cầu tối thiểu 3 nguyên liệu và 3 bước thực hiện

---

## 1. YÊU CẦU TỐI THIỂU TRONG UC3.1

### 1.1. Vị trí trong tài liệu

#### Trong Basic Flow (Bước 3):
```
3. Người dùng điền đầy đủ thông tin bắt buộc:
   ...
   - f. Thêm ít nhất 3 nguyên liệu
   - g. Viết ít nhất 3 bước thực hiện
```

#### Trong Business Rules:
```
- Phải có ít nhất 3 nguyên liệu và 3 bước thực hiện
```

---

## 2. LÝ DO THIẾT KẾ

### 2.1. Lý do yêu cầu tối thiểu 3 nguyên liệu

#### ✅ Đảm bảo chất lượng công thức
- **Mục đích:** Một công thức nấu ăn thực sự cần có đủ nguyên liệu để tạo ra món ăn hoàn chỉnh
- **Thực tế:** Hầu hết các món ăn đều cần ít nhất 3-4 nguyên liệu cơ bản (ví dụ: thịt, rau, gia vị)
- **Chống spam:** Ngăn chặn việc tạo công thức không đầy đủ hoặc spam

#### ✅ Trải nghiệm người dùng
- **Tìm kiếm:** Công thức có ít nguyên liệu sẽ khó tìm kiếm và lọc chính xác
- **Đề xuất:** Hệ thống tìm kiếm theo nguyên liệu (UC2) hoạt động tốt hơn với nhiều nguyên liệu
- **Chất lượng:** Công thức có ít nguyên liệu thường không đầy đủ hoặc không hữu ích

#### ✅ Business Logic
- **Tủ lạnh ảo (UC5):** Hệ thống cần đủ nguyên liệu để match với tủ lạnh của user
- **AI Generation (UC2.3):** AI cần ít nhất 2-3 nguyên liệu để tạo công thức, nên yêu cầu 3 là hợp lý
- **Tìm kiếm thông minh:** Càng nhiều nguyên liệu, kết quả tìm kiếm càng chính xác

---

### 2.2. Lý do yêu cầu tối thiểu 3 bước thực hiện

#### ✅ Đảm bảo công thức đầy đủ
- **Mục đích:** Một công thức nấu ăn cần có các bước rõ ràng từ chuẩn bị đến hoàn thành
- **Thực tế:** Hầu hết món ăn đều có ít nhất 3 bước:
  - Bước 1: Chuẩn bị nguyên liệu
  - Bước 2: Chế biến/nấu
  - Bước 3: Trình bày/hoàn thiện

#### ✅ Hướng dẫn rõ ràng
- **Người dùng:** Công thức có ít bước thường không đủ chi tiết để người dùng làm theo
- **Chất lượng:** Công thức có nhiều bước thường chi tiết và dễ làm theo hơn
- **Trải nghiệm:** Người dùng cảm thấy công thức đáng tin cậy hơn khi có nhiều bước

#### ✅ Chống spam và nội dung kém chất lượng
- **Spam:** Ngăn chặn việc tạo công thức không đầy đủ hoặc chỉ có 1-2 bước
- **Chất lượng:** Đảm bảo công thức có đủ thông tin để người dùng thực hiện được

---

## 3. SO SÁNH VỚI CÁC HỆ THỐNG KHÁC

### 3.1. Các nền tảng công thức phổ biến

| Nền tảng | Yêu cầu nguyên liệu | Yêu cầu bước thực hiện |
|----------|-------------------|----------------------|
| **AllRecipes** | Không có tối thiểu | Không có tối thiểu |
| **Food Network** | Không có tối thiểu | Không có tối thiểu |
| **BBC Good Food** | Không có tối thiểu | Không có tối thiểu |
| **Hệ thống này** | **Tối thiểu 3** | **Tối thiểu 3** |

### 3.2. Phân tích

- **Các nền tảng lớn:** Không có yêu cầu tối thiểu vì họ có đội ngũ biên tập và kiểm duyệt chặt chẽ
- **Hệ thống này:** Có yêu cầu tối thiểu vì:
  - User-generated content (người dùng tự tạo)
  - Cần đảm bảo chất lượng tự động
  - Có hệ thống duyệt (admin duyệt), nhưng cần filter sơ bộ

---

## 4. TÁC ĐỘNG CỦA YÊU CẦU NÀY

### 4.1. Tác động tích cực ✅

1. **Chất lượng công thức cao hơn**
   - Công thức đầy đủ và chi tiết hơn
   - Người dùng dễ làm theo hơn

2. **Tìm kiếm chính xác hơn**
   - Công thức có nhiều nguyên liệu → kết quả tìm kiếm tốt hơn
   - Hệ thống AI có thể đề xuất tốt hơn

3. **Giảm spam và nội dung kém chất lượng**
   - Ngăn chặn công thức không đầy đủ
   - Giảm công việc cho admin khi duyệt

### 4.2. Tác động tiêu cực ⚠️

1. **Có thể quá nghiêm ngặt**
   - Một số món đơn giản chỉ cần 1-2 nguyên liệu (ví dụ: trứng luộc)
   - Một số món chỉ cần 1-2 bước (ví dụ: salad trộn)

2. **Trải nghiệm người dùng**
   - Người dùng có thể cảm thấy bị ép buộc thêm nguyên liệu/bước không cần thiết
   - Có thể làm giảm số lượng công thức được tạo

---

## 5. ĐỀ XUẤT

### 5.1. Giữ nguyên yêu cầu (Khuyến nghị) ✅

**Lý do:**
- Yêu cầu này hợp lý và phù hợp với mục tiêu đảm bảo chất lượng
- Hầu hết công thức thực tế đều có ít nhất 3 nguyên liệu và 3 bước
- Giúp hệ thống hoạt động tốt hơn (tìm kiếm, AI, v.v.)

**Cách cải thiện:**
- Thêm validation message rõ ràng: "Công thức cần ít nhất 3 nguyên liệu để đảm bảo chất lượng"
- Cho phép lưu nháp với ít hơn 3 (đã có sẵn)
- Hướng dẫn người dùng: "Thêm ít nhất 3 nguyên liệu để công thức của bạn được duyệt nhanh hơn"

### 5.2. Giảm xuống 2 (Không khuyến nghị) ❌

**Lý do không nên:**
- Quá dễ dàng, có thể dẫn đến spam
- Công thức 2 nguyên liệu/bước thường không đầy đủ
- Không phù hợp với mục tiêu đảm bảo chất lượng

### 5.3. Tăng lên 4-5 (Không khuyến nghị) ❌

**Lý do không nên:**
- Quá nghiêm ngặt, có thể làm giảm số lượng công thức
- Một số món đơn giản chỉ cần 3 nguyên liệu
- Có thể làm người dùng cảm thấy khó khăn

### 5.4. Làm linh hoạt theo danh mục (Có thể xem xét) 🤔

**Ý tưởng:**
- Một số danh mục (ví dụ: "Món đơn giản") có thể yêu cầu ít hơn
- Một số danh mục (ví dụ: "Món phức tạp") có thể yêu cầu nhiều hơn

**Nhược điểm:**
- Phức tạp hơn trong implementation
- Khó quản lý và giải thích cho người dùng

---

## 6. KẾT LUẬN

### 6.1. Yêu cầu hiện tại là hợp lý ✅

- **3 nguyên liệu:** Phù hợp với thực tế, đảm bảo chất lượng
- **3 bước thực hiện:** Đảm bảo công thức đầy đủ và chi tiết

### 6.2. Khuyến nghị

1. **Giữ nguyên yêu cầu 3** ✅
2. **Cải thiện UX:**
   - Thêm tooltip/hint giải thích lý do
   - Cho phép lưu nháp với ít hơn 3 (đã có)
   - Hiển thị progress: "2/3 nguyên liệu - cần thêm 1 nguyên liệu nữa"

3. **Nếu muốn linh hoạt hơn:**
   - Có thể cho phép admin điều chỉnh yêu cầu tối thiểu theo danh mục
   - Hoặc có chế độ "Món đơn giản" với yêu cầu thấp hơn

---

## 7. TÀI LIỆU THAM KHẢO

- **UCS03-1_Them_cong_thuc_moi.md:**
  - Dòng 50: "f. Thêm ít nhất 3 nguyên liệu"
  - Dòng 51: "g. Viết ít nhất 3 bước thực hiện"
  - Dòng 123: "Phải có ít nhất 3 nguyên liệu và 3 bước thực hiện"

- **Implementation:**
  - Backend validator: `ingredients.min(3)`, `steps.min(3)`
  - Frontend validation: Check trong `validateStep()`

---

**Kết thúc phân tích**

