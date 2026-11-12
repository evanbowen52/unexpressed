# Somatic Refactor (PWA)

A zero-backend, offline-first Progressive Web App designed for **post-regulation memory reconsolidation** and interoceptive logging. Built to run on GitHub Pages with local-only persistence (`localStorage`) and one-tap Obsidian Markdown export.

---

## 1. Core Philosophy & Design Guardrails

1. **Pattern Atlas Over Empty Scratchpad:** The core value of this system is the personal catalog of recurrent loops (**Field Guide**). The primary screen is a searchable, categorized atlas of recurring triggers, SAIC threat analyses, reality anchors, and personal observations.
2. **Dynamic Section & Pattern Customization:** Anyone can add, edit, or remove categories/sections (`+` / `—`) and individual patterns (`+` / `—`) to match their own life context.
3. **Dual-Layer Notes:** Maintain actionable notes attached to individual patterns, plus a global conclusions notebook at the base of the atlas for meta-observations and longitudinal insights.
4. **No In-the-Moment Triage:** This app is *never* used while actively redlined or dysregulated. Acute regulation (breathing, cool water, feeling feet) happens offline in the physical world. The app is strictly for post-regulation reconsolidation once baseline calm is restored.
5. **Frictionless Pre-Populated Reconsolidation:** Tapping `Run Pass` on any pattern instantly pre-populates Step 1 (Domain, Pattern, Modifiers), Step 3 (SAIC), and Step 4 (Reality Anchors). The user only inputs the somatic delta (`code1 --> code2:xn --> code3` + release sign).
6. **No Terminal Rest Lockout:** Once a pass is logged, the active session remains accessible. If an unclosed mental loop or secondary somatic spike emerges, revisiting and refining the session is fully supported.
7. **Zero-Backend, Local-First Privacy:** Zero accounts, zero analytics, zero external cloud dependencies. All data persists directly in the browser (`localStorage`), with clean JSON/Markdown backup export options.

---

## 2. Current Architecture

* `index.html` — Single-file vanilla HTML/CSS/JS application containing the Pattern Atlas, dynamic section/pattern management, SVG quadrant/ring targets, and session telemetry logger.
* `FIELD_GUIDE.md` — Initial seed data and standalone reference for recurrent patterns, domain/pattern tags, SAIC threat diagnoses, and reality anchors.
* `MANUAL.md` — Protocol execution manual, somatic interoception principles, SAIC threat model definitions, and coordinate telemetry reference.
* `manifest.json` — PWA manifest for standalone mobile home-screen installation.
* `sw.js` — Service Worker caching static assets for offline reliability.

---

## 3. Sprint Roadmap: The Pattern Atlas Pivot

### Phase 0: Baseline Safety & Version Control (Completed)
- [x] **Initialize Git Repository:** Create `.gitignore`, initialize local git repo, stage baseline files (`index.html`, `FIELD_GUIDE.md`, `FIELD_GUIDE_PROMPT.md`, `MANUAL.md`, `README.md`, `manifest.json`, `sw.js`), and commit immutable pre-pivot baseline checkpoint (`2c426f6`).

### Phase 1: Data Store & Schema Migration
- [ ] **Reactive Local-First Store:** Define hierarchical schema (`sections[]` containing `patterns[]` with `id`, `title`, `domain`, `pattern`, `modifiers`, `saic`, `reality`, `notes`, plus top-level `globalConclusions`).
- [ ] **Default Seed Integration:** Seed `localStorage` on initial load using the curated entries from `FIELD_GUIDE.md`.

### Phase 2: Home Screen Pattern Atlas UI
- [ ] **Atlas Home View:** Transform initial landing view into the categorized Pattern Atlas with instant search/filter across titles, SAIC vectors, and notes.
- [ ] **Section CRUD (`+` / `—`):** Add `+ Add Section`, rename, and delete section controls.
- [ ] **Pattern CRUD (`+` / `—`):** Add `+ Add Pattern`, edit, and delete pattern controls.
- [ ] **Global Conclusions Notebook:** Persistent, auto-saving text area at the bottom of the atlas for longitudinal insights and emergent conclusions.

### Phase 3: Pattern Editor / Creator Form
- [ ] **Pattern Creation & Editing Modal:** Repurpose the chip selection interface (Domain, Pattern, Modifiers, SAIC, Reality statements) into an intuitive pattern editor with dedicated notes field.

### Phase 4: Streamlined Reconsolidation Pass & Simplification
- [ ] **1-Tap Pre-Populated "Run Pass":** Launching a pass from any pattern card instantly populates Steps 1, 3, and 4, letting the user focus solely on somatic coordinates (`code1 --> code2:xn --> code3` + release sign).
- [ ] **Simplify Quadrant Target from 5 Rings to 3 Rings:** Reduce rings to Mild/Moderate/High (`R1–R3`, `Y1–Y3`, `B1–B3`, `G1–G3`) for thumb-friendly mobile accuracy.
- [ ] **Decommission Obsidian Export & Lockout:** Strip out Obsidian clipboard push and remove the forced terminal rest screen lockout.

### Phase 5: PWA Polish & Deployment
- [ ] **Custom App Icons & Cache:** Generate minimalist `icon-192.png` / `icon-512.png` and wire into `manifest.json` and `sw.js`.
- [ ] **GitHub Pages Deployment:** Connect remote repository origin and publish live PWA on GitHub Pages.