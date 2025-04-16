---
title: "虎鲸笔记插件API开发文档"
source: "https://www.orca-studio.com/orcanote-docs/documents/Quick_Start.html"
author:
published: 2025-04-15
created: 2025-04-16
description: "A Starter template with Next.js, Nextra"
tags:
  - "clippings"
---

## Introduction 简介

The Orca Note plugin system is a powerful extension mechanism that allows developers to add new features, customize interface components, or integrate external services. Through plugins, you can:
Orca Note 插件系统是一个强大的扩展机制，允许开发者添加新功能、自定义界面组件或集成外部服务。通过插件，您可以：

- Add new block types and inline content renderers
  添加新的区块类型和内联内容渲染器
- Register custom commands and shortcuts
  注册自定义命令和快捷键
- Extend the application user interface (such as adding toolbar buttons, menu items, etc.)
  扩展应用程序用户界面（例如添加工具栏按钮、菜单项等）
- Implement integration with external services
  实现与外部服务的集成
- Customize themes and styles
  自定义主题和样式
- Enhance existing features or add entirely new functionality
  增强现有功能或添加全新的功能

This guide will help you quickly get started with Orca Note plugin development, from setting up your environment to developing your first plugin.
本指南将帮助您快速开始 Orca Note 插件开发，从设置开发环境到开发您的第一个插件。

## Environment Requirements环境要求

To develop Orca Note plugins, you’ll need the following environment and tools:
开发 Orca Note 插件，您需要以下环境和工具：

- **Node.js**: LTS version recommended
  Node.js：推荐使用 LTS 版本
- **Editor**: Visual Studio Code recommended
  编辑器：推荐使用 Visual Studio Code
- **Orca Note**: Latest version of Orca Note application installed
  Orca Note：请安装 Orca Note 应用的最新版本

## File Structure 文件结构

A typical Orca Note plugin project structure is as follows:
一个典型的 Orca Note 插件项目结构如下：

```
my-orca-plugin/
├── dist/                     # Compiled code
│   ├── index.js              # Compiled plugin file
├── src/
│   ├── main.ts               # Entry file, contains plugin registration and initialization logic
│   ├── orca.d.ts             # Plugin API type definition file
│   └── styles/               # CSS style files
├── icon.png                  # Plugin icon image
├── package.json              # Project configuration
├── tsconfig.json             # TypeScript configuration
├── vite.config.js            # Vite build configuration (if using Vite)
└── README.md                 # Plugin documentation
```

The plugin name is the name of its containing folder. To deploy a plugin, place the plugin folder containing the above files into the `orca/plugins` directory.
插件名称是其包含文件夹的名称。要部署插件，请将包含上述文件的插件文件夹放置到 `orca/plugins` 目录中。

## Minimum Required Files 最小必需文件

The following files are the minimum required for a functional Orca Note plugin:
以下文件是 Orca Note 插件功能所需的最低要求：

- `dist/index.js`: The compiled JavaScript file containing the plugin logic.
  `dist/index.js`: 包含插件逻辑的编译后的 JavaScript 文件。
- `icon.png`: An icon representing the plugin in the Orca Note interface.
  `icon.png`: 代表插件在 Orca Note 界面中的图标。

Ensure these files are present in your plugin folder before deployment.
确保在部署前这些文件存在于您的插件文件夹中。

## Main File Descriptions 主文件描述

### package.json

```
{
  "name": "my-orca-plugin",
  "version": "1.0.0",
  "description": "My Orca Note Plugin",
  "peerDependencies": {
    "react": "^18.2.0",
    "valtio": "^1.13.2"
  }
}
```

### Entry File (main.ts) 入口文件（main.ts）

The entry file needs to expose the following functions:
入口文件需要暴露以下函数：

- `load`: Called when the plugin is enabled
  插件启用时调用
- `unload`: Called when the plugin is disabled
  插件禁用时调用

For example:例如：

```
export async function load(pluginName: string) {
  // Plugin enable logic
  console.log('Plugin enabled')
}

export async function unload() {
  // Plugin disable logic
  console.log('Plugin disabled')
}
```

## Lifecycle 生命周期

Orca Note plugins follow a simple and clear lifecycle pattern, mainly including the following phases:
Orca Note 插件遵循简单明了的生命周期模式，主要包括以下阶段：

The plugin package is discovered and loaded into Orca Note but not yet enabled. Plugin metadata is parsed at this time.
插件包被发现并加载到 Orca Note 中，但尚未启用。此时会解析插件元数据。

## Enable Phase 启用阶段

When a user enables the plugin or the application automatically enables it at startup, the plugin’s `load` function is called. This is the main entry point for plugin initialization, typically used to:
当用户启用插件或应用程序在启动时自动启用它时，会调用插件中的 `load` 函数。这是插件初始化的主要入口点，通常用于：

- Register commands, renderers, converters, etc.
  注册命令、渲染器、转换器等
- Set up event listeners
  设置事件监听器
- Initialize plugin state 初始化插件状态
- Add UI elements (such as toolbar buttons, sidebars, etc.)
  添加 UI 元素（如工具栏按钮、侧边栏等）

## Disable Phase 禁用阶段

When a user disables the plugin or the application closes, the plugin’s `unload` function is called. At this time, you should:
当用户禁用插件或应用程序关闭时，插件中的 `unload` 函数将被调用。此时，您应该：

- Remove all registered commands, renderers, etc.
  删除所有已注册的命令、渲染器等。
- Clean up event listeners
  清理事件监听器
- Release resources 释放资源
- Remove added UI elements
  移除添加的 UI 元素

## Plugin Settings Management插件设置管理

Plugins can define their own settings and provide an UI interface for user configuration:
插件可以定义自己的设置并提供用户配置的 UI 界面：

## Model Overview 模型概述

The Orca Note plugin API provides rich functional interfaces. Here’s an overview of the most commonly used models and APIs:
Orca Note 插件 API 提供了丰富的功能接口。以下是常用模型和 API 的概述：

## Core Objects 核心对象

### orca 海豚

The global object `orca` is the main entry point for the plugin system, providing access to all plugin functionality.
全局对象 `orca` 是插件系统的主入口，提供对所有插件功能的访问。

### state 状态

`orca.state` contains the current state of the application, including current panels, block data, settings, etc.
`orca.state` 包含应用程序的当前状态，包括当前面板、块数据、设置等。

```
// Example: Get current language
const currentLocale = orca.state.locale

// Example: Get loaded block data
const currentBlock = orca.state.blocks[blockId]

// Example: Get application settings
const themeMode = orca.state.themeMode // "light" or "dark"
```

Orca Note uses the `valtio` library to manage application state (mounted to `window.Valtio`). You can listen to state changes using the `subscribe` function provided by `valtio` or other supported mechanisms.
Orca Note 使用 `valtio` 库来管理应用程序状态（挂载到 `window.Valtio` ）。您可以使用 `valtio` 提供的 `subscribe` 函数或其他支持的机制来监听状态变化。

## Main API Categories 主要 API 类别

### Command System 命令系统

The command system is the most basic extension point for plugins, allowing registration of executable function units:
命令系统是插件最基本扩展点，允许注册可执行的功能单元：

### Render System 渲染系统

Allows registration of custom block types and inline content renderers:
允许注册自定义块类型和内联内容渲染器：

Orca Note’s UI is based on React 18 (mounted to `window.React`). If you need to develop custom UI components, you can use the globally exposed React directly without importing the React library separately.
Orca Note 的 UI 基于 React 18（挂载到 `window.React` ）。如果您需要开发自定义 UI 组件，可以直接使用全局暴露的 React，无需单独导入 React 库。

### Converter System 转换系统

Responsible for converting block content between different formats:
负责将块内容在不同格式之间进行转换：

### UI Extensions 用户界面扩展

Allows adding custom UI elements:
允许添加自定义 UI 元素：

### Data Storage 数据存储

Plugins can persistently store data:
插件可以持久化存储数据：

```
// Set plugin data
await orca.plugins.setData('myplugin', 'key', 'value')

// Get plugin data
const value = await orca.plugins.getData('myplugin', 'key')

// Remove plugin data
await orca.plugins.removeData('myplugin', 'key')
```

### Notification System 通知系统

Display notification messages:
显示通知消息：

```
orca.notify(
  'info', // Type: "info" | "success" | "warn" | "error"
  'This is a notification message', // Message content
  {
    // Optional configuration
    title: 'Notification Title',
    action: () => {
      /* Execute when notification is clicked */
    },
  },
)
```

## Main Data Models 主数据模型

### Block 块

Blocks are the basic structural units of Orca Note:
块是 Orca Note 的基本结构单元：

```
interface Block {
  id: DbId // Block ID
  content?: ContentFragment[] // Block content
  text?: string // Plain text content
  created: Date // Creation time
  modified: Date // Modification time
  parent?: DbId // Parent block ID
  left?: DbId // Left block ID
  children: DbId[] // Child block ID list
  aliases: string[] // Alias list
  properties: BlockProperty[] // Property list
  refs: BlockRef[] // Reference list
  backRefs: BlockRef[] // Back reference list
}
```

### Panel 面板

Panels are the main organizational units of the UI:
面板是 UI 的主要组织单元：

```
interface ViewPanel {
  id: string // Panel ID
  view: PanelView // View type ("journal" | "block")
  viewArgs: Record<string, any> // View parameters
  viewState: Record<string, any> // View state
  width?: number // Width
  height?: number // Height
  locked?: boolean // Is locked
  wide?: boolean // Is wide screen
}
```

## Conventions 约定

When developing plugins for Orca Note, please adhere to the following conventions to ensure compatibility and maintainability:
开发 Orca Note 插件时，请遵循以下约定以确保兼容性和可维护性：

1. **Avoid Reserved Names**: Any name starting with an underscore (`_`) is reserved for system use. Plugin developers should not use such names for commands, renderers, settings, or any other identifiers.
   避免使用保留名称：任何以下划线（ `_` ）开头的名称都保留供系统使用。插件开发者不应使用此类名称用于命令、渲染器、设置或其他标识符。
2. **Use Unique Prefixes**: To avoid conflicts with other plugins, always include a unique prefix related to your plugin in the names of commands, renderers, and other identifiers. For example, if your plugin is named `myplugin`, use a prefix like `myplugin.` for all identifiers (e.g., `myplugin.commandName`, `myplugin.rendererName`).
   使用唯一前缀：为了避免与其他插件冲突，始终在命令、渲染器和其他标识符的名称中包含与您的插件相关的唯一前缀。例如，如果您的插件名称为 `myplugin` ，则所有标识符（例如 `myplugin.commandName` 、 `myplugin.rendererName` ）应使用前缀 `myplugin.` 。
3. **Follow Naming Standards**: Use descriptive and consistent naming conventions for all identifiers. This improves readability and helps other developers understand your code.
   遵循命名规范：对所有标识符使用描述性和一致的命名约定。这提高了可读性，并有助于其他开发者理解您的代码。
4. **Respect System Behavior**: Do not override or interfere with system-level commands, renderers, or UI elements unless explicitly allowed by the API.
   尊重系统行为：除非 API 明确允许，否则不要覆盖或干扰系统级命令、渲染器或 UI 元素。

By following these conventions, you can ensure that your plugin integrates seamlessly with Orca Note and coexists harmoniously with other plugins.
遵循这些约定，您可以确保您的插件与 Orca Note 无缝集成，并与其他插件和谐共存。

## Project Template 项目模板

To quickly start development, you can use the following project template:
要快速开始开发，您可以使用以下项目模板：

- [Basic Plugin Template](https://github.com/sethyuan/orca-plugin-template) - Plugin template with basic configuration
  基本插件模板 - 带有基本配置的插件模板

## Examples 示例

Here are several common plugin development examples to help you quickly get started with Orca Note plugin development:
这里有一些常见的插件开发示例，以帮助您快速开始 Orca Note 插件开发：

## 1\. Simple Command Plugin1. 简单命令插件

This example shows how to create a simple command that inserts a new block with the current time after the current block:
此示例展示了如何创建一个简单的命令，在当前块之后插入一个包含当前时间的新的块：

```
// src/main.ts
export async function load(pluginName: string) {
  const Button = orca.components.Button

  // Register command
  await orca.commands.registerEditorCommand(
    "myplugin.insertTimeBlock",
    async ([_panelId, _rootBlockId, cursor]) => {
      if (!cursor || !cursor.anchor) return null

      const currentBlock = orca.state.blocks[cursor.anchor.blockId]
      if (!currentBlock) return null

      // Get current time
      const now = new Date()
      const timeStr = now.toLocaleTimeString()

      // Create new block content
      const content = [{ t: "t", v: \\`Current time is: ${timeStr}\\` }]

      // Call editor command to insert new block
      await orca.commands.invokeEditorCommand(
        "core.editor.insertBlock",
        null,
        currentBlock,
        "after",
        content,
      )

      return null
    },
    () => {},
    { label: "Insert Time Block" },
  )

  // Register slash command
  await orca.slashCommands.registerSlashCommand("myplugin.insertTimeBlock", {
    icon: "ti ti-clock",
    group: "Utilities",
    title: "Insert Time Block",
    command: "myplugin.insertTimeBlock",
  })
}

export async function unload() {
  // Unregister command
  await orca.commands.unregisterCommand("myplugin.insertTimeBlock")

  // Remove slash command
  await orca.slashCommands.unregisterSlashCommand("myplugin.insertTimeBlock")
}
```

## 2\. Custom Block Renderer2. 自定义块渲染器

This example shows how to create a custom map block renderer:
本示例展示了如何创建自定义地图块渲染器：

```
// src/MapBlock.tsx
import type { Block, DbId } from "./orca.d.ts"

const { useRef, useMemo } = window.React
const { useSnapshot } = window.Valtio
const { BlockShell, BlockChildren } = orca.components

type Props = {
  panelId: string
  blockId: DbId
  rndId: string
  blockLevel: number
  indentLevel: number
  mirrorId?: DbId
  withBreadcrumb?: boolean
  initiallyCollapsed?: boolean
  renderingMode?: "normal" | "simple" | "simple-children" | "readonly"
  keyword: string // Prop to receive from _repr
}

export default function MapBlockRenderer({
  panelId,
  blockId,
  rndId,
  blockLevel,
  indentLevel,
  mirrorId,
  withBreadcrumb,
  initiallyCollapsed,
  renderingMode,
  keyword, // Received from _repr
}: Props) {
  const { blocks } = useSnapshot(orca.state)
  const block = blocks[mirrorId ?? blockId]
  const iframeRef = useRef<HTMLIFrameElement>(null)

  const childrenBlocks = useMemo(
    () => (
      <BlockChildren
        block={block as Block}
        panelId={panelId}
        blockLevel={blockLevel}
        indentLevel={indentLevel}
        renderingMode={renderingMode}
      />
    ),
    [block?.children],
  )

  return (
    <BlockShell
      panelId={panelId}
      blockId={blockId}
      rndId={rndId}
      mirrorId={mirrorId}
      blockLevel={blockLevel}
      indentLevel={indentLevel}
      withBreadcrumb={withBreadcrumb}
      initiallyCollapsed={initiallyCollapsed}
      renderingMode={renderingMode}
      reprClassName="myplugin-repr-map" // Custom class for the block shell
      contentClassName="myplugin-repr-map-content" // Custom class for the content area
      contentAttrs={{ contentEditable: false }} // Prevent editing the iframe itself
      contentJsx={
        <iframe
          ref={iframeRef}
          src={\\`https://ditu.amap.com/search?query=${encodeURIComponent(
            keyword,
          )}\\`}
          width="100%" // Example width
          height="400" // Example height
          style={{ border: 0 }} // Basic styling
          allow="geolocation" // Permissions for the iframe
        />
      }
      childrenJsx={childrenBlocks}
    />
  )
}

// src/main.ts
import MapBlockRenderer from "./MapBlock"

export async function load(pluginName: string) {
  // Register block renderer
  orca.renderers.registerBlock("myplugin.map", false, MapBlockRenderer)

  // Register block converter
  orca.converters.registerBlock("plain", "myplugin.map", (block, repr) => {
    return \\`Map of: ${repr.keyword}\\`
  })

  // Register editor command to insert the map block
  orca.commands.registerEditorCommand(
    "myplugin.insertMapBlockCommand",
    async ([_panelId, _rootBlockId, cursor]) => {
      if (!cursor || !cursor.anchor) return null
      const currentBlock = orca.state.blocks[cursor.anchor.blockId]
      if (!currentBlock) return null

      // Define the representation for the new map block
      const repr = { type: "myplugin.map", keyword: "Beijing" }

      // Insert the new map block after the current block using core.editor.insertBlock
      const { ret: newBlockId } = await orca.commands.invokeEditorCommand(
        "core.editor.insertBlock",
        null, // No initial content needed
        currentBlock, // Reference block
        "after", // Position
        null, // No content fragments
        repr, // Representation object
      )

      return null // Indicate success
    },
    () => {},
    { label: "Insert Map Block" },
  )

  // Register slash command to trigger the map block insertion
  orca.slashCommands.registerSlashCommand("myplugin.insertMapBlock", {
    icon: "ti ti-map-pin", // Icon for the slash command
    group: "Insert", // Group in the slash command menu
    title: "Insert Map Block", // Title displayed in the menu
    command: "myplugin.insertMapBlockCommand", // The editor command to execute
  })
}

export async function unload() {
  // Unregister block renderer
  orca.renderers.unregisterBlock("myplugin.map")

  // Unregister block converter
  orca.converters.unregisterBlock("plain", "myplugin.map")

  // Unregister the editor command
  orca.commands.unregisterCommand("myplugin.insertMapBlockCommand")

  // Unregister the slash command
  orca.slashCommands.unregisterSlashCommand("myplugin.insertMapBlock")
}
```

## 3\. Theme Plugin 3. 主题插件

This example shows how to create a custom theme:
这个示例展示了如何创建自定义主题：

```
// public/sand-yellow.css
@media (prefers-color-scheme: dark) {
  :root {
    /* Sand Yellow Dark Theme */
    --orca-color-bg-1: #3a3226; /* Dark sand/brown */
    --orca-color-bg-2: #4f4639; /* Slightly lighter sand/brown */
    --orca-color-text-1: #f0e6d6; /* Light sand/beige */
    --orca-color-text-2: #bfae90; /* Muted sand/light brown */
    --orca-color-primary-5: #d4ac0d; /* Golden yellow */
    --orca-color-dangerous-5: #e74c3c; /* Standard danger red, or adjust if needed */
    --orca-color-border: #6b5f4e; /* Mid-tone sand/brown */
    --orca-color-selection: oklch(from var(--orca-color-primary-5) l c h / 50%); /* Selection based on primary */
  }
}

// src/index.ts
export async function load(pluginName: string) {
  // Register custom theme
  orca.themes.register(
    "myplugin",           // Plugin name
    "sand-yellow",          // Theme name
    "sand-yellow.css"  // Theme CSS file path
  )
}

export async function unload() {
  // Unregister custom theme
  orca.themes.unregister("sand-yellow")
}
```

Last updated on:最后更新于：
