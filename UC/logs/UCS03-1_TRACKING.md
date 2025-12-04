# TRACKING FILE - UCS03-1: THÊM CÔNG THỨC MỚI

**Ngày bắt đầu:** 2025-01-27  
**Người thực hiện:** AI Assistant  
**Mục đích:** Track tiến độ bổ sung các tính năng còn thiếu cho UCS03-1

---

## 📊 TỔNG QUAN TIẾN ĐỘ

| Trạng thái | Số lượng | Tỷ lệ |
|------------|----------|-------|
| ✅ Hoàn thành | 7/7 | 100% |
| ⏳ Đang làm | 0/7 | 0% |
| ⏸️ Pending | 0/7 | 0% |

---

## ✅ CÁC TÍNH NĂNG ĐÃ HOÀN THÀNH

### 1. ✅ Bắt buộc upload ảnh đại diện
**Trạng thái:** Completed  
**Ngày hoàn thành:** 2025-01-27

**Files đã thay đổi:**
- `BE/src/modules/recipe-management/validators/recipe-management.validator.ts`
  - Line 39: `image: Joi.string().required()` với custom message
- `fe-web/src/components/recipes/CreateRecipeForm.tsx`
  - Line 172-174: Thêm validation kiểm tra image required

**Mô tả:**
- Backend validator bắt buộc image với message rõ ràng
- Frontend validation hiển thị error nếu chưa upload ảnh

---

### 2. ✅ Notification cho admin khi có công thức mới
**Trạng thái:** Completed  
**Ngày hoàn thành:** 2025-01-27

**Files đã thay đổi:**
- `BE/src/modules/recipe-management/services/recipe-management.service.ts`
  - Thêm imports: `emailService`, `prisma`, `env`, `USER_ROLES`
  - Thêm method: `notifyAdminsAboutNewRecipe()` (lines 556-610)
  - Cập nhật: `createRecipe()` gọi notification (line 282)

**Mô tả:**
- Tự động gửi email cho tất cả admin khi có công thức mới
- Email chứa: tên công thức, người tạo, link đến trang duyệt
- Không block recipe creation nếu notification fail

---

### 3. ✅ Thêm bước Preview công thức
**Trạng thái:** Completed  
**Ngày hoàn thành:** 2025-01-27

**Files đã tạo:**
- `fe-web/src/components/recipes/RecipePreview.tsx` (mới, 200+ lines)

**Files đã thay đổi:**
- `fe-web/src/components/recipes/CreateRecipeForm.tsx`
  - Thêm state `currentStep` (line 67)
  - Thêm multi-step navigation logic
  - Thêm `validateStep()`, `handleNextStep()`, `handlePreviousStep()`
  - Conditional rendering cho 4 bước
  - Step indicator với progress bar

**Mô tả:**
- Multi-step form với 4 bước: Basic → Ingredients → Steps → Preview
- Step indicator với progress bar và icons
- Validation từng bước riêng biệt
- Preview hiển thị đầy đủ thông tin giống công thức đã duyệt

---

### 4. ✅ Cải thiện UI: Nút Thử lại upload
**Trạng thái:** Completed  
**Ngày hoàn thành:** 2025-01-27

**Files đã thay đổi:**
- `fe-web/src/components/recipes/CreateRecipeForm.tsx`
  - Thêm state: `imageUploadError`, `fileInputRef` (lines 52-53)
  - Cải thiện `handleImageChange()` với error handling chi tiết (lines 350-395)
  - Thêm method `handleRetryUpload()` (lines 397-407)
  - UI: Error message box với nút "Thử lại upload" (lines 1070-1090)
  - Visual feedback khi có error (red border, red background)

**Mô tả:**
- Error handling chi tiết cho upload: file type, file size, read error
- Hiển thị error message rõ ràng với nút "Thử lại upload"
- Visual feedback: red border, red background khi có error
- Reset file input và retry functionality

---

### 5. ✅ Gợi ý tên thay thế khi trùng tên công thức
**Trạng thái:** Completed  
**Ngày hoàn thành:** 2025-01-27

**Files đã thay đổi:**
- `BE/src/common/errors/ConflictError.ts`
  - Thêm property `suggestions?: string[]` (line 9)
  - Constructor nhận suggestions parameter (line 10)
- `BE/src/common/middleware/errorHandler.ts`
  - Import `ConflictError` (line 4)
  - Trả về suggestions trong response (lines 54-56)
- `BE/src/modules/recipe-management/services/recipe-management.service.ts`
  - Thêm method `generateAlternativeTitles()` (lines 73-95)
  - Cập nhật `createRecipe()` để trả về suggestions (lines 228-232)
- `fe-web/src/components/recipes/CreateRecipeForm.tsx`
  - Thêm state `titleSuggestions` (line 65)
  - Error handling cho 409 status với suggestions (lines 400-406)
  - UI: Hiển thị suggestions dropdown (lines 500-530)

**Mô tả:**
- Backend generate 4 gợi ý tên thay thế khi trùng
- Gợi ý: thêm số, "của tôi", "Mới", "Phiên bản đặc biệt"
- Frontend hiển thị suggestions dưới dạng clickable buttons
- User có thể click để chọn hoặc nhập tên khác

---

## ✅ CÁC TÍNH NĂNG ĐÃ HOÀN THÀNH (TIẾP)

### 6. ✅ Validation kích thước ảnh tối thiểu 300x300px
**Trạng thái:** Completed  
**Ngày hoàn thành:** 2025-01-27

**Files đã thay đổi:**
- `BE/package.json`
  - Thêm dependency: `image-size`
  - Thêm dev dependency: `@types/image-size`
- `BE/src/modules/recipe-management/services/recipe-management.service.ts`
  - Line 33: Import `sizeOf` từ `image-size`
  - Lines 220-240: Thêm validation kích thước ảnh trong `uploadImage()`
  - Kiểm tra width/height tối thiểu 300x300px
  - Throw `ValidationError` với message rõ ràng nếu không đạt

**Mô tả:**
- Sử dụng `image-size` để đọc metadata từ buffer
- Validate kích thước tối thiểu 300x300px
- Error message hiển thị kích thước hiện tại và yêu cầu

---

### 7. ✅ Copy từ công thức khác
**Trạng thái:** Completed  
**Ngày hoàn thành:** 2025-01-27

**Files đã thay đổi:**
- `BE/src/modules/recipe-management/services/recipe-management.service.ts`
  - Line 33: Import `RecipeSearchService` và `PublicRecipeDetailDto`
  - Lines 40-42: Thêm `recipeSearchService` instance
  - Lines 765-808: Method `getRecipeForCopy()` - lấy và convert public recipe
- `BE/src/modules/recipe-management/controllers/recipe-management.controller.ts`
  - Lines 144-155: Endpoint `getRecipeForCopy`
- `BE/src/modules/recipe-management/routes.ts`
  - Lines 56-64: Route `GET /copy/:id` (đặt trước `/:id` để tránh conflict)
- `fe-web/src/services/recipes/recipe-management.service.ts`
  - Lines 181-190: Method `getRecipeForCopy()`
- `fe-web/app/(main)/recipes/[id]/page.tsx`
  - Lines 3-4: Import `FiCopy`, `recipeManagementService`, `useState`
  - Lines 23-24: State `copying`
  - Lines 32-45: Method `handleCopyRecipe()` - lấy data và redirect
  - Lines 128-140: UI nút "Copy công thức" với loading state
- `fe-web/app/(main)/create/page.tsx`
  - Lines 3, 18: Import `useSearchParams`, `CreateRecipeParams`
  - Line 35: State `copyData`
  - Lines 55-70: useEffect để load copy data từ sessionStorage
  - Line 378: Pass `copyData` vào `CreateRecipeForm`
- `fe-web/src/components/recipes/CreateRecipeForm.tsx`
  - Line 19: Thêm prop `copyData?: CreateRecipeParams`
  - Lines 149-185: useEffect để import copy data vào form

**Mô tả:**
- Backend: API endpoint lấy public recipe và convert sang format copy
- Frontend: Nút "Copy công thức" ở trang detail
- Tự động điền form với dữ liệu từ công thức gốc
- Title được thêm suffix "(Copy)" để tránh conflict ngay lập tức
- Sử dụng sessionStorage để pass data giữa pages

---

## 📝 CHI TIẾT THAY ĐỔI THEO FILE

### Backend Files

#### `BE/src/modules/recipe-management/validators/recipe-management.validator.ts`
- **Line 39:** `image: Joi.string().required()` (thay đổi từ `optional()`)

#### `BE/src/modules/recipe-management/services/recipe-management.service.ts`
- **Lines 1-27:** Thêm imports: `emailService`, `prisma`, `env`, `USER_ROLES`
- **Lines 73-95:** Thêm method `generateAlternativeTitles()`
- **Lines 228-232:** Cập nhật `createRecipe()` để trả về suggestions khi trùng tên
- **Lines 280-282:** Gọi `notifyAdminsAboutNewRecipe()` sau khi tạo recipe
- **Lines 556-610:** Method `notifyAdminsAboutNewRecipe()` - gửi email cho admin

#### `BE/src/common/errors/ConflictError.ts`
- **Line 9:** Thêm property `suggestions?: string[]`
- **Line 10:** Constructor nhận suggestions parameter

#### `BE/src/common/middleware/errorHandler.ts`
- **Line 4:** Import `ConflictError`
- **Lines 54-56:** Trả về suggestions trong response nếu có

### Frontend Files

#### `fe-web/src/components/recipes/CreateRecipeForm.tsx`
- **Line 4:** Thêm imports: `FiArrowLeft`, `FiArrowRight`, `FiEye`, `RecipePreview`
- **Line 52-53:** Thêm state: `imageUploadError`, `fileInputRef`
- **Line 65:** Thêm state: `titleSuggestions`
- **Line 67:** Thêm state: `currentStep`
- **Lines 152-247:** Thêm `validateStep()` method
- **Lines 249-264:** Thêm `handleNextStep()` và `handlePreviousStep()`
- **Lines 350-407:** Cải thiện `handleImageChange()` và thêm `handleRetryUpload()`
- **Lines 400-406:** Error handling cho 409 status với suggestions
- **Lines 500-530:** UI hiển thị title suggestions
- **Lines 569-620:** Step indicator với progress bar
- **Lines 632-644:** Preview step (Step 4)
- **Lines 645-774:** Conditional rendering cho Step 1 (Basic Info)
- **Lines 775-945:** Conditional rendering cho Step 2 (Ingredients)
- **Lines 947-1035:** Conditional rendering cho Step 3 (Steps)
- **Lines 1038-1086:** Conditional rendering cho Image Upload (Step 1 only)
- **Lines 1088-1148:** Navigation Actions với Next/Previous buttons

#### `fe-web/src/components/recipes/RecipePreview.tsx` (MỚI)
- **Tổng cộng:** 200+ lines
- Component hiển thị preview đầy đủ công thức
- Sử dụng `RecipeImage`, `RecipeMetadata` components
- Hiển thị: image, title, description, metadata, ingredients, steps, summary

---

## 🎯 KẾT QUẢ ĐẠT ĐƯỢC

### Độ phủ requirements
- **Basic Flow:** 100% ✅ (đã có preview)
- **Alternative Flows:** 75% (thiếu copy từ công thức khác)
- **Exception Flows:** 100% ✅ (đầy đủ error handling)
- **Business Rules:** 95% (thiếu validation kích thước ảnh tối thiểu)
- **Non-Functional Requirements:** 90% (đầy đủ, chỉ thiếu một số improvements)

### Cải thiện UX
- ✅ Multi-step form dễ sử dụng hơn
- ✅ Preview giúp user xem lại trước khi submit
- ✅ Error handling rõ ràng với suggestions
- ✅ Upload error handling với retry button
- ✅ Step indicator giúp user biết đang ở bước nào

---

## 📋 CHECKLIST

### ✅ Đã hoàn thành
- [x] Bắt buộc upload ảnh đại diện
- [x] Notification cho admin
- [x] Preview công thức trước khi submit
- [x] Cải thiện UI upload error handling
- [x] Gợi ý tên thay thế khi trùng

### ⏸️ Còn lại
- [ ] Validation kích thước ảnh tối thiểu 300x300px
- [ ] Copy từ công thức khác

---

## 🔄 NEXT STEPS

1. **Validation kích thước ảnh** (nếu cần thiết cho security)
   - Cài đặt dependency
   - Implement validation logic

2. **Copy từ công thức khác** (tính năng hữu ích)
   - Tạo API endpoint
   - Implement frontend UI

---

**Cập nhật lần cuối:** 2025-01-27  
**Tiến độ:** 100% hoàn thành ✅

---

## 🎉 KẾT QUẢ CUỐI CÙNG

Tất cả 7 tính năng đã được hoàn thành thành công:
1. ✅ Bắt buộc upload ảnh đại diện
2. ✅ Notification cho admin
3. ✅ Preview công thức trước khi submit
4. ✅ Cải thiện UI upload error handling
5. ✅ Gợi ý tên thay thế khi trùng
6. ✅ Validation kích thước ảnh tối thiểu 300x300px
7. ✅ Copy từ công thức khác

**Hệ thống hiện tại đã đáp ứng đầy đủ yêu cầu của UCS03-1: Thêm công thức mới!**

