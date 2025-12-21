# CLAUDE.md

Guidance for Claude Code (claude.ai/code) when working in this repository.

## Project Overview

YAQITH Sentinel is a React 19 + TypeScript + Vite prototype: an adaptive,
zero-trust authentication command center for Saudi Arabia's Absher e-government
platform. It demonstrates behavioral biometrics, real-time threat monitoring,
geographic analytics, and AI-driven risk scoring. Live data is backed by
Supabase; the accompanying Jupyter notebook contains the behavioral-AI analysis.

## Development Commands

```bash
npm install        # Install dependencies
npm run dev        # Dev server on http://localhost:3010
npm run build      # Production build
npm run preview    # Preview the production build (port 3010)

npm run seed       # Seed Supabase with ~500 synthetic auth records
npm run seed:stats # Print distribution stats for seeded data
npm run seed:verify# Validate seeded data integrity
```

The dev/preview servers run on **port 3010** (configured in `vite.config.ts`).

## Environment Setup

Copy `.env.example` to `.env.local` and fill in:

- `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY` — required for live data
- `GEMINI_API_KEY` — optional, only for the AI media features

## Architecture

### Tech Stack
- **React 19** + **TypeScript**, bundled with **Vite 6**
- **Tailwind CSS** via CDN (no local config)
- **Recharts** for charts, **Leaflet** / **react-leaflet** for the geo map
- **Lucide React** for icons
- **Supabase** for the real-time database and auth

### Layout

```
App.tsx            # Sidebar nav + client-side tab routing (no router lib)
index.tsx          # React root mount
types.ts           # Enums/interfaces: RiskLevel, ScenarioType, Scenario, Storyboard
constants.tsx      # Scenario definitions, storyboards, mock data
lib/supabase.ts    # Supabase client (reads VITE_SUPABASE_* env vars)
components/         # One component per page/tab
scripts/           # Node scripts to seed and verify the Supabase database
seeds/             # SQL that generates the ~500-record seed dataset
```

### Navigation

`activeTab` state in `App.tsx` selects which component renders. Tabs map to the
command-center modules (Live Monitor, Threat Analytics, Geo Map, Decision
Engine, Risk Policies) and interactive labs (Biometrics, ZKP).

### Path Alias

`@/*` resolves to the project root (set in both `tsconfig.json` and
`vite.config.ts`), e.g. `import { supabase } from '@/lib/supabase'`.
