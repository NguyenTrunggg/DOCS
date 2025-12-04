# DEBUG: Vấn đề tạo công thức không hoạt động

## 1. KIỂM TRA API CALL

### 1.1. Endpoint
- **Frontend gọi:** `POST /user/recipes`
- **Full URL:** `${API_BASE_URL}/api/v1/user/recipes`
- **Backend route:** `/api/v1/user/recipes` ✅

### 1.2. Payload Structure

**Frontend gửi:**
```typescript
{
  title: string,
  description?: string,
  categoryId: string,
  prepTime: number,
  cookTime: number,
  difficulty: 'EASY' | 'MEDIUM' | 'HARD',
  servings: number,
  ingredients: Array<{
    ingredientId: string,  // UUID
    quantity: number,
    unit: string,
    notes?: string
  }>,
  steps: Array<{
    stepNumber: number,
    description: string,
    image?: string,
    duration?: number
  }>,
  image?: string  // Base64 data URL
}
```

**Backend expect:**
```typescript
{
  title: string,
  description?: string,
  categoryId: string,
  prepTime: number,
  cookTime: number,
  difficulty: 'EASY' | 'MEDIUM' | 'HARD',
  servings: number,
  ingredients: Array<{
    ingredientId: string,  // UUID required
    quantity: number,
    unit: string,
    notes?: string
  }>,
  steps: Array<{
    stepNumber: number,
    description: string,
    image?: string,
    duration?: number
  }>,
  image?: string
}
```

✅ **Format khớp nhau**

---

## 2. CÁC VẤN ĐỀ CÓ THỂ XẢY RA

### 2.1. Authentication ❓
- **Kiểm tra:** Token có được gửi trong header không?
- **API Client:** Có interceptor thêm token ✅
- **Backend:** Route có `router.use(authenticate)` ✅

### 2.2. Validation ❓
- **Frontend:** Validate trước khi submit ✅
- **Backend:** Joi validator kiểm tra:
  - `title`: min 3, max 100 ✅
  - `categoryId`: UUID required ✅
  - `ingredients`: min 3 items, mỗi item có `ingredientId` UUID ✅
  - `steps`: min 3 items, mỗi step min 10 chars ✅

### 2.3. Network/Connection ❓
- **API_BASE_URL:** Default `http://localhost:3000`
- **CORS:** Backend có cho phép frontend không?
- **Timeout:** 10 seconds

### 2.4. Error Handling ❓
- Frontend catch error và hiển thị message
- Backend trả về error format: `{ success: false, message: string, errors?: [] }`

---

## 3. CÁCH DEBUG

### 3.1. Kiểm tra Console (Browser)
1. Mở DevTools (F12)
2. Tab Console - xem có error không
3. Tab Network - xem request có được gửi không
   - Status code?
   - Response body?
   - Request payload?

### 3.2. Kiểm tra Backend Logs
1. Xem server logs khi submit
2. Kiểm tra có request đến không
3. Kiểm tra validation errors

### 3.3. Test với Postman/Thunder Client
```http
POST http://localhost:3000/api/v1/user/recipes
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "Test Recipe",
  "categoryId": "<valid-uuid>",
  "prepTime": 10,
  "cookTime": 20,
  "difficulty": "EASY",
  "servings": 2,
  "ingredients": [
    {
      "ingredientId": "<valid-ingredient-uuid>",
      "quantity": 1,
      "unit": "cái"
    },
    {
      "ingredientId": "<valid-ingredient-uuid>",
      "quantity": 2,
      "unit": "cái"
    },
    {
      "ingredientId": "<valid-ingredient-uuid>",
      "quantity": 3,
      "unit": "cái"
    }
  ],
  "steps": [
    {
      "stepNumber": 1,
      "description": "Bước đầu tiên với mô tả đầy đủ"
    },
    {
      "stepNumber": 2,
      "description": "Bước thứ hai với mô tả đầy đủ"
    },
    {
      "stepNumber": 3,
      "description": "Bước thứ ba với mô tả đầy đủ"
    }
  ]
}
```

---

## 4. CHECKLIST DEBUG

- [ ] Backend server đang chạy?
- [ ] Frontend có kết nối được backend không?
- [ ] User đã đăng nhập? Token có hợp lệ?
- [ ] Payload có đúng format không?
- [ ] `ingredientId` có phải UUID hợp lệ không?
- [ ] `categoryId` có phải UUID hợp lệ không?
- [ ] Có ít nhất 3 nguyên liệu với `ingredientId` hợp lệ?
- [ ] Có ít nhất 3 bước với description >= 10 ký tự?
- [ ] Network request có được gửi không? (Check Network tab)
- [ ] Response status code là gì? (200, 400, 401, 500?)
- [ ] Error message từ backend là gì?

---

## 5. CÁC LỖI THƯỜNG GẶP

### 5.1. 401 Unauthorized
- **Nguyên nhân:** Token không hợp lệ hoặc hết hạn
- **Giải pháp:** Đăng nhập lại

### 5.2. 400 Bad Request
- **Nguyên nhân:** Validation error
- **Giải pháp:** Kiểm tra:
  - `ingredientId` có phải UUID không?
  - Có đủ 3 nguyên liệu không?
  - Có đủ 3 bước không?
  - Mỗi bước có >= 10 ký tự không?

### 5.3. 500 Internal Server Error
- **Nguyên nhân:** Lỗi server
- **Giải pháp:** Xem server logs

### 5.4. Network Error / Timeout
- **Nguyên nhân:** Backend không chạy hoặc không kết nối được
- **Giải pháp:** Kiểm tra `NEXT_PUBLIC_API_URL` và backend server

---

## 6. CODE ĐỂ THÊM LOGGING

Thêm vào `CreateRecipeForm.tsx`:

```typescript
const handleSubmit = useCallback(async (isDraft = false) => {
  // ... existing code ...
  
  try {
    // ... existing code ...
    
    console.log('📤 Submitting recipe:', {
      isDraft,
      params,
      ingredientsCount: params.ingredients.length,
      stepsCount: params.steps.length,
      hasImage: !!params.image
    });
    
    let result;
    if (isDraft) {
      result = await recipeManagementService.saveDraft(params);
    } else {
      result = await recipeManagementService.createRecipe(params);
    }
    
    console.log('✅ Recipe created successfully:', result);
    
    // ... existing code ...
  } catch (err: any) {
    console.error('❌ Failed to create recipe:', {
      error: err,
      response: err.response?.data,
      status: err.response?.status,
      message: err.message
    });
    // ... existing code ...
  }
}, [/* deps */]);
```

Thêm vào `recipe-management.service.ts`:

```typescript
async createRecipe(params: CreateRecipeParams): Promise<RecipeDetail> {
  console.log('🔵 API Call: POST /user/recipes', params);
  
  // ... existing code ...
  
  try {
    const response = await apiClient.post<ApiResponse<RecipeDetail>>(
      '/user/recipes',
      payload
    );
    console.log('✅ API Response:', response.data);
    return response.data.data;
  } catch (error) {
    console.error('❌ API Error:', error);
    throw error;
  }
}
```

---

## 7. NEXT STEPS

1. **Thêm logging** như trên
2. **Mở Browser Console** và thử tạo công thức
3. **Kiểm tra Network tab** để xem request/response
4. **Kiểm tra Backend logs** để xem có request đến không
5. **Test với Postman** để xác định vấn đề ở frontend hay backend

