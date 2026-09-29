# Architecture Context: SiteView Pro (Walnus SmartView Pro)

## Stack

| Layer | Technology | Role |
| --- | --- | --- |
| **Framework** | Next.js (App Router) + TypeScript | Core PWA web application frame, routing, and serverless API endpoints. |
| **UI & Tactical HUD** | Tailwind CSS + Lucide React + Framer Motion (`motion/react`) | Touch-first mobile HUD (48x48dp targets), dark glassmorphism layout, and gesture-driven panels. |
| **Edge Vision & Graphics** | WebGL / WebGPU + Web Workers (`OffscreenCanvas`) | Off-thread CPU/GPU image frame processing, Sobel edge extraction, and real-time reticle rendering. |
| **3D Rendering** | Three.js + `@react-three/fiber` | Client-side rendering of point clouds, spatial bounding boxes, and optimized `.splat`/`.glb` meshes. |
| **AI Orchestration** | Google Gemini API (via AI Studio) | Multimodal spatial interpretation, automated QA audits, and stakeholder summary generation. |
| **Auth & Workspace** | Google Identity (OAuth 2.0) + Google Workspace APIs | Zero-vendor-lock authentication, identity verification, and direct exports to Docs, Sheets, and Drive. |
| **Database & Caching** | Local IndexedDB (`idb`) + Cloudflare D1 (SQLite) | Offline-first local draft storage with zero-maintenance, paywall-free cloud synchronization. |
| **Storage & Compute** | Cloudflare R2 + Cloudflare Workers | Cost-effective object storage (zero egress fees) for 3D assets and photogrammetric worker queues. |

## System Boundaries

- `src/app` — Handles Next.js App Router layout, discipline routing, and serverless backend API handlers.
- `src/components/hud` — Holds mobile-first Tactical HUD components (`CameraViewfinder`, AR overlays, `WeatherCopilotSchedulerPanel`).
- `src/components/neural3d` — Manages WebGL viewports, Three.js canvas instances, and point cloud mesh rendering (`Neural3DGenerator`).
- `src/workers` — Web Worker scripts (`cv.worker.ts`) handling off-thread Sobel calculation and frame buffer operations.
- `src/lib/engine` — Mathematical equation engines (`weatherEquationEngine.ts`), material ratio calculators, and chemical formulation scrapers.
- `src/lib/db` — Local-first IndexedDB cache (`db.ts`) and state-machine sync logic (`DRAFT` -> `SUBMITTED` -> `LOCKED_IN_PRODUCTION`).

## Storage Model

- **Local Device Storage (IndexedDB via `db.ts`)**: Primary offline-first cache (`siteview_estimator_input_draft_v1`), saving spatial measurements, weather inputs, and formulation parameters locally.
- **Relational Cloud Metadata (Cloudflare D1)**: Stores transactional records, job status transitions, user permissions, and factory batch queue logs.
- **Blob & Asset Storage (Cloudflare R2)**: Holds captured image matrices, dense point clouds, exported quotation packages, and `.splat`/`.glb` 3D reconstruction files.

## Auth and Access Model

- Every user authenticates via Google OAuth 2.0 to grant native access to Google Workspace exports (Sheets, Docs, Drive).
- Operators certify their operational discipline (`PAINT`, `SCREED`, `PLASTER`) via an initiation gate to unlock trade-specific tools.
- Optimistic Concurrency Control (OCC): Field estimators edit local drafts freely (`DRAFT`), but once a job is submitted (`SUBMITTED`) and locked into production (`LOCKED_IN_PRODUCTION`), offline client overrides are rejected.

## Invariants

1. Main JS thread dispatches must remain under 150ms; heavy computer vision and 3D processing must run off-thread via Web Workers or WebGL fragment shaders.
2. Infrastructure must rely on open-source or pay-as-you-go serverless models without free-tier auto-pause or database lockouts.
3. All site inputs, surface dimensions, and formulation drafts must instantly persist to IndexedDB prior to any network dispatch.
4. Job status transitions must proceed monotonically (`DRAFT` -> `SUBMITTED` -> `QUEUED` -> `LOCKED_IN_PRODUCTION` -> `COMPLETED`).
