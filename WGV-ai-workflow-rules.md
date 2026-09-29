# AI Workflow Rules: SiteView Pro

## Approach

Build SiteView Pro incrementally using a spec-driven workflow[span_3](start_span)[span_3](end_span). Context files (`architecture.md`, `ui-context.md`, `code-standards.md`, and `Fixed Prj Overview.md`) define what to build, how to build it, and the current state of progress[span_4](start_span)[span_4](end_span). Always implement against these specs — do not infer, invent, or hallucinate behavior from scratch[span_5](start_span)[span_5](end_span).

## Scoping Rules

- Work on exactly one feature unit/step at a time as outlined in `progress-tracker.md`[span_6](start_span)[span_6](end_span).
- Prefer small, verifiable increments over large speculative changes[span_7](start_span)[span_7](end_span).
- Do not combine unrelated system boundaries in a single implementation step (e.g., do not mix Web Worker image processing with UI modal design in one commit)[span_8](start_span)[span_8](end_span).

## When to Split Work

Split an implementation step if it combines:

1. **Client UI & Off-Thread Logic:** Combining React HUD interfaces with Web Worker scripts or WebGL fragment shaders[span_9](start_span)[span_9](end_span).
2. **Multiple System Boundaries:** Modifying camera sensor pipelines alongside factory terminal database schemas in the same task[span_10](start_span)[span_10](end_span).
3. **Ambiguous Behavior:** Encountering logic or design parameters not clearly specified in the context files[span_11](start_span)[span_11](end_span).

If a change cannot be verified end-to-end quickly, the scope is too broad — split it[span_12](start_span)[span_12](end_span).

## Handling Missing Requirements

- Do not invent product behavior or chemical formulas not defined in the context files[span_13](start_span)[span_13](end_span).
- If a requirement is ambiguous, resolve it in `architecture.md` or `ui-context.md` before implementing[span_14](start_span)[span_14](end_span).
- If a requirement is missing or blocked, add it as an open question in `progress-tracker.md` before continuing[span_15](start_span)[span_15](end_span).

## Protected Files

Do not modify the following unless explicitly instructed:

- `src/components/ui/*` (Generated shadcn/ui or base component primitives)
- Third-party library internal types (`three`, `@react-three/fiber`, `framer-motion`)
- Master system context files (`Fixed Prj Overview.md`)

## Keeping Docs in Sync

Update the relevant context file whenever implementation decisions evolve:

- System architecture, database schemas, or Web Worker interfaces (`architecture.md`)[span_16](start_span)[span_16](end_span)
- HUD layout tokens, touch sizes, or color schemes (`ui-context.md`)[span_17](start_span)[span_17](end_span)
- Strict TypeScript interfaces or helper utilities (`code-standards.md`)[span_18](start_span)[span_18](end_span)
- Feature unit status and execution state (`progress-tracker.md`)[span_19](start_span)[span_19](end_span)

## Before Moving to the Next Unit

Before declaring a step complete and moving to the next item in `progress-tracker.md`, verify the following[span_20](start_span)[span_20](end_span):

1. The current unit works end-to-end within its defined scope[span_21](start_span)[span_21](end_span).
2. No invariant defined in `architecture.md` was violated (e.g., UI main thread remains under 150ms, offline state saved to IndexedDB)[span_22](start_span)[span_22](end_span)[span_23](start_span)[span_23](end_span).
3. `progress-tracker.md` reflects the completed work, and next step is clearly staged[span_24](start_span)[span_24](end_span).
4. Strict TypeScript build passes clean (`npm run build` or `npx tsc --noEmit` passes with 0 errors)[span_25](start_span)[span_25](end_span).
