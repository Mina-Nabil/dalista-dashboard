# Dalista Dashboard — Agent Instructions

Admin dashboard for Dalista, a shopping-list / shop-catalog sharing app. This file is the standing context for any Claude Code session working in this repo — read it before starting any ticket.

## Starting point — the Vela template

This repo is built on **Vela Dashboard**: React 19 + TypeScript + Vite + Tailwind CSS 4. A copy of the original template lives at `src/template/vela-dashboard` in the connected Dalista project folder — look there for anything this repo doesn't have yet or to compare against the original.

- No heavy chart library — charts (`AreaLineChart`, `BarChart`, `DonutChart`, `FunnelChart`, `GanttChart`, `Gauge`, `Heatmap`, `MapPins`, `Sparkline`) are hand-built SVG components under `src/components/charts/`.
- A full UI kit exists under `src/components/ui/`: Button, Card, DataTable, Modal, Drawer, Dropdown, Tabs, TabNav, Pagination, ProgressBar, StatCard, Timeline, Toast, Tooltip, Badge, Avatar, Accordion, EmptyState, Rating, Skeleton, Popover, Breadcrumbs, PageHeader, form primitives, Kanban.
- Routing via `react-router-dom` v7 — `router/paths.ts` + `router/router.tsx`, lazy-loaded pages (`lib/lazyPage.ts`).
- The template ships with example dashboard pages for unrelated domains (SaaS, Sales, CRM, HR, Finance, Logistics, E-commerce, etc.) with mock data in `src/data/*.ts`. These are reference material for component usage, not something to keep — Phase 0 strips them out.
- Sidebar/nav is centralized in `layout/nav-config.ts`.

**Directive: reuse, don't rebuild.** Build every Dalista screen from the existing `components/ui/*`, `components/charts/*`, and layout shell (`AppShell`, `Sidebar`, `Topbar`). Do not introduce a second component library or restyle from scratch — if a screen seems to need something the kit doesn't have, check the full template at `src/template/vela-dashboard` before reaching for a new dependency.

## Backend

The dashboard calls the same Dalista Golang API directly, as an admin-scoped client authenticated against the backend's permission system (admin role + permission strings like `admin:send_sms`, `categories:manage`). There is no separate dashboard-specific backend.

## v1 scope

Beyond basic data visibility, admin functions are intentionally minimal in v1:

1. **Category-interest export** — which users are interested in which categories (categories of items/lists a user owns, broadened in Phase 7 to also include starred public lists).
2. **Usage over time** — per-user or aggregate usage metrics (exact metric set — sessions, items added, lists created, storage — still open; check with the team before hardcoding a definition).
3. **New-user growth over time** — signup time-series using `AreaLineChart`/`BarChart`.
4. **Admin-initiated SMS** — compose and send an SMS to one or more selected users, via the backend's SMS Misr integration. This is the one functional (non-display) admin capability confirmed for v1.
5. Category management (add/edit/deprecate), storage usage monitoring, subscription oversight across both billing rails (Apple IAP / Google Play), and user suspend/reactivate round out Phase 11.

No public-list moderation queue in v1 — public list creation is open to any user. No email channel anywhere in the product, so nothing here sends email either.

## Known open items (don't block on these, but flag rather than silently guessing)

1. Precise definition of "interested in a category" for the export.
2. Which usage metrics to chart, and at what time granularity (daily/weekly/monthly) and default date range.
3. Export format — assumed CSV, not yet confirmed.

## Where the full spec lives

The complete requirements digest, module breakdown, technical architecture doc, and phased build plan live in the "Claude outputs" folder of the Dalista project, and the full ticket backlog is in Linear (team: Dalista, projects: API / Mobile App / Dashboard, labeled Phase 0 through Phase 12). When a ticket's one-line description isn't enough context, check those docs before guessing.
