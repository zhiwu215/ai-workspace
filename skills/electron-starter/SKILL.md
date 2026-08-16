---
name: electron-starter
description: Electron 项目交互式脚手架工具。支持包管理器与框架选择、自动化项目清理、以及样式库与 UI 组件库集成。
disable-model-invocation: true
---

# electron-starter

引导用户交互式创建 Electron 项目，并在初始化后执行代码清理与扩展套件（CSS框架/UI组件库）接入。

## 总原则

- **严格按本流程与 references 中的命令执行，禁止探查与多余验证命令**（如 `--help` 探测、`WebFetch`、反复 `install`、`tasklist` 查进程等；**集成完成后严禁执行 `typecheck`、`build`、`test`、`lint` 等任何验证性命令**）。
- **所有需要用户选择的项，都必须由用户决定，禁止用默认值/硬编码替用户决定。**（基础选项通过 `AskUserQuestion` 询问；脚手架交互题由用户手动执行时选择。）
- **搭建阶段不添加任何 UI 组件**，组件由用户在项目完成后手动添加。

## 交互与执行流程

### 步骤 1: 一次性问清基础选项
通过 `AskUserQuestion` 一次性询问以下两项（其余选项由用户在执行脚手架命令时交互选择）：
1. **包管理器**：`npm` / `pnpm` / `yarn`。
2. **项目框架**：读取 [`references/frameworks.md`](references/frameworks.md) 中的受支持框架列表（`electron-vite` / `electron-forge`）。
   - **严格限制**：仅能提供预设框架；选择未开放框架（如 `electron-forge`）时明确提示暂未支持。

### 步骤 2: 生成脚手架命令并交由用户手动执行
1. 依据步骤 1 选择的包管理器，参照 [`references/frameworks.md`](references/frameworks.md) 中的「脚手架手动执行命令」章节，给出对应的 `create` 命令（**不加 `--template`、不传管道**）：
   - **npm**：`npm create @quick-start/electron@latest`
   - **pnpm**：`pnpm create @quick-start/electron@latest`
   - **yarn**：`yarn create @quick-start/electron@latest`
2. **仅输出命令本身**，提示用户在**当前工作目录**下自行运行该命令即可，**不得罗列交互题顺序**（脚手架提问由用户运行时自行完成）。
3. 脚手架为交互式 TUI（`prompts` 库），无法通过管道可靠非交互驱动，**必须由用户手动执行**。等待用户确认完成后再继续（措辞中性，不指定用户必须回复特定几个字）。

### 步骤 3: 探测项目与技术栈并安装依赖
用户回复后，Agent 自动执行：
1. 定位当前目录下新生成的 Electron 项目目录（若无法唯一确定，询问用户项目名）。
2. 读取项目 `package.json` 与目录结构，识别前端框架（`React` / `Vue` / `Vanilla`）与语言（`TypeScript` / `JavaScript`），用于后续清理与 UI 库询问。
3. 在项目目录内执行 `<pm> install`（脚手架仅生成文件，不自动安装依赖）。

### 步骤 4: 处理 npm v12 脚本拦截（已知坑）
> npm v12 起默认开启 install-scripts 白名单，`electron` 的 postinstall（下载内核）会被拦截，导致后续 `npm run dev` 报错。

1. `npm install` 后若输出含 `install-scripts blocked` 且涉及 `electron`，按 [`references/frameworks.md`](references/frameworks.md) 的「已知问题」章节执行批准并重装依赖：
   - 批准脚本：`npm install-scripts approve electron esbuild electron-winstaller`
   - 清理重装：删除 `node_modules` 与 `package-lock.json` 后重新执行 `npm install`。
2. 若重新 `npm install` 后仍输出 `install-scripts blocked` 等错误，立即暂停并提示用户排查，禁止继续清理。

### 步骤 5: 自动清理冗余文件
Agent **自动执行代码清理**：
1. 参照 `references/frameworks.md` 中的清理规则，彻底清空 `src\renderer\src\assets\` 目录（删除 `base.css`、`main.css`、`electron.svg`、`wavy-lines.svg` 4个固定文件）。
2. 删除 `src\renderer\src\components\` 下的示例演示组件（如 `Versions.tsx` / `Versions.vue`）。
3. 清理主进程 `src\main\index.ts` 中的冗余 IPC 测试代码（如 `ipcMain.on('ping', ...)`）。
4. 移除 `main.tsx` / `main.ts` 中对已删除 `main.css` 的导入。
5. 重置主渲染入口文件（如 `App.tsx` 或 `App.vue`）为完全空白的纯空壳结构（无任何标题与占位文本）。
6. **完成条件**：项目代码库只保留最基础干净的入口与空壳组件。

### 步骤 6: 询问 CSS 样式工具与 UI 组件库
1. Agent 依据步骤 3 识别的前端技术栈（`React` / `Vue` / `Vanilla`），查阅 [`references/styling-and-ui.md`](references/styling-and-ui.md) 的兼容白名单，通过 `AskUserQuestion` 向用户发起询问：
   - **React 项目**（可一次性询问两项）：
     - **CSS 样式工具**：`Tailwind CSS` / `无`
     - **UI 组件库**：`shadcn/ui` / `无`
   - **Vue / Vanilla 项目**：
     - **CSS 样式工具**：`Tailwind CSS` / `无`
     - **UI 组件库**：提示“当前技术栈暂未开启 UI 组件库支持”，直接跳过或仅提供 `无`。
2. **严格限制**：选项必须严格基于白名单且与选定前端框架兼容，绝对禁止推荐未写入白名单的内容。

### 步骤 7: 执行配置与代码集成
1. 根据用户选定的 CSS 工具和 UI 组件库，读取 [`references/styling-and-ui.md`](references/styling-and-ui.md) 中对应的配置指导。
2. 配合选定的包管理器，依次执行依赖安装、配置文件创建/修改以及全局样式引入。
3. **不添加任何 UI 组件**：`shadcn add` 等组件添加命令由用户在项目搭建完成后手动执行，Agent 不得运行。
4. **严禁执行校验命令**：脚手架与扩展套件配置为确定性流程，配置与依赖就绪后，**严禁执行 `typecheck`、`build`、`test`、`lint` 等任何验证命令**，直接进入步骤 8 进行交付总结。
5. **完成条件**：所选组件库/样式库的依赖全部安装完成，配置文件与代码修改到位。

### 步骤 8: 交付总结
1. 向用户汇报搭建与集成完成结果，告知用户现在可以运行。
2. 告知用户启动开发服务器命令（`npm run dev` / `pnpm dev` / `yarn dev`），由用户运行确认。
3. **总结中不要提及组件添加命令**（如 `shadcn add`）；组件由用户后续自行添加。
