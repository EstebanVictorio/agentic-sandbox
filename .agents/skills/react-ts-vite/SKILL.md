---
name: react-ts-vite
description: Scaffold a new React + TypeScript Vite application with best-practice project structure, tooling, and styling conventions.
package-manager: yarn
---

# React + TypeScript + Vite Boilerplate Generator

## Overview

Use this skill whenever the user asks to create a new React application and either:
- explicitly requests React + Vite / React + TypeScript, or
- asks for a modern React SPA without specifying a framework.

Follow **every** step below in order. Do not skip steps.

---

## Step 1 — Check the target directory

Always scaffold the boilerplate **directly into the workspace root** (i.e. `./`). Do not create a subdirectory for the project unless the user explicitly provides a different path.

---

## Step 2 — Scaffold with Vite

Run `create-vite` in non-interactive mode. Always use `--help` first to confirm available flags for the installed version, then run the actual scaffold command.

```bash
# Inspect available flags
yarn create vite --help

# Scaffold (replace <project-dir> and adjust --template as needed)
yarn create vite <project-dir> --template react-ts
```

After scaffolding, verify the directory exists and contains `package.json`, `vite.config.ts`, `index.html`, and `src/`.

---

## Step 3 — Install dependencies

```bash
cd <project-dir>
yarn install
```

---

## Step 4 — Project structure conventions

Organise `src/` as follows (create any missing directories):

```
src/
├── assets/          # Static assets (images, fonts, svgs)
├── components/      # Shared, reusable UI components
│   └── ui/          # Primitive/design-system components
├── hooks/           # Custom React hooks
├── pages/           # Route-level page components
├── services/        # API clients and external service wrappers
├── store/           # Global state (Zustand / Context)
├── types/           # Shared TypeScript types and interfaces
├── utils/           # Pure utility functions
├── App.tsx
└── main.tsx
```

Create an empty `index.ts` barrel file in each directory so imports resolve cleanly.

---

## Step 5 — TypeScript configuration

Ensure `tsconfig.json` (and `tsconfig.app.json` if it exists) includes at minimum:

```jsonc
{
  "compilerOptions": {
    "target": "ES2020",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "moduleResolution": "bundler",
    "jsx": "react-jsx",
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

Add the corresponding `resolve.alias` to `vite.config.ts`:

```ts
import path from 'path'
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: { '@': path.resolve(__dirname, './src') },
  },
})
```

Install the `@types/node` dev dependency if path aliasing is used:

```bash
yarn add -D @types/node
```

---

## Step 6 — Linting and formatting

Install and configure ESLint + Prettier:

```bash
yarn add -D \
  eslint \
  @eslint/js \
  typescript-eslint \
  eslint-plugin-react-hooks \
  eslint-plugin-react-refresh \
  prettier \
  eslint-config-prettier
```

Create `eslint.config.json`:

```json
{
  "ignores": ["dist"],
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:react-hooks/recommended",
    "prettier"
  ],
  "plugins": ["@typescript-eslint", "react-hooks", "react-refresh"],
  "rules": {
    "react-refresh/only-export-components": ["warn", { "allowConstantExport": true }]
  }
}
```

> **Note**: The JSON flat-config format requires ESLint ≥ 9 and uses string-based preset names rather than JS imports.

Create `.prettierrc`:

```json
{
  "semi": false,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 100
}
```

Add lint scripts to `package.json`:

```json
"scripts": {
  "lint": "eslint .",
  "lint:fix": "eslint . --fix",
  "format": "prettier --write ."
},
"packageManager": "yarn@4"
```

---

## Step 7 — CSS / styling baseline

Always install and configure latest Tailwind version

```

Import Inter from Google Fonts by adding the following `<link>` to `index.html` inside `<head>`:

```html
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet" />
```

---

## Step 8 — Environment variables

Create `.env.example` (committed) and `.env.local` (gitignored):

```
# .env.example
VITE_APP_TITLE=My App
VITE_API_BASE_URL=http://localhost:3000
```

Add `.env.local` and `.env*.local` to `.gitignore` if not already present.

---

## Step 9 — README

Create `README.md` with at minimum:

```markdown
# <Project Name>

> Brief description of the project.

## Prerequisites
- Node.js ≥ 18
- Yarn ≥ 4

## Getting Started
```bash
yarn install
yarn dev
```

## Available Scripts

| Command | Description |
|---------|-------------|
| `yarn dev` | Start dev server |
| `yarn build` | Production build |
| `yarn preview` | Preview production build locally |
| `yarn lint` | Run ESLint |
| `yarn format` | Format with Prettier |


---

## Step 10 — Strip the default boilerplate UI

Now that the pristine app is verified, remove all default Vite/React demo content and replace it with a minimal Hello World.

---

### 10a — Delete the bundled SVG icons

```bash
rm src/assets/react.svg
rm public/vite.svg
```

---

### 10b — Replace `src/App.css`

Overwrite the file with an empty placeholder (styles will be added per-project as needed):

```css
/* App styles */
```

---

### 10c — Replace `src/App.tsx`

Replace the entire file with:

```tsx
function App() {
  return (
    <main>
      <h1>Hello World from React and Vite with TypeScript</h1>
    </main>
  )
}

export default App
```
---

## Notes & Conventions

- **Imports**: Always use the `@/` alias instead of relative `../../` imports.
- **Components**: Use named exports; avoid default exports except for page components used by a router.
- **Types**: Co-locate component-specific types in the same file; shared types go in `src/types/`.
- **Side effects**: Never import CSS files inside component files; keep all global styles in `src/index.css` or dedicated feature-level `.css` files imported at the top of the relevant page/component tree.
- **Environment variables**: All Vite env vars must be prefixed with `VITE_` to be exposed to the client bundle. Access them via `import.meta.env.VITE_*`.
