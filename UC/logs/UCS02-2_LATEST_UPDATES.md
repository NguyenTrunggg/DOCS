# Latest Updates: UCS02-2 - Additional Tasks Completed

**Ngày cập nhật:** $(date)  
**Status:** ✅ Timeout Handling & Special Characters Notification hoàn thành

---

## ✅ Task 6: Timeout Handling

### Backend

#### 1. `BE/src/common/errors/search-timeout.error.ts` (NEW)
**Status:** ✅ Created  
**Features:**
- Custom error class `SearchTimeoutError`
- Status code 408 (Request Timeout)
- Includes duration information
- Proper error message

#### 2. `BE/src/modules/recipe-search/services/name-search.service.ts` (MODIFIED)
**Changes:**
- ✅ Import `SearchTimeoutError`
- ✅ Add `SEARCH_TIMEOUT_MS = 5000` constant
- ✅ Check timeout before database query
- ✅ Check timeout after database query
- ✅ Throw `SearchTimeoutError` if exceeds 5 seconds

**Code:**
```typescript
const SEARCH_TIMEOUT_MS = 5000; // 5 seconds - UC2.2 requirement

// Check timeout before database query
const elapsed = Date.now() - startedAt;
if (elapsed > SEARCH_TIMEOUT_MS) {
  throw new SearchTimeoutError(elapsed);
}

// Check timeout after database query
const totalElapsed = Date.now() - startedAt;
if (totalElapsed > SEARCH_TIMEOUT_MS) {
  throw new SearchTimeoutError(totalElapsed);
}
```

#### 3. `BE/src/common/middleware/errorHandler.ts` (MODIFIED)
**Changes:**
- ✅ Import `SearchTimeoutError`
- ✅ Handle `SearchTimeoutError` với status 408
- ✅ Return error với duration information

### Frontend

#### 1. `fe-web/src/services/api/client.ts` (MODIFIED)
**Changes:**
- ✅ Add `searchApiClient` với timeout 5 giây
- ✅ Default `apiClient` timeout 10 giây

**Code:**
```typescript
export const searchApiClient: AxiosInstance = axios.create({
  baseURL: `${API_BASE_URL}/api/${API_VERSION}`,
  headers: {
    'Content-Type': 'application/json',
  },
  withCredentials: true,
  timeout: 5000, // 5 seconds - UC2.2 requirement
});
```

#### 2. `fe-web/src/services/recipes/recipe-search.service.ts` (MODIFIED)
**Changes:**
- ✅ Use `searchApiClient` cho `searchByName()` method
- ✅ Timeout 5 giây cho search requests

#### 3. `fe-web/src/hooks/useRecipeSearch.ts` (MODIFIED)
**Changes:**
- ✅ Improved timeout error detection
- ✅ Check for status 408
- ✅ Check for error code 'SEARCH_TIMEOUT'
- ✅ Display duration in error message

---

## ✅ Task 8: Handle Special Characters

### Backend

#### 1. `BE/src/common/utils/sanitize-keyword.ts` (NEW)
**Status:** ✅ Created  
**Features:**
- `sanitizeKeywordWithInfo()` function
- Returns cleaned keyword và removed characters
- Detect special characters: `!@#$%^&*()_+={}[]|\\:";'<>?,./`

### Frontend

#### 1. `fe-web/src/components/common/Notification.tsx` (NEW)
**Status:** ✅ Created  
**Features:**
- Toast-like notification component
- Types: info, warning, success, error
- Auto dismiss với duration
- Manual dismiss
- Icons cho mỗi type

**Props:**
```typescript
interface NotificationProps {
  message: string;
  type?: NotificationType;
  duration?: number;
  onDismiss?: () => void;
  className?: string;
}
```

#### 2. `fe-web/app/(main)/search/page.tsx` (MODIFIED)
**Changes:**
- ✅ Import `Notification` component
- ✅ Add `specialCharsNotification` state
- ✅ Detect special characters trong `handleSearchByName()`
- ✅ Show notification khi có special characters
- ✅ Auto-clean keyword và search với cleaned keyword
- ✅ Detect special characters trong auto-search effect

**Logic:**
```typescript
// Check for special characters
const specialCharsRegex = /[!@#$%^&*()_+={}\[\]|\\:";'<>?,./]/g;
const specialChars = trimmedKeyword.match(specialCharsRegex);

if (specialChars && specialChars.length > 0) {
  const uniqueChars = [...new Set(specialChars)];
  const cleanedKeyword = trimmedKeyword.replace(specialCharsRegex, '').trim();
  
  // Show notification
  setSpecialCharsNotification({
    message: `Đã loại bỏ ký tự đặc biệt: ${uniqueChars.join(', ')}`,
    removed: uniqueChars,
  });

  // Use cleaned keyword if still valid
  if (cleanedKeyword.length >= 3) {
    // Search with cleaned keyword
  }
}
```

---

## 📊 Summary

### New Files (3)
1. `BE/src/common/errors/search-timeout.error.ts`
2. `BE/src/common/utils/sanitize-keyword.ts`
3. `fe-web/src/components/common/Notification.tsx`

### Modified Files (5)
1. `BE/src/modules/recipe-search/services/name-search.service.ts`
2. `BE/src/common/middleware/errorHandler.ts`
3. `BE/src/common/errors/index.ts`
4. `fe-web/src/services/api/client.ts`
5. `fe-web/src/services/recipes/recipe-search.service.ts`
6. `fe-web/src/hooks/useRecipeSearch.ts`
7. `fe-web/app/(main)/search/page.tsx`

---

## ✅ Testing Checklist

### Timeout Handling
- [ ] Test với query chậm (> 5s) → throw SearchTimeoutError
- [ ] Test error handler trả về status 408
- [ ] Test frontend hiển thị timeout message
- [ ] Test frontend có nút "Thử lại"

### Special Characters
- [ ] Test với keyword có special characters
- [ ] Test notification hiển thị đúng
- [ ] Test cleaned keyword được sử dụng
- [ ] Test auto-dismiss sau 5 giây
- [ ] Test manual dismiss

---

**Status:** ✅ Timeout Handling & Special Characters Complete

