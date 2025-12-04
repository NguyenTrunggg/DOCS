# Tracklist · UCA03-4 “Xóa danh mục”

> **Scope**: Admin deletes categories with confirmation, ensuring recipes referencing the category are either transferred or blocked; supports optional soft delete & batch delete.

> **Architecture**: Leverage admin categories module (Layered API + FE separation).

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic**: Admin selects category → system checks recipe references → if none, confirm → delete → refresh list.
- **Alternative**: If recipes exist, require transfer to another category; optional soft delete (hide but retain data); batch delete.
- **Exception**: lacking permission (`Category.Delete`), category not found, DB constraint failure.
- **Rules/NFR**:
  - Cannot delete when recipes still link without transfer.
  - Audit action with metadata.
  - Keep referential integrity.

---

## 2. Backend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C4-BE-DTO-01 | `DeleteCategoryRequestDto` (transfer target) | `dto/request/delete-category.dto.ts` | ✅ | Already includes `transferToCategoryId` |
| C4-BE-VLD-01 | `deleteCategoryValidator` | `validators/admin-categories.validator.ts` | ✅ |
| C4-BE-SVC-01 | `AdminCategoriesService.deleteCategory` handles transfer logic | `services/admin-categories.service.ts` | ✅ | Already checks recipe count and transfers |
| C4-BE-REP-01 | Repository helpers (`countRecipesByCategory`, delete) | `repositories/admin-categories.repository.ts` | ✅ |
| C4-BE-CTL-01 | Controller route `DELETE /admin/categories/:id` | `controllers/...` + `routes.ts` | ✅ |
| C4-BE-CC-01 | Audit log entry `CATEGORY_DELETE` with metadata | admin audit service | ⬜ |
| C4-BE-OPT-01 | Batch delete endpoint (optional) | new | ⬜ |
| C4-BE-OPT-02 | Soft delete flag (optional) | repository/service | ⬜ |

Testing:
| Task | Status | Notes |
|------|--------|-------|
| Unit tests for delete service (transfer vs block) | ⬜ | `services/__tests__` |
| Integration tests for delete endpoint | ⬜ | `tests/integration/admin/categories.delete.spec.ts` |

---

## 3. Frontend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C4-FE-TYP-01 | `DeleteCategoryPayload` type | `fe-web/src/types/admin-categories.types.ts` | ⬜ |
| C4-FE-VLD-01 | Client validator (transfer selection) | `fe-web/src/validators/admin-categories.validator.ts` | ⬜ |
| C4-FE-SVC-01 | `adminCategoriesApi.delete(id, payload)` | `fe-web/src/services/admin-categories.service.ts` | ⬜ |
| C4-FE-HOOK-01 | `useDeleteCategory` hook | `fe-web/src/hooks/admin-categories/useDeleteCategory.ts` | ⬜ |
| C4-FE-CMP-01 | `DeleteCategoryDialog` | `fe-web/src/components/admin/categories/DeleteCategoryDialog.tsx` | ⬜ | Handles transfer selection |
| C4-FE-CMP-02 | Recipe usage warning list | same namespace | ⬜ |
| C4-FE-CMP-03 | Batch delete UI (optional) | same | ⬜ |
| C4-FE-TST-01 | Component/hook tests | ⬜ |

---

## 4. Cross-cutting
| Task | Status | Notes |
|------|--------|-------|
| Audit logging integration | ⬜ | Log adminId, transfer target, recipe count |
| Copy deck for confirm dialogs | ⬜ |
| Analytics event for delete | ⬜ |

---

## 5. QA Checklist
- [ ] API blocks delete if recipes exist without transfer.
- [ ] Transfer logic moves recipes correctly.
- [ ] FE displays warning & enforces transfer selection.
- [ ] Permissions enforced (`Category.Delete`).
- [ ] Audit log entries recorded.
- [ ] Optional soft delete/batch delete tested if implemented.

---

## 6. Assumptions
1. Hard delete (remove record) is current behavior; soft delete optional.
2. Transfer target must exist & differ from source.
3. Batch delete, soft delete future enhancements.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist created |


