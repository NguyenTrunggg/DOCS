# Tracklist · UCA04-1 “Xem danh sách tài khoản Admin”

> **Scope**: Super Admin view for admin accounts with search, role/status filter, pagination, export, saved filters. Drives subsequent actions (detail, role assignment, delete).

> **Architecture**: Admin accounts module (Layered API + FE separation).

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic**: list admin accounts showing ID, avatar, name, email, role(s), created date, status; search by name/email; filter by role/status; paginate 20/page.
- **Alternative**: export CSV, saved filters, quick jump to detail.
- **Exception**: empty dataset, permission errors.
- **Rules/NFR**:
  - Permission `AdminAccount.Read` (Super Admin only).
  - Audit access.
  - Perf <2s.

---

## 2. Backend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A4-BE-DTO-01 | Query DTO (`GetAdminAccountsQueryDto`) | `modules/admin/accounts/dto/request/get-admin-accounts.dto.ts` (if exists) | ⬜ | Create with search, role, status, paging |
| A4-BE-DTO-02 | Response DTO for list items | `dto/response/admin-account-response.dto.ts` | ⬜ | Fields per UC |
| A4-BE-VLD-01 | Query validator | `validators/admin-accounts.validator.ts` | ⬜ |
| A4-BE-REP-01 | Repository method `findAll` with filters | `repositories/admin-accounts.repository.ts` | ⬜ |
| A4-BE-REP-02 | `count(filters)` | same | ⬜ |
| A4-BE-SVC-01 | `AdminAccountsService.getAccounts(query)` returning data + total | `services/admin-accounts.service.ts` | ⬜ |
| A4-BE-SVC-02 | Export helper (limit 5000) | service | ⬜ |
| A4-BE-CTL-01 | Controller `getAccounts` with pagination utils + audit log | `controllers/admin-accounts.controller.ts` | ⬜ |
| A4-BE-CTL-02 | Export endpoint | same | ⬜ |
| A4-BE-RT-01 | Routes `GET /admin/admins` & `/export` with permission check | `routes.ts` | ⬜ |
| A4-BE-CC-01 | Audit logging for list/export | admin audit service | ⬜ |

Testing:
| Task | Status |
|------|--------|
| Unit tests for service filtering | ⬜ |
| Integration tests for list/export endpoints | ⬜ |

---

## 3. Frontend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A4-FE-TYP-01 | Admin account list/query types | `fe-web/src/types/admin-accounts.types.ts` | ⬜ |
| A4-FE-SVC-01 | `adminAccountsApi.list/export` | `fe-web/src/services/admin-accounts.service.ts` | ⬜ |
| A4-FE-HOOK-01 | `useAdminAccountsList` (filters, pagination) | `fe-web/src/hooks/admin-accounts/useAdminAccountsList.ts` | ⬜ |
| A4-FE-HOOK-02 | `useAdminAccountsExport` | same | ⬜ |
| A4-FE-CMP-01 | `AdminAccountsToolbar` | `fe-web/src/components/admin/accounts/AdminAccountsToolbar.tsx` | ⬜ | Search, role/status filters, export |
| A4-FE-CMP-02 | `AdminAccountsTable` | same | ⬜ | Columns per UC |
| A4-FE-CMP-03 | Pagination, empty/error state, skeleton | same | ⬜ |
| A4-FE-VIEW-01 | Admin accounts list screen | `fe-web/app/admin/(accounts)/page.tsx` (or similar) | ⬜ |
| A4-FE-TST-01 | Hook/component tests | ⬜ |
| A4-FE-TST-02 | E2E filter/export | ⬜ |

---

## 4. Cross-cutting
| Task | Status | Notes |
|------|--------|-------|
| Audit logging + analytics | ⬜ |
| Copy deck for empty/error states | ⬜ |
| Saved filters (optional) | ⬜ |

---

## 5. QA Checklist
- [ ] API filters (role/status/search) verified.
- [ ] Export limited to 5000 rows.
- [ ] Permission `AdminAccount.Read` enforced.
- [ ] UI handles empty/error.
- [ ] Audit log recorded.

---

## 6. Assumptions
1. Only Super Admin can view list (per UC).
2. Roles stored as string/enum; multi-role if needed.
3. Export currently CSV.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist |


