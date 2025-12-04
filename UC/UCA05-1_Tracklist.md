# Tracklist · UCA05-1 “Xem báo cáo, thống kê”

> **Scope**: Admin dashboard showing KPIs, charts, filters, drill-down, export, saved filters, built on pre-aggregated analytics data sources.

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- KPIs: total users, new users per period, total recipes, new recipes, approval rate, engagements (views/likes/comments).
- Charts: user growth line, recipes per category bar, recipe status pie, activity heatmap.
- Filters: time ranges (7/30/custom), category, user role, recipe status; persist favorites.
- Interactions: drill-down on KPI/chart to fetch detail list; export (CSV/PDF) current filter; error-empty states.
- Optional flows: custom reports builder, scheduled emails, realtime stats.

---

## 2. Backend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A51-BE-ARCH-01 | Define analytics architecture (aggregations, cache) | `DOCS/ARCH/analytics-dashboard.md` | ⬜ |
| A51-BE-DTO-01 | KPIs response DTO (`DashboardResponseDto`, chart series) | `src/modules/admin/analytics/dto/response/dashboard-response.dto.ts` | ✅ |
| A51-BE-DTO-02 | Filter DTO (`DashboardQueryDto`, `DrillDownQueryDto`) | `dto/request/dashboard-query.dto.ts` | ✅ |
| A51-BE-VLD-01 | Joi validator for query params (range validation, enums) | `validators/admin-analytics.validator.ts` | ✅ |
| A51-BE-REP-01 | Repository for aggregated metrics (Prisma/raw SQL) | `repositories/admin-analytics.repository.ts` | ✅ |
| A51-BE-SVC-01 | Service orchestrating KPI fetch, charts, caches | `services/admin-analytics.service.ts` | ✅ | Includes cache + KPI/charts build |
| A51-BE-SVC-02 | Drill-down data provider (recipes/users detail) | `services/admin-analytics.service.ts` + repository helpers | ✅ |
| A51-BE-CTL-01 | Controller endpoints: `/dashboard`, `/drill-down` | `controllers/admin-analytics.controller.ts` | ✅ |
| A51-BE-RT-01 | Routes with `Report.Read` guard | `routes.ts` | ✅ |
| A51-BE-CC-01 | Saved filter repository/service | reuse saved filters module or new | ⬜ |
| A51-BE-CC-02 | Export endpoints (CSV/PDF) and generator utilities | `services/export.service.ts` + `/export` route | ✅ |
| A51-BE-CC-03 | Scheduler to refresh aggregates (CRON/worker) | background job | ⬜ |
| A51-BE-OBS-01 | Metrics logging (dashboard load time) | instrumentation | ⬜ |

Testing:
| Task | Status | Notes |
|------|--------|-------|
| Unit tests for aggregation service (time filter) | ⬜ |
| Integration tests for `/dashboard/summary` | ⬜ |
| Load test baseline (<3s) | ⬜ |

---

## 3. Frontend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A51-FE-TYP-01 | Dashboard types (KPI, chart series) | `fe-web/src/types/analytics-dashboard.types.ts` | ⬜ |
| A51-FE-VLD-01 | Filter form validation (date range) | `fe-web/src/validators/analytics-dashboard.validator.ts` | ⬜ |
| A51-FE-SVC-01 | Gateway `analyticsApi.getSummary(filters)` etc. | `fe-web/src/services/analytics-dashboard.service.ts` | ⬜ |
| A51-FE-HOOK-01 | Hook `useDashboardData` handles filters, caching, error states | `fe-web/src/hooks/analytics/useDashboardData.ts` | ⬜ |
| A51-FE-CMP-01 | KPI cards component | `fe-web/src/components/analytics/KpiCards.tsx` | ⬜ |
| A51-FE-CMP-02 | Charts (line/bar/pie/heatmap) with lazy loading | `fe-web/src/components/analytics/*.tsx` | ⬜ |
| A51-FE-CMP-03 | Filter toolbar + saved filters UI | `fe-web/src/components/analytics/DashboardFilters.tsx` | ⬜ |
| A51-FE-CMP-04 | Drill-down modal/table | `fe-web/src/components/analytics/DrillDownModal.tsx` | ⬜ |
| A51-FE-CMP-05 | Export dropdown (CSV/PDF) | ⬜ |
| A51-FE-CMP-06 | Empty/error states components | ⬜ |
| A51-FE-TST-01 | Component & hook tests | ⬜ |

---

## 4. Cross-cutting / Data
| Task | Status | Notes |
|------|--------|-------|
| Data warehouse / pre-aggregation pipelines | ⬜ |
| Permission guard on dashboard route (Report.Read) | ✅ | Implemented via router-level middleware |
| Saved filter storage reuse from UC-A1 | ⬜ |
| Audit log for exports and drill-down access | ⬜ |
| Alerting when analytics fails (fallback message) | ⬜ |

---

## 5. QA Checklist
- [ ] KPIs match DB counts for selected period.
- [ ] Filters persist & reload correctly.
- [ ] Drill-down respects same filters and pagination.
- [ ] Export file reflects current filters and includes timestamp.
- [ ] Dashboard loads within 3s for aggregated data.
- [ ] Unauthorized users blocked from `/dashboard`.

---

## 6. Assumptions
1. Aggregated data stored in dedicated tables (daily granularity) refreshed hourly.
2. Export PDF uses existing reporting service; CSV generated on the fly.
3. Saved filters reuse admin saved-filter infrastructure; no separate implementation needed.
4. Scheduling/report builder tracked separately; not in first sprint.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist |


