# 样式工具与 UI 组件库白名单配置指南 (Styling Tools & UI Component Libraries)

本文件定义在 **`electron-vite`** 项目中允许调用的 CSS 样式工具、UI 组件库白名单，及其对应的适用前端框架（React / Vue）与安装配置指导。

> **严格提示规则**：Agent 向用户提问时，只能使用本文件中明确列出且与当前前端框架兼容的白名单选项。

---

## 第一部分：CSS 样式工具 / 原子化 CSS 框架 (CSS Tools & Utility Frameworks)

目前仅支持以下 CSS 样式工具选项：
1. **`Tailwind CSS`**
2. **`无`** (不添加 CSS 框架)

---

### 1. Tailwind CSS
- **支持的前端框架**: `React`, `Vue`, `Vanilla` (支持 TS 与 JS)
- **说明**: 现代原子化 CSS 框架。在 `electron-vite` 中推荐采用基于 Vite 插件的 Tailwind CSS v4 配置方案。

#### 安装与配置步骤
1. **安装依赖**:
   - **npm**: `npm install tailwindcss @tailwindcss/vite`
   - **pnpm**: `pnpm add tailwindcss @tailwindcss/vite`
   - **yarn**: `yarn add tailwindcss @tailwindcss/vite`

2. **配置 `electron.vite.config.ts` (或 `electron.vite.config.js`)**:
   - 顶部导入插件：`import tailwindcss from '@tailwindcss/vite'`
   - 在 `renderer` 对象的 `plugins` 列表中追加 `tailwindcss()`：
     ```ts
     renderer: {
       // ...
       plugins: [react(), tailwindcss()] // Vue 项目为 [vue(), tailwindcss()]
     }
     ```

3. **创建全局样式文件**:
   在渲染层新建 `src\renderer\src\assets\globals.css` 并添加 Tailwind 导入指令：
   ```css
   @import "tailwindcss";
   ```

4. **入口引入校验**:
   在 `src\renderer\src\main.tsx` (或 `main.jsx` / `main.ts` / `main.js` / `main.vue`) 中导入样式：
   ```ts
   import './assets/globals.css'
   ```

---

## 第二部分：UI 组件库 (UI Component Libraries)

目前仅支持以下 UI 组件库选项：
1. **`shadcn/ui`** (支持 React + TS 或 React + JS 项目)
2. **`无`** (不添加 UI 组件库)

*(注：如果当前脚手架选择的是 Vue 或 Vanilla 等非 React 框架，UI 组件库阶段应明确提示用户“当前技术栈暂未开启 UI 组件库支持”，直接跳过或仅选择“无”)*

---

### 1. shadcn/ui
- **支持的前端框架**: **`React`** (支持 TypeScript / JavaScript) *(严禁在 Vue 或其他非 React 项目中提供此选项)*
- **说明**: 零依赖、高定制性的 React 组件库范式。

#### electron-vite 适配核心要点
- **路径别名规范**：普通纯前端 Vite 项目通常将 `@` 别名映射到 `src`。而 `electron-vite` 是 Main（主进程）、Preload（预加载脚本）、Renderer（渲染进程）三进程架构，三套代码共享同一份根目录 `tsconfig.json`。`electron-vite` 默认已为渲染层配置了 `@renderer` 别名（映射到 `src/renderer/src`），因此无需修改 `tsconfig.json`，在 shadcn 配置中直接使用 `@renderer` 别名。

#### 安装与配置步骤
1. **安装基础与扩展依赖**:
   - **npm**: `npm install shadcn class-variance-authority clsx tailwind-merge lucide-react tw-animate-css`
   - **pnpm**: `pnpm add shadcn class-variance-authority clsx tailwind-merge lucide-react tw-animate-css`
   - **yarn**: `yarn add shadcn class-variance-authority clsx tailwind-merge lucide-react tw-animate-css`

2. **配置全局样式文件 `src\renderer\src\assets\globals.css`**:
   将全局样式文件更新为 shadcn / Tailwind v4 完整设计变量：
   ```css
   @import "tailwindcss";
   @import "tw-animate-css";
   @import "shadcn/tailwind.css";

   @custom-variant dark (&:is(.dark *));

   @theme inline {
     --color-background: var(--background);
     --color-foreground: var(--foreground);
     --color-card: var(--card);
     --color-card-foreground: var(--card-foreground);
     --color-popover: var(--popover);
     --color-popover-foreground: var(--popover-foreground);
     --color-primary: var(--primary);
     --color-primary-foreground: var(--primary-foreground);
     --color-secondary: var(--secondary);
     --color-secondary-foreground: var(--secondary-foreground);
     --color-muted: var(--muted);
     --color-muted-foreground: var(--muted-foreground);
     --color-accent: var(--accent);
     --color-accent-foreground: var(--accent-foreground);
     --color-destructive: var(--destructive);
     --color-destructive-foreground: var(--destructive-foreground);
     --color-border: var(--border);
     --color-input: var(--input);
     --color-ring: var(--ring);
     --color-chart-1: var(--chart-1);
     --color-chart-2: var(--chart-2);
     --color-chart-3: var(--chart-3);
     --color-chart-4: var(--chart-4);
     --color-chart-5: var(--chart-5);
     --radius-sm: calc(var(--radius) * 0.6);
     --radius-md: calc(var(--radius) * 0.8);
     --radius-lg: var(--radius);
     --radius-xl: calc(var(--radius) * 1.4);
     --radius-2xl: calc(var(--radius) * 1.8);
     --radius-3xl: calc(var(--radius) * 2.2);
     --radius-4xl: calc(var(--radius) * 2.6);
     --color-sidebar: var(--sidebar);
     --color-sidebar-foreground: var(--sidebar-foreground);
     --color-sidebar-primary: var(--sidebar-primary);
     --color-sidebar-primary-foreground: var(--sidebar-primary-foreground);
     --color-sidebar-accent: var(--sidebar-accent);
     --color-sidebar-accent-foreground: var(--sidebar-accent-foreground);
     --color-sidebar-border: var(--sidebar-border);
     --color-sidebar-ring: var(--sidebar-ring);
   }

   :root {
     --radius: 0.625rem;
     --background: oklch(1 0 0);
     --foreground: oklch(0.145 0 0);
     --card: oklch(1 0 0);
     --card-foreground: oklch(0.145 0 0);
     --popover: oklch(1 0 0);
     --popover-foreground: oklch(0.145 0 0);
     --primary: oklch(0.205 0 0);
     --primary-foreground: oklch(0.985 0 0);
     --secondary: oklch(0.97 0 0);
     --secondary-foreground: oklch(0.205 0 0);
     --muted: oklch(0.97 0 0);
     --muted-foreground: oklch(0.556 0 0);
     --accent: oklch(0.97 0 0);
     --accent-foreground: oklch(0.205 0 0);
     --destructive: oklch(0.577 0.245 27.325);
     --border: oklch(0.922 0 0);
     --input: oklch(0.922 0 0);
     --ring: oklch(0.708 0 0);
     --chart-1: oklch(0.646 0.222 41.116);
     --chart-2: oklch(0.6 0.118 184.704);
     --chart-3: oklch(0.398 0.07 227.392);
     --chart-4: oklch(0.828 0.189 84.429);
     --chart-5: oklch(0.769 0.188 70.08);
     --sidebar: oklch(0.985 0 0);
     --sidebar-foreground: oklch(0.145 0 0);
     --sidebar-primary: oklch(0.205 0 0);
     --sidebar-primary-foreground: oklch(0.985 0 0);
     --sidebar-accent: oklch(0.97 0 0);
     --sidebar-accent-foreground: oklch(0.205 0 0);
     --sidebar-border: oklch(0.922 0 0);
     --sidebar-ring: oklch(0.708 0 0);
   }

   .dark {
     --background: oklch(0.145 0 0);
     --foreground: oklch(0.985 0 0);
     --card: oklch(0.205 0 0);
     --card-foreground: oklch(0.985 0 0);
     --popover: oklch(0.205 0 0);
     --popover-foreground: oklch(0.985 0 0);
     --primary: oklch(0.922 0 0);
     --primary-foreground: oklch(0.205 0 0);
     --secondary: oklch(0.269 0 0);
     --secondary-foreground: oklch(0.985 0 0);
     --muted: oklch(0.269 0 0);
     --muted-foreground: oklch(0.708 0 0);
     --accent: oklch(0.269 0 0);
     --accent-foreground: oklch(0.985 0 0);
     --destructive: oklch(0.704 0.191 22.216);
     --border: oklch(1 0 0 / 10%);
     --input: oklch(1 0 0 / 15%);
     --ring: oklch(0.556 0 0);
     --chart-1: oklch(0.488 0.243 264.376);
     --chart-2: oklch(0.696 0.17 162.48);
     --chart-3: oklch(0.769 0.188 70.08);
     --chart-4: oklch(0.627 0.265 303.9);
     --chart-5: oklch(0.645 0.246 16.439);
     --sidebar: oklch(0.205 0 0);
     --sidebar-foreground: oklch(0.985 0 0);
     --sidebar-primary: oklch(0.488 0.243 264.376);
     --sidebar-primary-foreground: oklch(0.985 0 0);
     --sidebar-accent: oklch(0.269 0 0);
     --sidebar-accent-foreground: oklch(0.985 0 0);
     --sidebar-border: oklch(1 0 0 / 10%);
     --sidebar-ring: oklch(0.556 0 0);
   }

   @layer base {
     * {
       @apply border-border outline-ring/50;
     }
     body {
       @apply bg-background text-foreground;
     }
   }
   ```

3. **创建工具函数 `src\renderer\src\lib\utils.ts` (或 `utils.js`)**:
   - **TS 版本 (`utils.ts`)**:
     ```ts
     import { clsx, type ClassValue } from 'clsx'
     import { twMerge } from 'tailwind-merge'

     export function cn(...inputs: ClassValue[]) {
       return twMerge(clsx(inputs))
     }
     ```
   - **JS 版本 (`utils.js`)**:
     ```js
     import { clsx } from 'clsx'
     import { twMerge } from 'tailwind-merge'

     export function cn(...inputs) {
       return twMerge(clsx(inputs))
     }
     ```

4. **创建根目录配置文件 `components.json`**:
   在项目根目录创建 `components.json`（统一使用 `@renderer` 别名）：
   - **TypeScript 项目**:
     ```json
     {
       "$schema": "https://ui.shadcn.com/schema.json",
       "style": "base-nova",
       "rsc": false,
       "tsx": true,
       "tailwind": {
         "config": "",
         "css": "@renderer/assets/globals.css",
         "baseColor": "neutral",
         "cssVariables": true,
         "prefix": ""
       },
       "aliases": {
         "components": "@renderer/components",
         "utils": "@renderer/lib/utils",
         "ui": "@renderer/components/ui",
         "lib": "@renderer/lib",
         "hooks": "@renderer/hooks"
       },
       "iconLibrary": "lucide"
     }
     ```
   - **JavaScript 项目**:
     将上述配置中的 `"tsx": true` 调整为 `"tsx": false`。

5. **入口引入与组件添加**:
   - **入口引入**: 确认 `src\renderer\src\main.tsx` (或 `main.jsx`) 中已引入 `import './assets/globals.css'`。
   - **添加组件（用户手动执行，Agent 不得运行）**: 搭建阶段 **不添加任何组件**。组件由用户在项目完成后手动执行 shadcn add 指令添加（例如 `npx shadcn@latest add button` / `pnpm dlx shadcn@latest add button`），组件将生成在 `src/renderer/src/components/ui/` 目录下。

---

## 新增组件库 / CSS 工具拓展指南

后续需要增加新库时，请在对应分类下按格式声明：
1. 库名称与描述
2. 明确标注 `支持的前端框架: React / Vue`
3. 安装与配置文件修改命令

