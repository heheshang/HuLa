# HuLa Instant Messaging - Developer Guide

## Table of Contents

1. [Project Overview](#project-overview)
2. [Project Structure](#project-structure)
3. [API Documentation](#api-documentation)
4. [Component Documentation](#component-documentation)
5. [Development Workflow](#development-workflow)
6. [Platform-Specific Development](#platform-specific-development)
7. [Testing and Quality](#testing-and-quality)
8. [Deployment and Release](#deployment-and-release)

## Project Overview

HuLa is a cross-platform instant messaging application built with modern web technologies and Tauri for native performance. The application supports desktop (Windows, macOS, Linux) and mobile (iOS, Android) platforms with a unified codebase.

### Technology Stack

**Frontend:**
- **Vue 3** - Progressive JavaScript framework with Composition API
- **TypeScript** - Type-safe JavaScript development
- **Vite 7** - Fast build tool and development server
- **Pinia** - State management with persistence
- **UnoCSS** - Atomic CSS framework
- **SCSS** - CSS preprocessor

**Desktop UI:**
- **Naive UI** - Vue 3 component library for desktop

**Mobile UI:**
- **Vant** - Mobile UI component library

**Backend/Core:**
- **Tauri 2.x** - Rust-based app framework for cross-platform development
- **Rust** - Systems programming language for core functionality
- **SQLite** - Local database with Sea-ORM
- **WebSocket** - Real-time communication

**Development Tools:**
- **Vitest** - Unit testing with Vue Test Utils
- **Biome** - Code formatting and linting
- **Commitizen** - Conventional commit messages
- **Husky** - Git hooks for quality control

### Supported Platforms

- **Desktop:** Windows, macOS, Linux
- **Mobile:** iOS, Android
- **Architecture:** Single codebase with platform-specific optimizations

## Key Features

- **Real-time messaging** with WebSocket support
- **File sharing** (images, documents, audio, video)
- **Voice and video calling**
- **AI assistant integration**
- **Community features** (dynamic content, announcements)
- **Cross-platform synchronization**
- **Offline support** with local database
- **Plugin architecture** for extensibility

---

## Getting Started

See individual sections for detailed setup instructions:

1. **[Project Structure](#project-structure)** - Understanding the codebase architecture
2. **[Development Environment Setup](#development-environment-setup)** - Install dependencies and tools
3. **[API Documentation](#api-documentation)** - Backend and frontend APIs
4. **[Component Library](#component-documentation)** - Reusable UI components
5. **[Development Workflow](#development-workflow)** - Build, test, and deployment processes

## Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│                    HuLa Architecture                      │
├─────────────────────────────────────────────────────────┤
│  Frontend (Vue 3 + TypeScript)                        │
│  ├─ Desktop UI (Naive UI)                             │
│  ├─ Mobile UI (Vant)                                  │
│  ├─ State Management (Pinia)                           │
│  └─ Services (WebSocket, API, File Management)          │
├─────────────────────────────────────────────────────────┤
│  Core Layer (Tauri + Rust)                            │
│  ├─ Cross-platform APIs                               │
│  ├─ Native Functionality                              │
│  ├─ Database Integration (SQLite)                     │
│  └─ WebSocket Client                                 │
└─────────────────────────────────────────────────────────┘
           ↓                              ↓
    Desktop Platforms              Mobile Platforms
(Windows, macOS, Linux)           (iOS, Android)
```

## Next Steps

Continue reading through this documentation to understand:

1. How the project is structured and organized
2. API interfaces for different functionality
3. Available components and how to use them
4. Development processes and best practices
5. Platform-specific considerations

---

*This documentation is actively maintained. Contributions are welcome!*