# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

本文件为在此代码库中工作的 Claude Code (claude.ai/code) 提供指导。

## Project Overview / 项目概述

Legado (阅读) is a free and open-source Android e-book reader application written in Kotlin. The app allows users to read books from custom book sources, local files (TXT, EPUB), and includes features like text-to-speech, customizable reading interfaces, and web-based management.

Legado（阅读）是一个用 Kotlin 编写的免费开源 Android 电子书阅读器应用程序。该应用允许用户从自定义书源、本地文件（TXT、EPUB）中阅读书籍，并包含文本转语音、可自定义阅读界面和基于 Web 的管理等功能。

## Build Commands / 构建命令

### Prerequisites / 先决条件
- **Java 17+**: Required for Kotlin compilation / Kotlin 编译需要 Java 17+
- **Android SDK**: Compile SDK 35, Min SDK 21, Target SDK 35 / 编译 SDK 35，最小 SDK 21，目标 SDK 35
- **Gradle**: Uses Gradle wrapper (./gradlew) / 使用 Gradle 包装器 (./gradlew)

### Android App / Android 应用
```bash
# Build debug APK / 构建调试 APK
./gradlew assembleDebug

# Build release APK / 构建发布 APK
./gradlew assembleRelease

# Run tests / 运行测试
./gradlew test
./gradlew connectedAndroidTest

# Clean build / 清理构建
./gradlew clean

# Install debug version / 安装调试版本
./gradlew installDebug
```

### Web Module (Vue.js) / Web 模块 (Vue.js)
```bash
# Navigate to web module / 进入 web 模块
cd modules/web

# Install dependencies / 安装依赖
pnpm install

# Development server / 开发服务器
pnpm dev

# Build for production / 生产构建
pnpm build

# Type checking / 类型检查
pnpm type-check

# Lint and format / 代码检查和格式化
pnpm lint:fix
pnpm format
```

## Architecture Overview / 架构概述

### Core Components / 核心组件

1. **Main App Module** (`app/`) / **主应用模块** (`app/`)
   - **api/**: External API interfaces and data models / 外部 API 接口和数据模型
   - **base/**: Base classes for Activities, Fragments, ViewModels / Activity、Fragment、ViewModel 的基类
   - **constant/**: Application constants and configurations / 应用常量和配置
   - **data/**: Database entities, DAOs, and Room database setup / 数据库实体、DAO 和 Room 数据库设置
   - **help/**: Utility classes and helper functions / 工具类和辅助函数
   - **model/**: Business logic and data processing / 业务逻辑和数据处理
   - **service/**: Background services (reading aloud, downloading, web server) / 后台服务（朗读、下载、Web 服务器）
   - **ui/**: User interface components organized by feature / 按功能组织的用户界面组件
   - **utils/**: General utility functions / 通用工具函数

2. **Web Module** (`modules/web/`) / **Web 模块** (`modules/web/`)
   - Vue.js 3 application for web-based book management / 用于基于 Web 的图书管理的 Vue.js 3 应用程序
   - Uses Element Plus UI components / 使用 Element Plus UI 组件
   - Communicates with Android app via REST API / 通过 REST API 与 Android 应用通信

3. **Book Module** (`modules/book/`) / **图书模块** (`modules/book/`)
   - Book parsing and processing libraries / 图书解析和处理库
   - EPUB, TXT, and other format handlers / EPUB、TXT 和其他格式处理器

4. **Rhino Module** (`modules/rhino/`) / **Rhino 模块** (`modules/rhino/`)
   - JavaScript engine integration for book source scripts / 用于书源脚本的 JavaScript 引擎集成
   - Custom script execution environment / 自定义脚本执行环境

### Key Architectural Patterns / 关键架构模式

- **MVVM**: Uses Android Architecture Components (ViewModel, LiveData) / 使用 Android 架构组件（ViewModel、LiveData）
- **Repository Pattern**: Data layer abstraction with Room database / 使用 Room 数据库的数据层抽象
- **Dependency Injection**: Uses manual DI, no framework like Dagger/Hilt / 使用手动依赖注入，不使用 Dagger/Hilt 等框架
- **Modular Design**: Separate modules for different functionalities / 模块化设计，不同功能分离模块
- **Event Bus**: LiveEventBus for decoupled communication / 使用 LiveEventBus 进行解耦通信

### Database

- **Room Database**: Primary storage for books, chapters, sources, settings
- **Schema Migrations**: Located in `app/schemas/` directory
- **Entities**: Book, BookChapter, BookSource, RssSource, etc.

### Network Layer

- **OkHttp**: HTTP client with custom interceptors
- **Cronet**: Google's networking library for improved performance
- **WebDAV**: For cloud backup and synchronization

### UI Framework

- **View Binding**: Modern view binding instead of findViewById
- **Material Design**: Google's design system
- **Custom Views**: Extensive customization for reading interface
- **Multiple Themes**: Day/night modes with customizable colors

## Development Guidelines

### Code Style
- Follow Kotlin coding conventions
- Use meaningful variable and function names
- Prefer immutable data structures where possible
- Use coroutines for asynchronous operations

### Key Dependencies
- **Kotlin Coroutines**: For async/concurrent operations
- **Room**: Database ORM
- **OkHttp**: HTTP networking
- **Gson**: JSON serialization
- **Glide**: Image loading and caching
- **JSoup**: HTML parsing for book sources
- **Material Components**: UI components

### Testing
- Unit tests in `app/src/test/`
- Instrumentation tests in `app/src/androidTest/`
- Test book source functionality with mock data

### Build Configuration
- Uses Gradle Version Catalogs (`gradle/libs.versions.toml`)
- Multi-flavor builds (app, debug, release)
- ProGuard/R8 obfuscation for release builds
- Automatic version naming based on git commits

## Common Development Tasks

### Adding a New Book Source Feature
1. Update relevant entities in `data/entities/`
2. Modify DAOs in `data/dao/`
3. Update database schema and migration
4. Implement business logic in `model/`
5. Create/update UI components in `ui/`

### Debugging Book Sources
- Use the built-in debug tools in `ui/book/source/debug/`
- Check network requests and responses
- Validate CSS selectors and XPath expressions
- Test with different book source rules

### Adding New File Format Support
1. Create parser in `model/localBook/`
2. Update `LocalBook.kt` to handle new format
3. Add MIME type detection
4. Test with various file samples

### Web Interface Development
- Navigate to `modules/web/`
- Use Vue 3 Composition API
- Follow Element Plus design guidelines
- Ensure responsive design for mobile browsers

## API Integration

The app provides both Content Provider and HTTP API interfaces:
- Web API endpoints documented in `api.md`
- Deep link support: `legado://import/{type}?src={url}`
- Supported import types: bookSource, rssSource, replaceRule, theme, etc.

## Important Files

- `App.kt`: Application entry point and initialization
- `AppDatabase.kt`: Room database configuration
- `AppConfig.kt`: Application configuration management
- `ReadBookActivity.kt`: Main reading interface
- `BookSourceActivity.kt`: Book source management
- `WebService.kt`: HTTP server for web interface