# Báo Cáo Kiểm Tra: UCS02-3 - Tạo Công Thức Bằng AI

**Ngày kiểm tra:** $(date)  
**Người kiểm tra:** AI Assistant  
**Use Case:** UCS02-3 - Tạo công thức bằng AI [HIGH PRIORITY]

---

## TÓM TẮT

Hệ thống hiện tại **ĐÃ CÓ CẤU TRÚC CƠ BẢN** nhưng **CHƯA HOÀN THIỆN** để đáp ứng đầy đủ yêu cầu của UCS02-3.

### Trạng thái tổng quan:
- ✅ **Backend Infrastructure:** Đã có đầy đủ (70%)
- ⚠️ **AI Integration:** Chưa tích hợp thực sự (0%)
- ⚠️ **Frontend UI:** Đã có button nhưng chưa implement handler (30%)
- ✅ **Business Rules:** Đã implement (80%)

---

## CHI TIẾT KIỂM TRA

### 1. BACKEND IMPLEMENTATION

#### ✅ **1.1. API Endpoint**
- **File:** `src/modules/recipe-search/routes.ts`
- **Route:** `POST /api/v1/recipes/ai/generate`
- **Status:** ✅ Đã có
- **Middleware:** 
  - ✅ `optionalAuthenticate` - Cho phép cả user và anonymous
  - ✅ `validate({ body: aiGenerateRecipeValidator })` - Validation đầy đủ

```112:117:d:\DATN\BE\src\modules\recipe-search\routes.ts
router.post(
  '/ai/generate',
  optionalAuthenticate,
  validate({ body: aiGenerateRecipeValidator }),
  recipeSearchController.generateAiRecipe
);
```

#### ✅ **1.2. Controller**
- **File:** `src/modules/recipe-search/controllers/recipe-search.controller.ts`
- **Status:** ✅ Đã implement
- **Functionality:** 
  - ✅ Nhận request từ client
  - ✅ Gọi service để generate recipe
  - ✅ Trả về response với status 201 (CREATED)

```109:116:d:\DATN\BE\src\modules\recipe-search\controllers\recipe-search.controller.ts
  generateAiRecipe = asyncHandler(async (req: AuthRequest, res: Response) => {
    const payload = req.body as AiGenerateRecipeRequestDto;
    const result = await this.aiRecipeService.generateRecipe(payload, {
      userId: req.user?.id,
      requesterIp: req.ip,
    });
    successResponse(res, result, 'AI đã tạo công thức thành công', HTTP_STATUS.CREATED);
  });
```

#### ✅ **1.3. Service Layer**
- **File:** `src/modules/recipe-search/services/ai-recipe.service.ts`
- **Status:** ✅ Đã implement logic xử lý
- **Functionality:**
  - ✅ Validate ít nhất 2 nguyên liệu
  - ✅ Kiểm tra usage limit (5 lần/ngày)
  - ✅ Gọi AI service để generate
  - ✅ Xử lý error handling
  - ✅ Lưu vào session store (24 giờ)

```14:61:d:\DATN\BE\src\modules\recipe-search\services\ai-recipe.service.ts
export class AiRecipeService {
  async generateRecipe(
    payload: AiGenerateRecipeRequestDto,
    context: GenerateContext
  ): Promise<AiRecipeResponseDto> {
    if (payload.ingredients.length < 2) {
      throw new ValidationError('Cần ít nhất 2 nguyên liệu để tạo công thức AI');
    }

    const limiterKey = context.userId ?? context.requesterIp ?? 'anonymous';
    if (!aiUsageLimiter.canUse(limiterKey)) {
      throw new TooManyRequestsError('Bạn đã vượt quá giới hạn 5 lần tạo công thức AI mỗi ngày.');
    }

    aiUsageLimiter.consume(limiterKey);

    const aiResponse = await aiService.generateRecipe({
      ingredients: payload.ingredients,
      cuisine: payload.options?.cuisine,
      dietaryRestrictions: payload.options?.dietaryPreference ? [payload.options.dietaryPreference] : undefined,
      cookingTime: payload.options?.difficulty === 'EASY' ? 20 : payload.options?.difficulty === 'HARD' ? 60 : 40,
    });

    if (!aiResponse) {
      throw new ServiceUnavailableError('AI hiện đang bận. Vui lòng thử lại sau ít phút.');
    }

    const recipe = {
      title: aiResponse.recipe.title,
      description: aiResponse.recipe.description,
      ingredients: aiResponse.recipe.ingredients,
      steps: aiResponse.recipe.instructions.map((instruction, index) => ({
        order: index + 1,
        description: instruction,
      })),
      cookingTime: aiResponse.recipe.cookingTime,
      servings: payload.options?.servings ?? aiResponse.recipe.servings,
      difficulty: payload.options?.difficulty,
      dietaryPreference: payload.options?.dietaryPreference,
    };

    const session = aiRecipeSessionStore.save(recipe, context.userId);

    return {
      sessionId: session.id,
      recipe,
      warnings: payload.options?.notes ? [payload.options.notes] : undefined,
    };
  }
}
```

#### ❌ **1.4. AI Service Integration**
- **File:** `src/infrastructure/ai/ai.service.ts`
- **Status:** ❌ **CHƯA TÍCH HỢP THỰC SỰ**
- **Vấn đề:** 
  - ⚠️ Chỉ có placeholder implementation
  - ⚠️ Có TODO comment: "TODO: Implement actual AI API integration"
  - ⚠️ Trả về dữ liệu mock/placeholder
  - ✅ Có cấu hình env cho AI_API_KEY và AI_API_URL

```43:75:d:\DATN\BE\src\infrastructure\ai\ai.service.ts
  async generateRecipe(request: AIRecipeRequest): Promise<AIRecipeResponse | null> {
    try {
      // TODO: Implement actual AI API integration
      // This is a placeholder implementation
      
      logger.info('Generating recipe with AI:', request);

      // Example implementation - replace with actual API call
      if (!this.apiKey) {
        logger.warn('AI API key not configured');
        return null;
      }

      // Placeholder response
      return {
        recipe: {
          title: 'AI Generated Recipe',
          description: 'A recipe generated by AI',
          ingredients: request.ingredients.map(ing => ({
            name: ing,
            amount: '1',
            unit: 'cup',
          })),
          instructions: ['Step 1', 'Step 2', 'Step 3'],
          cookingTime: request.cookingTime || 30,
          servings: 4,
        },
      };
    } catch (error) {
      logger.error('Failed to generate recipe with AI:', error);
      return null;
    }
  }
```

**Cần làm:**
- Tích hợp với OpenAI API hoặc AI service khác
- Implement prompt engineering để tạo công thức chất lượng
- Parse và validate JSON response từ AI
- Xử lý timeout và retry logic

#### ✅ **1.5. Usage Limiter**
- **File:** `src/modules/recipe-search/utils/ai-usage-limiter.ts`
- **Status:** ✅ Đã implement đầy đủ
- **Functionality:**
  - ✅ Giới hạn 5 lần/ngày (đúng yêu cầu)
  - ✅ Reset tự động sau 24 giờ
  - ✅ Cleanup expired records
  - ✅ Hỗ trợ cả user ID và IP address

```9:48:d:\DATN\BE\src\modules\recipe-search\utils\ai-usage-limiter.ts
class AiUsageLimiter {
  private usage = new Map<string, AiUsageRecord>();

  canUse(key: string, limit: number = DAILY_LIMIT): boolean {
    this.cleanup();
    const record = this.usage.get(key);
    if (!record) {
      this.usage.set(key, { count: 0, resetAt: Date.now() + DAY_MS });
      return true;
    }
    if (record.count >= limit) {
      return false;
    }
    return true;
  }

  consume(key: string, limit: number = DAILY_LIMIT): void {
    const record = this.usage.get(key);
    if (!record) {
      this.usage.set(key, { count: 1, resetAt: Date.now() + DAY_MS });
      return;
    }
    if (record.count >= limit) {
      return;
    }
    record.count += 1;
    this.usage.set(key, record);
  }

  private cleanup(): void {
    const now = Date.now();
    for (const [key, record] of this.usage.entries()) {
      if (record.resetAt < now) {
        this.usage.delete(key);
      }
    }
  }
}
```

#### ✅ **1.6. Session Store**
- **File:** `src/modules/recipe-search/utils/ai-session.store.ts`
- **Status:** ✅ Đã implement đầy đủ
- **Functionality:**
  - ✅ Lưu công thức tạm thời trong memory
  - ✅ Tự động expire sau 24 giờ (đúng yêu cầu)
  - ✅ Hỗ trợ user ID để lưu theo user
  - ✅ Cleanup tự động

```14:53:d:\DATN\BE\src\modules\recipe-search\utils\ai-session.store.ts
class AiRecipeSessionStore {
  private sessions = new Map<string, AiSessionEntry>();

  save(recipe: AiRecipeResponseDto['recipe'], userId?: string): AiSessionEntry {
    const entry: AiSessionEntry = {
      id: randomUUID(),
      data: recipe,
      userId,
      createdAt: Date.now(),
      expiresAt: Date.now() + TWENTY_FOUR_HOURS_MS,
    };
    this.sessions.set(entry.id, entry);
    this.cleanup();
    return entry;
  }

  get(sessionId: string): AiSessionEntry | undefined {
    this.cleanup();
    const session = this.sessions.get(sessionId);
    if (!session) {
      return undefined;
    }
    if (session.expiresAt < Date.now()) {
      this.sessions.delete(sessionId);
      return undefined;
    }
    return session;
  }

  private cleanup(): void {
    const now = Date.now();
    for (const [id, entry] of this.sessions.entries()) {
      if (entry.expiresAt < now) {
        this.sessions.delete(id);
      }
    }
  }
}
```

#### ✅ **1.7. Validators**
- **File:** `src/modules/recipe-search/validators/recipe-search.validator.ts`
- **Status:** ✅ Đã validate đầy đủ
- **Validation Rules:**
  - ✅ ingredients: array, min 2, max 20 items
  - ✅ options.difficulty: EASY | MEDIUM | HARD
  - ✅ options.servings: 1-10
  - ✅ options.dietaryPreference: VEGAN | VEGETARIAN | LOW_CARB | GLUTEN_FREE | NONE
  - ✅ options.cuisine: max 120 chars
  - ✅ options.notes: max 500 chars

```71:83:d:\DATN\BE\src\modules\recipe-search\validators\recipe-search.validator.ts
export const aiGenerateRecipeValidator = Joi.object({
  ingredients: Joi.array().items(ingredientNameSchema).min(2).max(20).required(),
  options: Joi.object({
    difficulty: Joi.string().valid(...difficultyEnum).optional(),
    servings: Joi.number().integer().min(1).max(10).optional(),
    dietaryPreference: Joi.string()
      .valid('VEGAN', 'VEGETARIAN', 'LOW_CARB', 'GLUTEN_FREE', 'NONE')
      .optional(),
    cuisine: Joi.string().trim().max(120).optional(),
    notes: Joi.string().trim().max(500).optional(),
  }).optional(),
  sessionRef: Joi.string().uuid().optional(),
});
```

#### ✅ **1.8. DTOs**
- **Request DTO:** `src/modules/recipe-search/dto/request/ai-generate.dto.ts`
- **Response DTO:** `src/modules/recipe-search/dto/response/ai-recipe-response.dto.ts`
- **Status:** ✅ Đã định nghĩa đầy đủ

---

### 2. FRONTEND IMPLEMENTATION

#### ✅ **2.1. Service Layer**
- **File:** `src/services/recipes/recipe-search.service.ts`
- **Status:** ✅ Đã có method `generateAiRecipe`
- **Functionality:**
  - ✅ Gọi API endpoint `/recipes/ai/generate`
  - ✅ Type-safe với TypeScript

```97:107:d:\DATN\fe-web\src\services\recipes\recipe-search.service.ts
  /**
   * UC2.3: Tạo công thức bằng AI
   * POST /recipes/ai/generate
   */
  async generateAiRecipe(params: AiGenerateRecipeParams): Promise<AiRecipeResponse> {
    const response = await apiClient.post<ApiResponse<AiRecipeResponse>>(
      '/recipes/ai/generate',
      params
    );
    return response.data.data;
  },
```

#### ✅ **2.2. Custom Hook**
- **File:** `src/hooks/useAIGenerate.ts`
- **Status:** ✅ Đã implement đầy đủ
- **Functionality:**
  - ✅ State management (generating, error, result)
  - ✅ Error handling
  - ✅ Success/Error callbacks

```24:65:d:\DATN\fe-web\src\hooks\useAIGenerate.ts
export function useAIGenerate(options: UseAIGenerateOptions = {}): UseAIGenerateReturn {
  const { onSuccess, onError } = options;

  const [generating, setGenerating] = useState(false);
  const [error, setError] = useState<string | null>(null);
  const [result, setResult] = useState<AiRecipeResponse | null>(null);

  const generate = useCallback(
    async (params: AiGenerateRecipeParams) => {
      setGenerating(true);
      setError(null);
      setResult(null);

      try {
        const response = await recipeSearchService.generateAiRecipe(params);
        setResult(response);
        onSuccess?.(response);
      } catch (err) {
        const errorMessage = err instanceof Error ? err.message : 'Không thể tạo công thức. Vui lòng thử lại.';
        setError(errorMessage);
        onError?.(err instanceof Error ? err : new Error(errorMessage));
      } finally {
        setGenerating(false);
      }
    },
    [onSuccess, onError]
  );

  const reset = useCallback(() => {
    setGenerating(false);
    setError(null);
    setResult(null);
  }, []);

  return {
    generating,
    error,
    result,
    generate,
    reset,
  };
}
```

#### ✅ **2.3. Types**
- **File:** `src/types/recipe-search.types.ts`
- **Status:** ✅ Đã định nghĩa đầy đủ
- **Types:**
  - ✅ `AiGenerateRecipeParams`
  - ✅ `AiGenerateRecipeOptions`
  - ✅ `AiRecipeResponse`
  - ✅ `AiRecipeData`
  - ✅ `AiRecipeIngredient`
  - ✅ `AiRecipeStep`

#### ⚠️ **2.4. UI Components**
- **File:** `app/(main)/search/page.tsx`
- **Status:** ⚠️ **CHƯA HOÀN THIỆN**

**Đã có:**
- ✅ Logic để hiển thị AI suggestion khi không có kết quả
- ✅ Button "Tạo công thức bằng AI"
- ✅ UI hiển thị suggestion card

**Chưa có:**
- ❌ Handler `handleAIGenerateClick` chỉ có TODO comment
- ❌ Modal/Dialog để nhập options (difficulty, servings, dietary preference, etc.)
- ❌ Loading state khi AI đang generate
- ❌ UI hiển thị kết quả công thức AI
- ❌ Các action buttons: "Lưu vào Yêu thích", "Chia sẻ", "Báo cáo", "Tạo công thức khác"

```289:296:d:\DATN\fe-web\app\(main)\search\page.tsx
  // Check if should show AI suggestion (UC2.3)
  const shouldShowAISuggestion = !loading && recipes.length === 0 && sessionId && activeTab === 'ingredients';

  // Handle AI generate click
  const handleAIGenerateClick = () => {
    // TODO: Open AI modal
    console.log('Open AI generate modal with ingredients:', ingredients);
  };
```

```554:576:d:\DATN\fe-web\app\(main)\search\page.tsx
        {/* AI Suggestion (UC2.3) */}
        {shouldShowAISuggestion && (
          <div className="bg-gradient-to-r from-purple-50 to-pink-50 border border-purple-200 rounded-xl p-6 mb-6">
            <div className="flex items-start gap-4">
              <div className="flex-shrink-0">
                <FiZap className="w-6 h-6 text-purple-600" />
              </div>
              <div className="flex-1">
                <h3 className="text-lg font-semibold text-gray-900 mb-2">
                  Không tìm thấy công thức phù hợp
                </h3>
                <p className="text-gray-600 mb-4">
                  Sử dụng AI để tạo công thức mới từ những nguyên liệu bạn có
                </p>
                <Button
                  onClick={handleAIGenerateClick}
                  className="bg-purple-600 hover:bg-purple-700 text-white"
                >
                  Tạo công thức bằng AI
                </Button>
              </div>
            </div>
          </div>
        )}
```

---

### 3. BUSINESS RULES COMPLIANCE

#### ✅ **3.1. AI chỉ kích hoạt khi không có kết quả**
- **Status:** ✅ Đã implement
- **Logic:** `shouldShowAISuggestion` chỉ true khi `recipes.length === 0`

#### ✅ **3.2. Giới hạn 5 lần/ngày**
- **Status:** ✅ Đã implement
- **Implementation:** `AiUsageLimiter` với `DAILY_LIMIT = 5`

#### ✅ **3.3. Prompt chuẩn hóa**
- **Status:** ⚠️ Chưa có (cần implement trong AI service)

#### ✅ **3.4. Công thức không lưu vào DB chính**
- **Status:** ✅ Đã implement
- **Implementation:** Chỉ lưu trong session store (memory)

#### ✅ **3.5. Session 24 giờ**
- **Status:** ✅ Đã implement
- **Implementation:** `TWENTY_FOUR_HOURS_MS` trong session store

#### ✅ **3.6. Validate nguyên liệu tối thiểu**
- **Status:** ✅ Đã implement
- **Validation:** Min 2 nguyên liệu (backend validator + service check)

---

### 4. EXCEPTION HANDLING

#### ✅ **4.1. API AI bị lỗi/quá tải**
- **Status:** ✅ Đã có
- **Implementation:** `ServiceUnavailableError` trong service

#### ✅ **4.2. Kết quả không đúng định dạng**
- **Status:** ⚠️ Chưa có validation chi tiết
- **Cần:** Parse và validate JSON response từ AI

#### ✅ **4.3. Mất kết nối mạng**
- **Status:** ✅ Đã có (thông qua error handling của API client)

#### ✅ **4.4. Nguyên liệu quá ít**
- **Status:** ✅ Đã validate (min 2 nguyên liệu)

#### ✅ **4.5. Rate limit**
- **Status:** ✅ Đã có (`TooManyRequestsError`)

---

### 5. NON-FUNCTIONAL REQUIREMENTS

#### ⚠️ **5.1. Performance: < 15 giây**
- **Status:** ⚠️ Chưa đo được (cần test với AI thực)
- **Note:** Phụ thuộc vào AI service response time

#### ⚠️ **5.2. Usability: Loading state rõ ràng**
- **Status:** ⚠️ Chưa có UI loading
- **Cần:** Spinner/loading indicator khi AI đang generate

#### ✅ **5.3. Security: API key không expose**
- **Status:** ✅ Đã đảm bảo
- **Implementation:** API key chỉ ở backend env, không gửi ra client

#### ⚠️ **5.4. Reliability: Fallback khi AI không khả dụng**
- **Status:** ⚠️ Chưa có fallback mechanism
- **Cần:** Có thể cache một số công thức mẫu hoặc gợi ý công thức tương tự

---

## ĐÁNH GIÁ TỔNG QUAN

### ✅ **ĐÃ HOÀN THÀNH:**

1. **Backend Infrastructure (70%)**
   - ✅ API endpoint đầy đủ
   - ✅ Controller, Service, Repository pattern
   - ✅ Validation và error handling
   - ✅ Usage limiter và session store
   - ✅ DTOs và types

2. **Business Rules (80%)**
   - ✅ Giới hạn 5 lần/ngày
   - ✅ Session 24 giờ
   - ✅ Validate nguyên liệu
   - ✅ Không lưu vào DB chính

3. **Frontend Foundation (30%)**
   - ✅ Service layer
   - ✅ Custom hook
   - ✅ Types
   - ✅ UI suggestion card

### ❌ **CHƯA HOÀN THÀNH:**

1. **AI Integration (0%)**
   - ❌ Chưa tích hợp với OpenAI hoặc AI service thực
   - ❌ Chưa có prompt engineering
   - ❌ Chưa parse/validate JSON response từ AI

2. **Frontend UI (30%)**
   - ❌ Chưa có modal để nhập options
   - ❌ Chưa có loading state
   - ❌ Chưa có UI hiển thị kết quả
   - ❌ Chưa có action buttons (Save, Share, Report, Regenerate)

3. **Alternative Flows**
   - ❌ Chưa có "Tạo từ Tủ lạnh ảo"
   - ❌ Chưa có "Tạo với yêu cầu đặc biệt"
   - ❌ Chưa có "Tạo nhiều công thức"

4. **Exception Handling UI**
   - ❌ Chưa có UI cho các exception flows
   - ❌ Chưa có retry mechanism

---

## KHUYẾN NGHỊ

### **Ưu tiên cao:**

1. **Tích hợp AI Service (CRITICAL)**
   - Implement OpenAI API integration
   - Tạo prompt template chuẩn hóa
   - Parse và validate JSON response
   - Xử lý timeout và retry

2. **Hoàn thiện Frontend UI (HIGH)**
   - Tạo AI Generate Modal component
   - Implement loading states
   - Tạo AI Recipe Display component
   - Thêm action buttons (Save, Share, Report, Regenerate)

3. **Exception Handling (MEDIUM)**
   - UI cho các error cases
   - Retry mechanism
   - Fallback suggestions

4. **Alternative Flows (LOW)**
   - Tích hợp với Pantry (UC5)
   - Advanced options UI

---

## KẾT LUẬN

Hệ thống hiện tại **CÓ NỀN TẢNG TỐT** nhưng **CHƯA SẴN SÀNG** để sử dụng trong production. Cần hoàn thiện:

1. ✅ **Backend:** Tích hợp AI service thực (CRITICAL)
2. ✅ **Frontend:** Hoàn thiện UI flow (HIGH)
3. ✅ **Testing:** Test với AI thực và các edge cases

**Ước tính thời gian hoàn thiện:** 2-3 ngày làm việc

---

## PHỤ LỤC: Các File Liên Quan

### Backend:
- `src/modules/recipe-search/routes.ts` - Route definition
- `src/modules/recipe-search/controllers/recipe-search.controller.ts` - Controller
- `src/modules/recipe-search/services/ai-recipe.service.ts` - Service logic
- `src/infrastructure/ai/ai.service.ts` - AI service (cần implement)
- `src/modules/recipe-search/utils/ai-usage-limiter.ts` - Usage limiter
- `src/modules/recipe-search/utils/ai-session.store.ts` - Session store
- `src/modules/recipe-search/validators/recipe-search.validator.ts` - Validators
- `src/modules/recipe-search/dto/request/ai-generate.dto.ts` - Request DTO
- `src/modules/recipe-search/dto/response/ai-recipe-response.dto.ts` - Response DTO

### Frontend:
- `src/services/recipes/recipe-search.service.ts` - API service
- `src/hooks/useAIGenerate.ts` - Custom hook
- `src/types/recipe-search.types.ts` - TypeScript types
- `app/(main)/search/page.tsx` - Search page (cần hoàn thiện UI)

