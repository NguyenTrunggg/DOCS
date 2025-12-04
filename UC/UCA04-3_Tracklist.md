# Tracklist · UCA04-3 “Phân quyền cho tài khoản Admin”

> **Scope**: Super Admin assigns/removes roles & ad-hoc permissions for any admin account, preventing loss of last Super Admin and logging all changes.

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic**: open admin detail → load assignable roles/permissions → select roles/perms → optional expiry for temporary roles → submit → backend validates (no last Super Admin removal, valid role IDs) → update DB → audit log before/after → refresh effective permissions.
- **Alternative**: apply from template; temporary roles with expiry; preview effective permissions before saving.
- **Exception**: insufficient permission, invalid role/perms, trying to remove last Super Admin, DB failure.
- **Rules/NFR**: permission `AdminAccount.ManageRoles`; must log who/when/changes; immediate effect (no cache).

---

## 2. Backend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A43-BE-DTO-01 | Ensure `UpdateAdminRolesRequestDto` supports roleIds, permissionIds, expiresAt | `dto/request/update-admin-roles.dto.ts` | ✅ |
| A43-BE-VLD-01 | `updateAdminRolesValidator` (or-condition roleIds/permissionIds, expiry validation) | `validators/admin-accounts.validator.ts` | ✅ |
| A43-BE-REP-01 | Repo method `updateUserRoles` + `getUserRolesWithPermissions` | `repositories/admin-accounts.repository.ts` | ✅ |
| A43-BE-SVC-01 | Service validation (last Super Admin guard, role existence) | `services/admin-accounts.service.ts` | ✅ |
| A43-BE-SVC-02 | Apply role updates with expiry + optional permissions | `services/admin-accounts.service.ts` | ✅ |
| A43-BE-CC-01 | Audit log `ADMIN_ROLE_UPDATE` (before/after, expiresAt) | service + audit repo | ✅ |
| A43-BE-CTL-01 | Controller `PUT /admin/accounts/:id/roles` | `controllers/admin-accounts.controller.ts` + `routes.ts` | ✅ |
| A43-BE-OPT-01 | Effective permissions preview endpoint | service/controller | ⬜ |
| A43-BE-OPT-02 | Role template apply endpoint | service/controller | ⬜ |

Testing:
| Task | Status | Notes |
|------|--------|-------|
| Unit tests for `updateAdminRoles` (last Super Admin, invalid roleId) | ⬜ |
| Integration test `PUT /admin/accounts/:id/roles` | ⬜ |

---

## 3. Frontend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A43-FE-TYP-01 | `UpdateAdminRolesPayload` | `fe-web/src/types/admin-accounts.types.ts` | ⬜ |
| A43-FE-VLD-01 | Client-side validator (at least one change, expiry ISO) | `fe-web/src/validators/admin-accounts.validator.ts` | ⬜ |
| A43-FE-SVC-01 | API gateway `adminAccountsApi.updateRoles(id,payload)` | `fe-web/src/services/admin-accounts.service.ts` | ⬜ |
| A43-FE-HOOK-01 | Hook/controller `useAdminRoleAssignment` | `fe-web/src/hooks/admin-accounts/useAdminRoleAssignment.ts` | ⬜ |
| A43-FE-CMP-01 | Role & permission picker UI | `fe-web/src/components/admin/accounts/AdminRoleDrawer.tsx` | ⬜ |
| A43-FE-CMP-02 | Effective permissions preview component | ⬜ |
| A43-FE-CMP-03 | Success/error toast handling | ⬜ |
| A43-FE-TST-01 | Component/hook tests | ⬜ |

---

## 4. Cross-cutting
| Task | Status | Notes |
|------|--------|-------|
| Audit log analytics dashboard update | ⬜ |
| Copywriting for warnings (last Super Admin, conflicts) | ⬜ |
| Feature flag / permission guard on UI route | ⬜ |

---

## 5. QA Checklist
- [ ] Cannot remove final Super Admin role.
- [ ] Invalid roleIds rejected.
- [ ] Audit log records actor, before/after, expiry.
- [ ] Permissions update effective immediately in subsequent fetch.
- [ ] Expiry dates respected (roles expire automatically).

---

## 6. Assumptions
1. Permissions assigned indirectly via roles; direct permission assignment optional future scope.
2. `expiresAt` applies to entire role assignment batch.
3. Effective permissions preview computed client-side using data from detail endpoint unless dedicated API added.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist |


