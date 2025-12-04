# Tracklist · UCA01-3 “Vô hiệu hóa tài khoản người dùng”

> **Scope**: Admin flow to lock/disable user accounts, covering single-user action from list/detail, preset/custom durations, reason capture, session invalidation, audit logging, batch lock, email notification. Aligns with UC-A1-1/2 triggers and future unlock (UC-A1-4).

> **Architecture**: Maintain Layered API (`DTO → Validator → Repository → Service → Controller → Routes`) & UI separation (`Validator → Service gateway → Hook → Component → View`). Must respect SOLID, guard rails, existing admin namespace conventions.

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Analysis
- **Basic Flow**: open lock dialog → input reason + duration → confirm → BE updates status, logs action, invalidates sessions → FE shows success.
- **Alternative**: preset durations (15m, 1h, 24h, permanent + custom), batch lock multiple users, optional email notification toggle.
- **Exception**:
  - Already locked user → conflict message.
  - Missing permission (`User.Disable`) → 403 & FE error.
  - DB failure → error toast with retry.
- **Rules/NFR**:
  - Always record reason & performedBy.
  - Auto unlock on expiry (cron).
  - Session invalidation must be immediate.
  - Audit entries for each lock.
  - UI confirm modal states cause & effect.

---

## 2. Backend Tasks

### 2.1 DTO & Validators
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A3-BE-DTO-01 | Ensure `DisableUserRequestDto` supports preset/custom duration, severity, note, sendEmail flag | `src/modules/admin/users/dto/request/disable-user.dto.ts` | ✅ | Added `sendEmail?: boolean` |
| A3-BE-DTO-02 | Update `BatchLockRequestDto` to include `sendEmail` | same | ✅ | Payload now includes optional flag |
| A3-BE-VLD-01 | Extend `disableUserValidator` / `batchLockValidator` with new fields | `src/modules/admin/users/validators/admin-users.validator.ts` | ✅ | Added `sendEmail` default true |

### 2.2 Repository Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A3-BE-REP-01 | `AdminUsersRepository.lockUser` ensure `lockSeverity`, `lockExpiresAt`, `lockReason` fields updated | `src/modules/admin/users/repositories/admin-users.repository.ts` | ⬜ | Already exists; verify supports severity/duration |
| A3-BE-REP-02 | `AdminUsersRepository.createModerationLog` store metadata (duration, custom minutes, severity) | same | ⬜ | Already partly done; confirm payload coverage |

### 2.3 Service Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A3-BE-SVC-01 | `AdminUsersService.disableUser` enforce flow: check status, calc expiry, lock user, log, invalidate sessions, send email, return detail | `src/modules/admin/users/services/admin-users.service.ts` | ✅ | Email send toggles when `sendEmail !== false`; metadata unchanged |
| A3-BE-SVC-02 | `AdminUsersService.batchDisableUsers` handle partial success + reason logs | same | ⬜ | Ensure aggregated results, error logging |
| A3-BE-SVC-03 | `AdminAutoUnlockService` (or scheduler) to auto unlock when `lockExpiresAt` reached | `src/modules/admin/users/services/admin-auto-unlock.service.ts` | ⬜ | Confirm job logic & add doc pointers |

### 2.4 Controller & Routes
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A3-BE-CTL-01 | `AdminUsersController.disableUser` ensure permission check + error responses per UC | `src/modules/admin/users/controllers/admin-users.controller.ts` | ⬜ | Already present; review responses |
| A3-BE-CTL-02 | `batchLock` endpoint handles aggregated result formatting | same | ⬜ | Should return success/failure per user |
| A3-BE-RT-01 | Routes already exist (PATCH `/users/:id/disable`, POST `/users/batch/lock`) | `src/modules/admin/users/routes.ts` | ⬜ | Confirm validators updated & permission `User.Disable` applied |

### 2.5 Notifications & Audit
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A3-BE-NOT-01 | Email service templates for lock (reason + duration) | `src/infrastructure/email/email.service.ts` | ⬜ | Ensure toggled by `sendEmail` |
| A3-BE-AUD-01 | Audit log entry `USER_LOCK` capturing reason/duration/performedBy | `src/modules/admin/users/services/admin-audit.service.ts` | ⬜ | Already integrated? verify metadata |

### 2.6 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A3-BE-TST-01 | Unit tests `disableUser` (success, already locked, validation) | `src/modules/admin/users/services/__tests__/admin-users.service.spec.ts` | ⬜ | Mock repo + email service |
| A3-BE-TST-02 | Unit tests `batchDisableUsers` for partial failure handling | same | ⬜ | Ensure aggregated result |
| A3-BE-TST-03 | Integration tests for `/admin/users/:id/disable` & `/batch/lock` | `src/tests/integration/admin/users.lock.spec.ts` | ⬜ | Cover 200, 409, permission |

---

## 3. Frontend Tasks

### 3.1 Types, Validators, Services
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A3-FE-TYP-01 | Define `LockDurationPreset`, `DisableUserPayload` types | `fe-web/src/types/admin-users.types.ts` | ⬜ | Mirror BE enum + payload |
| A3-FE-VLD-01 | Client-side schema for lock form | `fe-web/src/validators/admin-users.validator.ts` | ⬜ | Reason length, custom duration, severity, sendEmail |
| A3-FE-SVC-01 | `adminUsersApi.disableUser(userId, payload)` | `fe-web/src/services/admin-users.service.ts` | ⬜ | PATCH `/admin/users/:id/disable` |
| A3-FE-SVC-02 | `adminUsersApi.batchDisableUsers(payload)` | same | ⬜ | POST `/admin/users/batch/lock` |

### 3.2 Hooks & State
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A3-FE-HOOK-01 | `useAdminUserActions` expose `disable(userId)` with optimistic updates | `fe-web/src/hooks/admin-users/useAdminUserActions.ts` | ⬜ | Already referenced in UI guide |
| A3-FE-HOOK-02 | `useBatchLockController` manage selection & bulk action | same or new | ⬜ | Interacts with list selection state |

### 3.3 Components
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A3-FE-CMP-01 | `DisableUserDialog` (single) | `fe-web/src/components/admin/users/DisableUserDialog.tsx` | ⬜ | Reason textarea, duration presets, severity, sendEmail toggle |
| A3-FE-CMP-02 | `BatchLockSheet` | `fe-web/src/components/admin/users/BatchLockSheet.tsx` | ⬜ | Show selected count, apply shared payload |
| A3-FE-CMP-03 | `LockDurationPresets` subcomponent | same namespace | ⬜ | Buttons + custom input |
| A3-FE-CMP-04 | `LockSeverityBadge` | same | ⬜ | Display severity context in detail view |
| A3-FE-CMP-05 | Toast/notification messages for success/error | global components? | ⬜ | Ensure consistent copy |

### 3.4 Screens Integration
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A3-FE-VIEW-01 | Integrate dialog into user list (toolbar or row action) | `app/admin/(users)/index.tsx` + `UsersTable` | ⬜ | Row-level action & selection state |
| A3-FE-VIEW-02 | Integrate into user detail page | `app/admin/(users)/[userId]/index.tsx` | ⬜ | CTA button triggering dialog |
| A3-FE-VIEW-03 | Batch lock entry point (toolbar multi-select) | same list view | ⬜ | Manage selection + open sheet |

### 3.5 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A3-FE-TST-01 | Component tests for `DisableUserDialog` | `fe-web/src/components/admin/users/__tests__/DisableUserDialog.test.tsx` | ⬜ | Validate form, submission, error states |
| A3-FE-TST-02 | Hook tests for `useAdminUserActions` | `fe-web/src/hooks/admin-users/__tests__/useAdminUserActions.test.ts` | ⬜ | Success, conflict, permission error |
| A3-FE-TST-03 | E2E scenario: lock single user & batch lock | `fe-web/tests/admin/user-lock.spec.ts` | ⬜ | Ensure UI + BE integration |

---

## 4. Cross-cutting / Operational
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A3-CC-01 | Add copy decks for dialog text | `DOCS/UI/content/admin-users.md` (if exists) | ⬜ | Ensure localized strings |
| A3-CC-02 | Update auto-unlock cron doc & schedule | `scripts/auto-unlock.ts` + README | ⬜ | Document invocation |
| A3-CC-03 | Feature flag to toggle email notifications vs silent mode | `config/featureFlags` (BE & FE) | ⬜ | e.g., `ENABLE_USER_LOCK_EMAIL` |
| A3-CC-04 | Analytics tracking (optional) when admin locks user | FE analytics service | ⬜ | For monitoring |

---

## 5. QA Checklist
- [ ] API unit/integration tests cover lock & batch flows (success/conflict/permission).
- [ ] Email preview verified in staging (reason/duration).
- [ ] Auto unlock job tested with expired entries.
- [ ] FE interaction tests confirm validation + error messages.
- [ ] Accessibility: dialog focus trap, labels, keyboard nav.
- [ ] Audit logs list lock events with metadata (reason/duration/performedBy).

---

## 6. Assumptions & Notes
1. Lock severity default `NORMAL`; `CRITICAL` reserved for severe violations (only Super Admin can unlock).
2. Batch lock shares same payload for all selected users; advanced per-user reason not supported in this iteration.
3. Email notification optional; toggled by checkbox (default true).
4. Auto unlock cron already exists (UC-A1 doc) but ensure lock reason/expiry data kept consistent.
5. UI uses same validators as BE to avoid mismatch; custom duration limited to 15–43200 minutes (30 days).

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist derived from UC specs & architecture guides |


