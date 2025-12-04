# Tracklist · UCA04-4 “Xóa tài khoản Admin”

> **Scope**: Super Admin permanently deletes an admin account (or blocks deletion if it is the last Super Admin), ensuring audit log capture and confirmation UX.

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic**: click delete → confirm dialog → backend validates permission + last Super Admin guard → delete record → audit log → success toast.
- **Alternative**: soft delete (mark inactive) or bulk delete (multi-select).
- **Exception**: cannot delete last Super Admin, insufficient permission, DB failure.
- **Rules/NFR**: permission `AdminAccount.Delete`; action must be audited; ensure referential integrity (related logs/orders handled).

---

## 2. Backend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A44-BE-DTO-01 | Confirm no extra DTO needed (uses `adminIdParamValidator`) | `validators/admin-accounts.validator.ts` | ✅ |
| A44-BE-PRM-01 | Route guard `AdminAccount.Delete` | `routes.ts` | ✅ |
| A44-BE-SVC-01 | Service guard for last Super Admin | `services/admin-accounts.service.ts` | ✅ |
| A44-BE-SVC-02 | Delete implementation + audit log metadata | `services/admin-accounts.service.ts` | ✅ |
| A44-BE-CTL-01 | Controller `DELETE /admin/accounts/:id` | `controllers/admin-accounts.controller.ts` | ✅ |
| A44-BE-OPT-01 | Soft delete pipeline | repository/service | ⬜ |
| A44-BE-OPT-02 | Bulk delete endpoint | controller/service | ⬜ |

Testing:
| Task | Status | Notes |
|------|--------|-------|
| Unit test `deleteAdminAccount` (last Super Admin, success) | ⬜ |
| Integration test `DELETE /admin/accounts/:id` | ⬜ |

---

## 3. Frontend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A44-FE-UX-01 | Confirmation modal component | `fe-web/src/components/admin/accounts/DeleteAdminModal.tsx` | ⬜ |
| A44-FE-SVC-01 | API call `adminAccountsApi.delete(id)` | `fe-web/src/services/admin-accounts.service.ts` | ⬜ |
| A44-FE-HOOK-01 | Hook `useDeleteAdminAccount` for mutation | `fe-web/src/hooks/admin-accounts/useDeleteAdminAccount.ts` | ⬜ |
| A44-FE-UX-02 | Bulk delete UI (multi select + confirm) | ⬜ |
| A44-FE-UX-03 | Surface warning when deleting potential last Super Admin | ⬜ |
| A44-FE-TST-01 | Tests for modal + hook | ⬜ |

---

## 4. Cross-cutting
| Task | Status | Notes |
|------|--------|-------|
| Audit dashboard entry for deletes | ⬜ |
| Copy deck for confirmation + exception states | ⬜ |
| Analytics ping when deletion occurs | ⬜ |

---

## 5. QA Checklist
- [ ] Deleting last Super Admin blocked with correct message.
- [ ] Successful delete removes account from list immediately.
- [ ] Audit log contains actor, target, email, role snapshot.
- [ ] Permission enforcement (non-Super cannot delete).
- [ ] Bulk delete respects same safety checks.

---

## 6. Assumptions
1. Hard delete is acceptable (no foreign key issues) until soft delete prioritized.
2. Bulk delete not required for initial release; tracked as optional.
3. Confirmation modal handled client-side; backend expects single ID.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist |


