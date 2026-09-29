SiteView Pro (Walnus SmartView Pro) - System Specification & Architecture Overview
1. Executive Summary & Vision
SiteView Pro (Walnus SmartView Pro) is an enterprise-grade, mobile-first, full-stack application built for industrial and commercial structural estimation, surface coating analysis, material formulation, and field-to-factory synchronization.
The platform bridges physical site measurements with industrial chemical production by integrating real-time computer vision, device sensor telemetry (GPS, compass, accelerometer), photogrammetric 3D surface reconstruction, and automated chemical supplier data integration with factory mixer routing.
2. Core Goals & Technical Objectives
 * Absolute Measurement Precision: Deliver sub-millimeter accurate surface area, volume, and material coating calculations (Paint, Wall Screed, Wall Plaster) driven by real-time computer vision and sensor fusion.
 * Mobile-First Tactical HUD (HUD v2): Provide a clutter-free, responsive interactive workspace optimized for single-handed mobile operation in field conditions (min 48x48dp touch targets, full-bleed 100vw/100vh viewport canvas, collapsible toolbars).
 * Environmental Formulation Engine: Automatically recalculate material viscosity, curing time, and required coats based on live site weather telemetry (Temperature, Humidity, Wind Speed, Porosity via the Weather Copilot).
 * Chemical Supplier & Factory Sync: Map scanned surface metrics directly to chemical ingredient breakdowns (Binders, Pigments, Solvents, Additives), match local material suppliers (BASF, Dow, Evonik, Covestro), and seamlessly transmit field-approved formulas directly to the factory mixer queue with real-time stock tracking.
 * Zero-Latency Performance & Offloading: Execute camera edge detection via WebGPU/WebGL shaders or OffscreenCanvas Web Workers to preserve main UI thread dispatches under 150ms.
 * Stakeholder Collaboration: Integrate embedded chat, Google Workspace synchronization, and video conferencing to keep clients, field engineers, and factory operators aligned.
3. System Architecture & Component Mapping
+-----------------------------------------------------------------------------------+
|                                  SiteView Pro                                     |
|                       App.tsx (Root Header & Routing)                              |
+-----------------------------------------------------------------------------------+
                                          |
          +-------------------------------+-------------------------------+
          |                                                               |
+-------------------+                                           +-------------------+
|   EstimatorApp    | (Field Estimator Console)                 |     MixerApp      | (Factory Terminal)
+-------------------+                                           +-------------------+
  |-- CameraViewfinder (Computer Vision & Sensors)                |-- Batch Queue Reorder
  |-- Neural3DGenerator (Point Clouds & Cloud Meshes)             |-- D3 Inventory Charts
  |-- WeatherCopilotSchedulerPanel (Site Weather Engine)          |-- Threshold Warnings
  |-- ChemicalAnalysisSupplierHUD (Formulation Engine)            |-- Supplier GPS Map
  |-- MarketIntelligenceHUD (Commodity Pricing)
  |-- AnalysisRoutingView (Discipline Switcher)

3.1. AI Copilot & Orchestration (AICopilotDrawer / ImmersiveJobCertificationModal)
 * Pre-Initiation Gate: Requires operators to certify their job type (Paint, Screed, Plaster) before unlocking the workspace, calibrating estimation algorithms to the specific discipline.
 * Autonomous Agents: Specialized AI sub-agents capable of running QA audits, generating stakeholder summaries, and analyzing spatial discrepancies.
3.2. Communication & Workspace (ChatSystem, VideoConferenceModal)
 * Encrypted Channels: Real-time text channels organized by site, formulation, and logistics.
 * Live Video Connect: WebRTC-ready interface for instant visual validation of site conditions with remote supervisors.
 * Export & Sync: Google Workspace integration for exporting specifications to Sheets/Docs and driving formal stakeholder sign-offs.
4. Primary Functional Modules
4.1. Camera Viewfinder & AR Spatial Engine (CameraViewfinder.tsx)
 * Project Execution Modes: Context-switching between Architectural Paint (volume/gallons), Wall Screed (thickness/bags), and Wall Plaster (depth/bags).
 * Full-Bleed Viewport Canvas: Rendered at 100vw x 100vh (fixed inset-0 z-0) underneath floating glassmorphic HUD controls.
 * Aspect Ratio & Zoom Controls: Continuous 1.0x to 5.0x digital zoom slider, 4-way direction pan joystick, and framing toggles (FULL, 16:9, 4:3, 1:1).
 * Hardware-Accelerated Frame Processing: Offloads Sobel edge extraction and target corner locking off the main thread to Web Workers via OffscreenCanvas or WebGL fragment shaders to maintain UI responsiveness.
 * Filtered Sensor Telemetry: Integrates GPS, pitch/roll, and compass heading with a Complementary/Kalman filter to prevent reticle jitter during target lock.
 * Immersive HUD Mode: Single-tap toggle hides non-essential telemetry overlays to clear the viewport during active scans.
4.2. Neural 3D Reconstruction Studio (Neural3DGenerator.tsx)
 * Hybrid Photogrammetric Pipeline: Captures image matrices and low-density point clouds on-device. Offloads dense Poisson Reconstruction, Gaussian Splatting, and NeRF rendering to asynchronous server/cloud workers, streaming optimized .splat or .glb models back to the WebGL client.
 *  3D Manipulation: Orbit, Pan, Zoom, and Perspective View Presets (3D, Front, Side, Top).
 * Measurement HUD: Toggleable 3D dimension ruler lines, wireframe mesh grids, and adjustable lighting presets (Daylight, Tactical, Sunset, Studio).
 * Fullscreen Portal: Detached full-screen view with contained, scrollable toolbars (max-w-[calc(100vw-2rem)], max-h-[calc(100vh-8.5rem)]) preventing button cutoff on mobile viewports.
4.3. Weather Copilot & Site Equation Engine (WeatherCopilotSchedulerPanel.tsx, weatherEquationEngine.ts)
 * Site Parameter Inputs: Captures surface area (sq ft / sq m), coating key (acrylic_latex, epoxy_coating, polyurethane, screed, plaster), quality grade, coats count, porosity (low, medium, high), and application method (airless_spray, roller, trowel).
 * Environmental Telemetry: Evaluates ambient temperature (°C), relative humidity (%), wind speed (km/h), and barometric pressure.
 * Mathematical Output: Calculates total volume/weight required, recommended dry/wet film thickness (DFT/WFT in microns), curing window duration, and viscosity adjustment index.
4.4. Chemical Analysis & Supplier Engine (ChemicalAnalysisSupplierHUD.tsx)
 * Formulation Breakdown: Scrapes industrial formulation databases to generate chemical ingredient ratios across Binders, Pigments, Solvents, Fillers, and Additives.
 * Local Supplier Integration: Matches scanned material specifications with local supplier inventory datasets (e.g., BASF, Dow, Covestro, Clariant, Evonik, Huntsman).
 * Cost & Location Optimization: Ranks material blends based on geographic proximity to the construction site, lead time, and regional market commodity pricing.
4.5. Factory Mixer Terminal (MixerApp.tsx)
 * Production Queue: Manages incoming field estimation payloads and allows factory operators to prioritize, batch, and execute production orders.
 * Raw Material Inventory Matrix: Real-time visual tracking of chemical stock levels with D3 data visualizers (RawMaterialsInventoryD3Chart.tsx).
 * Safety Alerts: Triggers automated warnings when raw inventory levels fall below critical operational thresholds.
5. UI/UX Design System Directives (HUD v2)
 * Tactical Workspace Palette:
   * Backgrounds: bg-slate-950, bg-slate-900/90 with backdrop-blur-md.
   * Borders: Hairline border-slate-800/80 with glow accents (border-indigo-500/50, border-emerald-500/50).
   * Accents: Material 3 dark tokens featuring Night Vision Green (#10b981), Deep Indigo (#6366f1), Tactical Red (#f43f5e), and Construction Amber (#f59e0b).
 * Standard Modal & Drawer Architecture:
   * Zero Draggable Handles: Replaced custom drag/swipe handles with standard, clean, enterprise modal dialogs and slide-over side sheets.
   * Header Controls: Clean title banners with category badges and explicit X close buttons.
 * Mobile Touch Target Scale:
   * Minimum hitboxes: 48x48dp for main actions; w-8 h-8 to w-10 h-10 for compact floating controls.
   * Mobile Viewport Bounds: All floating panels use max-w-full, max-h-[90vh], and overflow-x-auto to eliminate button truncation.
6. Data & Synchronization Model
 * Offline-First Storage (db.ts): Structured IndexedDB/LocalStorage layer using key siteview_estimator_input_draft_v1 to persist project drafts, surface parameters, and weather inputs during connectivity drops.
 * Optimistic Concurrency & State Locking: Implements explicit state machines (DRAFT -> SUBMITTED -> LOCKED_IN_PRODUCTION). Local offline updates to drafts are rejected if the job state has already been locked by the factory mixer terminal.
 * Google Workspace Integration: Direct export of field specifications and quotation packages to Google Sheets, Google Docs, and Drive.
7. Quality Assurance & Code Standards
 * TypeScript Strictness: 100% type safety with zero warnings (tsc --noEmit).
 * Production Build: Bundled server entry point (dist/server.cjs) compiled via esbuild and Vite SPA client output.
 * Fluid Motion: Physics-based layout transitions powered by motion/react.

Here is an ordered, step-by-step implementation roadmap designed specifically for coding agents to execute the SiteView Pro spec without architectural conflicts:
 ==* Set Up Project Architecture & Type Definitions
   * Configure Vite SPA client and dist/server.cjs Node/Express server entry points compiled via esbuild.
   * Enforce strict TypeScript (tsc --noEmit) and establish shared interfaces for core state entities (ProjectExecutionMode, EnvironmentalTelemetry, ChemicalFormulation, JobStatus).
 * Implement Off-Thread Computer Vision & Sensor Pipeline
   * Build CameraViewfinder.tsx with full-bleed layout (100vw x 100vh) and aspect ratio/zoom controls.
   * Offload continuous frame edge detection (Sobel) and corner locking to a Web Worker via OffscreenCanvas or WebGL fragment shaders to maintain UI dispatches under 150ms.
   * Integrate GPS, compass heading, and accelerometer telemetry passing through a Kalman/Complementary filter before updating HUD reticles.
 * Construct Off-Thread Neural 3D Photogrammetry Studio
   * Build Neural3DGenerator.tsx with orbit/pan/zoom controls and toggleable 3D measurement rulers.
   * Create an asynchronous client-to-cloud API bridge that captures image matrices locally and sends them to server workers for NeRF, Poisson, and Gaussian Splatting processing, streaming back .splat or .glb files for WebGL rendering.
 * Build Site Telemetry & Formulation Engines
   * Implement weatherEquationEngine.ts to process surface parameters, coating keys, porosity, and live weather inputs (temperature, humidity, wind).
   * Program WeatherCopilotSchedulerPanel.tsx to compute total volume/weight, wet/dry film thickness (WFT/DFT), curing duration, and viscosity adjustment indices.
 * Build Chemical Supplier & Market Intelligence HUD
   * Build ChemicalAnalysisSupplierHUD.tsx to map required material volumes to chemical ingredient ratios (Binders, Pigments, Solvents, Additives).
   * Implement local supplier matching (BASF, Dow, Evonik, Covestro) ranked by geographic distance, lead time, and current commodity pricing.
 * Develop Factory Mixer Terminal & State-Locked Sync
   * Implement MixerApp.tsx featuring production queue reordering and raw material stock tracking using RawMaterialsInventoryD3Chart.tsx.
   * Program automated threshold warnings for low inventory levels.
 * Implement Offline-First Storage & State Machine Guardrails
   * Set up IndexedDB/LocalStorage persistence in db.ts under key siteview_estimator_input_draft_v1 for offline estimation drafts.
   * Implement the job state machine (DRAFT -> SUBMITTED -> LOCKED_IN_PRODUCTION) to reject offline client overwrites once a job is locked in production.
 * Build HUD v2 Design System & Pre-Initiation Gates
   * Apply slate/emerald/indigo/amber tactical color tokens, backdrop-blur-md panels, and minimum 48x48dp touch targets across all mobile viewports.
   * Integrate AICopilotDrawer / ImmersiveJobCertificationModal pre-initiation gate requiring job certification (Paint, Screed, Plaster) before unlocking the workspace.
 * Integrate Real-Time Workspace & External Exports
   * Build ChatSystem and WebRTC-based VideoConferenceModal for field-to-factory communication.
   * Connect Google Workspace APIs for automated exports to Google Sheets, Google Docs, and Drive.


Here is the integrated, combined user flow and UI/business logic specification. It blends the physical, on-site reality for field workers (painters, screeders, plasterers) with the necessary technical safeguards (Web Workers, state-machine locking, offline caching) into one master execution blueprint:
SiteView Pro — Integrated User Flow & Technical Specification
1. Primary User Flow Overview
[1. Job Certification & Calibration]
                 │
                 ▼
[2. Capture, Alignment & Sensor Telemetry]
                 │
                 ▼
[3. Off-Thread CV & 3D Surface Reconstruction]
                 │
                 ▼
[4. Surface Isolation & Discipline Execution]
                 │
                 ▼
[5. Environmental Formulation & Labor Engine]
                 │
                 ▼
[6. Quiet Background Intelligence & Supplier Sync]
                 │
                 ▼
[7. State Machine Lock & Stakeholder Export]

Step 1: Pre-Initiation Certification Gate
 * Action: The worker launches the app and certifies their active job discipline (Architectural Paint, Wall Screed, or Wall Plaster) via the ImmersiveJobCertificationModal.
 * Logic: The choice calibrates calculation formulas, target thickness profiles, and UI HUD controls specifically to that trade.
Step 2: Camera Capture, AR Alignment & Sensor Telemetry
 * Action: The worker points the mobile camera at the structure.
 * Logic: The full-bleed camera HUD (100vw x 100vh) overlays semi-transparent tactical grids, crosshairs, and dynamic alignment lines. Device GPS, compass heading, and pitch/roll sensors capture geolocation and scale, passing telemetry through a Kalman filter to eliminate reticle jitter.
Step 3: Off-Thread CV & 3D Surface Reconstruction
 * Action: The worker sweeps the device across the target area to capture spatial boundaries.
 * Logic: Frame processing (Sobel edge extraction and corner locking) is handled off the main thread via Web Workers (OffscreenCanvas) or WebGL fragment shaders to maintain UI dispatches under 150ms. Image matrices generate a low-density point cloud locally, while dense NeRF/Poisson/Gaussian Splatting reconstruction is processed asynchronously by cloud workers to stream back optimized .splat/.glb models.
Step 4: Surface Isolation & Discipline Execution
 * Action: The worker views the textured 3D mesh model overlaid at the precise geographic coordinate.
 * Logic: Using single-tap raycasting, the worker taps individual building faces or walls to isolate surface areas and apply material properties (coatings, textures, porosity grade).
Step 5: Environmental Telemetry & Labor Estimation
 * Action: The worker specifies the number of coats, surface porosity, and application method (airless spray, roller, trowel).
 * Logic: The weatherEquationEngine.ts pulls live site weather data (temperature, humidity, wind) to instantly calculate total required material volume/weight (gallons or bags), recommended wet/dry film thickness (WFT/DFT), curing time, and viscosity adjustments. The business logic automatically factors in customizable workmanship multipliers and labor rates.
Step 6: Quiet Background Intelligence & Supplier Sync
 * Action: The system automatically cross-references material ratios with local supplier inventory (e.g., BASF, Dow, Evonik, Covestro) without interrupting the worker.
 * Logic: Scheduled background agents scrape local commodity pricing and rank supplier options based on geographic proximity, lead time, and current market rates.
Step 7: State-Locked Sync & Stakeholder Export
 * Action: The worker submits the job estimate or generates an interactive export link.
 * Logic: Local drafts cached in IndexedDB (siteview_estimator_input_draft_v1) transition from DRAFT to SUBMITTED. Once ingested and approved by the factory terminal (MixerApp), the job state locks to LOCKED_IN_PRODUCTION, preventing offline client overwrites. Stakeholders receive an interactive 3D link with orbit/zoom controls and live color/cost recalculations.
2. UI Layout Architecture
 * Camera HUD Layer:
   * Full-bleed native viewport (100vw x 100vh) with dark tactical palette (bg-slate-950, Night Vision Green #10b981, Construction Amber #f59e0b).
   * Floating, collapsible toolbars housing mode selectors, digital zoom sliders (1.0x–5.0x), direction joysticks, and quick-access hardware toggles with minimum 48x48dp touch targets.
 * 3D Reconstruction Viewer Screen:
   * Interactive 3D viewport supporting gesture controls (pinch-to-zoom, orbital rotation, 3D/Front/Side/Top view presets).
   * Side-anchored material property drawer for selecting colors, finishes, and toggling 3D dimension ruler lines.
 * Admin & Formulation Panel:
   * Card-based dashboard with form inputs and sliders for adjusting workmanship multipliers, raw material mix ratios, and factory synchronization triggers.
   * Direct D3 visualizers (RawMaterialsInventoryD3Chart.tsx) for factory operators to track chemical stock levels in real time.
3. Business Logic & Processing Standards
 * Formula Processing: Surface square footage is evaluated against discipline coverage coefficients, quality grades, and environmental weather factors, adding labor costs derived from the dynamic workmanship variable.
 * Offline-First Storage: Local database layer (db.ts using IndexedDB) manages immediate offline data caching and queued updates.
 * Optimistic Concurrency Control: State machine guardrails (DRAFT \rightarrow SUBMITTED \rightarrow LOCKED_IN_PRODUCTION) automatically reject offline sync attempts if a factory operator has locked the batch in production.
 * Non-Blocking Background Scraping: Scheduled workers run queries for supplier pricing and inventory updates off the main UI thread.

Here is a concise, well-structured System Scope Outline tailored specifically for your design documentation and ready for coding agents to execute.
System Scope Outline for Design Documentation
1. Hardware Integration & Telemetry Boundary
 * In Scope: Mobile camera stream ingestion, 2D frame buffer extraction, hardware device sensor telemetries (GPS, digital compass, accelerometer/gyroscope) via standard Web APIs, and Kalman filtering for orientation stabilization.
 * Out of Scope: Direct low-level firmware modification or hardware-level sensor calibration outside standard browser permissions.
2. Spatial Capture & 3D Surface Reconstruction
 * In Scope: On-device low-density point cloud generation, Web Worker-based Sobel edge detection, single-tap raycasting for wall/surface face isolation, and asynchronous cloud pipeline integration for high-density 3D model generation (.splat/.glb).
 * Out of Scope: On-device, client-side NeRF/Gaussian Splatting compilation that exceeds browser WebGL/VRAM memory limits.
3. Environmental & Material Formulation Engine
 * In Scope: Real-time formula adjustments for Architectural Paint, Wall Screed, and Wall Plaster based on surface area, porosity, application method, and live local weather inputs (temperature, humidity, wind) via weatherEquationEngine.ts.
 * Out of Scope: Manual raw chemical manufacturing synthesis beyond calculated binder, pigment, solvent, and additive ratios.
4. Commercial Intelligence & Supplier Optimization
 * In Scope: Automated mapping of material specifications to open chemical database ratios, quiet background scraping of regional supplier inventories (e.g., BASF, Dow, Evonik), and ranking based on distance, lead time, and commodity price indices.
 * Out of Scope: Financial transaction processing or direct payment gateway execution within the estimator HUD.
5. Field-to-Factory Synchronization & Data Persistence
 * In Scope: Offline-first draft persistence using IndexedDB (siteview_estimator_input_draft_v1), state-machine guardrails (DRAFT \rightarrow SUBMITTED \rightarrow LOCKED_IN_PRODUCTION), optimistic concurrency control, and factory terminal queue ingestion (MixerApp).
 * Out of Scope: Continuous bi-directional multi-user live cursors during active camera scanning.
6. External Integrations & Stakeholder Workspace
 * In Scope: Interactive WebGL preview links with orbit/zoom controls, embedded encrypted chat, WebRTC video conferencing, and automated export of quotation packages to Google Workspace (Docs, Sheets, Drive).
Key TypeScript Interfaces for Agent Implementation
You can hand these core schemas directly to your coding agents:
//
