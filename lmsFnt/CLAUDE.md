# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Vue 3 + TypeScript + Vite frontend application for an LMS (Learning Management System). The project uses:
- Vue 3 with Composition API
- TypeScript for type safety
- Vite for build tooling and development server
- Pinia for state management
- Vue Router for client-side routing
- PrimeVue for UI components with Aura theme
- PrimeIcons for iconography
- Vitest for unit testing
- Playwright for end-to-end testing

## Development Commands

### Essential Commands
- `npm run dev` - Start development server with hot-reload
- `npm run build` - Build for production (includes type checking)
- `npm run preview` - Preview production build locally
- `npm run type-check` - Run TypeScript type checking
- `npm run lint` - Run ESLint with auto-fix
- `npm run format` - Format code with Prettier

### Testing
- `npm run test:unit` - Run unit tests with Vitest
- `npm run test:e2e` - Run end-to-end tests with Playwright
- `npm run test:e2e -- --project=chromium` - Run e2e tests on Chromium only
- `npm run test:e2e -- --debug` - Run e2e tests in debug mode

### First-time Setup
```bash
npm install
npx playwright install  # Required for e2e tests
```

## Architecture

### Directory Structure
- `src/` - Main source code
  - `components/` - Reusable Vue components
  - `views/` - Page-level components
  - `router/` - Vue Router configuration
  - `stores/` - Pinia store definitions
  - `assets/` - Static assets (CSS, images)
- `e2e/` - End-to-end tests
- `src/components/__tests__/` - Unit tests

### Key Files
- `src/main.ts` - Application entry point, sets up Vue app with Pinia, Router, and PrimeVue
- `src/App.vue` - Root component with navigation structure
- `src/router/index.ts` - Route definitions with lazy loading
- `vite.config.ts` - Vite configuration with path aliases (@/ → src/)

### PrimeVue Integration
- **Theme**: Uses Aura theme from @primeuix/themes
- **Configuration**: Set up in main.ts with global PrimeVue plugin
- **Components**: Import individually from 'primevue/[component]' for tree-shaking
- **Icons**: PrimeIcons available with 'pi pi-[icon-name]' class syntax
- **Styling**: Theme CSS automatically injected, PrimeIcons CSS imported in main.ts

### State Management
- Uses Pinia stores with Composition API syntax
- Stores are located in `src/stores/`
- Example store: `useCounterStore` in `src/stores/counter.ts`

### Testing Setup
- Unit tests use Vitest with jsdom environment
- E2E tests use Playwright with automatic browser installation
- Tests run against dev server (port 5173) locally, preview server (port 4173) on CI
- Playwright configured for Chromium, Firefox, and WebKit

### Code Quality
- ESLint with Vue 3, TypeScript, and Prettier configurations
- Automatic formatting on save recommended
- Type checking with vue-tsc
- Path alias `@/` maps to `src/` directory

### TypeScript Configuration
- **Project References**: Uses TypeScript project references with separate configs
  - `tsconfig.app.json` - Application source code
  - `tsconfig.node.json` - Node.js tooling and configuration
  - `tsconfig.vitest.json` - Vitest testing environment
- **Build Process**: `npm run build` includes both type checking and Vite build
- **Path Aliases**: `@/` → `src/` configured in both Vite and TypeScript configs

## Component Development Patterns

### Vue 3 Best Practices
- Use `<script setup>` syntax with TypeScript for components
- Prefer Composition API over Options API
- Import PrimeVue components individually: `import Button from 'primevue/button'`
- Use `defineProps()` and `defineEmits()` for component interfaces
- Leverage `computed()` and `ref()` for reactive state

### State Management with Pinia
- Store definition example (Composition API style):
```typescript
export const useExampleStore = defineStore('example', () => {
  const state = ref(initialValue)
  const getters = computed(() => derivedValue)
  const actions = function() { /* mutations */ }
  return { state, getters, actions }
})
```

### Routing Architecture
- Routes use lazy loading: `() => import('@/views/ExampleView.vue')`
- Route definitions in `src/router/index.ts`
- Navigation guards available for authentication/authorization