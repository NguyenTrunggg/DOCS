# Tracklist · UCA01-2 “Xem chi tiết người dùng”

> **Scope**: Admin user detail experience (UC-A1-2) rendered after selecting a user from the list or by direct ID entry. Includes profile data, status, activity stats, recent recipes, moderation logs, CTA to lock/unlock (handoff to UC-A1-3/4), export detail to PDF, audit logging, permission guard `User.Read`.

> **Architecture**: Follow Layered API (`DTO → Validator → Repository → Service → Controller → Routes`) and UI separation (`Validator → Service gateway → Hook/Controller → Presentational components → View`). Keep SOLID & existing admin namespace rules from `UI-Auth-Accounts-Build-Guide.md`.

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic Flow**: fetch detail by ID → render profile/contacts/status/ activity stats/ recent recipes/ moderation logs → CTA for lock/unlock.
- **Alternative Flow**:
  - Direct open by ID (deep link, e.g. `/admin/users/:id`).
  - Export single-user report (PDF/CSV) for archival.
  - Quick navigation to related entities (recipes).
- **Exception**:
  - User not found / deleted → show 404 card + back to list.
  - Missing permission → 403 error state.
  - Data load error → retry banner.
- **Business Rules & NFR**:
  - Respect PII; only Admin/Super Admin view stats.
  - Response <2s (prefetch composite queries in parallel).
  - Audit viewing detail (action `USER_DETAIL_VIEW`).
  - Breadcrumb back to list; sticky CTA bar.

---

## 2. Backend Tasks

### 2.1 DTO & Types
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A2-BE-DTO-01 | Ensure `AdminUserDetailResponseDto` fully represents profile/contact/status/activity/logs | `src/modules/admin/users/dto/response/admin-user-response.dto.ts` | ⬜ | Add optional fields for exports (e.g., `recentRecipes`) |
| A2-BE-DTO-02 | Create `AdminUserExportResponseDto` (for PDF/CSV) | same module | ✅ | Reused detail DTO output + CSV builder; no extra response needed currently |
| A2-BE-DTO-03 | Extend `AdminUserActivityStatsDto` with totals needed for UI (recipes approved/pending/rejected, comments, reviews) | same | ⬜ | Already partly present—verify coverage |

### 2.2 Validators
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A2-BE-VLD-01 | `userIdParamValidator` reuse & ensure exported | `src/modules/admin/users/validators/admin-users.validator.ts` | ⬜ | Already exists; confirm direct route uses it |
| A2-BE-VLD-02 | `exportUserDetailValidator` for PDF export query (format, timezone) | same | ✅ | Added `exportUserDetailQueryValidator` (format=csv, includeLogs, optional timezone) |

### 2.3 Repository Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A2-BE-REP-01 | `AdminUsersRepository.findDetailById(id)` include relations (moderation logs, stats) | `src/modules/admin/users/repositories/admin-users.repository.ts` | ⬜ | Compose aggregated detail in one request if possible |
| A2-BE-REP-02 | `getRecentModerationLogs(userId, limit=10)` (ensure order) | same | ⬜ | Already 5 entries; extend limit param for detail view |
| A2-BE-REP-03 | `getRecentUserRecipes(userId, take=5)` | same or dedicated repo | ⬜ | Provide for recent activity list |
| A2-BE-REP-04 | `AdminUsersRepository.mapForExport(userId)` | same | ⬜ | Return dataset ready for PDF/CSV |

### 2.4 Service Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A2-BE-SVC-01 | `AdminUsersService.getUserById` return detail DTO | `src/modules/admin/users/services/admin-users.service.ts` | ⬜ | Already exists; review to ensure stats/logs/recent recipes & 404 handling |
| A2-BE-SVC-02 | `AdminUsersService.exportUserDetail(id, query)` | same | ✅ | Implemented CSV export (format cap, includeLogs toggle, CSV escape helper) |
| A2-BE-SVC-03 | Inject `AdminAuditService` to log detail view & export actions | same / `admin-audit.service.ts` | ✅ | Controller logs `USER_DETAIL_EXPORT` with metadata |

### 2.5 Controller & Routes
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A2-BE-CTL-01 | `getUserById` ensure audit logging and error mapping | `src/modules/admin/users/controllers/admin-users.controller.ts` | ⬜ | Already logging; confirm metadata includes reason |
| A2-BE-CTL-02 | Add `exportUserDetail` endpoint (`GET /admin/users/:id/export`) | same | ✅ | Streams CSV + audit log, supports format/includeLogs |
| A2-BE-RT-01 | Wire new route with validators & permissions | `src/modules/admin/users/routes.ts` | ✅ | Added `/admin/users/:id/export` with param/query validation |
| A2-BE-RT-02 | Update server registration if needed | `src/server.ts` | ⬜ | Ensure new endpoint accessible |

### 2.6 Reporting/Export Utilities
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A2-BE-RPT-01 | Shared exporter for user detail PDF | `src/common/utils/exporter.ts` (or new) | ⬜ | Accept template & data, return buffer |
| A2-BE-RPT-02 | CSV builder reuse from list export | same | ✅ | Inline CSV formatter & escape helper in service |

### 2.7 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A2-BE-TST-01 | Unit test `AdminUsersService.getUserById` (PII & stats) | `src/modules/admin/users/services/__tests__/admin-users.service.spec.ts` | ⬜ | Ensure missing user → NotFoundError |
| A2-BE-TST-02 | Integration test `/api/v1/admin/users/:id` | `src/tests/integration/admin/users.detail.spec.ts` | ⬜ | Covers 200, 404, 403 |
| A2-BE-TST-03 | Integration test export endpoint | same suite | ⬜ | Validates content headers & audit log |

---

## 3. Frontend Tasks

### 3.1 Types, Validators, Services
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A2-FE-TYP-01 | Define `AdminUserDetail` types mirroring DTO | `fe-web/src/types/admin-users.types.ts` | ⬜ | Include stats, logs, recent recipes |
| A2-FE-VLD-01 | Detail route param validator (userId) | `fe-web/src/validators/admin-users.validator.ts` | ⬜ | For router guard/hook |
| A2-FE-SVC-01 | `adminUsersApi.getDetail(userId)` | `fe-web/src/services/admin-users.service.ts` | ⬜ | GET `/admin/users/:id` |
| A2-FE-SVC-02 | `adminUsersApi.exportDetail(userId, params)` | same | ⬜ | Handles PDF/CSV download |

### 3.2 Hooks & State
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A2-FE-HOOK-01 | `useAdminUserDetail(userId)` | `fe-web/src/hooks/admin-users/useAdminUserDetail.ts` | ⬜ | Fetch detail, handle loading/error, refetch after lock/unlock |
| A2-FE-HOOK-02 | `useUserDetailExport` | same dir | ⬜ | Manage export action (progress, toast) |
| A2-FE-HOOK-03 | `useUserDetailBreadcrumbs` | same dir | ⬜ | Provide navigation info |

### 3.3 Components
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A2-FE-CMP-01 | `UserProfileCard` (detail variant) | `components/admin/users/UserProfileCard.tsx` | ⬜ | Avatar, name, email, role, joined date |
| A2-FE-CMP-02 | `UserStatusBanner` | `components/admin/users/UserStatusBanner.tsx` | ⬜ | Shows lock status, reason, CTA to lock/unlock |
| A2-FE-CMP-03 | `UserStatsGrid` | `components/admin/users/UserStatsGrid.tsx` | ⬜ | Recipes summary, comments, reviews |
| A2-FE-CMP-04 | `RecentRecipesList` | `components/admin/users/RecentRecipesList.tsx` | ⬜ | Up to 5 items with links |
| A2-FE-CMP-05 | `ModerationLogsTimeline` | `components/admin/users/ModerationLogsTimeline.tsx` | ⬜ | Chronological list with action badges |
| A2-FE-CMP-06 | `DetailHeaderActions` | `components/admin/users/DetailHeaderActions.tsx` | ⬜ | Export button, lock/unlock action, refresh |
| A2-FE-CMP-07 | `DetailSkeleton` | `components/admin/users/UserDetailSkeleton.tsx` | ⬜ | Loading state |
| A2-FE-CMP-08 | `NotFoundState` & `ForbiddenState` | `components/admin/users/UserDetailStates.tsx` | ⬜ | Show 404/403 messages |

### 3.4 Screens & Navigation
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A2-FE-VIEW-01 | Detail screen route | `fe-web/app/(admin)/(users)/[userId]/index.tsx` | ⬜ | Compose components, handle states |
| A2-FE-VIEW-02 | Breadcrumb integration | same | ⬜ | Provide “Quay lại danh sách” |
| A2-FE-VIEW-03 | Link from list row | `UsersTable` | ⬜ | Ensure row click pushes to detail route with userId |

### 3.5 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A2-FE-TST-01 | Hook tests `useAdminUserDetail` | `fe-web/src/hooks/admin-users/__tests__/useAdminUserDetail.test.ts` | ⬜ | Covers success, 404, error, refresh |
| A2-FE-TST-02 | Component tests (ProfileCard, StatusBanner, LogsTimeline) | `fe-web/src/components/admin/users/__tests__/` | ⬜ | Snapshot & interaction |
| A2-FE-TST-03 | E2E test: open detail, export, trigger lock/unlock | `fe-web/tests/admin/user-detail.spec.ts` | ⬜ | Verify route guard, states |

---

## 4. Cross-cutting / Infrastructure
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A2-CC-01 | Audit log extension for detail export | `src/modules/admin/users/services/admin-audit.service.ts` | ✅ | `USER_DETAIL_EXPORT` action logged with format/includeLogs |
| A2-CC-02 | Permission guard in FE route | `fe-web/app/(admin)/layout.tsx` | ⬜ | Redirect if lacking `User.Read` |
| A2-CC-03 | Feature flag for detail mock (`USE_MOCK_ADMIN_USER_DETAIL`) | `fe-web/src/config/featureFlags.ts` | ⬜ | Hook fallback |
| A2-CC-04 | PDF template asset | `fe-web/public/templates/user-detail` or server-side template | ⬜ | Document layout for export |
| A2-CC-05 | Accessibility review (ARIA for timeline, stats) | `components/admin/users` | ⬜ | Ensure screen reader labels |

---

## 5. QA Checklist
- [ ] API integration tests pass for detail + export endpoints.
- [ ] FE hook/component tests cover loading/error/empty states.
- [ ] Audit logs verify `USER_DETAIL_VIEW` + export events.
- [ ] Performance: detail endpoint < 2 s with data volume (parallel queries).
- [ ] Security: 403 on missing permission, 404 on deleted user, PII masked in logs.
- [ ] UX: breadcrumb/back link, CTA accessible, export disabled while loading.

---

## 6. Assumptions & Open Points
1. Export currently supports CSV via `format` query (default `csv`); PDF implementation pending.
2. Recent activity limited to 5 recipes; more advanced timeline handled later.
3. Detail screen reuses lock/unlock dialogs from UC-A1-3/4 (same hook).
4. Stats visible to Admin & Super Admin; if Moderator role allowed `User.Read`, certain metrics may be hidden—require feature flag check.
5. When user not found, FE routes to `/admin/users` with toast while showing NotFound card for context.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist drafted from UC spec & architecture guides |


