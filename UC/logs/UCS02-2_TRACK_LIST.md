# Track List: Xây dựng UCS02-2 - Tìm kiếm theo Tên món ăn

**Use Case:** UCS02-2: Tìm kiếm theo Tên món ăn [MEDIUM PRIORITY]  
**Mức độ đáp ứng hiện tại:** 57% (17/30 yêu cầu)  
**Mục tiêu:** Đạt 100% đáp ứng Use Case

---

## 📋 Tổng quan Track List

| Ưu tiên | Số lượng | Trạng thái |
|---------|----------|------------|
| 🔴 Critical | 4 | **4/4** ✅ |
| 🟡 Important | 6 | 3/6 |
| 🟢 Nice to have | 4 | 0/4 |
| **UI Improvements** | **8** | **5/8** ✅ |
| **TỔNG CỘNG** | **22** | **12/22** |

---

## 🔴 PHASE 1: CRITICAL PRIORITY (Ưu tiên cao)

### Task 1: Implement Autocomplete API Endpoint
**Mục tiêu:** Tạo API endpoint để lấy suggestions real-time khi user gõ (phản hồi < 300ms)

**Backend:**
- [ ] Tạo DTO: `BE/src/modules/recipe-search/dto/request/autocomplete.dto.ts`
  - `keyword: string` (min 1, max 120)
  - `limit?: number` (default 10, max 20)
- [ ] Tạo validator: Thêm vào `recipe-search.validator.ts`
  - `autocompleteValidator` với keyword validation
- [ ] Tạo repository method: `BE/src/modules/recipe-search/repositories/recipe-search.repository.ts`
  - `findRecipeTitlesForAutocomplete(keyword: string, limit: number): Promise<string[]>`
  - Query: SELECT DISTINCT title FROM recipes WHERE status = 'APPROVED' AND title LIKE '%keyword%' LIMIT limit
  - Sử dụng normalized text cho search
- [ ] Tạo service method: `BE/src/modules/recipe-search/services/autocomplete.service.ts`
  - `getAutocompleteSuggestions(keyword: string, limit?: number): Promise<string[]>`
  - Cache suggestions trong 5 phút
  - Đảm bảo response time < 300ms
- [ ] Tạo controller method: `BE/src/modules/recipe-search/controllers/recipe-search.controller.ts`
  - `GET /api/v1/recipes/search/autocomplete?keyword=xxx&limit=10`
- [ ] Thêm route: `BE/src/modules/recipe-search/routes.ts`
  - GET `/autocomplete` với validator

**Frontend:**
- [ ] Tạo service method: `fe-web/src/services/recipes/recipe-search.service.ts`
  - `getAutocompleteSuggestions(keyword: string): Promise<string[]>`
- [ ] Update SearchBar component: `fe-web/src/components/recipes/SearchBar.tsx`
  - Thêm debounce 300ms cho autocomplete API call
  - Gọi API khi user gõ (min 1 ký tự)
  - Hiển thị loading state
  - Update suggestions từ API response
- [ ] Tạo hook: `fe-web/src/hooks/useAutocomplete.ts` (optional)
  - Quản lý autocomplete state và API calls

**Testing:**
- [ ] Unit test: Autocomplete service
- [ ] Integration test: API endpoint
- [ ] E2E test: Autocomplete flow trong SearchBar
- [ ] Performance test: Đảm bảo < 300ms response time

**Files cần tạo/sửa:**
- `BE/src/modules/recipe-search/dto/request/autocomplete.dto.ts` (NEW)
- `BE/src/modules/recipe-search/services/autocomplete.service.ts` (NEW)
- `BE/src/modules/recipe-search/controllers/recipe-search.controller.ts` (MODIFY)
- `BE/src/modules/recipe-search/routes.ts` (MODIFY)
- `BE/src/modules/recipe-search/validators/recipe-search.validator.ts` (MODIFY)
- `BE/src/modules/recipe-search/repositories/recipe-search.repository.ts` (MODIFY)
- `fe-web/src/services/recipes/recipe-search.service.ts` (MODIFY)
- `fe-web/src/components/recipes/SearchBar.tsx` (MODIFY)

---

### Task 2: Implement Fuzzy Search với Levenshtein Distance
**Mục tiêu:** Thêm thuật toán fuzzy search để tìm kiếm liên quan (score 0.6)

**Backend:**
- [ ] Install package: `npm install fast-levenshtein` hoặc `npm install levenshtein`
- [ ] Tạo utility: `BE/src/common/utils/fuzzy-search.ts`
  - `calculateLevenshteinDistance(str1: string, str2: string): number`
  - `calculateSimilarity(str1: string, str2: string): number` (0-1)
  - `fuzzyMatch(text: string, keyword: string, threshold: number = 0.6): boolean`
- [ ] Update service: `BE/src/modules/recipe-search/services/name-search.service.ts`
  - Update `calculateMatchScore()`:
    - Exact match: score = 1.0
    - Partial match (contains): score = 0.85
    - Fuzzy match (similarity >= 0.6): score = 0.6
    - Related (similarity >= 0.4): score = 0.4
  - Update `findRecipesByName()` trong repository để lấy thêm recipes cho fuzzy search
  - Apply fuzzy matching sau khi lấy từ DB
- [ ] Tối ưu performance:
  - Chỉ apply fuzzy search khi không có exact/partial match
  - Giới hạn số lượng recipes để fuzzy match (max 200)
  - Cache fuzzy results

**Testing:**
- [ ] Unit test: Fuzzy search utility functions
- [ ] Integration test: Fuzzy search trong name-search service
- [ ] Test cases:
  - "phở" → tìm "pho", "phở bò", "phở gà"
  - "thịt kho" → tìm "thịt kho tàu", "thịt kho tiêu"
  - "bánh mì" → tìm "banh mi", "bánh mỳ"

**Files cần tạo/sửa:**
- `BE/src/common/utils/fuzzy-search.ts` (NEW)
- `BE/src/modules/recipe-search/services/name-search.service.ts` (MODIFY)
- `BE/src/modules/recipe-search/repositories/recipe-search.repository.ts` (MODIFY - optional)

---

### Task 3: Add XSS Sanitization
**Mục tiêu:** Sanitize keyword input để tránh XSS attacks

**Backend:**
- [ ] Install package: `npm install dompurify` hoặc `npm install xss`
- [ ] Tạo utility: `BE/src/common/utils/sanitize.ts`
  - `sanitizeInput(input: string): string`
  - Escape HTML entities: `<`, `>`, `&`, `"`, `'`
  - Remove script tags và dangerous patterns
- [ ] Update validator: `BE/src/modules/recipe-search/validators/recipe-search.validator.ts`
  - Sanitize keyword trước khi validate
- [ ] Update service: `BE/src/modules/recipe-search/services/name-search.service.ts`
  - Sanitize keyword trong `searchByName()` method
- [ ] Update controller: `BE/src/modules/recipe-search/controllers/recipe-search.controller.ts`
  - Sanitize input từ request body

**Frontend:**
- [ ] Update SearchBar: `fe-web/src/components/recipes/SearchBar.tsx`
  - Sanitize input trước khi gửi API
  - Hoặc để backend xử lý (recommended)

**Testing:**
- [ ] Unit test: Sanitize utility với các XSS patterns
- [ ] Security test: Test với các XSS payloads
- [ ] Test cases:
  - `<script>alert('xss')</script>` → sanitized
  - `"onclick="alert('xss')"` → sanitized
  - `javascript:alert('xss')` → sanitized

**Files cần tạo/sửa:**
- `BE/src/common/utils/sanitize.ts` (NEW)
- `BE/src/modules/recipe-search/validators/recipe-search.validator.ts` (MODIFY)
- `BE/src/modules/recipe-search/services/name-search.service.ts` (MODIFY)
- `BE/src/modules/recipe-search/controllers/recipe-search.controller.ts` (MODIFY)

---

### Task 4: Implement Search History API
**Mục tiêu:** Tạo API để lấy 50 từ khóa gần nhất của user

**Backend:**
- [ ] Tạo DTO: `BE/src/modules/recipe-search/dto/response/search-history-response.dto.ts`
  - `queries: string[]` (max 50)
  - `total: number`
- [ ] Tạo repository method: `BE/src/modules/recipe-search/repositories/recipe-search.repository.ts`
  - `getSearchHistory(userId?: string, limit: number = 50): Promise<string[]>`
  - Query: SELECT DISTINCT query FROM search_history WHERE userId = ? ORDER BY createdAt DESC LIMIT 50
  - Nếu không có userId, lấy từ session hoặc không trả về
- [ ] Tạo service method: `BE/src/modules/recipe-search/services/search-history.service.ts`
  - `getSearchHistory(userId?: string): Promise<string[]>`
  - Cache trong 5 phút
- [ ] Tạo controller method: `BE/src/modules/recipe-search/controllers/recipe-search.controller.ts`
  - `GET /api/v1/recipes/search/history`
  - Yêu cầu authentication (optional - có thể lấy từ session)
- [ ] Thêm route: `BE/src/modules/recipe-search/routes.ts`
  - GET `/history` với auth middleware (optional)

**Frontend:**
- [ ] Tạo service method: `fe-web/src/services/recipes/recipe-search.service.ts`
  - `getSearchHistory(): Promise<string[]>`
- [ ] Update SearchBar: `fe-web/src/components/recipes/SearchBar.tsx`
  - Gọi API khi component mount hoặc khi focus vào input
  - Hiển thị history trong dropdown
  - Thêm icon clock cho history items
- [ ] Update hook: `fe-web/src/hooks/useRecipeSearch.ts` (optional)
  - Cache search history

**Testing:**
- [ ] Unit test: Search history service
- [ ] Integration test: API endpoint
- [ ] Test với user đã đăng nhập và chưa đăng nhập
- [ ] Test giới hạn 50 items

**Files cần tạo/sửa:**
- `BE/src/modules/recipe-search/dto/response/search-history-response.dto.ts` (NEW)
- `BE/src/modules/recipe-search/services/search-history.service.ts` (NEW)
- `BE/src/modules/recipe-search/repositories/recipe-search.repository.ts` (MODIFY)
- `BE/src/modules/recipe-search/controllers/recipe-search.controller.ts` (MODIFY)
- `BE/src/modules/recipe-search/routes.ts` (MODIFY)
- `fe-web/src/services/recipes/recipe-search.service.ts` (MODIFY)
- `fe-web/src/components/recipes/SearchBar.tsx` (MODIFY)

---

## 🟡 PHASE 2: IMPORTANT PRIORITY (Ưu tiên trung bình)

### Task 5: Implement Popular Searches
**Mục tiêu:** Hiển thị danh sách tìm kiếm phổ biến dựa trên searchHistory

**Backend:**
- [ ] Tạo repository method: `BE/src/modules/recipe-search/repositories/recipe-search.repository.ts`
  - `getPopularSearches(limit: number = 10): Promise<Array<{query: string, count: number}>>`
  - Query: SELECT query, COUNT(*) as count FROM search_history WHERE createdAt >= DATE_SUB(NOW(), INTERVAL 30 DAY) GROUP BY query ORDER BY count DESC LIMIT 10
- [ ] Tạo service method: `BE/src/modules/recipe-search/services/popular-searches.service.ts`
  - `getPopularSearches(limit?: number): Promise<string[]>`
  - Cache trong 1 giờ
- [ ] Tạo controller method: `BE/src/modules/recipe-search/controllers/recipe-search.controller.ts`
  - `GET /api/v1/recipes/search/popular?limit=10`
- [ ] Thêm route: `BE/src/modules/recipe-search/routes.ts`

**Frontend:**
- [ ] Tạo component: `fe-web/src/components/recipes/PopularSearches.tsx`
  - Hiển thị danh sách popular searches
  - Click vào item → trigger search
- [ ] Tạo service method: `fe-web/src/services/recipes/recipe-search.service.ts`
  - `getPopularSearches(): Promise<string[]>`
- [ ] Update search page: `fe-web/app/(main)/search/page.tsx`
  - Hiển thị PopularSearches component khi chưa có kết quả
  - Hiển thị trong EmptyState khi không tìm thấy kết quả

**Testing:**
- [ ] Unit test: Popular searches service
- [ ] Integration test: API endpoint
- [ ] Test với database có nhiều search history

**Files cần tạo/sửa:**
- `BE/src/modules/recipe-search/services/popular-searches.service.ts` (NEW)
- `BE/src/modules/recipe-search/repositories/recipe-search.repository.ts` (MODIFY)
- `BE/src/modules/recipe-search/controllers/recipe-search.controller.ts` (MODIFY)
- `BE/src/modules/recipe-search/routes.ts` (MODIFY)
- `fe-web/src/components/recipes/PopularSearches.tsx` (NEW)
- `fe-web/src/services/recipes/recipe-search.service.ts` (MODIFY)
- `fe-web/app/(main)/search/page.tsx` (MODIFY)
- `fe-web/src/components/recipes/EmptyState.tsx` (MODIFY)

---

### Task 6: Add Timeout Handling
**Mục tiêu:** Xử lý timeout 5 giây cho search API

**Backend:**
- [ ] Update service: `BE/src/modules/recipe-search/services/name-search.service.ts`
  - Thêm timeout check trong `searchByName()`
  - Throw error nếu > 5 giây
- [ ] Tạo custom error: `BE/src/common/errors/search-timeout.error.ts`
  - `SearchTimeoutError` extends Error

**Frontend:**
- [ ] Update service: `fe-web/src/services/recipes/recipe-search.service.ts`
  - Thêm timeout 5 giây cho axios request
  - `axios.create({ timeout: 5000 })`
- [ ] Update hook: `fe-web/src/hooks/useRecipeSearch.ts`
  - Handle timeout error
  - Hiển thị message: "Tìm kiếm đang mất quá nhiều thời gian. Vui lòng thử lại."
  - Thêm nút "Thử lại" và "Hủy"
- [ ] Update EmptyState: `fe-web/src/components/recipes/EmptyState.tsx`
  - Thêm type 'timeout' với message phù hợp

**Testing:**
- [ ] Unit test: Timeout handling
- [ ] Integration test: Simulate slow query (> 5s)
- [ ] E2E test: Timeout flow

**Files cần tạo/sửa:**
- `BE/src/common/errors/search-timeout.error.ts` (NEW)
- `BE/src/modules/recipe-search/services/name-search.service.ts` (MODIFY)
- `fe-web/src/services/recipes/recipe-search.service.ts` (MODIFY)
- `fe-web/src/hooks/useRecipeSearch.ts` (MODIFY)
- `fe-web/src/components/recipes/EmptyState.tsx` (MODIFY)

---

### Task 7: Improve Error Messages
**Mục tiêu:** Cải thiện error messages theo UC

**Frontend:**
- [ ] Update EmptyState: `fe-web/src/components/recipes/EmptyState.tsx`
  - Thêm prop `keyword?: string`
  - Message: "Không tìm thấy kết quả cho '[từ khóa]'"
  - Thêm gợi ý: "Bạn có thể thử với từ khóa khác hoặc kiểm tra chính tả"
  - Hiển thị PopularSearches component
- [ ] Update search page: `fe-web/app/(main)/search/page.tsx`
  - Pass keyword vào EmptyState
  - Hiển thị PopularSearches khi không có kết quả
- [ ] Update hook: `fe-web/src/hooks/useRecipeSearch.ts`
  - Thêm nút "Thử lại" rõ ràng trong error state

**Testing:**
- [ ] Test error messages với các scenarios khác nhau
- [ ] Test UI với empty results

**Files cần tạo/sửa:**
- `fe-web/src/components/recipes/EmptyState.tsx` (MODIFY)
- `fe-web/app/(main)/search/page.tsx` (MODIFY)
- `fe-web/src/hooks/useRecipeSearch.ts` (MODIFY)

---

### Task 8: Handle Special Characters
**Mục tiêu:** Tự động loại bỏ ký tự đặc biệt và thông báo cho user

**Backend:**
- [ ] Tạo utility: `BE/src/common/utils/sanitize-keyword.ts`
  - `sanitizeKeyword(keyword: string): { cleaned: string, removed: string[] }`
  - Loại bỏ ký tự đặc biệt: `!@#$%^&*()_+={}[]|\\:";'<>?,./`
  - Giữ lại: chữ cái, số, dấu cách, dấu tiếng Việt
- [ ] Update service: `BE/src/modules/recipe-search/services/name-search.service.ts`
  - Sanitize keyword trước khi search
  - Return thông tin về ký tự đã loại bỏ (nếu có)

**Frontend:**
- [ ] Update SearchBar: `fe-web/src/components/recipes/SearchBar.tsx`
  - Sanitize input khi user gõ
  - Hiển thị warning nếu có ký tự đặc biệt bị loại bỏ
  - Message: "Đã loại bỏ ký tự đặc biệt: [ký tự]"
- [ ] Update search page: `fe-web/app/(main)/search/page.tsx`
  - Hiển thị notification khi có ký tự bị loại bỏ

**Testing:**
- [ ] Unit test: Sanitize keyword utility
- [ ] Test với các ký tự đặc biệt khác nhau
- [ ] Test UI notification

**Files cần tạo/sửa:**
- `BE/src/common/utils/sanitize-keyword.ts` (NEW)
- `BE/src/modules/recipe-search/services/name-search.service.ts` (MODIFY)
- `fe-web/src/components/recipes/SearchBar.tsx` (MODIFY)
- `fe-web/app/(main)/search/page.tsx` (MODIFY)

---

### Task 9: Improve Match Score Sorting
**Mục tiêu:** Cải thiện thuật toán sắp xếp theo độ liên quan

**Backend:**
- [ ] Update service: `BE/src/modules/recipe-search/services/name-search.service.ts`
  - Cải thiện `calculateMatchScore()`:
    - Exact match: 1.0
    - Starts with keyword: 0.95
    - Contains keyword: 0.85
    - Fuzzy match (similarity >= 0.7): 0.7
    - Fuzzy match (similarity >= 0.6): 0.6
    - Related (similarity >= 0.4): 0.4
  - Sort theo score giảm dần
  - Nếu score bằng nhau, sort theo averageRating, totalFavorites

**Testing:**
- [ ] Unit test: Match score calculation
- [ ] Integration test: Search results sorting
- [ ] Test với các từ khóa khác nhau

**Files cần tạo/sửa:**
- `BE/src/modules/recipe-search/services/name-search.service.ts` (MODIFY)

---

### Task 10: Add Too Many Results Warning
**Mục tiêu:** Hiển thị warning khi > 100 kết quả

**Backend:**
- [ ] Update service: `BE/src/modules/recipe-search/services/name-search.service.ts`
  - Return `hasTooManyResults: boolean` trong meta nếu total > 100

**Frontend:**
- [ ] Update search page: `fe-web/app/(main)/search/page.tsx`
  - Hiển thị warning banner khi `meta.hasTooManyResults === true`
  - Message: "Tìm thấy [số] kết quả. Hãy thử từ khóa cụ thể hơn để thu hẹp kết quả."
- [ ] Update DTO: `fe-web/src/types/recipe-search.types.ts`
  - Thêm `hasTooManyResults?: boolean` vào SearchMeta

**Testing:**
- [ ] Test với search trả về > 100 kết quả
- [ ] Test UI warning banner

**Files cần tạo/sửa:**
- `BE/src/modules/recipe-search/services/name-search.service.ts` (MODIFY)
- `BE/src/modules/recipe-search/dto/response/search-response.dto.ts` (MODIFY)
- `fe-web/src/types/recipe-search.types.ts` (MODIFY)
- `fe-web/app/(main)/search/page.tsx` (MODIFY)

---

## 🟢 PHASE 3: NICE TO HAVE (Ưu tiên thấp)

### Task 11: Implement Voice Search
**Mục tiêu:** Tìm kiếm bằng giọng nói

**Frontend:**
- [ ] Install package: `npm install react-speech-recognition` hoặc sử dụng Web Speech API
- [ ] Tạo hook: `fe-web/src/hooks/useVoiceSearch.ts`
  - Sử dụng Web Speech API hoặc react-speech-recognition
  - Handle start/stop recording
  - Convert speech to text
- [ ] Update SearchBar: `fe-web/src/components/recipes/SearchBar.tsx`
  - Thêm icon micro
  - Click micro → start recording
  - Hiển thị recording state
  - Auto search khi có text từ voice
- [ ] Add permissions handling:
  - Request microphone permission
  - Handle permission denied

**Testing:**
- [ ] Test với microphone permission
- [ ] Test speech-to-text accuracy
- [ ] Test với tiếng Việt

**Files cần tạo/sửa:**
- `fe-web/src/hooks/useVoiceSearch.ts` (NEW)
- `fe-web/src/components/recipes/SearchBar.tsx` (MODIFY)

---

### Task 12: Add Search Statistics
**Mục tiêu:** Dashboard/API để xem thống kê tìm kiếm

**Backend:**
- [ ] Tạo repository method: `BE/src/modules/recipe-search/repositories/recipe-search.repository.ts`
  - `getSearchStatistics(startDate?: Date, endDate?: Date): Promise<SearchStatistics>`
  - Thống kê: total searches, unique keywords, popular keywords, avg result count
- [ ] Tạo service: `BE/src/modules/recipe-search/services/search-statistics.service.ts`
- [ ] Tạo controller: Admin endpoint để xem statistics
- [ ] Tạo DTO: `BE/src/modules/recipe-search/dto/response/search-statistics.dto.ts`

**Frontend (Admin):**
- [ ] Tạo page: `fe-web/app/(admin)/analytics/search-statistics/page.tsx`
- [ ] Hiển thị charts và tables

**Files cần tạo/sửa:**
- `BE/src/modules/recipe-search/services/search-statistics.service.ts` (NEW)
- `BE/src/modules/recipe-search/dto/response/search-statistics.dto.ts` (NEW)
- `BE/src/modules/recipe-search/repositories/recipe-search.repository.ts` (MODIFY)
- `fe-web/app/(admin)/analytics/search-statistics/page.tsx` (NEW)

---

### Task 13: Add Rate Limiting
**Mục tiêu:** Giới hạn số lượng requests để tránh abuse

**Backend:**
- [ ] Install package: `npm install express-rate-limit`
- [ ] Tạo middleware: `BE/src/common/middleware/rateLimiter.ts`
  - Rate limit cho search endpoints:
    - Autocomplete: 30 requests/phút
    - Search by name: 20 requests/phút
    - Search history: 10 requests/phút
- [ ] Apply middleware: `BE/src/modules/recipe-search/routes.ts`
  - Apply rate limiter cho các routes

**Testing:**
- [ ] Test rate limiting với nhiều requests
- [ ] Test error message khi vượt quá limit

**Files cần tạo/sửa:**
- `BE/src/common/middleware/rateLimiter.ts` (NEW)
- `BE/src/modules/recipe-search/routes.ts` (MODIFY)

---

### Task 14: Performance Monitoring
**Mục tiêu:** Monitoring và alerting khi search time > 2s

**Backend:**
- [ ] Tạo middleware: `BE/src/common/middleware/performance-monitor.ts`
  - Log search duration
  - Alert nếu > 2s
- [ ] Tích hợp với logging service (nếu có)
- [ ] Tạo metrics endpoint (optional)

**Files cần tạo/sửa:**
- `BE/src/common/middleware/performance-monitor.ts` (NEW)
- `BE/src/modules/recipe-search/routes.ts` (MODIFY)

---

## 🎨 UI IMPROVEMENTS (Completed)

### ✅ Task UI-1: Improve EmptyState Component
**Status:** ✅ Completed  
**Files:**
- `fe-web/src/components/recipes/EmptyState.tsx` (MODIFIED)
- Thêm prop `keyword`, `children`, type `'timeout'`
- Message cụ thể với từ khóa
- Gợi ý "kiểm tra chính tả"

### ✅ Task UI-2: Create PopularSearches Component
**Status:** ✅ Completed  
**Files:**
- `fe-web/src/components/recipes/PopularSearches.tsx` (NEW)
- Component hiển thị popular searches dạng tags
- Click → auto search

### ✅ Task UI-3: Add Too Many Results Warning
**Status:** ✅ Completed  
**Files:**
- `fe-web/src/components/recipes/WarningBanner.tsx` (NEW)
- `fe-web/app/(main)/search/page.tsx` (MODIFIED)
- `fe-web/src/types/recipe-search.types.ts` (MODIFIED)
- Warning banner khi > 100 kết quả

### ✅ Task UI-4: Improve Error Messages
**Status:** ✅ Completed  
**Files:**
- `fe-web/src/hooks/useRecipeSearch.ts` (MODIFIED)
- `fe-web/app/(main)/search/page.tsx` (MODIFIED)
- Timeout error handling
- Nút "Thử lại" rõ ràng

### ✅ Task UI-5: Add Voice Search Icon
**Status:** ✅ Completed (UI only)  
**Files:**
- `fe-web/src/components/recipes/SearchBar.tsx` (MODIFIED)
- Icon micro added (API integration pending)

### ⏳ Task UI-6: Add Special Characters Notification
**Status:** ⏳ Pending  
**Note:** Cần Toast component hoặc notification system

### ⏳ Task UI-7: Add Search History Icon Integration
**Status:** ⏳ Pending (UI done, API pending)  
**Files:**
- `fe-web/src/components/recipes/SearchBar.tsx` (MODIFIED - icon added)
- **TODO:** Connect với Search History API

### ⏳ Task UI-8: Improve SearchBar Loading State
**Status:** ⏳ Pending  
**Note:** Có thể cải thiện thêm UX

---

## 📊 Tracking Progress

### Checklist tổng thể

**UI Improvements (8 tasks):**
- [x] Task UI-1: Improve EmptyState ✅
- [x] Task UI-2: Create PopularSearches ✅
- [x] Task UI-3: Add Too Many Results Warning ✅
- [x] Task UI-4: Improve Error Messages ✅
- [x] Task UI-5: Add Voice Search Icon ✅
- [ ] Task UI-6: Add Special Characters Notification
- [ ] Task UI-7: Add Search History Icon Integration (UI done, API pending)
- [ ] Task UI-8: Improve SearchBar Loading State

**Phase 1 - Critical (4 tasks):**
- [x] Task 1: Autocomplete API ✅
- [x] Task 2: Fuzzy Search ✅
- [x] Task 3: XSS Sanitization ✅
- [x] Task 4: Search History API ✅

**Phase 2 - Important (6 tasks):**
- [x] Task 5: Popular Searches API ✅
- [ ] Task 6: Timeout Handling (Backend)
- [x] Task 7: Improve Error Messages ✅ (Frontend done)
- [ ] Task 8: Handle Special Characters
- [x] Task 9: Improve Match Score Sorting ✅ (Fuzzy search done)
- [x] Task 10: Too Many Results Warning ✅ (Frontend + Backend done)

**Phase 3 - Nice to Have (4 tasks):**
- [ ] Task 11: Voice Search (API integration)
- [ ] Task 12: Search Statistics
- [ ] Task 13: Rate Limiting
- [ ] Task 14: Performance Monitoring

---

## 🎯 Milestones

### Milestone 1: Core Features (Phase 1)
**Mục tiêu:** Hoàn thành 4 tasks critical priority  
**Thời gian ước tính:** 2-3 tuần  
**Kết quả:** ✅ **Đạt 72% đáp ứng UC** - **HOÀN THÀNH**

### Milestone 2: Enhanced Features (Phase 2)
**Mục tiêu:** Hoàn thành 6 tasks important priority  
**Thời gian ước tính:** 2-3 tuần  
**Kết quả:** Đạt 90% đáp ứng UC

### Milestone 3: Advanced Features (Phase 3)
**Mục tiêu:** Hoàn thành 4 tasks nice to have  
**Thời gian ước tính:** 1-2 tuần  
**Kết quả:** Đạt 100% đáp ứng UC

---

## 📝 Notes

- **Testing:** Mỗi task cần có unit test và integration test
- **Documentation:** Update API documentation sau mỗi task
- **Code Review:** Review code trước khi merge
- **Performance:** Đảm bảo không làm giảm performance hiện tại
- **Backward Compatibility:** Đảm bảo không break existing features

---

**Last Updated:** $(date)

