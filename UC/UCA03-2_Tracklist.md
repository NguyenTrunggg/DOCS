# Tracklist · UCA03-2 “Thêm danh mục mới”

> **Scope**: Admin creates new categories with name & optional description, ensuring uniqueness & order handling. Includes validation, slug generation, audit logging, optional bulk import.

> **Architecture**: Uses admin categories module (Layered API & FE separation).

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic**: open “Add category” form → enter name (required) & description (optional) → submit → backend validates uniqueness, generates slug/order, stores record → success toast.
- **Alternative**: bulk import via CSV; order assignment; ability to set active status.
- **Exception**: duplicate name, invalid input, DB errors.
- **Rules/NFR**:
  - Name unique, case-insensitive.
  - Response <1s; Admin permission only.

---

## 2. Backend Tasks (status)

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C2-BE-DTO-01 | `CreateCategoryRequestDto` definitions | `dto/request/create-category.dto.ts` | ✅ | Already includes name, description, order, isActive |
| C2-BE-VLD-01 | `createCategoryValidator` | `validators/admin-categories.validator.ts` | ✅ | Enforces name length, optional description |
| C2-BE-SVC-01 | `AdminCategoriesService.createCategory` (validation, slug, order) | `services/admin-categories.service.ts` | ✅ | Handles uniqueness, slugify |
| C2-BE-REP-01 | Repo uniqueness helpers `isNameTaken`/`isSlugTaken` | `repositories/admin-categories.repository.ts` | ✅ |
| C2-BE-CTL-01 | Controller route `POST /admin/categories` | `controllers/admin-categories.controller.ts` & `routes.ts` | ✅ |
| C2-BE-CC-01 | Audit log `CATEGORY_CREATE` | `admin-audit` service | ⬜ | Add metadata (categoryId, name) |
| C2-BE-OPT-01 | Bulk import endpoint (CSV) | new | ⬜ | Optional future |

### Testing
| Task | File | Status |
|------|------|--------|
| Unit tests for service (duplicate name, success) | `services/__tests__/admin-categories.service.spec.ts` | ⬜ |
| Integration test `POST /admin/categories` | `tests/integration/admin/categories.create.spec.ts` | ⬜ |

---

## 3. Frontend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C2-FE-TYP-01 | `CreateCategoryPayload` type | `fe-web/src/types/admin-categories.types.ts` | ⬜ |
| C2-FE-VLD-01 | Client validator (name length, uniqueness hint) | `fe-web/src/validators/admin-categories.validator.ts` | ⬜ |
| C2-FE-SVC-01 | `adminCategoriesApi.create(payload)` | `fe-web/src/services/admin-categories.service.ts` | ⬜ |
| C2-FE-HOOK-01 | `useCreateCategoryController` | `fe-web/src/hooks/admin-categories/useCreateCategoryController.ts` | ⬜ |
| C2-FE-CMP-01 | `CategoryForm` component (name, description, order toggle) | `fe-web/src/components/admin/categories/CategoryForm.tsx` | ⬜ |
| C2-FE-CMP-02 | `CreateCategoryModal/Page` | `fe-web/app/admin/(catalog)/categories/new.tsx` (or modal) | ⬜ |
| C2-FE-TST-01 | Component/hook tests | `fe-web/src/components/admin/categories/__tests__/CategoryForm.test.tsx` | ⬜ |

Optional bulk import:
| Task | Status |
|------|--------|
| CSV upload UI + backend integration | ⬜ |

---

## 4. Cross-cutting / Observability
| Task | Status | Notes |
|------|--------|-------|
| Audit logging for create | ⬜ | Integrate with admin audit service |
| Copy deck for success/error | ⬜ |
| Analytics event for new category | ⬜ |

---

## 5. QA Checklist
- [ ] Duplicate name returns validation error.
- [ ] Slug generated properly (no collisions).
- [ ] API rejects when missing required fields.
- [ ] FE form shows validation messages & success toast.
- [ ] (Optional) Bulk import validates rows before create.

---

## 6. Assumptions
1. Order defaults to `count + 1` if not provided.
2. Only Admin/Super Admin have Create permission.
3. Bulk import is future scope; base UC covers single create.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist |


