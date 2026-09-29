# Code Standards: SiteView Pro

## General

- Keep modules small, focused, and single-purpose.
- All hardware sensor interactions must pass through filter pipelines (Kalman/Complementary filter) to prevent UI reticle jitter.
- Avoid introducing third-party libraries that require mandatory paid subscriptions or feature automatic database sleeping/pausing.

## TypeScript

- Strict mode is required throughout the project (`tsc --noEmit` must pass with zero errors).
- Explicitly define interfaces for all data state representations (`JobDiscipline`, `EnvironmentalTelemetry`, `MaterialFormulation`, `JobStatus`).
- Avoid `any` type annotations under all circumstances.

## React & Next.js

- Default to React Server Components where applicable; use `'use client'` explicitly for HUD controls, WebGL viewports, and Web Worker hooks.
- Keep camera frame consumption and WebGL loops bound to requestAnimationFrame / Web Workers to prevent UI frame drops.

## Styling

- Use Tailwind CSS tokens and the CSS variables defined in `ui-context.md`.
- Enforce `48x48dp` touch targets and overflow safeguards (`max-h-[90vh]`, `overflow-y-auto`) to prevent button truncation on small screens.

## Data & Storage

- Always perform offline writes to IndexedDB (`siteview_estimator_input_draft_v1`) before initiating network sync calls.
- Honor job state machine transition guards (`DRAFT` -> `SUBMITTED` -> `LOCKED_IN_PRODUCTION`); reject offline updates if the state is `LOCKED_IN_PRODUCTION`.

## File Organization

- `src/app/` — Next.js pages, API route handlers, layout definitions.
- `src/components/hud/` — Camera viewfinder, AR controls, job certification modals.
- `src/components/neural3d/` — Three.js canvas, point cloud visualizers, measurement rulers.
- `src/workers/` — Web Worker source files (`cv.worker.ts`).
- `src/lib/engine/` — Weather equation engines, formulation scrapers, and calculation utilities.
- `src/lib/db/` — IndexedDB database initialization and sync logic.
