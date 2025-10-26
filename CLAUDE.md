# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**宝宝闯关 (Baby's Find-It Game)** is an educational web game for young children to learn English words through interactive "find the item" gameplay. Built as a Vue 3 + TypeScript single-page application with dynamic level generation.

## Development Commands

All commands should be run from the `baby-word-game` directory:

```bash
cd baby-word-game

# Development
npm run dev          # Start development server (Vite)
npm run build        # Build for production with type checking
npm run preview      # Preview production build locally

# Testing & Quality
npm run test:unit    # Run unit tests (Vitest)
npm run lint         # Run ESLint with auto-fix
npm run format       # Format code with Prettier
npm run type-check   # Run TypeScript type checking
```

## Architecture Overview

### Core Technologies
- **Vue 3** with Composition API and `<script setup>` syntax
- **TypeScript** for type safety
- **Vite** for fast development and building
- **Pinia** for state management (progress tracking)
- **Vue Router** for navigation
- **Vitest** for unit testing with jsdom environment

### Project Structure
```
baby-word-game/
├── src/
│   ├── components/          # Vue components
│   │   ├── GamePage.vue    # Main game logic and state management
│   │   ├── GameScene.vue   # Scene display and click detection
│   │   ├── ItemList.vue    # Word selection interface
│   │   └── SuccessModal.vue # Victory screen
│   ├── stores/
│   │   └── progress.ts     # Pinia store for level completion tracking
│   ├── router/
│   │   └── index.ts        # Vue Router configuration
│   ├── data/               # Theme and emoji data
│   ├── levelEngine.js      # Dynamic level generation logic
│   └── levelData.js        # Static SVG level definitions
├── public/                 # Static assets
└── dist/                   # Build output
```

### Key Architectural Patterns

1. **Dynamic Level Generation**: The game uses `levelEngine.js` to generate levels dynamically based on themes (animals, fruits, transportation, etc.) with configurable parameters:
   - `ITEMS_PER_LEVEL = 8` (items to find per level)
   - `MAX_OCCUR_PER_ITEM = 3` (maximum times an item can appear)
   - Uses seeded random generation for consistent level content

2. **Progressive Difficulty**: Levels are organized by themes with increasing complexity, stored in localStorage using Pinia store.

3. **SVG-Based Scenes**: Uses inline SVG data URLs for scene graphics with hotspot-based click detection for item finding.

4. **Component Communication**:
   - Parent-child: Props down, events up (`@found`, `@select`)
   - State management: Pinia store for cross-component progress tracking

### Game Flow
1. User selects a level from `/levels` view
2. Game loads dynamic level data via `getLevelByIndex()`
3. Scene displays SVG with clickable hotspots
4. User clicks items after selecting Chinese words
5. Progress tracked in Pinia store and localStorage
6. Victory modal shown on completion

### Testing Approach
- Unit tests use Vitest with Vue Test Utils
- Test files located in `src/components/__tests__/`
- jsdom environment for DOM testing

### Deployment
- Configured for Vercel deployment with SPA routing
- Build output goes to `dist/` directory
- Uses `vercel.json` for deployment configuration