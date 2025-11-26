# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

HuLa is a cross-platform instant messaging system built with Tauri, Vue 3, TypeScript, and Vite 7. It supports Windows, macOS, Linux, iOS, and Android platforms, providing features like user authentication, chat (private and group), file sharing, theme switching, and AI integration.

## Development Commands

### Core Development
- `pnpm run tauri:dev` - Start desktop application in development mode
- `pnpm run td` - Simplified command for tauri dev
- `pnpm run dev` - Start Vue development server (desktop platform)
- `pnpm run tauri:build` - Build desktop application for production

### Mobile Development
- `pnpm run tauri:ios:dev` - Start iOS application development
- `pnpm run tauri:android:dev` - Start Android application development
- `pnpm run idev:mac` - Start iOS frontend on macOS
- `pnpm run adev:win` - Start Android frontend on Windows

### Code Quality & Testing
- `pnpm run check` - Check code issues and formatting (read-only)
- `pnpm run check:write` - Fix code issues and formatting
- `pnpm run lint:staged` - Run pre-commit code checks
- `pnpm run test:run` - Run unit tests
- `pnpm run test:ui` - Run tests with Vitest UI
- `pnpm run coverage` - Generate test coverage report

### Build & Release
- `pnpm run build` - Build Vue frontend (type check + Vite build)
- `pnpm run tb` - Interactive build with platform selection
- `pnpm run release` - Create new release with release-it

### Git Operations
- `pnpm run commit` - Interactive commit with git-cz
- `pnpm run gitcz` - Alternative commit command

## Architecture

### Frontend Structure
- **Framework**: Vue 3 with Composition API and TypeScript
- **Build Tool**: Vite 7 with custom configuration for cross-platform support
- **UI Libraries**: Naive UI (desktop), Vant (mobile)
- **Styling**: UnoCSS with atomic CSS, SCSS for complex styles
- **State Management**: Pinia with persistence
- **Routing**: Vue Router 4
- **Testing**: Vitest with Vue Test Utils

### Backend Structure
- **Desktop Core**: Tauri 2.x with Rust backend
- **Database**: SQLite with Sea-ORM
- **WebSocket**: Custom implementation with tokio-tungstenite
- **File Storage**: Qiniu Cloud integration

### Platform-Specific Architecture
- **Desktop**: Uses Tauri plugins for system integration (tray, notifications, screenshots)
- **Mobile**: Native plugins for safe area, barcode scanning, and platform-specific features
- **Shared**: Common Vue components and business logic

### Key Directories
- `src/` - Main Vue.js application source
- `src-tauri/` - Rust backend code and Tauri configuration
- `src/mobile/` - Mobile-specific Vue components
- `src/plugins/` - Feature modules (AI chat, dynamic content)
- `tauri-plugin-hula/` - Custom Tauri plugins
- `build/` - Build configuration and scripts

## Development Workflow

### Environment Setup
1. Use Node.js 20.19.0+ and pnpm 10.x
2. Run `pnpm install` (preinstall hook validates environment)
3. Configure environment variables in `.env`
4. Start development with `pnpm run tauri:dev`

### Platform Development
- **Desktop**: Default platform, uses `127.0.0.1:6130`
- **Mobile**: Requires IP configuration for device access
- **Build Configurations**: Platform-specific chunk splitting and optimization

### Code Standards
- **Linting**: Biome for code formatting and quality
- **Type Checking**: vue-tsc for Vue TypeScript compilation
- **Commit Convention**: Conventional commits with git-cz
- **Pre-commit**: Husky with lint-staged for quality gates

## Key Features Implementation

### Multi-Platform Support
- Conditional component loading based on `TAURI_ENV_PLATFORM`
- Platform-specific API integrations
- Responsive design with mobile-first components

### Plugin Architecture
- Modular feature system in `src/plugins/`
- Dynamic component loading
- Shared utilities across platforms

### State Management
- Pinia stores with persistence
- Cross-tab state synchronization
- Reactive data flow

### Build Optimization
- Code splitting by platform and feature
- Tree shaking for unused dependencies
- Asset optimization and compression

## Testing Strategy
- Unit tests with Vitest
- Component testing with Vue Test Utils
- Coverage reporting with `@vitest/coverage-v8`
- Test configuration in `vitest.config.ts`

## Release Process
- Automated version management with release-it
- Platform-specific build artifacts
- Update notification system via Tauri updater plugin