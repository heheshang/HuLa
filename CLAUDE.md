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
- **Desktop Core**: Tauri 2.x with Rust backend for system integration
- **Database**: SQLite with Sea-ORM for local data storage and migrations
- **WebSocket**: Custom implementation with tokio-tungstenite for real-time communication
- **File Storage**: Qiniu Cloud integration for file uploads and CDN
- **External Services**: Youdao/Tencent translation APIs, AI service integrations

### Platform-Specific Architecture
- **Desktop**: Uses Tauri plugins for system integration (tray, notifications, screenshots, file system access)
- **Mobile**: Native plugins for safe area, barcode scanning, splash screen, and platform-specific features
- **Shared**: Common Vue components and business logic in `src/`
- **Configuration Management**: YAML-based configuration system with environment-specific files

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
2. Run `pnpm install` (preinstall hook validates environment and creates local config)
3. Configure environment variables in `src-tauri/configuration/local.yaml` (auto-generated from production.yaml)
4. Start development with `pnpm run tauri:dev`

#### Platform Development
- **Desktop**: Default platform, uses `127.0.0.1:6130`
- **Mobile**: Requires IP configuration for device access, development servers run on different ports (5210 for mobile)
- **Build Configurations**: Platform-specific chunk splitting and optimization

### Configuration Management
- **Local Configuration**: Edit `src-tauri/configuration/local.yaml` to modify:
  - Backend API base URL and WebSocket URL
  - Database configuration (SQLite file path)
  - Translation service settings (Youdao, Tencent)
- **Environment Switching**: Restart development server after config changes
- **Platform Detection**: Uses `TAURI_ENV_PLATFORM` environment variable for conditional loading

### Code Standards
- **Linting**: Biome for code formatting and quality
- **Type Checking**: vue-tsc for Vue TypeScript compilation
- **Commit Convention**: Conventional commits with git-cz
- **Pre-commit**: Husky with lint-staged for quality gates
- **File Structure**: Platform-specific components in `src/mobile/` vs `src/components/`
- **Import Strategy**: Auto-import for Vue composition API and component libraries

## Key Features Implementation

### Core Communication Features
- **Message Types**: Text, image, file, voice, video, location, emoji reactions
- **Chat Types**: Private chat, group chat with @mentions and replies
- **File Management**: File upload/download with Qiniu Cloud integration
- **Real-time Updates**: WebSocket implementation for instant messaging
- **Message Status**: Read receipts, message recall, forwarding

### Multi-Platform Support
- Conditional component loading based on `TAURI_ENV_PLATFORM`
- Platform-specific API integrations (Tauri plugins for desktop, native plugins for mobile)
- Responsive design with mobile-first components in `src/mobile/`
- Shared business logic and utilities in `src/`

### Plugin Architecture
- Modular feature system in `src/plugins/` (AI chat, dynamic content/community)
- Dynamic component loading with platform detection
- Shared utilities across platforms through `src/`

### State Management
- Pinia stores with persistence using pinia-plugin-persistedstate
- Cross-tab state synchronization with pinia-shared-state
- Reactive data flow with Vue 3 Composition API
- Database layer: SQLite with Sea-ORM for local data persistence

### Build Optimization
- Code splitting by platform and feature
- Tree shaking for unused dependencies
- Asset optimization and compression

## Testing Strategy
- Unit tests with Vitest and Vue Test Utils
- Coverage reporting with `@vitest/coverage-v8`
- Test configuration in `vitest.config.ts`
- Run single test: Use `pnpm run test:run -- filename.test.ts`
- Interactive test UI: `pnpm run test:ui`

## Release Process
- Automated version management with release-it
- Platform-specific build artifacts with interactive build script
- Update notification system via Tauri updater plugin
- Build commands: `pnpm run tb` (interactive) or `pnpm run tauri:build` (direct)

## Mobile-Specific Notes
- **iOS Development**: Requires macOS with Xcode and iOS development tools
- **Android Development**: Requires Android Studio and SDK setup
- **Platform Detection**: Use `TAURI_ENV_PLATFORM` environment variable in code
- **Component Structure**: Mobile-specific components in `src/mobile/`, shared in `src/`
- **Build Targets**: Use `pnpm run tauri:android:dev` or `pnpm run tauri:ios:dev`