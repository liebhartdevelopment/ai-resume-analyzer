**Repository Overview**

- **Purpose**: `ai-resume-analyzer` is a small React + React Router (v7) TypeScript app that provides AI-powered resume feedback UI using static sample data in `app/constants`.
- **Major pieces**: UI routes in `app/routes/*`, root layout in `app/root.tsx`, UI components in `app/components/`, and static sample data + AI prompt format in `app/constants/index.ts`.

**How to run (developer workflows)**

- Install dependencies: `npm install`
- Development server (hot reload via React Router dev): `npm run dev` (uses `react-router dev` / Vite plugin)
- Build: `npm run build` (uses `react-router build`)
- Serve production build locally: `npm run start` (runs `react-router-serve ./build/server/index.js`)
- Typecheck: `npm run typecheck` (runs `react-router typegen && tsc`)
- Docker: build with `docker build -t my-app .` and run `docker run -p 3000:3000 my-app` (see `README.md` / `Dockerfile`).

**Project architecture & routing**

- Routes are declared using the React Router dev route helpers. The top-level route list is in `app/routes.ts` which references files under `app/routes/` (e.g. `routes/home.tsx`).
- `app/root.tsx` provides the HTML shell, `Layout`, `Outlet` mounting, and `ErrorBoundary` used across routes.
- Add a new route by creating `app/routes/<name>.tsx` and adding an index entry in `app/routes.ts` (example: `index("routes/new.tsx")`).

**Key conventions & patterns (codebase-specific)**

- Imports use the `~` alias (see `vite.config.ts` with `vite-tsconfig-paths`). Use `~/` to import from `app/` (e.g. `~/components/Navbar`).
- Routing uses `react-router` v7 packages (see `package.json`). Links and navigation import from `react-router` (not `react-router-dom`).
- Global TS types are declared in `app/types/index.d.ts` (e.g. `Resume`, `Feedback`). These are ambient/global interfaces the app expects.
- Tailwind CSS is used for styling; classes are applied inline via `className` in JSX and configured through `tailwindcss` and `@tailwindcss/vite` in `vite.config.ts`.
- Small local dataset: `app/constants/index.ts` exports `resumes`, `AIResponseFormat`, and `prepareInstructions` — this is the single source for example resume data and the exact AI prompt/response format used by the UI.

**Integration points & external deps to be aware of**

- AI prompt/response format: `app/constants/index.ts` contains `AIResponseFormat` string and `prepareInstructions(...)` helper — update these when altering the schema consumed by UI.
- PDF rendering: `pdfjs-dist` is included and `public/pdf.worker.min.mjs` is present — host static worker under `public/` as the project expects.
- State management: `zustand` is included in `package.json` but not mandatory; check for any store usage when adding global state.

**Files to inspect for common tasks**

- Layout & app shell: `app/root.tsx`
- Routes list: `app/routes.ts` and files in `app/routes/`
- Components: `app/components/*` (e.g. `Navbar.tsx`, `ResumeCard.tsx`)
- Sample data and AI prompt format: `app/constants/index.ts`
- Types: `app/types/index.d.ts`
- Build/dev tooling: `package.json`, `vite.config.ts`, `Dockerfile`

**Examples & micro-guidance for an AI coding agent**

- To wire new AI output fields into the UI: update `AIResponseFormat` in `app/constants/index.ts`, then update the `Feedback` interface in `app/types/index.d.ts`, then modify components that render `feedback` (search for `feedback` across `app/`).
- To add a new page: create `app/routes/yourPage.tsx`, export a default component and optional `meta`/`loader` functions, then add an index entry in `app/routes.ts`.
- To run type checks and generate route types: `npm run typecheck` (calls `react-router typegen` used by the template).

**Notable quirks discovered**

- The codebase uses `Link` from `react-router` (v7) rather than the `react-router-dom` import pattern commonly seen in older templates.
- `app/constants/index.ts` contains the canonical AI prompt and must remain a single source of truth for any AI-related changes.
- The project currently uses local sample data (`resumes`) rather than a backend; integrate an API by replacing the data source in `app/constants` or adding a loader on a route.

**What I did / next steps for you**

- Created this file to help AI agents get productive quickly. Tell me if you want extra examples (e.g. common code edits, tests, or a short checklist for safe refactors) and I'll expand.
