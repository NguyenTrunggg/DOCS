# UCS02-3: Migration từ OpenAI sang Gemini AI

**Ngày migration:** $(date)  
**Status:** ✅ Completed

---

## 📋 TỔNG QUAN

Đã chuyển đổi AI service từ OpenAI sang Google Gemini AI.

---

## ✅ CÁC THAY ĐỔI

### 1. Package Dependencies
- ❌ Removed: `openai`
- ✅ Added: `@google/generative-ai`

### 2. Environment Variables
**File:** `BE/src/config/env.ts`

**Thay đổi:**
- ❌ `AI_API_KEY` → ✅ `GEMINI_API_KEY`
- ❌ `AI_API_URL` (không cần nữa)
- ✅ `AI_MODEL` default: `'gpt-4'` → `'gemini-pro'`
- ✅ `AI_MAX_TOKENS` (giữ nguyên)
- ✅ `AI_TEMPERATURE` (giữ nguyên)

**Cấu trúc mới:**
```typescript
ai: {
  apiKey: string;        // GEMINI_API_KEY
  model: string;         // gemini-pro (default)
  maxTokens: number;    // 2000 (default)
  temperature: number;  // 0.7 (default)
}
```

### 3. AI Service Implementation
**File:** `BE/src/infrastructure/ai/ai.service.ts`

**Thay đổi chính:**
- ❌ `OpenAI` client → ✅ `GoogleGenerativeAI` client
- ❌ `chat.completions.create()` → ✅ `generateContent()`
- ❌ System/User messages → ✅ Combined prompt
- ❌ `response.choices[0].message.content` → ✅ `response.response.text()`
- ✅ Thêm logic để extract JSON từ markdown code blocks (nếu có)

**API Call Pattern:**
```typescript
// Old (OpenAI)
const response = await openai.chat.completions.create({
  model: 'gpt-4',
  messages: [
    { role: 'system', content: systemPrompt },
    { role: 'user', content: userPrompt }
  ]
});

// New (Gemini)
const model = genAI.getGenerativeModel({
  model: 'gemini-pro',
  generationConfig: {
    maxOutputTokens: 2000,
    temperature: 0.7
  }
});
const response = await model.generateContent(fullPrompt);
const content = response.response.text();
```

### 4. Prompt Template
**File:** `BE/src/infrastructure/ai/prompts/recipe-generation.prompt.ts`

**Thay đổi:**
- ✅ Cập nhật instruction để rõ ràng hơn về JSON format
- ✅ Prompt được combine thành một string (không có system/user riêng)
- ✅ Thêm instruction cuối: "Hãy trả về kết quả dưới dạng JSON thuần"

### 5. JSON Parsing
**File:** `BE/src/infrastructure/ai/ai.service.ts`

**Cải thiện:**
- ✅ Tự động remove markdown code blocks nếu có
- ✅ Support cả ````json` và ```` blocks
- ✅ Better error handling cho JSON parsing

---

## 🔧 CẤU HÌNH

### Environment Variables (.env)
```env
# Gemini AI Configuration
GEMINI_API_KEY=your_gemini_api_key_here
AI_MODEL=gemini-pro
AI_MAX_TOKENS=2000
AI_TEMPERATURE=0.7
```

### Available Gemini Models
- `gemini-pro` - Recommended for most use cases
- `gemini-pro-vision` - For multimodal (text + images)
- `gemini-ultra` - Most capable (if available)

---

## 📝 NOTES

### Differences from OpenAI:
1. **No separate system/user messages:** Gemini uses a single prompt string
2. **Response format:** `response.response.text()` instead of `response.choices[0].message.content`
3. **JSON mode:** Gemini doesn't have built-in JSON mode, so we need to parse manually
4. **Timeout:** Implemented via Promise.race() instead of SDK timeout option
5. **Error handling:** Similar retry logic, but error messages may differ

### Benefits of Gemini:
- ✅ Free tier available
- ✅ Good performance
- ✅ Supports Vietnamese well
- ✅ No separate API URL needed

### Considerations:
- ⚠️ JSON parsing: Gemini may wrap JSON in markdown, so we handle that
- ⚠️ Model availability: Some models may have regional restrictions
- ⚠️ Rate limits: Check Gemini API quotas

---

## 🧪 TESTING

### Test Cases:
- [ ] Test với GEMINI_API_KEY thực
- [ ] Test JSON parsing với markdown code blocks
- [ ] Test JSON parsing với JSON thuần
- [ ] Test timeout scenario
- [ ] Test retry logic
- [ ] Test với các model khác nhau (gemini-pro, gemini-ultra)
- [ ] Test performance (< 15 giây)

---

## 📚 TÀI LIỆU THAM KHẢO

- [Google Generative AI SDK](https://ai.google.dev/docs)
- [Gemini API Documentation](https://ai.google.dev/api)
- [Gemini Models](https://ai.google.dev/models/gemini)

---

**Migration Status:** ✅ **COMPLETE**  
**Ready for:** Testing với Gemini API key thực

---

**Last Updated:** $(date)

