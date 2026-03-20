# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

plugNmeet-client is the React frontend for [plugNmeet-server](https://github.com/mynaparrot/plugNmeet-server), a real-time video conferencing platform. Features include video/audio streaming (LiveKit), chat, whiteboard (Excalidraw), polls, breakout rooms, shared notepad, translation/transcription, and AI insights.

## Commands

```bash
# Install dependencies (pnpm is enforced — npm/yarn will fail)
pnpm install

# Dev server at http://localhost:3000
pnpm start

# Production build to dist/
pnpm run build

# Lint (oxlint with auto-fix + prettier auto-format)
pnpm run lint

# CI checks (no auto-fix)
pnpm run lint:check
pnpm run format:check
```

## Setup

Copy `src/assets/config_sample.js` to `src/assets/config.js` and configure it before running the dev server. The login page is at `/login.html`.

## Architecture

- **React 19 + TypeScript** with Vite 8 as the build tool
- **State management**: Redux Toolkit with slices in `src/store/slices/` and RTK Query services in `src/store/services/`
- **Real-time messaging**: NATS (connection and handlers in `src/helpers/nats/`)
- **Video streaming**: LiveKit client integration in `src/helpers/livekit/`
- **Styling**: Tailwind CSS v4 with custom CSS layers in `src/styles/` (including dark mode and responsive breakpoints in `media_screens/`)
- **i18n**: i18next with 34 languages, locale files in `src/assets/locales/`
- **Protocol types**: Generated from `plugnmeet-protocol-js` package (protobuf-based)

## Key Directories

- `src/components/` — Feature-based React components (app, chat, whiteboard, polls, breakout-room, media-elements, etc.)
- `src/store/` — Redux store config, slices (session, participants, chat, room settings, etc.), and RTK Query APIs
- `src/helpers/` — API client (`api/plugNmeetAPI.ts`), custom hooks (`hooks/`), LiveKit/NATS integrations, utilities
- `src/assets/` — Static assets: config, icons, locales, audio, backgrounds, TensorFlow models for video processing

## Code Quality

- **Linter**: oxlint with react, unicorn, typescript, oxc plugins (config in `.oxlintrc.json`)
- **Formatter**: Prettier with `@prettier/plugin-oxc` (config in `.prettierrc.json` — single quotes, 2-space indent, trailing commas)
- **Pre-commit hook**: Husky runs Prettier + oxlint auto-fix on staged files
- **CI**: PR checks run lint:check, format:check, and build on Node LTS with pnpm 10
- **TypeScript**: Strict mode enabled with `noImplicitAny: false`
- **No unit test framework** — CI validates via lint + format + build checks

## Build Notes

- Vite config uses manual chunk splitting for large dependencies (mermaid, excalidraw, react-libs, pnm vendor)
- Static assets (config, locales, audio, backgrounds) are copied to dist via `vite-plugin-static-copy`
- Source maps are generated only in non-production builds
