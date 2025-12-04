# HƯỚNG DẪN DEBUG: Vấn đề không thể tạo công thức

## 🔍 CÁCH KIỂM TRA

### Bước 1: Mở Browser Console
1. Mở trang tạo công thức
2. Nhấn **F12** hoặc **Ctrl+Shift+I** (Windows) / **Cmd+Option+I** (Mac)
3. Chọn tab **Console**

### Bước 2: Thử tạo công thức
1. Điền đầy đủ thông tin
2. Nhấn nút "Tạo công thức"
3. Quan sát các log trong Console

### Bước 3: Kiểm tra các log

#### ✅ Nếu thấy log này → API đang được gọi:
```
📤 [CreateRecipe] Submitting recipe: { ... }
🔵 [API] POST /user/recipes - Request params: { ... }
📤 [API] Sending payload: { ... }
```

#### ❌ Nếu thấy log này → Lỗi API:
```
❌ [API] Request failed: { ... }
❌ [CreateRecipe] Failed to create recipe: { ... }
```

#### ⚠️ Nếu KHÔNG thấy log nào → Form không submit được:
- Có thể do validation fail
- Kiểm tra các error message trên form

---

## 🐛 CÁC LỖI THƯỜNG GẶP

### 1. Lỗi 401 Unauthorized
**Triệu chứng:**
```
❌ [API] Request failed: { status: 401, ... }
```

**Nguyên nhân:**
- Token hết hạn hoặc không hợp lệ
- Chưa đăng nhập

**Giải pháp:**
- Đăng nhập lại
- Kiểm tra token trong localStorage/sessionStorage

---

### 2. Lỗi 400 Bad Request
**Triệu chứng:**
```
❌ [API] Request failed: { status: 400, data: { errors: [...] } }
```

**Nguyên nhân:**
- Validation error từ backend
- `ingredientId` không phải UUID hợp lệ
- Thiếu nguyên liệu hoặc bước thực hiện
- Mô tả bước quá ngắn (< 10 ký tự)

**Giải pháp:**
- Kiểm tra error message chi tiết trong log
- Đảm bảo:
  - Có ít nhất 3 nguyên liệu với `ingredientId` hợp lệ (UUID)
  - Có ít nhất 3 bước với mô tả >= 10 ký tự
  - `categoryId` là UUID hợp lệ

---

### 3. Lỗi Network / Timeout
**Triệu chứng:**
```
❌ [API] Request failed: { type: 'Network Error', ... }
```

**Nguyên nhân:**
- Backend server không chạy
- URL không đúng
- CORS issue
- Timeout (quá 10 giây)

**Giải pháp:**
1. Kiểm tra backend có đang chạy không:
   ```bash
   # Kiểm tra backend
   curl http://localhost:3000/health
   ```

2. Kiểm tra biến môi trường:
   - File `.env.local` hoặc `.env` trong `fe-web`
   - `NEXT_PUBLIC_API_URL=http://localhost:3000` (hoặc URL backend của bạn)

3. Kiểm tra CORS:
   - Backend phải cho phép origin của frontend

---

### 4. Không có log nào xuất hiện
**Triệu chứng:**
- Không thấy log `📤 [CreateRecipe] Submitting recipe`
- Form không submit

**Nguyên nhân:**
- Validation fail ở frontend
- Button bị disable
- JavaScript error

**Giải pháp:**
1. Kiểm tra error message trên form (màu đỏ)
2. Kiểm tra Console có error JavaScript không
3. Kiểm tra Network tab xem có request nào được gửi không

---

## 📋 CHECKLIST DEBUG

### Frontend
- [ ] Console có log `📤 [CreateRecipe] Submitting recipe`?
- [ ] Console có log `🔵 [API] POST /user/recipes`?
- [ ] Console có error nào không?
- [ ] Network tab có request đến `/user/recipes`?
- [ ] Request có status code gì? (200, 400, 401, 500?)
- [ ] Request payload có đúng format không?

### Backend
- [ ] Backend server có đang chạy?
- [ ] Backend logs có nhận được request không?
- [ ] Backend logs có error gì không?
- [ ] Database có kết nối được không?

### Data
- [ ] `ingredientId` có phải UUID hợp lệ? (format: `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`)
- [ ] `categoryId` có phải UUID hợp lệ?
- [ ] Có ít nhất 3 nguyên liệu?
- [ ] Có ít nhất 3 bước?
- [ ] Mỗi bước có >= 10 ký tự?

---

## 🔧 CÁCH SỬA LỖI

### Nếu lỗi do `ingredientId` không hợp lệ:
1. Đảm bảo chọn nguyên liệu từ danh sách (không tự nhập)
2. Nếu import từ AI, đợi hệ thống tự động tìm `ingredientId`
3. Nếu không tìm thấy, chọn lại từ danh sách

### Nếu lỗi do thiếu nguyên liệu/bước:
1. Thêm đủ 3 nguyên liệu
2. Thêm đủ 3 bước
3. Mỗi bước phải có mô tả >= 10 ký tự

### Nếu lỗi do backend không kết nối:
1. Kiểm tra backend có chạy không
2. Kiểm tra `NEXT_PUBLIC_API_URL` trong `.env`
3. Kiểm tra CORS settings

---

## 📞 THÔNG TIN CẦN CUNG CẤP KHI BÁO LỖI

Nếu vẫn không giải quyết được, cung cấp:

1. **Console logs:**
   - Copy tất cả log có icon 📤, 🔵, ✅, ❌

2. **Network request:**
   - Screenshot Network tab
   - Request URL
   - Request payload
   - Response status
   - Response body

3. **Error message:**
   - Error message hiển thị trên form
   - Error trong Console

4. **Thông tin môi trường:**
   - Backend URL
   - Frontend URL
   - Browser (Chrome, Firefox, Safari, Edge)

---

## ✅ SAU KHI SỬA

Sau khi sửa xong, các log sẽ giúp bạn:
- Xác định chính xác vấn đề ở đâu
- Biết được request có được gửi không
- Biết được response từ backend là gì
- Debug nhanh hơn trong tương lai

**Lưu ý:** Các log này chỉ hiển thị trong development mode. Trong production, các log sẽ được tắt tự động.

