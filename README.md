# AI Workspace

这是一个个人 AI 工作区仓库，用于集中管理、积累和维护各类 AI Agent 的**指令规范（Instructions）**、**提示词模板（Prompts）**以及**技能扩展（Skills）**。

---

## 目录结构

```text
ai-workspace\
  ├── instructions\             # 全局或常驻说明文档 (按需复制为 AGENTS.md / CLAUDE.md)
  └── skills\                   # 按需加载的 Agent 技能集 (遵循 writing-for-agents 规范)
        ├── repo-replay\            # 仓库重演：以作者视角从零复刻项目
        └── electron-starter\       # Electron 项目脚手架与扩展配置
```

---

## 模块说明

### 1. skills 目录 (推荐)

遵循渐进式披露（Progressive Disclosure）与按需加载规范，供 Agent 在特定任务场景下精准激活或通过斜杠命令直接调用：

- **[`skills/repo-replay`](./skills/repo-replay/SKILL.md)**
  - **适用场景**：研读开源代码、复刻项目、向他人讲解复杂系统
  - **核心特色**：站在开发者从 0 构建视角，以“Single-File Spike ➔ 痛点驱动三段式 ➔ 增量挂载增强模块 ➔ Diff Matrix 对照”的方式复刻项目。
- **[`skills/electron-starter`](./skills/electron-starter/SKILL.md)**
  - **适用场景**：初始化 Electron 工程
  - **核心特色**：交互式脚手架引导、代码自动化清理、Tailwind / shadcn/ui 集成。

### 2. instructions 目录

记录适合直接作为项目全局约束或注入至通用系统提示词的说明文档：

- **[`instructions/project-learning-dev-evolution.md`](./instructions/project-learning-dev-evolution.md)**
  - **适用场景**：若需要在无 Skill 支持的轻量工具中直接使用，可作为静态规则参考（复杂场景推荐直接使用 `skills/repo-replay`）。

---

## 使用指南

### 使用 Skills (技能)
- **调用方式**：在对话中输入 `/repo-replay` 或描述“请用仓库重演的方式带我学习这个项目”，Agent 会自动加载技能执行。

### 使用 Instructions (指令)
1. **选择指令文档**：在 `instructions\` 目录下选择符合需求的文档。
2. **复制到目标项目**：将该文件复制到目标项目的根目录。
3. **改名为对应平台的标准文件名**：修改为目标工具识别的文件名（如 `AGENTS.md` 或 `CLAUDE.md`）。