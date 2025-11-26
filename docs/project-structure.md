# HuLa Project Structure Documentation

## 📁 Overview

HuLa is a sophisticated cross-platform instant messaging system built with modern web technologies. This documentation provides a comprehensive guide to understanding the project architecture, component organization, and development workflows.

## 🏗️ Architecture Overview

### Frontend Stack
- **Framework**: Vue 3 with Composition API
- **Language**: TypeScript for type safety
- **Build Tool**: Vite 7 for fast development and optimized builds
- **UI Libraries**:
  - Desktop: Naive UI for modern, accessible components
  - Mobile: Vant for mobile-optimized UI components
- **Styling**: UnoCSS for atomic CSS + SCSS for complex styles
- **State Management**: Pinia with persistence for reactive state
- **Testing**: Vitest with Vue Test Utils for unit and component testing

### Backend Stack
- **Desktop Runtime**: Tauri 2.x with Rust backend
- **Database**: SQLite with Sea-ORM for data persistence
- **Real-time Communication**: Custom WebSocket implementation
- **File Storage**: Qiniu Cloud integration
- **Authentication**: Custom system with QR code support

### Platform Support
- **Desktop**: Windows, macOS, Linux (via Tauri)
- **Mobile**: iOS, Android (via Tauri mobile)

## 📂 Directory Structure

### Root Level
```
HuLa/
├── 📁 src/                    # Main Vue.js application
├── 📁 src-tauri/             # Rust backend and Tauri configuration
├── 📁 src/mobile/              # Mobile-specific Vue components
├── 📁 public/                   # Static assets
├── 📁 build/                    # Build configuration and scripts
├── 📁 docs/                     # Project documentation
├── 📁 scripts/                  # Build and utility scripts
├── 📁 tauri-plugin-hula/      # Custom Tauri plugins
├── 📁 preview/                  # Preview images and assets
└── 📄 package.json              # Project dependencies and scripts
```

### Source Code Structure

#### Frontend Core (`src/`)
```
src/
├── 📁 components/              # Reusable Vue components
│   ├── 📁 common/              # Desktop common components
│   │   ├── AvatarCropper.vue     # Avatar cropping functionality
│   │   ├── LoadingSpinner.vue   # Loading state indicator
│   │   └── VirtualList.vue      # Virtual scrolling component
│   ├── 📁 fileManager/         # File management components
│   │   ├── FileContent.vue      # File preview and display
│   │   ├── UserItem.vue         # User file listing
│   │   └── UserList.vue         # User file management
│   └── 📁 rightBox/           # Chat and interaction components
│       ├── chatBox/            # Chat interface components
│       ├── renderMessage/       # Message rendering by type
│       └── location/            # Location sharing components
├── 📁 hooks/                   # Vue composition functions
│   ├── useAudioManager.ts          # Audio playback and recording
│   ├── useFileUploadQueue.ts      # File upload management
│   ├── useNetworkStatus.ts        # Network connection monitoring
│   ├── useTauriListener.ts       # Tauri event handling
│   └── useWebSocketAdapter.ts     # WebSocket communication
├── 📁 services/                # API and external service integrations
│   ├── tauriCommand.ts           # Tauri backend commands
│   ├── translate.ts               # Translation services
│   └── webSocketAdapter.ts        # WebSocket abstraction layer
├── 📁 stores/                  # Pinia state management
│   ├── contacts.ts                # Contact and friend management
│   ├── file.ts                    # File transfer state
│   ├── group.ts                   # Group chat management
│   └── user.ts                    # User authentication and profile
├── 📁 utils/                   # Utility functions and helpers
│   ├── AudioCompression.ts        # Audio processing utilities
│   ├── FileUtil.ts                # File handling helpers
│   └── MessageSelect.ts           # Message selection and operations
├── 📁 plugins/                 # Feature-specific modules
│   ├── dynamic/                   # Dynamic content features
│   └── robot/                     # AI chat assistant features
├── 📁 mobile/                  # Mobile-specific components
│   ├── components/               # Mobile-optimized UI
│   ├── layout/                   # Mobile layout components
│   └── views/                    # Mobile page components
├── 📁 router/                  # Vue Router configuration
├── 📁 typings/                 # TypeScript type definitions
└── App.vue                    # Root Vue component
```

#### Backend Core (`src-tauri/`)
```
src-tauri/
├── 📄 Cargo.toml               # Rust project configuration
├── 📄 tauri.conf.json          # Tauri application configuration
├── 📁 src/                      # Rust source code
│   ├── main.rs                  # Tauri application entry point
│   ├── commands/                # Tauri command handlers
│   ├── plugins/                 # Custom Tauri plugins
│   └── entities/                # Database entity definitions
└── 📁 migration/                # Database migration scripts
```

## 🔧 Key Components

### Chat System Components

#### Message Rendering (`src/components/rightBox/renderMessage/`)
- **Text.vue**: Standard text message display
- **Image.vue**: Image message with preview and zoom
- **File.vue**: File attachment with download/upload progress
- **Audio.vue**: Voice message player and recorder
- **VideoCall.vue**: Video call interface
- **Location.vue**: Location sharing with map integration
- **Emoji.vue**: Emoji rendering and picker integration

#### Chat Interface (`src/components/rightBox/chatBox/`)
- **Chat.vue**: Main chat interface with message list
- **ChatMultiMsg.vue**: Multi-message handling (forward/reply)
- **HuLaAssistant.vue**: AI assistant integration

### File Management System

#### File Components (`src/components/fileManager/`)
- **UserList.vue**: File browser with user/group filtering
- **UserItem.vue**: Individual file item with actions
- **FileContent.vue**: File preview and media viewer
- **SideNavigation.vue**: File category navigation
- **EmptyState.vue**: Empty file list placeholder

#### Upload System (`src/components/rightBox/`)
- **FileUploadModal.vue**: Drag-and-drop file upload
- **FileUploadProgress.vue**: Upload progress tracking
- **VoiceRecorder.vue**: Voice message recording interface

### Mobile-Specific Components

#### Layout (`src/mobile/layout/`)
- **ChatRoomLayout.vue**: Chat interface for mobile
- **FriendsLayout.vue**: Contact list layout
- **MyLayout.vue**: User profile and settings layout
- **index.vue**: Main mobile layout controller

#### Views (`src/mobile/views/`)
- **chat-room/**: Chat-specific mobile views
- **friends/**: Contact management views
- **my/**: User profile and settings views
- **community/**: Community and group features

## 🔌 Plugin Architecture

### Dynamic Content Plugin (`src/plugins/dynamic/`)
- **index.vue**: Dynamic content management interface
- **detail.vue**: Content detail view and editing

### Robot/AI Assistant Plugin (`src/plugins/robot/`)
- **index.vue**: Main AI chat interface
- **components/**:
  - **ChatRoleManagement.vue**: AI role and personality management
  - **ModelManagement.vue**: AI model selection and configuration
  - **ChatRoleSelector.vue**: Quick role switching
  - **ApiKeyManagement.vue**: API key management for different services

## 🗃 State Management (Pinia Stores)

### Core Stores (`src/stores/`)
- **user.ts**: User authentication, profile, and preferences
- **contacts.ts**: Friend list, contact management, and online status
- **group.ts**: Group chat management, member lists, and permissions
- **file.ts**: File transfer state, upload/download queues
- **config.ts**: Application configuration and user settings
- **ws.ts**: WebSocket connection state and message handling
- **bot.ts**: AI assistant configuration and conversation history
- **emoji.ts**: Emoji package management and custom emojis
- **announcement.ts**: System announcements and notifications

### Specialized Stores
- **downloadQueue.ts**: File download queue management
- **fileDownload.ts**: Active download state tracking
- **imageViewer.ts**: Image viewing state and history
- **videoViewer.ts**: Video player state and controls
- **alwaysOnTop.ts**: Window management for desktop
- **networkStatus.ts**: Connection quality and status monitoring

## 🔌 Service Layer

### API Services (`src/services/`)
- **tauriCommand.ts**: Tauri backend command interface
- **webSocketAdapter.ts**: WebSocket connection management
- **translate.ts**: Multi-language translation service
- **mapApi.ts**: Location and mapping services
- **types.ts**: TypeScript type definitions for APIs

### WebSocket Services
- **webSocketRust.ts**: Rust-based WebSocket implementation
- **wsType.ts**: WebSocket message type definitions
- Real-time message handling with reconnection logic
- Message queuing and offline support

## 🎣 Hooks and Composables

### Core Hooks (`src/hooks/`)
- **useNetworkStatus.ts**: Network connectivity monitoring
- **useTauriListener.ts**: Tauri event subscription handling
- **useFileUploadQueue.ts**: File upload queue management
- **useAudioManager.ts**: Audio recording and playback
- **useGeolocation.ts**: Location services integration
- **useImageViewer.ts**: Image viewing and zoom functionality
- **useVideoViewer.ts**: Video playback and controls
- **useVoiceRecordRust.ts**: Voice recording with Rust backend

### UI Hooks
- **useChatLayout.ts**: Chat interface responsive layout
- **useViewport.ts**: Viewport size and orientation detection
- **usePopover.ts**: Popover and dropdown management
- **useTrigger.ts**: Event trigger and gesture handling

## 🔧 Build System

### Vite Configuration (`vite.config.ts`)
- **Platform Detection**: Automatic configuration based on `TAURI_ENV_PLATFORM`
- **Component Auto-Import**: Automatic Vue component registration
- **Path Aliases**: Intelligent path resolution (`@/` for src, `#/` for mobile)
- **CSS Processing**: PostCSS with px-to-rem conversion, UnoCSS integration
- **Code Splitting**: Platform-specific chunk optimization
- **Development Server**: Network-aware configuration for mobile development

### Build Configuration (`build/`)
- **Manual Chunks**: Intelligent bundle splitting by dependencies
- **Path Resolution**: Platform-specific path handling
- **Component Discovery**: Automatic component directory detection
- **Development vs Production**: Environment-specific optimizations

## 📱 Platform-Specific Features

### Desktop Features
- **Multi-Window Management**: Multiple chat windows and panels
- **System Tray Integration**: Background operation and notifications
- **Screenshot Capture**: Built-in screenshot functionality
- **File Association**: Handle file types and deep links
- **Auto-Update**: Tauri updater integration
- **Global Shortcuts**: System-wide hotkeys

### Mobile Features
- **Safe Area Insets**: Notch and gesture area handling
- **Barcode Scanner**: QR code scanning for adding contacts
- **Push Notifications**: Native notification handling
- **Background Sync**: Background data synchronization
- **Gesture Navigation**: Mobile-optimized navigation patterns

## 🧪 Testing Strategy

### Test Configuration (`vitest.config.ts`)
- **Component Testing**: Vue Test Utils integration
- **Coverage Reporting**: `@vitest/coverage-v8` for comprehensive coverage
- **Test UI**: Vitest UI for interactive test development
- **Happy DOM**: Lightweight DOM implementation for testing

### Test Structure
- **Unit Tests**: Individual component and function testing
- **Integration Tests**: Component interaction testing
- **E2E Testing**: Full user flow testing (planned)
- **Performance Tests**: Bundle size and runtime performance

## 🔧 Development Workflow

### Environment Setup
1. **Prerequisites**: Node.js 20.19.0+, pnpm 10.x
2. **Installation**: `pnpm install` with pre-commit hook validation
3. **Configuration**: Environment-specific configuration via `.env` files
4. **Development**: Platform-specific development servers

### Development Commands
- **Desktop**: `pnpm run tauri:dev` or `pnpm run td`
- **Mobile iOS**: `pnpm run tauri:ios:dev` or `pnpm run idev:mac`
- **Mobile Android**: `pnpm run tauri:android:dev` or `pnpm run adev:win`
- **Code Quality**: `pnpm run check` (read-only) / `pnpm run check:write` (auto-fix)
- **Testing**: `pnpm run test:run` / `pnpm run test:ui`

### Build Process
- **Frontend Build**: Vue compilation with `vue-tsc --noEmit && vite build`
- **Desktop Build**: Tauri bundling with platform-specific optimization
- **Mobile Build**: Native app compilation with platform-specific configurations
- **Interactive Build**: `pnpm run tb` for guided build process

## 📊 Cross-References

### Related Components
- **Chat System**: `chatBox/` ↔ `renderMessage/` ↔ `stores/ws.ts`
- **File Management**: `fileManager/` ↔ `hooks/useFileUploadQueue.ts` ↔ `stores/file.ts`
- **Mobile Layout**: `mobile/layout/` ↔ `mobile/components/` ↔ `hooks/useViewport.ts`
- **AI Integration**: `plugins/robot/` ↔ `stores/bot.ts` ↔ `services/translate.ts`

### Data Flow
- **WebSocket Messages**: `services/webSocketAdapter.ts` → `stores/ws.ts` → UI components
- **File Operations**: `hooks/useFileUploadQueue.ts` → `services/tauriCommand.ts` → Tauri backend
- **User Actions**: UI components → Pinia stores → Tauri commands → Backend services

### Platform Integration
- **Desktop Specific**: `src-tauri/` plugins ↔ `hooks/useTauriListener.ts`
- **Mobile Specific**: Mobile components ↔ Platform APIs via Tauri mobile plugins
- **Shared Logic**: Common utilities and stores used across all platforms

---

This documentation serves as the foundation for understanding HuLa's architecture. For specific API documentation and implementation details, refer to the individual component and service files.