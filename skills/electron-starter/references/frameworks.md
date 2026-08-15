# 框架配置与清理规则指南 (Frameworks & Cleanup Rules)

本文件定义 `electron-starter` 允许使用的基础框架选项、包管理器创建指令及对应的自动清理逻辑。

---

## 受支持的框架选项列表

Agent 向用户询问时，**只允许**提供以下固定选项：
1. **`electron-vite`** (现代 Vite 驱动的 Electron 脚手架)
2. **`electron-forge`** (暂未配置，选择时提示用户暂未开启)

---

## 1. electron-vite

### 构建指令

- **npm**: `npm create @quick-start/electron@latest`
- **pnpm**: `pnpm create @quick-start/electron@latest`
- **yarn**: `yarn create @quick-start/electron@latest`

---

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
     function App(): JSX.Element {
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
