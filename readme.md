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

### Phase 1: Data Store & Schema Migration (Completed)
- [x] **Reactive Local-First Store:** Defined hierarchical schema (`sections[]` containing `patterns[]` with `id`, `title`, `domain`, `pattern`, `modifiers`, `saic`, `reality`, `notes`, plus top-level `globalConclusions`).
- [x] **Default Seed Integration:** Seeded `localStorage` on initial load using the curated entries from `FIELD_GUIDE.md`.

### Phase 2: Home Screen Pattern Atlas UI (Completed)
- [x] **Atlas Home View:** Primary home view is now the categorized Pattern Atlas with instant search/filter across titles, SAIC vectors, and notes.
- [x] **Section CRUD (`+` / `—`):** Added `+ Section` creation and section deletion controls.
- [x] **Pattern CRUD (`+` / `—`):** Added `+ Pattern`, inline editing, and deletion controls.
- [x] **Global Conclusions Notebook:** Persistent, auto-saving text area at the bottom of the atlas for longitudinal insights and emergent conclusions.

### Phase 3: Pattern Editor / Creator Form (Completed)
- [x] **Pattern Creation & Editing Modal:** Chip selection interface (Domain, Pattern, Modifiers, SAIC, Reality statements) wired into the Pattern Editor modal with dedicated notes and category assignment.

### Phase 4: Streamlined Reconsolidation Pass & Simplification (Completed)
- [x] **1-Tap Pre-Populated "Run Pass":** Launching a pass from any pattern card instantly populates Steps 1, 3, and 4, letting the user focus solely on somatic coordinates (`code1 --> code2:xn --> code3` + release sign).
- [x] **Simplify Quadrant Target from 5 Rings to 3 Rings:** Reduced rings to Mild/Moderate/High (`R1–R3`, `Y1–Y3`, `B1–B3`, `G1–G3`) with larger `maxR / 3` tap targets for thumb-friendly mobile accuracy.
- [x] **Decommission Obsidian Export & Lockout:** Stripped out Obsidian vault dependency and removed the forced terminal rest screen lockout.

### Phase 5: PWA Polish & Deployment (Completed)
- [x] **GitHub Pages Deployment:** Published live PWA via GitHub Pages at `https://evanbowen52.github.io/unexpressed/`.
- [x] **Network-First Service Worker:** Upgraded to `v2` service worker with network-first caching and 1-tap repo sync.
- [ ] **Custom App Icons:** Generate minimalist `icon-192.png` / `icon-512.png` and wire into `manifest.json`.

---

## 4. Future Roadmap: Multi-User Cloud Architecture (Supabase Migration)

When expanding the application from single-user local storage to a multi-user, cross-device platform, **Supabase (PostgreSQL + Auth + Realtime)** is the designated backend of choice. Somatic refactor logs and threat models represent sensitive personal psychological data, requiring strict multi-tenant isolation, strong encryption, and offline-first durability.

### 4.1. Authentication & Onboarding
* **Passwordless Auth:** Supabase Auth with Email Magic Links or OAuth providers (GitHub/Google) to eliminate password friction.
* **Anonymous Session Promotion:** Enable guests to use the Pattern Atlas immediately via `localStorage`. When the user chooses to create an account, an onboarding migration hook automatically upserts the local `localStorage` Atlas into their newly created cloud database.

### 4.2. Relational Database Schema (PostgreSQL)

```sql
-- 1. Profiles & Global User Settings
create table public.profiles (
  id uuid references auth.users on delete cascade primary key,
  display_name text,
  fast_diagnostic text,
  global_conclusions text,
  created_at timestamptz default timezone('utc'::text, now()) not null,
  updated_at timestamptz default timezone('utc'::text, now()) not null
);

-- 2. Atlas Sections
create table public.sections (
  id uuid default gen_random_uuid() primary key,
  user_id uuid references auth.users on delete cascade not null,
  title text not null,
  sort_order int default 0 not null,
  created_at timestamptz default timezone('utc'::text, now()) not null,
  updated_at timestamptz default timezone('utc'::text, now()) not null
);

-- 3. Individual Patterns & Scenarios
create table public.patterns (
  id uuid default gen_random_uuid() primary key,
  section_id uuid references public.sections on delete cascade not null,
  user_id uuid references auth.users on delete cascade not null,
  title text not null,
  domain text not null,                    -- 'Interpersonal' | 'Environmental' | 'Personal'
  pattern_type text not null,              -- 'Criticism / Correction' | 'Boundary / Demand' | etc.
  modifiers text[] default '{}'::text[],   -- ARRAY['Memory Loop', 'Third Arrow', ...]
  saic text[] default '{}'::text[],        -- ARRAY['Safety', 'Agency', 'Identity', 'Connection']
  reality text not null,                   -- Anchor statement + parenthetical rationale
  notes text,                              -- Personal reflective notes
  sort_order int default 0 not null,
  created_at timestamptz default timezone('utc'::text, now()) not null,
  updated_at timestamptz default timezone('utc'::text, now()) not null
);

-- 4. Somatic Reconsolidation Session Telemetry
create table public.session_logs (
  id uuid default gen_random_uuid() primary key,
  user_id uuid references auth.users on delete cascade not null,
  pattern_id uuid references public.patterns on delete set null,
  code1 text not null,                     -- e.g. "R3"
  code2 text not null,                     -- e.g. "Y1"
  code3 text not null,                     -- e.g. "G1"
  loops text not null,                     -- "few" | "moderate" | "persistent"
  sensations text[] default '{}'::text[],
  locations text[] default '{}'::text[],
  releases text[] default '{}'::text[],
  telemetry_summary text not null,         -- "R3 --> 00:14 --> Y1:moderate --> 00:32 --> G1"
  created_at timestamptz default timezone('utc'::text, now()) not null
);
```

### 4.3. Row-Level Security (RLS) Policies
Because somatic interoception records require complete privacy, Row-Level Security must be enabled and enforced across all tables:

```sql
alter table public.profiles enable row level security;
alter table public.sections enable row level security;
alter table public.patterns enable row level security;
alter table public.session_logs enable row level security;

-- Enforce strict single-owner access:
create policy "Users own their profile" on public.profiles
  for all using (auth.uid() = id);

create policy "Users own their sections" on public.sections
  for all using (auth.uid() = user_id);

create policy "Users own their patterns" on public.patterns
  for all using (auth.uid() = user_id);

create policy "Users own their session logs" on public.session_logs
  for all using (auth.uid() = user_id);
```

### 4.4. Offline-First Sync Engine (Local-First Durability)
* **Zero UI Latency:** The UI continues writing to `IndexedDB` or `localStorage` first; users never wait for a network round-trip while logging an acute somatic shift.
* **Background Sync Queue:** A lightweight sync coordinator queues dirty mutations and flushes to Supabase via `@supabase/supabase-js` whenever network connectivity is active.
* **Conflict Resolution:** Last-Write-Wins (LWW) driven by PostgreSQL `updated_at` timestamps.

### 4.5. Data Sovereignty & Portability
* Retain the existing Markdown bridge (`generateAtlasMarkdown()` and `parseFieldGuideMarkdown()`).
* Even with cloud sync enabled, users retain one-tap **"Download FIELD_GUIDE.md"** and **"Copy Markdown"** functionality to ensure complete independence from the hosted service.
