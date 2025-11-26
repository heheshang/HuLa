# HuLa 系统架构文档

## 1. 系统概述

HuLa 是一个基于 Tauri 2.x 构建的跨平台即时通讯系统，支持 Windows、macOS、Linux、iOS 和 Android 平台。系统采用现代化的技术栈，提供完整的即时通讯功能，包括用户认证、聊天（私聊和群聊）、文件分享、主题切换和 AI 集成等特性。

## 2. 技术架构

### 2.1 整体架构图

```mermaid
graph TB
    subgraph "前端层 (Frontend)"
        A[Vue 3 + TypeScript] --> B[Vite 7 构建工具]
        B --> C[Naive UI (桌面)]
        B --> D[Vant (移动端)]
        A --> E[UnoCSS 原子CSS]
        A --> F[Pinia 状态管理]
        A --> G[Vue Router 4]
    end

    subgraph "跨平台层 (Cross-Platform)"
        H[Tauri 2.x] --> I[Rust 后端]
        H --> J[WebView 容器]
        H --> K[原生插件系统]
    end

    subgraph "数据层 (Data Layer)"
        L[SQLite 本地数据库]
        I --> M[Sea-ORM 数据库框架]
        M --> L
        I --> N[WebSocket 客户端]
        N --> O[Tokio-Tungstenite]
    end

    subgraph "外部服务 (External Services)"
        P[七牛云存储]
        Q[后端 API 服务]
        R[AI 服务接口]
    end

    A --> H
    I --> N
    I --> P
    I --> Q
    I --> R
```

### 2.2 平台特定架构

```mermaid
graph LR
    subgraph "桌面端 (Desktop)"
        A1[Tauri 桌面应用] --> B1[系统托盘集成]
        A1 --> C1[系统通知]
        A1 --> D1[屏幕截图]
        A1 --> E1[音频录制]
        A1 --> F1[目录扫描]
        A1 --> G1[文件关联]
    end

    subgraph "移动端 (Mobile)"
        A2[Tauri 移动应用] --> B2[安全区域适配]
        A2 --> C2[二维码扫描]
        A2 --> D2[键盘自适应]
        A2 --> E2[启动画面]
        A2 --> F2[原生导航栏]
    end

    subgraph "共享核心 (Shared Core)"
        C[Vue 3 组件库]
        S[状态管理]
        R[路由系统]
        U[工具函数库]
    end

    C --> A1
    C --> A2
    S --> A1
    S --> A2
    R --> A1
    R --> A2
    U --> A1
    U --> A2
```

## 3. 核心模块架构

### 3.1 前端模块结构

```mermaid
graph TD
    subgraph "Vue 应用结构"
        A[main.ts 入口] --> B[App.vue 根组件]
        B --> C[layout/ 布局层]
        B --> D[views/ 页面层]
        B --> E[components/ 组件层]
        B --> F[mobile/ 移动端]
        B --> G[plugins/ 插件系统]
        B --> H[stores/ 状态管理]
        B --> I[utils/ 工具函数]
        B --> J[services/ 服务层]
        B --> K[router/ 路由]
        B --> L[assets/ 资源文件]
    end

    subgraph "布局组件"
        C --> C1[left/ 左侧栏]
        C --> C2[center/ 中心区]
        C --> C3[right/ 右侧栏]
    end

    subgraph "核心页面"
        D --> D1[homeWindow/ 主窗口]
        D --> D2[loginWindow/ 登录窗口]
        D --> D3[chatHistory/ 聊天历史]
        D --> D4[settings/ 设置页面]
        D --> D5[fileManagerWindow/ 文件管理]
    end

    subgraph "移动端页面"
        F --> F1[message/ 消息页面]
        F --> F2[chat-room/ 聊天室]
        F --> F3[my/ 个人中心]
        F --> F4[friends/ 好友页面]
        F --> F5[community/ 社区页面]
    end
```

### 3.2 插件系统架构

```mermaid
graph LR
    subgraph "插件系统"
        A[动态加载器] --> B[机器人插件]
        A --> C[AI 助手插件]
        A --> D[自定义插件]
    end

    subgraph "机器人插件"
        B --> B1[Chat.vue 聊天界面]
        B --> B2[ModelManagement 模型管理]
        B --> B3[ApiKeyManagement API管理]
        B --> B4[ChatRoleManagement 角色管理]
        B --> B5[ImageGeneration 图像生成]
        B --> B6[VideoGeneration 视频生成]
    end

    subgraph "AI 插件功能"
        C --> C1[多模型支持]
        C --> C2[流式响应]
        C --> C3[上下文管理]
        C --> C4[预设配置]
    end
```

### 3.3 数据库架构

```mermaid
erDiagram
    im_user {
        string id PK
        string username
        string avatar
        string token
        string refresh_token
        datetime last_login
        datetime created_at
        datetime updated_at
    }

    im_room {
        string id PK
        string name
        string avatar
        string description
        string type
        string owner_id FK
        datetime created_at
        datetime updated_at
    }

    im_contact {
        string id PK
        string user_id FK
        string contact_id FK
        string remark
        datetime created_at
    }

    im_room_member {
        string id PK
        string room_id FK
        string user_id FK
        string role
        datetime joined_at
    }

    im_message {
        string id PK
        string room_id FK
        string sender_id FK
        string content
        string type
        string status
        datetime created_at
        datetime updated_at
    }

    im_config {
        string id PK
        string key
        string value
        string user_id FK
        datetime created_at
        datetime updated_at
    }

    im_user ||--o{ im_contact : "用户联系人"
    im_user ||--o{ im_room_member : "房间成员"
    im_user ||--o{ im_message : "发送消息"
    im_room ||--o{ im_room_member : "房间成员"
    im_room ||--o{ im_message : "房间消息"
    im_user ||--o{ im_config : "用户配置"
```

## 4. 通信架构

### 4.1 WebSocket 通信流程

```mermaid
sequenceDiagram
    participant C as 客户端
    participant W as WebSocket客户端
    participant S as 服务器

    Note over C,S: 连接建立阶段
    C->>W: ws_init_connection
    W->>S: WebSocket连接请求
    S-->>W: 连接确认
    W-->>C: 连接成功状态

    Note over C,S: 消息传输阶段
    C->>W: ws_send_message(消息)
    W->>S: 发送消息到服务器
    S-->>W: 消息确认
    W-->>C: 更新消息状态

    Note over C,S: 状态同步阶段
    C->>W: ws_get_state
    W-->>C: 返回当前连接状态
    C->>W: ws_get_health
    W-->>C: 健康检查结果
```

### 4.2 API 请求架构

```mermaid
graph TD
    A[前端组件] --> B[Tauri IPC 调用]
    B --> C[Rust 命令处理器]
    C --> D[HTTP 客户端]
    D --> E[后端 API 服务]
    E --> F[数据库操作]
    F --> G[返回响应]
    G --> H[Rust 响应处理]
    H --> I[IPC 响应]
    I --> J[前端状态更新]

    subgraph "错误处理"
        K[错误捕获] --> L[日志记录]
        L --> M[用户通知]
        M --> N[重试机制]
    end

    D -.-> K
    E -.-> K
```

## 5. 状态管理架构

### 5.1 Pinia Store 结构

```mermaid
graph LR
    subgraph "全局状态"
        A[global Store] --> A1[应用配置]
        A --> A2[主题设置]
        A --> A3[语言设置]
    end

    subgraph "用户状态"
        B[user Store] --> B1[用户信息]
        B --> B2[登录状态]
        B --> B3[权限管理]
    end

    subgraph "聊天状态"
        C[chat Store] --> C1[消息列表]
        C --> C2[房间信息]
        C --> C3[未读计数]
    end

    subgraph "文件状态"
        D[file Store] --> D1[上传队列]
        D --> D2[下载进度]
        D --> D3[文件缓存]
    end

    subgraph "设置状态"
        E[setting Store] --> E1[应用设置]
        E --> E2[通知设置]
        E --> E3[隐私设置]
    end
```

## 6. 安全架构

### 6.1 认证授权流程

```mermaid
flowchart TD
    A[用户输入凭据] --> B[前端验证]
    B --> C{验证通过?}
    C -->|否| A
    C -->|是| D[发送登录请求]
    D --> E[后端验证]
    E --> F{认证成功?}
    F -->|否| G[返回错误信息]
    F -->|是| H[生成 JWT Token]
    H --> I[返回用户信息和 Token]
    I --> J[存储 Token]
    J --> K[更新登录状态]
    K --> L[跳转到主界面]

    subgraph "Token 管理"
        M[Token 刷新机制]
        N[Token 过期检测]
        O[自动重新登录]
    end

    L --> M
    M --> N
    N --> O
```

### 6.2 数据安全

```mermaid
graph TD
    A[敏感数据] --> B[加密存储]
    B --> C[本地 SQLite 加密]
    C --> D[传输层 HTTPS/WSS]
    D --> E[Token 认证]
    E --> F[权限验证]
    F --> G[数据访问控制]

    subgraph "安全措施"
        H[输入验证]
        I[XSS 防护]
        J[CSRF 防护]
        K[SQL 注入防护]
    end

    G --> H
    H --> I
    I --> J
    J --> K
```

## 7. 性能优化架构

### 7.1 前端性能优化

```mermaid
graph LR
    subgraph "代码优化"
        A[代码分割] --> A1[路由懒加载]
        A --> A2[组件异步加载]
        A --> A3[Tree Shaking]
    end

    subgraph "资源优化"
        B[图片优化] --> B1[WebP 格式]
        B --> B2[懒加载]
        B --> B3[CDN 加速]
    end

    subgraph "缓存策略"
        C[HTTP 缓存] --> C1[浏览器缓存]
        C --> C2[Service Worker]
        C --> C3[内存缓存]
    end

    subgraph "渲染优化"
        D[虚拟滚动] --> D1[大列表优化]
        D --> D2[无限滚动]
        D --> D3[分页加载]
    end
```

### 7.2 数据库性能优化

```mermaid
graph TD
    A[数据库查询] --> B[索引优化]
    B --> C[分页查询]
    C --> D[连接池管理]
    D --> E[查询缓存]

    subgraph "优化策略"
        F[复合索引]
        G[查询计划优化]
        H[批量操作]
        I[事务管理]
    end

    E --> F
    F --> G
    G --> H
    H --> I
```

## 8. 部署架构

### 8.1 构建流程

```mermaid
flowchart TD
    A[源代码] --> B[类型检查 vue-tsc]
    B --> C[代码格式化 Biome]
    C --> D[Vite 构建]
    D --> E[Tauri 打包]
    E --> F{目标平台}

    F -->|Windows| G[Windows 安装包]
    F -->|macOS| H[macOS 应用包]
    F -->|Linux| I[Linux AppImage]
    F -->|iOS| J[iOS IPA]
    F -->|Android| K[Android APK]

    G --> L[自动发布]
    H --> L
    I --> L
    J --> M[App Store 发布]
    K --> N[Google Play 发布]
```

### 8.2 CI/CD 流水线

```mermaid
graph LR
    subgraph "代码提交"
        A[Git Push] --> B[Husky 预提交检查]
        B --> C[Lint 检查]
        C --> D[测试运行]
    end

    subgraph "构建阶段"
        E[GitHub Actions] --> F[多平台构建]
        F --> G[单元测试]
        G --> H[集成测试]
    end

    subgraph "发布阶段"
        I[构建产物] --> J[版本标记]
        J --> K[创建 Release]
        K --> L[上传到仓库]
    end

    D --> E
    H --> I
```

## 9. 监控与日志架构

### 9.1 日志系统

```mermaid
graph TD
    A[应用日志] --> B[日志级别分类]
    B --> C[控制台输出]
    B --> D[文件存储]
    B --> E[远程上报]

    subgraph "日志类型"
        F[错误日志]
        G[访问日志]
        H[性能日志]
        I[用户行为日志]
    end

    subgraph "日志处理"
        J[日志格式化]
        K[日志过滤]
        L[日志聚合]
        M[日志分析]
    end

    C --> J
    D --> J
    E --> J
    J --> K
    K --> L
    L --> M
```

### 9.2 性能监控

```mermaid
graph LR
    subgraph "前端监控"
        A[Web Vitals] --> A1[LCP 指标]
        A --> A2[FID 指标]
        A --> A3[CLS 指标]
    end

    subgraph "资源监控"
        B[内存使用] --> B1[堆内存监控]
        B --> B2[内存泄漏检测]
    end

    subgraph "网络监控"
        C[请求监控] --> C1[响应时间]
        C --> C2[错误率]
        C --> C3[吞吐量]
    end

    subgraph "用户体验监控"
        D[交互响应] --> D1[点击延迟]
        D --> D2[滚动性能]
        D --> D3[动画流畅度]
    end
```

## 10. 扩展性架构

### 10.1 插件扩展机制

```mermaid
graph TD
    A[插件系统] --> B[插件注册]
    B --> C[插件加载]
    C --> D[插件执行]
    D --> E[插件卸载]

    subgraph "插件类型"
        F[UI 插件]
        G[功能插件]
        H[主题插件]
        I[语言插件]
    end

    subgraph "插件管理"
        J[插件配置]
        K[依赖管理]
        L[权限控制]
        M[版本管理]
    end

    E --> F
    E --> G
    E --> H
    E --> I

    J --> K
    K --> L
    L --> M
```

### 10.2 多平台适配

```mermaid
graph LR
    subgraph "平台抽象层"
        A[平台检测] --> B[条件编译]
        B --> C[平台特定代码]
    end

    subgraph "桌面平台"
        C --> D[Windows API]
        C --> E[macOS API]
        C --> F[Linux API]
    end

    subgraph "移动平台"
        C --> G[iOS 原生功能]
        C --> H[Android 原生功能]
    end

    subgraph "共享功能"
        I[核心业务逻辑]
        J[数据模型]
        K[通用工具]
    end

    D --> I
    E --> I
    F --> I
    G --> I
    H --> I
```

## 11. 总结

HuLa 系统采用现代化的跨平台架构设计，通过 Tauri 框架实现了高性能的桌面和移动端应用。系统具备以下特点：

1. **跨平台统一**: 单一代码库支持多平台部署
2. **高性能**: Rust 后端 + Vue 前端的优化组合
3. **模块化设计**: 清晰的分层架构和插件系统
4. **安全可靠**: 完善的认证授权和数据保护机制
5. **易于维护**: 良好的代码组织和文档体系
6. **可扩展性**: 灵活的插件系统和平台适配机制

该架构为即时通讯应用提供了坚实的技术基础，支持未来的功能扩展和性能优化需求。