# Tracklist · UCA03-3 “Sửa tên danh mục”

> **Scope**: Admin updates category name/description/order/active state, ensuring uniqueness and slug updates. Optionally handle slug sync, recipe counts display, audit logs.

> **Architecture**: Existing admin categories module (Layered API + FE separation).

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic**: open category detail or inline edit → update name/description → submit → backend validates uniqueness, updates slug/order, logs, returns updated info.
- **Alternative**: auto-update slug, toggle active state, reorder categories.
- **Exception**: duplicate name, DB error, missing permission.
- **Rules/NFR**:
  - Name unique case-insensitively.
  - Update <1s; track updated timestamp.

---

## 2. Backend Tasks (status)

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C3-BE-DTO-01 | `UpdateCategoryRequestDto` | `dto/request/update-category.dto.ts` | ✅ | Includes name, description, order, isActive |
| C3-BE-VLD-01 | `updateCategoryValidator` | `validators/admin-categories.validator.ts` | ✅ |
| C3-BE-SVC-01 | `AdminCategoriesService.updateCategory` (unique name, slug regen, payload) | `services/admin-categories.service.ts` | ✅ |
| C3-BE-REP-01 | Repository helpers `isNameTaken`, `isSlugTaken`, `update` | `repositories/admin-categories.repository.ts` | ✅ |
| C3-BE-CTL-01 | Controller route `PUT /admin/categories/:id` | `controllers/admin-categories.controller.ts`/`routes.ts` | ✅ |
| C3-BE-CC-01 | Audit log `CATEGORY_UPDATE` | admin audit service | ⬜ |
| C3-BE-OPT-01 | Allow slug auto-update toggle | service/controller | ⬜ | If slug changes should be optional |

Testing:
| Task | File | Status |
|------|------|--------|
| Unit tests (duplicate name, slug change) | `services/__tests__/admin-categories.service.spec.ts` | ⬜ |
| Integration test `PUT /admin/categories/:id` | `tests/integration/admin/categories.update.spec.ts` | ⬜ |

---

## 3. Frontend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C3-FE-TYP-01 | `UpdateCategoryPayload` | `fe-web/src/types/admin-categories.types.ts` | ⬜ |
| C3-FE-VLD-01 | Client validator | `fe-web/src/validators/admin-categories.validator.ts` | ⬜ |
| C3-FE-SVC-01 | `adminCategoriesApi.update(id, payload)` | `fe-web/src/services/admin-categories.service.ts` | ⬜ |
| C3-FE-HOOK-01 | `useEditCategoryController` | `fe-web/src/hooks/admin-categories/useEditCategoryController.ts` | ⬜ |
| C3-FE-CMP-01 | `CategoryEditForm` (prefilled) | `fe-web/src/components/admin/categories/CategoryEditForm.tsx` | ⬜ |
| C3-FE-CMP-02 | Inline edit (optional) | `CategoriesTable` | ⬜ |
| C3-FE-TST-01 | Component/hook tests | `fe-web/src/components/admin/categories/__tests__/...` | ⬜ |

---

## 4. Cross-cutting
| Task | Status | Notes |
|------|--------|-------|
| Audit log integration | ⬜ | log adminId, old → new name |
| Copy deck for success/error | ⬜ |
| Analytics event for edits | ⬜ |

---

## 5. QA Checklist
- [ ] Duplicate name blocked with friendly message.
- [ ] Slug updates when name changes (or optional toggle).
- [ ] Updated category reappears correctly in list.
- [ ] Permission enforcement (`Category.Update` required).
- [ ] Tests cover success/error cases.

---

## 6. Assumptions
1. Changing name regenerates slug automatically (existing behavior).
2. Order field optional; FE may expose reorder UI later.
3. Audit logs & analytics not implemented yet but planned.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist drafted |


