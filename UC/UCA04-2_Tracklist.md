# Tracklist · UCA04-2 “Tạo tài khoản Admin mới”

> **Scope**: Super Admin creates new admin accounts with name, email, optional role, sending activation email and logging action.

> **Architecture**: Admin accounts module (Layered API + FE separation).

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic**: open “Create admin” form → enter full name + email (+ optional role) → submit → backend validates uniqueness, creates account (pending status), assigns role if provided, sends activation email → success toast.
- **Alternative**: assign role immediately; bulk import from CSV; resend activation email on failure.
- **Exception**: duplicate email, email send failure (should still create), DB error.
- **Rules/NFR**:
  - Permission `AdminAccount.Create` (Super Admin only).
  - Account initially pending & requires activation.
  - Provide ability to resend activation link.

---

## 2. Backend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A42-BE-DTO-01 | `CreateAdminAccountRequestDto` | `dto/request/create-admin-account.dto.ts` | ✅ |
| A42-BE-VLD-01 | `createAdminAccountValidator` | `validators/admin-accounts.validator.ts` | ✅ |
| A42-BE-SVC-01 | `AdminAccountsService.createAdminAccount` (email check, slug, role assignment, activation email) | `services/admin-accounts.service.ts` | ✅ |
| A42-BE-CTL-01 | Controller route `POST /admin/accounts` | `controllers/admin-accounts.controller.ts` + `routes.ts` | ✅ |
| A42-BE-REP-01 | Repo helper `isEmailExists`, `create`, `updateUserRoles` | `repositories/admin-accounts.repository.ts` | ✅ |
| A42-BE-CC-01 | Audit log `ADMIN_ACCOUNT_CREATE` | admin audit service | ✅ | Logged with activation email metadata |
| A42-BE-OPT-01 | Resend activation endpoint | service/controller | ⬜ |
| A42-BE-OPT-02 | Bulk import endpoint | service/controller | ⬜ |

Testing:
| Task | Status | Notes |
|------|--------|-------|
| Unit tests for `createAdminAccount` (duplicate email, email failure) | ⬜ |
| Integration test `POST /admin/accounts` | ⬜ |

---

## 3. Frontend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A42-FE-TYP-01 | `CreateAdminAccountPayload` type | `fe-web/src/types/admin-accounts.types.ts` | ⬜ |
| A42-FE-VLD-01 | Client validator (name/email) | `fe-web/src/validators/admin-accounts.validator.ts` | ⬜ |
| A42-FE-SVC-01 | `adminAccountsApi.create(payload)` | `fe-web/src/services/admin-accounts.service.ts` | ⬜ |
| A42-FE-HOOK-01 | `useCreateAdminAccount` hook/controller | `fe-web/src/hooks/admin-accounts/useCreateAdminAccount.ts` | ⬜ |
| A42-FE-CMP-01 | `AdminAccountForm` (name/email/role select) | `fe-web/src/components/admin/accounts/AdminAccountForm.tsx` | ⬜ |
| A42-FE-CMP-02 | Create modal/page | `fe-web/app/admin/(accounts)/new.tsx` | ⬜ |
| A42-FE-CMP-03 | Resend activation banner (optional) | same | ⬜ |
| A42-FE-TST-01 | Component/hook tests | ⬜ |
| A42-FE-OPT-01 | Bulk import UI | ⬜ |

---

## 4. Cross-cutting
| Task | Status | Notes |
|------|--------|-------|
| Audit log + analytics integration | 🔄 | Audit log done; analytics pending |
| Copy deck for success/error/resend messages | ⬜ |
| Email template check (activation) | ⬜ |

---

## 5. QA Checklist
- [ ] Duplicate email returns error without creating account.
- [ ] Activation email failure surfaces warning but account exists.
- [ ] Account initially `PENDING` and requires activation.
- [ ] Permissions enforced (only Super Admin).
- [ ] UI/form validation mirrors backend.

---

## 6. Assumptions
1. Role assignment optional; defaults to `ADMIN`.
2. Activation email contains unique token; resend endpoint handles re-queueing.
3. Bulk import not in current scope unless prioritized.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist |


