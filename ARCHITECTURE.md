# Technical Architecture Guide

This document provides a comprehensive overview of bolt.diy's technical architecture, system design, and component interactions.

## 🏗️ System Overview

bolt.diy is built as a modern web application that brings AI-powered development tools directly to the browser. The system consists of several key layers working together to provide a seamless development experience.

```mermaid
graph TB
    subgraph "Frontend Layer"
        UI[React/Remix UI]
        State[Zustand State Management]
        Router[Remix Router]
    end
    
    subgraph "AI Provider Layer"
        AIManager[AI Provider Manager]
        OpenAI[OpenAI]
        Anthropic[Anthropic]
        Google[Google Gemini]
        Ollama[Ollama Local]
        Others[+13 More Providers]
    end
    
    subgraph "Runtime Layer"
        WebContainer[WebContainer API]
        NodeJS[Browser Node.js]
        FileSystem[Virtual File System]
        Terminal[Integrated Terminal]
    end
    
    subgraph "Backend Services"
        API[Remix API Routes]
        Auth[Authentication]
        Config[Configuration]
    end
    
    UI --> State
    UI --> Router
    State --> AIManager
    AIManager --> OpenAI
    AIManager --> Anthropic
    AIManager --> Google
    AIManager --> Ollama
    AIManager --> Others
    UI --> WebContainer
    WebContainer --> NodeJS
    WebContainer --> FileSystem
    WebContainer --> Terminal
    Router --> API
    API --> Auth
    API --> Config
```

## 🎯 Core Components

### Frontend Architecture

The frontend is built with React and Remix, providing a modern, server-side rendered application with excellent performance and SEO.

```mermaid
graph TD
    subgraph "React Component Hierarchy"
        App[App Root]
        Layout[Layout Component]
        Chat[Chat Interface]
        Editor[Code Editor]
        Preview[Live Preview]
        Terminal[Terminal Component]
        FileTree[File Explorer]
    end
    
    subgraph "State Management"
        WorkspaceStore[Workspace Store]
        ChatStore[Chat Store]
        SettingsStore[Settings Store]
        AIStore[AI Provider Store]
    end
    
    subgraph "Hooks & Utilities"
        UseAI[useAI Hook]
        UseWebContainer[useWebContainer Hook]
        UseFileSystem[useFileSystem Hook]
        UseTerminal[useTerminal Hook]
    end
    
    App --> Layout
    Layout --> Chat
    Layout --> Editor
    Layout --> Preview
    Layout --> Terminal
    Layout --> FileTree
    
    Chat --> ChatStore
    Editor --> WorkspaceStore
    Terminal --> WorkspaceStore
    FileTree --> WorkspaceStore
    
    Chat --> UseAI
    Editor --> UseWebContainer
    Terminal --> UseTerminal
    FileTree --> UseFileSystem
    
    UseAI --> AIStore
    UseWebContainer --> WorkspaceStore
    UseFileSystem --> WorkspaceStore
    UseTerminal --> WorkspaceStore
```

### AI Provider Integration

bolt.diy supports multiple AI providers through a unified interface, allowing users to choose their preferred AI service.

```mermaid
graph LR
    subgraph "AI Provider Interface"
        BaseProvider[Base Provider Class]
        ProviderConfig[Provider Configuration]
        ProviderManager[Provider Manager]
    end
    
    subgraph "Supported Providers"
        OpenAI[OpenAI GPT-4]
        Anthropic[Claude 3.5]
        Google[Gemini Pro]
        Ollama[Ollama Local]
        Groq[Groq]
        Mistral[Mistral AI]
        Cohere[Cohere]
        Perplexity[Perplexity]
        Fireworks[Fireworks AI]
        Together[Together AI]
        Deepseek[Deepseek]
        OpenRouter[OpenRouter]
        xAI[xAI Grok]
        LMStudio[LM Studio]
        HuggingFace[HuggingFace]
        Azure[Azure OpenAI]
        AWSBedrock[AWS Bedrock]
    end
    
    ProviderManager --> BaseProvider
    BaseProvider --> ProviderConfig
    
    ProviderManager --> OpenAI
    ProviderManager --> Anthropic
    ProviderManager --> Google
    ProviderManager --> Ollama
    ProviderManager --> Groq
    ProviderManager --> Mistral
    ProviderManager --> Cohere
    ProviderManager --> Perplexity
    ProviderManager --> Fireworks
    ProviderManager --> Together
    ProviderManager --> Deepseek
    ProviderManager --> OpenRouter
    ProviderManager --> xAI
    ProviderManager --> LMStudio
    ProviderManager --> HuggingFace
    ProviderManager --> Azure
    ProviderManager --> AWSBedrock
```

### WebContainer Integration

The WebContainer API enables a full Node.js runtime environment directly in the browser, providing file system access, terminal capabilities, and package management.

```mermaid
graph TB
    subgraph "Browser Environment"
        MainThread[Main Thread]
        WebWorker[Web Worker]
        ServiceWorker[Service Worker]
    end
    
    subgraph "WebContainer Runtime"
        Container[WebContainer Instance]
        FS[Virtual File System]
        Shell[Shell Environment]
        NodeRuntime[Node.js Runtime]
        NPM[Package Manager]
    end
    
    subgraph "Development Tools"
        Terminal[Terminal Interface]
        FileManager[File Manager]
        ProcessManager[Process Manager]
        PreviewServer[Preview Server]
    end
    
    MainThread --> Container
    WebWorker --> Container
    ServiceWorker --> Container
    
    Container --> FS
    Container --> Shell
    Container --> NodeRuntime
    Container --> NPM
    
    FS --> FileManager
    Shell --> Terminal
    NodeRuntime --> ProcessManager
    NPM --> ProcessManager
    Container --> PreviewServer
```

## 🔄 Data Flow & Communication

### Request/Response Flow

```mermaid
sequenceDiagram
    participant User
    participant UI
    participant AIProvider
    participant WebContainer
    participant FileSystem
    
    User->>UI: Send message/request
    UI->>AIProvider: Process with AI
    AIProvider->>UI: Return generated code
    UI->>WebContainer: Execute code changes
    WebContainer->>FileSystem: Update files
    FileSystem->>WebContainer: Confirm changes
    WebContainer->>UI: Update preview
    UI->>User: Show results
```

### File Management Flow

```mermaid
graph LR
    subgraph "File Operations"
        UserAction[User Action]
        UIRequest[UI Request]
        FSOperation[File System Operation]
        WebContainerUpdate[WebContainer Update]
        PreviewRefresh[Preview Refresh]
    end
    
    UserAction --> UIRequest
    UIRequest --> FSOperation
    FSOperation --> WebContainerUpdate
    WebContainerUpdate --> PreviewRefresh
    PreviewRefresh --> UserAction
```

## 🚀 Technology Stack

### Frontend Technologies

- **React 18**: Modern React with concurrent features
- **Remix**: Full-stack React framework with SSR/SSG
- **TypeScript**: Type-safe development
- **Vite**: Fast build tool and development server
- **UnoCSS**: Instant atomic CSS engine
- **Zustand**: Lightweight state management
- **Monaco Editor**: VS Code editor experience
- **WebContainer API**: Browser-based Node.js runtime

### AI & Machine Learning

- **Multiple LLM Providers**: 17+ supported AI providers
- **Streaming Responses**: Real-time AI response streaming
- **Context Management**: Conversation history and context
- **Code Generation**: AI-powered code creation and editing
- **Natural Language Processing**: Intent understanding and parsing

### Development & Build Tools

- **pnpm**: Fast, efficient package manager
- **ESLint**: Code linting and quality checks
- **Prettier**: Code formatting
- **Husky**: Git hooks for quality control
- **TypeScript**: Static type checking
- **Electron**: Desktop application wrapper

### Deployment & Infrastructure

- **Vercel**: Web deployment platform
- **Docker**: Containerization support
- **GitHub Pages**: Documentation hosting
- **Cloudflare Workers**: Edge computing support
- **Netlify**: Alternative deployment option

## 🔒 Security Considerations

### Browser Security

- **Sandboxed Execution**: WebContainer provides isolated runtime
- **Content Security Policy**: CSP headers for XSS protection  
- **CORS Configuration**: Proper cross-origin resource sharing
- **Input Validation**: Sanitization of user inputs and AI responses

### AI Provider Security

- **API Key Management**: Secure storage and transmission
- **Rate Limiting**: Protection against API abuse
- **Response Filtering**: Content moderation and safety checks
- **Data Privacy**: Conversations and code stay in browser

### Code Execution Security

- **Isolated Environment**: Code runs in WebContainer sandbox
- **File System Restrictions**: Limited access to browser file system
- **Network Isolation**: Controlled external resource access
- **Process Isolation**: Separate processes for different operations

## ⚡ Performance Optimization

### Frontend Performance

- **Code Splitting**: Lazy loading of components and routes
- **Bundle Optimization**: Tree shaking and minification
- **Caching Strategies**: Browser and CDN caching
- **Progressive Enhancement**: Core functionality without JavaScript

### Runtime Performance

- **Memory Management**: Efficient WebContainer resource usage
- **Stream Processing**: Real-time response handling
- **Background Tasks**: Web Workers for heavy operations
- **Virtual Scrolling**: Efficient rendering of large file lists

### AI Response Optimization

- **Streaming Responses**: Immediate feedback during generation
- **Context Optimization**: Efficient prompt engineering
- **Caching**: Response caching for repeated requests
- **Parallel Processing**: Multiple AI requests when beneficial

## 🔧 Configuration & Customization

### Environment Configuration

```typescript
interface AppConfig {
  // AI Provider settings
  providers: {
    openai?: { apiKey: string; model: string; }
    anthropic?: { apiKey: string; model: string; }
    google?: { apiKey: string; model: string; }
    // ... other providers
  }
  
  // WebContainer settings
  webContainer: {
    memoryLimit: number
    cpuLimit: number
    networkAccess: boolean
  }
  
  // UI preferences
  ui: {
    theme: 'light' | 'dark' | 'system'
    codeTheme: string
    fontSize: number
    layout: 'default' | 'compact'
  }
}
```

### Provider Configuration

Each AI provider can be configured with specific settings:

```typescript
interface ProviderConfig {
  name: string
  apiKey: string
  model: string
  maxTokens?: number
  temperature?: number
  topP?: number
  frequencyPenalty?: number
  presencePenalty?: number
}
```

## 🏭 Build & Deployment Architecture

### Development Build

```mermaid
graph LR
    Source[Source Code] --> Vite[Vite Dev Server]
    Vite --> HMR[Hot Module Reload]
    HMR --> Browser[Browser Preview]
    Source --> TypeScript[TypeScript Check]
    TypeScript --> ESLint[ESLint]
    ESLint --> Prettier[Prettier]
```

### Production Build

```mermaid
graph LR
    Source[Source Code] --> Build[Remix Build]
    Build --> Bundle[Bundle Optimization]
    Bundle --> Assets[Asset Processing]
    Assets --> Output[Production Output]
    Output --> Deploy[Deployment]
    Deploy --> CDN[CDN Distribution]
```

### Deployment Options

```mermaid
graph TB
    BuildOutput[Build Output]
    
    subgraph "Web Deployment"
        Vercel[Vercel]
        Netlify[Netlify]
        CloudflarePages[Cloudflare Pages]
        AWSAmplify[AWS Amplify]
    end
    
    subgraph "Container Deployment"
        Docker[Docker Container]
        Kubernetes[Kubernetes]
        CloudRun[Google Cloud Run]
        AWSFargate[AWS Fargate]
    end
    
    subgraph "Desktop Deployment"
        Electron[Electron App]
        Tauri[Tauri App]
        PWA[Progressive Web App]
    end
    
    BuildOutput --> Vercel
    BuildOutput --> Netlify
    BuildOutput --> CloudflarePages
    BuildOutput --> AWSAmplify
    BuildOutput --> Docker
    BuildOutput --> Kubernetes
    BuildOutput --> CloudRun
    BuildOutput --> AWSFargate
    BuildOutput --> Electron
    BuildOutput --> Tauri
    BuildOutput --> PWA
```

## 🔄 State Management Architecture

### Global State Structure

```mermaid
graph TB
    subgraph "Application State"
        WorkspaceState[Workspace State]
        ChatState[Chat State]
        SettingsState[Settings State]
        UIState[UI State]
    end
    
    subgraph "Workspace State"
        Files[File Tree]
        ActiveFile[Active File]
        PreviewUrl[Preview URL]
        Terminal[Terminal State]
    end
    
    subgraph "Chat State"
        Messages[Message History]
        ActiveProvider[AI Provider]
        StreamingState[Streaming State]
        Context[Conversation Context]
    end
    
    subgraph "Settings State"
        Theme[Theme Settings]
        Providers[Provider Config]
        UserPrefs[User Preferences]
        APIKeys[API Keys]
    end
    
    WorkspaceState --> Files
    WorkspaceState --> ActiveFile
    WorkspaceState --> PreviewUrl
    WorkspaceState --> Terminal
    
    ChatState --> Messages
    ChatState --> ActiveProvider
    ChatState --> StreamingState
    ChatState --> Context
    
    SettingsState --> Theme
    SettingsState --> Providers
    SettingsState --> UserPrefs
    SettingsState --> APIKeys
```

## 🌐 Extensibility & Plugins

### Extension Points

- **AI Providers**: Add custom AI provider implementations
- **File Types**: Support for new file types and languages
- **Themes**: Custom UI and editor themes
- **Templates**: Project templates and scaffolding
- **Deployment Targets**: Additional deployment platforms

### Plugin Architecture

```mermaid
graph LR
    Core[Core System]
    PluginAPI[Plugin API]
    
    subgraph "Plugin Types"
        AIPlugin[AI Provider Plugin]
        ThemePlugin[Theme Plugin]
        TemplatePlugin[Template Plugin]
        DeployPlugin[Deploy Plugin]
    end
    
    Core --> PluginAPI
    PluginAPI --> AIPlugin
    PluginAPI --> ThemePlugin
    PluginAPI --> TemplatePlugin
    PluginAPI --> DeployPlugin
```

## 🔍 Monitoring & Analytics

### Performance Monitoring

- **Core Web Vitals**: LCP, FID, CLS tracking
- **Bundle Analysis**: Size and performance metrics
- **Runtime Performance**: Memory and CPU usage
- **Error Tracking**: Exception monitoring and reporting

### Usage Analytics

- **Feature Usage**: Component and feature adoption
- **AI Provider Performance**: Response times and success rates
- **User Flows**: Common usage patterns and workflows
- **Performance Metrics**: Build times and deployment success

This architecture provides a robust, scalable foundation for AI-powered web development in the browser, with clear separation of concerns and extensibility for future enhancements.