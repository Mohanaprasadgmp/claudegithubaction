# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm start              # Dev server at http://localhost:4200 (auto-reloads)
npm run build          # Production build
npm run watch          # Dev build with watch mode
npm test               # Unit tests via Karma/Jasmine
npm run serve:ssr:SampleAngularCodeClaude  # Serve SSR build via Express at port 4000
```

To run a single test file, use Angular CLI directly:
```bash
npx ng test --include='**/app.spec.ts'
```

There is no separate lint command configured — the project uses Angular CLI defaults.

## Architecture

This is an **Angular 20 starter app** demonstrating modern Angular patterns. The app itself is a single welcome/showcase page; the main value is its architectural setup.

### Key Patterns

- **Standalone Components**: No NgModules — all components use the standalone API with per-component imports.
- **Zoneless Change Detection**: Uses `provideZonelessChangeDetection()` instead of NgZone for better performance (`src/app/app.config.ts`).
- **Server-Side Rendering (SSR)**: Dual bootstrap — `main.ts` for the browser, `main.server.ts` for the server, served via Express (`src/server.ts`). All routes prerender by default (`src/app/app.routes.server.ts`).
- **Hydration with Event Replay**: Configured with `withEventReplay()` so user interactions during hydration are not lost.

### Config Split

| File | Purpose |
|---|---|
| `src/app/app.config.ts` | Client-side providers (router, zoneless CD, hydration) |
| `src/app/app.config.server.ts` | Merges client config with server-specific providers |
| `src/server.ts` | Express server — static files from `/browser` (1-year cache), SSR via `AngularNodeAppEngine` |

### TypeScript

Strict mode is fully enabled in `tsconfig.json`, including `noImplicitOverride`, `noImplicitReturns`, `noFallthroughCasesInSwitch`, and Angular-specific strict template/injection checking.

### Styling & Formatting

EditorConfig enforces 2-space indentation and UTF-8 encoding. TypeScript files use single quotes.
