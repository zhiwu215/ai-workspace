# 框架配置与清理规则指南 (Frameworks & Cleanup Rules)

本文件定义 `electron-starter` 允许使用的基础框架选项、包管理器创建指令及对应的自动清理逻辑。

---

## 受支持的框架选项列表

Agent 向用户询问时，**只允许**提供以下固定选项：
1. **`electron-vite`** (现代 Vite 驱动的 Electron 脚手架)
2. **`electron-forge`** (暂未配置，选择时提示用户暂未开启)

---

## 脚手架手动执行命令

`<pm> create @quick-start/electron@latest` 为**交互式 TUI 命令**（基于 `prompts` 库），无法通过管道可靠非交互驱动，**必须由用户手动执行**；脚手架提问由用户运行时自行完成，**Agent 不得向用户罗列交互题顺序**。

### 手动执行命令
- **npm**: `npm create @quick-start/electron@latest`
- **pnpm**: `pnpm create @quick-start/electron@latest`
- **yarn**: `yarn create @quick-start/electron@latest`

> 注：脚手架仅生成文件，不自动安装依赖。提示用户创建完成后**切勿手动运行 install 安装依赖**（后续需由 Agent 先配置 `package.json` 中的 `allowScripts` 脚本白名单后再统一执行 `<pm> install`）。

---

## 已知问题与配置：npm v12 脚本白名单预设

npm v12 起默认开启 install-scripts 白名单，`electron` 的 postinstall（下载内核）会被拦截，导致后续运行报错。

**处理方式**：
在执行依赖安装（`<pm> install`）**之前**，在项目 `package.json` 的末尾添加 `"allowScripts"` 配置：

```json
"allowScripts": {
  "electron": true,
  "esbuild": true,
  "electron-winstaller": true
}
```

配置完成后直接执行 `<pm> install` 即可正常安装依赖并自动放行 electron 内核下载脚本。

---

## 1. electron-vite

### 脚手架清理规则（构建完成后自动执行）

项目搭建完成后，Agent 需自动扫描并执行以下明确清理：

1. **彻底清空 `src\renderer\src\assets\` 目录**:
   删除 `src\renderer\src\assets\` 下所有默认生成的文件：
   - `base.css`
   - `main.css`
   - `electron.svg`
   - `wavy-lines.svg`

2. **清理示例组件与主进程测试代码**:
   - **示例组件**: 删除 `src\renderer\src\components\` 目录下的演示组件（如 `Versions.tsx` / `Versions.vue`）。
   - **主进程逻辑 (`src\main\index.ts`)**: 删除无用的 IPC 测试示例代码（如 `ipcMain.on('ping', ...)` 及其注释）。

3. **修复入口文件与重置主组件**:
   - **`src\renderer\src\main.tsx` / `main.ts` / `main.js`**:
     移除默认导入的 `import './assets/main.css'`。
   - **`App.tsx` / `App.jsx`**:
     重置为空白空壳代码（不包含任何标题与占位文字）：
     ```tsx
     function App(): React.JSX.Element {
       return (
         <div></div>
       )
     }

     export default App
     ```
   - **`App.vue`**:
     重置为空白最简模板（不包含任何标题与占位文字）：
     ```vue
     <script setup lang="ts">
     </script>

     <template>
       <div></div>
     </template>
     ```

---

## 2. electron-forge (暂未配置)

目前暂未配置具体的构建与清理指令。若用户选择，请提示该选项暂未支持。
