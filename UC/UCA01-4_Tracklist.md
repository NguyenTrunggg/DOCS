# Tracklist · UCA01-4 “Kích hoạt lại tài khoản người dùng”

> **Scope**: Admin unlock flow triggered from user list/detail or auto unlock process. Covers manual unlock confirmation, optional note, severity guard (Super Admin for CRITICAL), batch unlock, email notification, audit logging, session sync.

> **Architecture**: Maintain Layered API (`DTO → Validator → Repository → Service → Controller → Routes`) and UI separation (`Validator → Service gateway → Hook → Component → View`). Keep SOLID + namespace rules from `UI-Auth-Accounts-Build-Guide`.

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic flow**: admin clicks unlock → confirm dialog with optional note → BE unlocks (status ACTIVE), logs audit, optionally email user → FE refresh detail/list.
- **Alternative**: auto unlock via cron when expiry reached; batch unlock from user selection; ability to include note in email.
- **Exception**: missing permission (`User.Enable`), user not locked, DB failure; severity CRITICAL restricts to Super Admin.
- **Rules/NFR**:
  - Audit each unlock (`USER_UNLOCK`/`USER_DETAIL_UNLOCK`).
  - CRITICAL locked accounts require Super Admin.
  - Auto unlock job should reuse same service path.
  - Response time <1s; sessions reflect state immediately.

---

## 2. Backend Tasks

### 2.1 DTO & Validators
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A4-BE-DTO-01 | Ensure `EnableUserRequestDto` includes optional `note` & `sendEmail` | `src/modules/admin/users/dto/request/enable-user.dto.ts` | ✅ | Added `sendEmail?: boolean` |
| A4-BE-DTO-02 | Add `sendEmail` to `BatchUnlockRequestDto` | `src/modules/admin/users/dto/request/batch-unlock.dto.ts` | ✅ | Payload now mirrors disable request |
| A4-BE-VLD-01 | Update validators for enable/batch unlock payloads | `src/modules/admin/users/validators/admin-users.validator.ts` | ✅ | Added `sendEmail` with default `true` |

### 2.2 Repository Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A4-BE-REP-01 | `AdminUsersRepository.unlockUser` ensures all lock fields reset | `src/modules/admin/users/repositories/admin-users.repository.ts` | ⬜ | Already resets; double-check for new fields |
| A4-BE-REP-02 | `createModerationLog` entry for unlock | same | ⬜ | Already used; confirm metadata for note |

### 2.3 Service Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A4-BE-SVC-01 | `AdminUsersService.enableUser` enforce status guard, severity check, session handling, email optional | `src/modules/admin/users/services/admin-users.service.ts` | ✅ | Email send now respects `sendEmail !== false` |
| A4-BE-SVC-02 | `AdminUsersService.batchEnableUsers` to propagate note/sendEmail | same | ✅ | Passes `note` + `sendEmail` to each unlock |
| A4-BE-SVC-03 | `AdminAutoUnlockService` ensures same logging/notifications when unlocking automatically | `src/modules/admin/users/services/admin-auto-unlock.service.ts` | ⬜ | Confirm hooking into `enableUser` or handle separately |

### 2.4 Controller & Routes
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A4-BE-CTL-01 | `enableUser` endpoint ensures permission & note/sendEmail parsing | `src/modules/admin/users/controllers/admin-users.controller.ts` | ⬜ | Already exists; adjust payload typing |
| A4-BE-CTL-02 | `batchUnlock` endpoint handles new payload fields | same | ⬜ | Validate sendEmail presence |
| A4-BE-RT-01 | Routes already defined (PATCH `/users/:id/enable`, POST `/batch/unlock`) | `src/modules/admin/users/routes.ts` | ⬜ | Ensure validators updated and permissions `User.Enable` |

### 2.5 Notifications & Audit
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A4-BE-NOT-01 | Email template for unlock including optional note | `src/infrastructure/email/email.service.ts` | ⬜ | Already `sendAccountUnlockedEmail`; add note param |
| A4-BE-AUD-01 | Audit log entry `USER_UNLOCK` with performedBy, note | `src/modules/admin/users/services/admin-audit.service.ts` | ⬜ | Confirm metadata stored |

### 2.6 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A4-BE-TST-01 | Unit tests `enableUser` (success, not locked, severity guard) | `src/modules/admin/users/services/__tests__/admin-users.service.spec.ts` | ⬜ | Mock repos + email service |
| A4-BE-TST-02 | Unit tests `batchEnableUsers` (partial failures) | same | ⬜ | Validate aggregated results |
| A4-BE-TST-03 | Integration tests for `/users/:id/enable` & `/batch/unlock` | `src/tests/integration/admin/users.unlock.spec.ts` | ⬜ | Cover permission, conflict scenarios |

---

## 3. Frontend Tasks

### 3.1 Types, Validators, Services
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A4-FE-TYP-01 | Define `EnableUserPayload` with optional note/sendEmail | `fe-web/src/types/admin-users.types.ts` | ⬜ | For service layer |
| A4-FE-VLD-01 | Client validator for unlock dialog | `fe-web/src/validators/admin-users.validator.ts` | ⬜ | Note length, sendEmail toggle |
| A4-FE-SVC-01 | `adminUsersApi.enableUser(userId, payload)` | `fe-web/src/services/admin-users.service.ts` | ⬜ | PATCH `/admin/users/:id/enable` |
| A4-FE-SVC-02 | `adminUsersApi.batchEnableUsers(payload)` | same | ⬜ | POST `/batch/unlock` |

### 3.2 Hooks & State
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A4-FE-HOOK-01 | Extend `useAdminUserActions` with `enable(userId)` and `batchEnable` | `fe-web/src/hooks/admin-users/useAdminUserActions.ts` | ⬜ | Manage loading + cache invalidation |
| A4-FE-HOOK-02 | Hook for auto unlock notifications (optional) | same namespace | ⬜ | Show status change toast |

### 3.3 Components
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A4-FE-CMP-01 | `EnableUserDialog` | `fe-web/src/components/admin/users/EnableUserDialog.tsx` | ⬜ | Note input, sendEmail toggle |
| A4-FE-CMP-02 | `BatchUnlockSheet` | `fe-web/src/components/admin/users/BatchUnlockSheet.tsx` | ⬜ | Manage selection summary |
| A4-FE-CMP-03 | `UnlockSuccessBanner/Toast` | `components/admin/common` | ⬜ | Provide feedback |
| A4-FE-CMP-04 | Update `UserStatusBanner` to show unlock CTA state | `components/admin/users/UserStatusBanner.tsx` | ⬜ | Show severity warning |

### 3.4 Screens Integration
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A4-FE-VIEW-01 | Integrate unlock dialog into list row actions | `app/admin/(users)/index.tsx` | ⬜ | Row menu or toolbar button |
| A4-FE-VIEW-02 | Integrate into detail header action area | `app/admin/(users)/[userId]/index.tsx` | ⬜ | CTA near status banner |
| A4-FE-VIEW-03 | Batch unlock entry point (toolbar selection) | same list view | ⬜ | Mirrors batch lock UX |

### 3.5 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A4-FE-TST-01 | Component tests for `EnableUserDialog` | `fe-web/src/components/admin/users/__tests__/EnableUserDialog.test.tsx` | ⬜ | Validate note & sendEmail toggles |
| A4-FE-TST-02 | Hook tests for enable actions | `fe-web/src/hooks/admin-users/__tests__/useAdminUserActions.test.ts` | ⬜ | Success, permission error |
| A4-FE-TST-03 | E2E scenario: unlock single + batch | `fe-web/tests/admin/user-unlock.spec.ts` | ⬜ | Confirm UI + BE |

---

## 4. Cross-cutting
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A4-CC-01 | Auto unlock cron documentation | `scripts/auto-unlock.ts` + README | ⬜ | Note interplay with manual unlock |
| A4-CC-02 | Feature flag `ENABLE_USER_UNLOCK_EMAIL` (if needed) | Config (BE/FE) | ⬜ | Control email behavior |
| A4-CC-03 | Analytics event for unlock | FE analytics service | ⬜ | Track audit/training |
| A4-CC-04 | Localization strings for unlock dialogs/emails | `DOCS/UI/content/admin-users.md` | ⬜ | Ensure consistent copy |

---

## 5. QA Checklist
- [ ] API tests pass for enable/batch unlock (200/403/409).
- [ ] Email notifications verified (note + severity mention).
- [ ] Auto unlock job gracefully handles CRITICAL (skips unless allowed).
- [ ] FE validation ensures note length + required selections.
- [ ] Accessibility on dialogs (focus trap, labels).
- [ ] Audit log contains unlock events with metadata.

---

## 6. Assumptions
1. Default behavior sends email; admin can uncheck `sendEmail`.
2. Batch unlock note applies to all selected users; no per-user note.
3. Auto unlock uses same email + log logic but performedBy = `system-auto`.
4. Unlock action invalidates stale caches (list/detail) via hooks.
5. Severity CRITICAL check enforced server-side even for batch unlock.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist drafted from UC spec |


