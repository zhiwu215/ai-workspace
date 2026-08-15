---
name: electron-starter
description: Electron 项目交互式脚手架工具。支持包管理器与框架选择、自动化项目清理、以及样式库与 UI 组件库集成。
disable-model-invocation: true
---

# electron-starter

引导用户交互式创建 Electron 项目，并在初始化后执行代码清理与扩展套件（CSS框架/UI组件库）接入。

## 交互与执行流程

### 步骤 1: 选择包管理器与项目框架
1. **询问包管理器**：向用户询问希望使用哪个包管理器（`npm` / `pnpm` / `yarn`）。
2. **询问项目框架**：读取 [`references/frameworks.md`](references/frameworks.md) 中定义的**受支持框架列表**，请用户选择项目框架（例如：`electron-vite` / `electron-forge`）。
   - **严格限制**：框架选项仅能提供 `references/frameworks.md` 中预设的框架。若选择未开放框架（如 `electron-forge`），明确提示暂未支持。

### 步骤 2: 初始化脚手架
根据用户选定的包管理器与框架，运行 `references/frameworks.md` 中对应的官方构建指令（例如 `pnpm create @quick-start/electron@latest`）。

### 步骤 3: 脚手架初始阶段运行验证 (Gate Check)
构建完成后，在执行任何代码修改或文件清理之前，**首先运行开发服务器进行验证**：
1. 运行选定包管理器的开发指令（如 `npm run dev` / `pnpm dev` / `yarn dev`）。
2. 校验初始脚手架能否正常编译启动并打开应用窗口。
3. **完成条件**：初始项目正常运行无报错。验证成功后结束临时测试服务，进入步骤 4。若启动报致命错误，立即暂停并提示用户排查，禁止直接清理文件。

### 步骤 4: 自动清理冗余文件
初始阶段运行验证通过后，Agent **自动执行代码清理**：
1. 参照 `references/frameworks.md` 中的清理规则，彻底清空 `src\renderer\src\assets\` 目录（删除 `base.css`、`main.css`、`electron.svg`、`wavy-lines.svg` 4个固定文件）。
2. 删除 `src\renderer\src\components\` 下的示例演示组件（如 `Versions.tsx` / `Versions.vue`）。
3. 清理主进程 `src\main\index.ts` 中的冗余 IPC 测试代码（如 `ipcMain.on('ping', ...)`）。
4. 移除 `main.tsx` / `main.ts` 中对已删除 `main.css` 的导入。
5. 重置主渲染入口文件（如 `App.tsx` 或 `App.vue`）为完全空白的纯空壳结构（无任何标题与占位文本）。
6. **完成条件**：项目代码库只保留最基础干净的入口与空壳组件。

### 步骤 5: 询问 CSS 样式工具 / 原子化 CSS 框架
1. 向用户发起询问：“请选择需要集成的 **CSS 样式工具 / 原子化 CSS 框架**”（例如：`Tailwind CSS`、`UnoCSS`、`无（不添加 CSS 框架）`）。
2. **严格限制**：只能提供 [`references/styling-and-ui.md`](references/styling-and-ui.md) 第一部分中写入白名单的 CSS 工具选项。

### 步骤 6: 根据前端框架 (React / Vue) 针对性询问 UI 组件库
1. Agent 识别步骤 2 中用户构建的项目前端技术栈（`React` 或 `Vue`）。
2. 查阅 [`references/styling-and-ui.md`](references/styling-and-ui.md) 第二部分中**与该前端框架匹配的兼容白名单**，向用户发起针对性询问：
   - **React 项目**：仅可提供适用于 React 的组件库选项（如 `shadcn/ui`、`无`）。*（严禁提供 Element Plus 等 Vue 专属库）*
   - **Vue 项目**：仅可提供适用于 Vue 的组件库选项（如 `Element Plus`、`无`）。*（严禁提供 shadcn/ui React 原生版）*
3. **严格限制**：选项必须严格基于白名单且与选定前端框架兼容，绝对禁止推荐未写入白名单的内容。

### 步骤 7: 执行配置与代码集成
1. 根据用户在步骤 5、6 中选定的 CSS 工具和 UI 组件库，读取 [`references/styling-and-ui.md`](references/styling-and-ui.md) 中对应的配置指导。
2. 配合步骤 1 中选择的包管理器（`npm` / `yarn` / `pnpm`），依次执行依赖安装、配置文件创建/修改以及全局样式引入。
3. **完成条件**：所选组件库/样式库的依赖全部安装完成，配置文件与代码修改无语法或类型错误。

### 步骤 8: 最终验证与交付总结
1. 检查最终 `package.json` 中的 `scripts` 命令。
2. 向用户汇报搭建与集成完成结果，并告知启动开发服务器命令（如 `pnpm dev` / `yarn dev` / `npm run dev`）。
