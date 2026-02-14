# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

This project uses **bun** as the package manager.

```bash
# Install dependencies
bun install

# Start development server with hot-reload
bun dev

# Type-check, compile and minify for production
bun run build

# Preview production build locally
bun run preview

# Run linters (oxlint + eslint with auto-fix)
bun lint

# Format code with oxfmt
bun run format
```

To type-check without building: `bun run type-check`

## Tech Stack & Architecture

- **Vue 3** with Composition API (`<script setup>`)
- **TypeScript** (strict mode)
- **Vite** for build tooling
- **Pinia** for state management
- **Vue Router** for routing
- **Tailwind CSS v4** (via Vite plugin)

### Project Structure

```
src/
├── App.vue           # Root component
├── main.ts           # Application entry point
├── router/index.ts   # Vue Router configuration
├── stores/           # Pinia stores
└── assets/           # Static assets (CSS, images, etc.)
```

### Path Aliases

`@` is aliased to `./src` in Vite config. Use it for imports:
```ts
import { something } from '@/stores/counter'
```

## Code Style

The project uses a dual-linting setup:

1. **oxlint** (primary linter) - Fast Rust-based linter with plugins for eslint, typescript, unicorn, oxc, and vue
2. **ESLint** with `@vue/eslint-config-typescript` - Additional Vue-specific linting

Formatting is handled by **oxfmt**:
- No semicolons
- Single quotes for strings
- Auto-fix on lint: `bun lint`

Configuration files:
- `.oxlintrc.json` - oxlint rules (correctness set to error level)
- `.oxfmtrc.json` - oxfmt formatting rules
- `eslint.config.ts` - ESLint configuration (imports oxlint config)

## Node Version

Required: `^20.19.0 || >=22.12.0` (specified in package.json engines)
