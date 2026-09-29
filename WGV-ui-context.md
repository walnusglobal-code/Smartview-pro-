# UI Context: SiteView Pro Tactical HUD v2

## Theme

Dark technical workspace only. High-contrast, slate-tinted background surfaces (`#020617`), glassmorphic panels with backdrop blur (`backdrop-blur-md`), and vivid tactical indicators for interactive field status elements.

## Colors

| Role | CSS Variable | Value |
| --- | --- | --- |
| Page background | `--bg-base` | `#020617` (slate-950) |
| Surface panel | `--bg-surface` | `rgba(15, 23, 42, 0.90)` (slate-900/90) |
| Primary text | `--text-primary` | `#f8fafc` (slate-50) |
| Muted text | `--text-muted` | `#94a3b8` (slate-400) |
| Night Vision Green | `--accent-green` | `#10b981` (emerald-500) |
| Deep Indigo | `--accent-indigo` | `#6366f1` (indigo-500) |
| Tactical Red | `--state-error` | `#f43f5e` (rose-500) |
| Construction Amber | `--state-warning` | `#f59e0b` (amber-500) |
| Border Default | `--border-default` | `rgba(51, 65, 85, 0.80)` (slate-700/80) |

## Typography

| Role | Font | Variable |
| --- | --- | --- |
| UI text | Inter / Sans | `--font-sans` |
| Code/Telemetry | JetBrains Mono / Mono | `--font-mono` |

## Border Radius

| Context | Class |
| --- | --- |
| Compact HUD toggles | `rounded-lg` |
| Floating cards / drawers | `rounded-2xl` |
| Modals / Certification overlay | `rounded-3xl` |

## Component Library

Tailwind CSS + Lucide React + custom glassmorphic wrappers. Components live in `src/components/`. Touch hitboxes must maintain a minimum scale of `48x48dp` on all mobile viewports.

## Layout Patterns

- **Camera Viewfinder**: Full-bleed `100vw x 100vh` canvas with overlaid floating glassmorphic HUD controls.
- **Side Drawer**: Slide-over panel with `max-w-full` or `max-w-md` for chemical breakdown and environmental parameters.
- **Modal Dialogs**: Centered overlay with dark backdrop blur (`bg-slate-950/80 backdrop-blur-md`) and explicit header close buttons.

## Icons

Lucide React stroke-based icons. Sizes: `h-5 w-5` for standard controls, `h-6 w-6` for primary action buttons.
