# HuLa API Documentation

## 🌐 WebSocket API

### Connection Management

#### WebSocket Adapter (`src/services/webSocketAdapter.ts`)
```typescript
class WebSocketAdapter {
  // Connection lifecycle
  connect(url: string, protocols?: string[]): Promise<void>
  disconnect(code?: number, reason?: string): void
  reconnect(): Promise<void>

  // Message handling
  send(message: WebSocketMessage): void
  sendHeartbeat(): void

  // Event handling
  onMessage(callback: (message: WebSocketMessage) => void): void
  onOpen(callback: () => void): void
  onClose(callback: (event: CloseEvent) => void): void
  onError(callback: (error: Event) => void): void
}
```

#### WebSocket Message Types (`src/services/wsType.ts`)
```typescript
interface WebSocketMessage {
  type: 'message' | 'system' | 'heartbeat' | 'file' | 'call'
  data: any
  timestamp: number
  messageId?: string
  userId?: string
  groupId?: string
}

interface ChatMessage extends WebSocketMessage {
  type: 'message'
  content: string
  messageType: 'text' | 'image' | 'file' | 'audio' | 'video' | 'location'
  replyTo?: string
  mentions?: string[]
  atUsers?: string[]
}

interface FileMessage extends WebSocketMessage {
  type: 'file'
  fileInfo: {
    name: string
    size: number
    type: string
    url: string
    thumbnail?: string
  }
}
```

## 🔧 Tauri Commands API

### File System Commands (`src/services/tauriCommand.ts`)

#### File Operations
```typescript
// File reading and writing
export const readTextFile = async (path: string): Promise<string> =>
  invoke('read_text_file', { path })

export const writeTextFile = async (path: string, content: string): Promise<void> =>
  invoke('write_text_file', { path, content })

export const readFile = async (path: string): Promise<ArrayBuffer> =>
  invoke('read_file', { path })

export const writeFile = async (path: string, data: ArrayBuffer): Promise<void> =>
  invoke('write_file', { path, data: Array.from(data) })
```

#### Directory Operations
```typescript
// Directory management
export const createDirectory = async (path: string): Promise<void> =>
  invoke('create_directory', { path })

export const listDirectory = async (path: string): Promise<string[]> =>
  invoke('list_directory', { path })

export const exists = async (path: string): Promise<boolean> =>
  invoke('path_exists', { path })
```

#### File System Watching
```typescript
// Watch for file changes
export const watchFile = async (path: string, callback: (event: FileEvent) => void): Promise<void> =>
  invoke('watch_file', { path })

interface FileEvent {
  type: 'created' | 'modified' | 'deleted'
  path: string
  timestamp: number
}
```

### System Commands

#### Window Management
```typescript
export const minimizeWindow = async (): Promise<void> =>
  invoke('window_minimize')

export const maximizeWindow = async (): Promise<void> =>
  invoke('window_maximize')

export const closeWindow = async (): Promise<void> =>
  invoke('window_close')

export const isWindowMaximized = async (): Promise<boolean> =>
  invoke('is_window_maximized')
```

#### System Integration
```typescript
// System tray and notifications
export const showNotification = async (title: string, body: string): Promise<void> =>
  invoke('show_notification', { title, body })

export const setTrayIcon = async (iconPath: string): Promise<void> =>
  invoke('set_tray_icon', { iconPath })

// Screenshot capture
export const captureScreenshot = async (): Promise<string> =>
  invoke('capture_screenshot')
```

#### Audio/Video Commands
```typescript
// Audio recording and playback
export const startAudioRecording = async (): Promise<void> =>
  invoke('start_audio_recording')

export const stopAudioRecording = async (): Promise<AudioData> =>
  invoke('stop_audio_recording')

export const playAudio = async (audioData: ArrayBuffer): Promise<void> =>
  invoke('play_audio', { data: Array.from(audioData) })

interface AudioData {
  data: number[]
  format: 'wav' | 'mp3' | 'ogg'
  duration: number
  sampleRate: number
}
```

## 📱 Mobile-Specific APIs

### Safe Area Handling
```typescript
// Safe area insets for mobile devices
export const getSafeAreaInsets = async (): Promise<SafeAreaInsets> =>
  invoke('get_safe_area_insets')

interface SafeAreaInsets {
  top: number
  right: number
  bottom: number
  left: number
}
```

### Barcode Scanner
```typescript
// QR code and barcode scanning
export const startBarcodeScanner = async (): Promise<void> =>
  invoke('start_barcode_scanner')

export const stopBarcodeScanner = async (): Promise<void> =>
  invoke('stop_barcode_scanner')

export const onBarcodeScanned = (callback: (result: BarcodeResult) => void): void =>
  listen('barcode-scanned', callback)

interface BarcodeResult {
  type: 'qr' | 'barcode'
  data: string
  format?: string
}
```

### Push Notifications
```typescript
// Mobile push notification handling
export const requestNotificationPermission = async (): Promise<boolean> =>
  invoke('request_notification_permission')

export const showPushNotification = async (notification: PushNotification): Promise<void> =>
  invoke('show_push_notification', { notification })

interface PushNotification {
  title: string
  body: string
  icon?: string
  data?: any
}
```

## 🔐 Authentication API

### User Authentication
```typescript
// Login and authentication services
export const loginWithPassword = async (credentials: LoginCredentials): Promise<AuthResult> =>
  invoke('login_with_password', { credentials })

export const loginWithQRCode = async (qrData: string): Promise<AuthResult> =>
  invoke('login_with_qrcode', { qrData })

export const logout = async (): Promise<void> =>
  invoke('logout')

export const refreshToken = async (): Promise<string> =>
  invoke('refresh_token')

interface LoginCredentials {
  username: string
  password: string
  deviceInfo?: DeviceInfo
}

interface AuthResult {
  success: boolean
  token?: string
  refreshToken?: string
  userInfo?: UserInfo
  expiresIn?: number
}

interface UserInfo {
  id: string
  username: string
  nickname?: string
  avatar?: string
  status: 'online' | 'offline' | 'away' | 'busy'
}
```

### Device Management
```typescript
// Multi-device login management
export const getDeviceList = async (): Promise<DeviceInfo[]> =>
  invoke('get_device_list')

export const removeDevice = async (deviceId: string): Promise<void> =>
  invoke('remove_device', { deviceId })

export const setCurrentDevice = async (deviceName: string): Promise<void> =>
  invoke('set_current_device', { deviceName })

interface DeviceInfo {
  id: string
  name: string
  type: 'desktop' | 'mobile' | 'web'
  platform: string
  lastActive: number
  isCurrent: boolean
}
```

## 📁 File Management API

### File Upload
```typescript
// File upload with progress tracking
export const uploadFile = async (
  file: File,
  options?: UploadOptions
): Promise<UploadResult> => {
  // Use FileUploadQueue hook for progress tracking
  return new Promise((resolve, reject) => {
    const uploadId = generateId()
    // Upload implementation with progress callbacks
  })
}

interface UploadOptions {
  onProgress?: (progress: UploadProgress) => void
  chunkSize?: number
  retryCount?: number
  compress?: boolean
}

interface UploadProgress {
  loaded: number
  total: number
  percentage: number
  speed: number
  remainingTime?: number
}

interface UploadResult {
  success: boolean
  fileId?: string
  url?: string
  error?: string
}
```

### File Download
```typescript
// File download with resume capability
export const downloadFile = async (
  url: string,
  options?: DownloadOptions
): Promise<DownloadResult> =>
  invoke('download_file', { url, options })

interface DownloadOptions {
  savePath?: string
  onProgress?: (progress: DownloadProgress) => void
  resumeFrom?: number
  timeout?: number
}

interface DownloadProgress {
  downloaded: number
  total: number
  percentage: number
  speed: number
  remainingTime?: number
}

interface DownloadResult {
  success: boolean
  filePath?: string
  error?: string
}
```

## 🎥 Media Processing API

### Image Processing
```typescript
// Image manipulation and optimization
export const processImage = async (
  imageSource: ArrayBuffer,
  operations: ImageOperations
): Promise<ProcessedImage> =>
  invoke('process_image', { imageSource: Array.from(imageSource), operations })

interface ImageOperations {
  resize?: { width: number; height: number }
  crop?: { x: number; y: number; width: number; height: number }
  rotate?: number
  quality?: number // 0-100
  format?: 'jpeg' | 'png' | 'webp'
  watermark?: WatermarkOptions
}

interface WatermarkOptions {
  text?: string
  position?: 'top-left' | 'top-right' | 'bottom-left' | 'bottom-right'
  opacity?: number
  fontSize?: number
}

interface ProcessedImage {
  data: ArrayBuffer
  width: number
  height: number
  format: string
  size: number
}
```

### Audio Processing
```typescript
// Audio compression and format conversion
export const processAudio = async (
  audioSource: ArrayBuffer,
  targetFormat: 'mp3' | 'wav' | 'ogg'
): Promise<ProcessedAudio> =>
  invoke('process_audio', {
    audioSource: Array.from(audioSource),
    targetFormat
  })

interface ProcessedAudio {
  data: ArrayBuffer
  format: string
  duration: number
  size: number
  bitRate?: number
  sampleRate: number
}
```

## 🗺️ Location Services API

### Geolocation
```typescript
// Location sharing and mapping
export const getCurrentLocation = async (): Promise<LocationData> =>
  invoke('get_current_location')

export const watchLocation = async (
  callback: (location: LocationData) => void
): Promise<void> =>
  invoke('watch_location')

interface LocationData {
  latitude: number
  longitude: number
  accuracy: number
  altitude?: number
  timestamp: number
  address?: string
}
```

### Map Integration
```typescript
// Map services integration
export const searchLocation = async (query: string): Promise<LocationResult[]> =>
  invoke('search_location', { query })

export const getStaticMap = async (
  location: LocationData,
  options: MapOptions
): Promise<string> => // Returns map image URL
  invoke('get_static_map', { location, options })

interface MapOptions {
  width?: number
  height?: number
  zoom?: number
  mapType?: 'roadmap' | 'satellite' | 'hybrid'
  markers?: LocationMarker[]
}

interface LocationMarker {
  latitude: number
  longitude: number
  label?: string
  color?: string
}
```

## 🤖 AI Integration API

### Translation Services
```typescript
// Multi-language translation
export const translateText = async (
  text: string,
  from: string,
  to: string
): Promise<TranslationResult> =>
  invoke('translate_text', { text, from, to })

export const detectLanguage = async (text: string): Promise<string> =>
  invoke('detect_language', { text })

interface TranslationResult {
  originalText: string
  translatedText: string
  fromLanguage: string
  toLanguage: string
  confidence: number
  provider: 'google' | 'baidu' | 'azure'
}
```

### AI Chat Assistant
```typescript
// AI assistant integration
export const sendToAI = async (
  message: string,
  context?: AIContext
): Promise<AIResponse> =>
  invoke('send_to_ai', { message, context })

export const getAIResponse = async (
  messageId: string
): Promise<AIResponse> =>
  invoke('get_ai_response', { messageId })

interface AIContext {
  conversationId?: string
  previousMessages?: ChatMessage[]
  systemPrompt?: string
  personality?: string
}

interface AIResponse {
  id: string
  message: string
  timestamp: number
  confidence: number
  provider: 'openai' | 'claude' | 'local'
  tokensUsed?: number
}
```

## 🔔 Notification API

### System Notifications
```typescript
// Cross-platform notification system
export const showNotification = async (
  notification: NotificationOptions
): Promise<NotificationResult> =>
  invoke('show_notification', { notification })

interface NotificationOptions {
  title: string
  body: string
  icon?: string
  image?: string
  actions?: NotificationAction[]
  priority?: 'default' | 'high' | 'low'
  timeout?: number
  silent?: boolean
}

interface NotificationAction {
  id: string
  title: string
  icon?: string
}

interface NotificationResult {
  id: string
  shown: boolean
  clicked?: boolean
  actionId?: string
}
```

## 🎨 UI Component APIs

### Component Auto-Import
```typescript
// Automatic component registration system
export const registerComponent = (name: string, component: Component): void =>
  // Components are auto-registered via unplugin-vue-components
  // Custom registration available for dynamic components
```

### Theme System
```typescript
// Theme management and customization
export const setTheme = (theme: ThemeConfig): void => {
  // Use Pinia config store for theme persistence
  configStore.setTheme(theme)
}

export interface ThemeConfig {
  mode: 'light' | 'dark' | 'auto'
  primaryColor: string
  accentColor: string
  customCSS?: string
  compactMode?: boolean
}
```

## 📊 Performance Monitoring API

### Application Metrics
```typescript
// Performance and usage analytics
export const trackPerformance = async (
  metric: PerformanceMetric
): Promise<void> =>
  invoke('track_performance', { metric })

interface PerformanceMetric {
  type: 'memory' | 'cpu' | 'network' | 'render'
  value: number
  unit: string
  timestamp: number
  context?: string
}

export const getPerformanceStats = async (): Promise<PerformanceStats> =>
  invoke('get_performance_stats')

interface PerformanceStats {
  memoryUsage: number
  cpuUsage: number
  networkLatency: number
  renderTime: number
  activeConnections: number
  uptime: number
}
```

## 🔧 Configuration API

### Application Settings
```typescript
// Configuration management
export const getConfig = async (key?: string): Promise<any> =>
  invoke('get_config', { key })

export const setConfig = async (key: string, value: any): Promise<void> =>
  invoke('set_config', { key, value })

export const resetConfig = async (): Promise<void> =>
  invoke('reset_config')
```

### Environment Detection
```typescript
// Platform and environment detection
export const getPlatformInfo = async (): Promise<PlatformInfo> =>
  invoke('get_platform_info')

interface PlatformInfo {
  platform: 'windows' | 'macos' | 'linux' | 'ios' | 'android'
  version: string
  arch: 'x64' | 'arm64' | 'x86'
  isMobile: boolean
  isDesktop: boolean
  capabilities: string[]
}
```

---

This API documentation covers the core interfaces and services used throughout the HuLa application. For specific implementation details, refer to the individual service files in the `src/services/` directory.