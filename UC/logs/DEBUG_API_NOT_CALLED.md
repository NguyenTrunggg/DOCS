# DEBUG: API không được gọi khi tạo công thức

## 🔍 VẤN ĐỀ

Khi nhấn nút "Tạo công thức", API không được gọi và form quay về bước 1.

## 📋 CÁCH KIỂM TRA

### Bước 1: Mở Browser Console
1. Mở trang tạo công thức
2. Nhấn **F12** hoặc **Ctrl+Shift+I**
3. Chọn tab **Console**

### Bước 2: Thử tạo công thức
1. Điền đầy đủ thông tin ở cả 3 bước
2. Nhấn nút **"Tạo công thức"**
3. Quan sát các log trong Console

---

## 🔍 CÁC LOG CẦN KIỂM TRA

### ✅ Nếu thấy log này → Button click hoạt động:
```
🚀 [CreateRecipe] handleSubmit called { isDraft: false, currentStep: 3 }
```

### ✅ Nếu thấy log này → Validation đang chạy:
```
🔍 [CreateRecipe] Starting validation...
🔍 [CreateRecipe] Validation results: { step1Valid: true/false, ... }
```

### ❌ Nếu thấy log này → Validation fail:
```
❌ [CreateRecipe] Step X validation failed, going to step X
⚠️ [CreateRecipe] Validation failed, stopping submit
```

**→ API KHÔNG được gọi vì validation fail**

### ✅ Nếu thấy log này → Validation pass, đang chuẩn bị gọi API:
```
✅ [CreateRecipe] All validations passed, proceeding to API call...
🔍 [CreateRecipe] Validating ingredients...
✅ [CreateRecipe] Ingredients validation passed
📤 [CreateRecipe] About to call API with params: { ... }
```

### ✅ Nếu thấy log này → API đang được gọi:
```
🌐 [CreateRecipe] Calling API service...
✨ [CreateRecipe] Calling createRecipe API...
🔵 [API] POST /user/recipes - Request params: { ... }
📤 [API] Sending payload: { ... }
```

### ❌ Nếu KHÔNG thấy log `🚀 [CreateRecipe] handleSubmit called`:
**→ Button click KHÔNG hoạt động**

---

## 🐛 CÁC NGUYÊN NHÂN THƯỜNG GẶP

### 1. Validation fail ở Step 1
**Triệu chứng:**
```
❌ [CreateRecipe] Step 1 validation failed, going to step 1
```

**Nguyên nhân:**
- Tên món ăn < 3 ký tự
- Chưa chọn danh mục
- Thời gian nấu < 1 phút
- Khẩu phần < 1 người

**Giải pháp:**
- Kiểm tra lại thông tin ở bước 1
- Đảm bảo tất cả field đã điền đúng

---

### 2. Validation fail ở Step 2
**Triệu chứng:**
```
❌ [CreateRecipe] Step 2 validation failed, going to step 2
```

**Nguyên nhân:**
- Chưa đủ 3 nguyên liệu
- Nguyên liệu chưa được chọn từ danh sách (thiếu `ingredientId`)
- `ingredientId` không phải UUID hợp lệ

**Giải pháp:**
- Đảm bảo có ít nhất 3 nguyên liệu
- Tất cả nguyên liệu phải được chọn từ danh sách (không tự nhập)
- Kiểm tra log `🔍 [CreateRecipe] Validating ingredients...` để xem nguyên liệu nào không hợp lệ

---

### 3. Validation fail ở Step 3
**Triệu chứng:**
```
❌ [CreateRecipe] Step 3 validation failed, going to step 3
```

**Nguyên nhân:**
- Chưa đủ 3 bước
- Mô tả bước < 10 ký tự

**Giải pháp:**
- Đảm bảo có ít nhất 3 bước
- Mỗi bước phải có mô tả >= 10 ký tự

---

### 4. Ingredients validation fail
**Triệu chứng:**
```
⚠️ [CreateRecipe] Ingredient missing ID: ...
⚠️ [CreateRecipe] Ingredient has invalid UUID: ...
❌ [CreateRecipe] Not enough valid ingredients, stopping submit
```

**Nguyên nhân:**
- Nguyên liệu không có `ingredientId`
- `ingredientId` không phải UUID hợp lệ

**Giải pháp:**
- Chọn lại nguyên liệu từ danh sách
- Nếu import từ AI, đợi hệ thống tự động tìm `ingredientId`
- Nếu không tìm thấy, chọn lại từ danh sách

---

### 5. Button không được click
**Triệu chứng:**
- KHÔNG thấy log `🚀 [CreateRecipe] handleSubmit called`
- Button có thể bị disabled

**Nguyên nhân:**
- Button bị disable (`isSubmitting` hoặc `isSavingDraft` = true)
- Có lỗi JavaScript ngăn event handler

**Giải pháp:**
- Kiểm tra button có bị disabled không
- Kiểm tra Console có lỗi JavaScript không
- Refresh trang và thử lại

---

## 📊 FLOW CHART DEBUG

```
Nhấn "Tạo công thức"
    ↓
🚀 handleSubmit called? 
    ├─ NO → Button không hoạt động → Kiểm tra button/JavaScript
    └─ YES
        ↓
🔍 Validation Step 1
    ├─ FAIL → ❌ Quay về Step 1 → Sửa thông tin cơ bản
    └─ PASS
        ↓
🔍 Validation Step 2
    ├─ FAIL → ❌ Quay về Step 2 → Sửa nguyên liệu
    └─ PASS
        ↓
🔍 Validation Step 3
    ├─ FAIL → ❌ Quay về Step 3 → Sửa các bước
    └─ PASS
        ↓
🔍 Validate Ingredients UUID
    ├─ FAIL → ❌ Quay về Step 2 → Chọn lại nguyên liệu
    └─ PASS
        ↓
📤 Prepare params
    ↓
🌐 Call API
    ├─ SUCCESS → ✅ Công thức được tạo
    └─ ERROR → ❌ Xem error message
```

---

## ✅ CHECKLIST

- [ ] Console có log `🚀 [CreateRecipe] handleSubmit called`?
- [ ] Console có log `🔍 [CreateRecipe] Starting validation...`?
- [ ] Console có log `🔍 [CreateRecipe] Validation results`?
- [ ] Tất cả steps đều `valid: true`?
- [ ] Console có log `✅ [CreateRecipe] All validations passed`?
- [ ] Console có log `🔍 [CreateRecipe] Validating ingredients...`?
- [ ] Console có log `✅ [CreateRecipe] Ingredients validation passed`?
- [ ] Console có log `📤 [CreateRecipe] About to call API`?
- [ ] Console có log `🌐 [CreateRecipe] Calling API service...`?
- [ ] Console có log `🔵 [API] POST /user/recipes`?

---

## 🔧 CÁCH SỬA

### Nếu validation fail:
1. Xem log để biết step nào fail
2. Sửa thông tin ở step đó
3. Thử lại

### Nếu button không click:
1. Kiểm tra button có bị disabled không
2. Kiểm tra Console có lỗi JavaScript không
3. Refresh trang và thử lại

### Nếu API không được gọi sau khi validation pass:
1. Kiểm tra log `📤 [CreateRecipe] About to call API`
2. Nếu không thấy log này, có thể có lỗi JavaScript
3. Kiểm tra Network tab xem có request nào không

---

## 📞 THÔNG TIN CẦN CUNG CẤP

Nếu vẫn không giải quyết được, cung cấp:

1. **Tất cả logs từ Console** (copy toàn bộ)
2. **Screenshot Console**
3. **Screenshot Network tab** (nếu có request)
4. **Thông tin:**
   - Đã điền đầy đủ thông tin chưa?
   - Có bao nhiêu nguyên liệu?
   - Có bao nhiêu bước?
   - Nguyên liệu có được chọn từ danh sách không?

