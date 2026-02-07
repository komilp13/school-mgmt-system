# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

School management system frontend — a Next.js 16 application (App Router) with TypeScript, Tailwind CSS v4, and shadcn/ui components. Part of a larger full-stack system that will include a Node.js API backend and PostgreSQL database.

## Commands

```bash
npm run dev      # Start dev server (http://localhost:3000)
npm run build    # Production build
npm run start    # Start production server
npm run lint     # Run ESLint (eslint-config-next with core-web-vitals + typescript rules)
```

## Architecture

- **Framework:** Next.js 16 with App Router (`app/` directory) and React 19
- **Styling:** Tailwind CSS v4 via PostCSS (`@tailwindcss/postcss`), with CSS variables for theming defined in `app/globals.css`
- **UI Components:** shadcn/ui (new-york style, zinc base color, RSC-enabled) — add components via `npx shadcn add <component>`
- **Icons:** lucide-react
- **Path aliases:** `@/*` maps to project root (e.g., `@/components`, `@/lib/utils`)

### shadcn/ui Configuration

Configured in `components.json`. Key aliases:
- `@/components/ui` — shadcn UI primitives (built on Radix UI)
- `@/components` — custom/composite components
- `@/lib/utils` — `cn()` helper (clsx + tailwind-merge)
- `@/hooks` — custom React hooks

### Styling Conventions

- Dark mode uses the `.dark` class strategy (`@custom-variant dark (&:is(.dark *))`)
- Design tokens are CSS custom properties in `app/globals.css` (oklch color space)
- Use the `cn()` utility from `@/lib/utils` for conditional class merging
- Animation support via `tw-animate-css`

### Fonts

Geist Sans and Geist Mono loaded via `next/font/google`, exposed as `--font-geist-sans` and `--font-geist-mono` CSS variables.
