# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev          # Start development server
npm run build        # Production build (outputs to out/)
npm start            # Serve static exports on port 3333
npm run type-check   # TypeScript check without emit
npm run lint         # ESLint with auto-fix
npm run test         # Jest test suite
npm run format       # Prettier format (CSS, TSX, TS)
```

## Architecture

This is a static-exported Next.js app (App Router) that visualizes Japanese prefectural population data, deployed to GitHub Pages.

**Key constraint:** `next.config.js` sets `output: "export"`, so all pages must be statically renderable. Every page/component uses `"use client"` — there is no server-side rendering.

**Data flow:** Static JSON files in `public/json/` are fetched at runtime by `hooks/useFetch.ts`, which caches responses in a `useRef`. Components receive data as props from `app/page.tsx`.

**Routing:** Single-page app. `app/page.tsx` is the only route. Tab state (`/types/TabState.ts`) drives which chart renders in `components/GraphTab.tsx`.

**Styling:** MUI v5 with Emotion CSS-in-JS. The Emotion setup requires a custom registry in `app/emotion.tsx` for Next.js App Router compatibility. Theme is in `app/theme/theme.ts`.

**Charts:** Chart.js via `react-chartjs-2`. Four chart components live in `components/Graph/`, each receiving filtered/sorted prefecture data.

**Base path:** Configurable via `NEXT_PUBLIC_BASE_PATH` env var — used for GitHub Pages subdirectory deployment (set via GitHub Actions `BASE_PATH` secret).

## CI/CD

GitHub Actions (`.github/workflows/static.yml`) builds and deploys to GitHub Pages on push to `main`. It runs `npm ci && npm run build` then uploads `./out/`.
