---
title: Claude Code 入门指南：用 AI 辅助编程的全新体验
published: 2026-05-21
pinned: false
description: 从安装到精通，全面了解 Claude Code 这款强大的 AI 编程助手工具。
tags: [Claude Code, AI, 编程工具, 教程]
category: 技术教程
draft: false
---

Claude Code 是 Anthropic 推出的一款命令行 AI 编程助手，能够直接在你的终端中理解代码库、执行操作并辅助开发。本文将带你从零开始掌握它。

- [什么是 Claude Code](#什么是-claude-code)
- [安装与配置](#安装与配置)
- [核心概念](#核心概念)
- [日常使用技巧](#日常使用技巧)
- [进阶功能](#进阶功能)
- [最佳实践](#最佳实践)

## 什么是 Claude Code

Claude Code 是一个**终端原生的 AI 编程代理**。不同于传统的代码补全工具，它能：

- 理解整个项目结构和代码上下文
- 自主执行文件读写、搜索、终端命令等操作
- 通过工具链完成复杂的多步骤开发任务
- 支持自定义技能和记忆系统，适配你的工作流

简单来说，你只需要用自然语言描述需求，Claude Code 就能帮你完成从代码编写到测试验证的全流程。

## 安装与配置

### 安装

```bash
npm install -g @anthropic-ai/claude-code
```

安装完成后，在任意项目目录下运行：

```bash
claude
```

首次使用需要完成 Anthropic API 的认证配置。

### 基础配置

Claude Code 的配置文件位于 `~/.claude/` 目录下：

- `settings.json` — 全局设置，包括权限策略、MCP 服务器等
- `CLAUDE.md` — 用户级别的全局指令，Claude Code 每次对话都会加载
- `keybindings.json` — 自定义键盘快捷键

项目级别的配置可以放在项目根目录的 `CLAUDE.md` 或 `.claude/settings.json` 中，这些配置会与全局配置叠加生效。

### CLAUDE.md 的作用

`CLAUDE.md` 是 Claude Code 最核心的配置文件之一。你可以在其中编写：

- 项目架构说明和技术栈信息
- 编码规范和风格偏好
- 常用命令和构建步骤
- 自定义的行为指令

每次启动对话时，Claude Code 都会自动读取这些内容作为上下文，让 AI 更好地理解你的项目。

```markdown
# CLAUDE.md 示例

本项目使用 Next.js 14 + TypeScript + Tailwind CSS

## 常用命令
- pnpm dev — 启动开发服务器
- pnpm build — 构建生产版本
- pnpm test — 运行测试

## 编码规范
- 使用函数组件，避免类组件
- 优先使用 Server Components
- 状态管理使用 Zustand
```

## 核心概念

### 工具系统

Claude Code 通过一系列内置工具来与你的环境交互：

| 工具 | 功能 |
| :--- | :--- |
| `Read` | 读取文件内容 |
| `Write` | 创建或覆写文件 |
| `Edit` | 精确替换文件中指定字符串 |
| `Glob` | 按文件模式快速匹配文件路径 |
| `Grep` | 基于正则表达式搜索代码内容 |
| `Bash` | 执行终端命令 |
| `Agent` | 启动子代理处理复杂多步骤任务 |
| `TaskCreate` | 创建任务列表跟踪进度 |

每个工具都有明确的使用场景。比如 `Edit` 优于直接使用 `sed` 命令，因为它是精确的字符串替换；`Grep` 优于 `grep` 命令，因为它针对代码搜索进行了深度优化。

### Agent 子代理

当你面对复杂任务时，Claude Code 可以启动专门的子代理并行工作：

- **Explore** — 快速只读搜索代理，适合定位代码位置
- **Plan** — 软件架构设计代理，帮你制定实现计划
- **code-reviewer** — 独立的代码审查代理
- **general-purpose** — 通用代理，适合多步骤搜索和研究

子代理独立运行，不会污染主会话的上下文窗口，多个子代理可以同时并行执行。

### Skills 技能系统

技能是可复用的专业知识和操作流程。Claude Code 内置了大量技能，覆盖：

- 前端设计（`frontend-design`）
- Java SpringBoot 开发（`java-springboot`）
- MySQL 最佳实践（`mysql-best-practices`）
- Element Plus Vue3 组件库（`element-plus-vue3`）
- 代码审查（`review`）和安全审计（`security-review`）
- 项目初始化（`init`）

你还可以通过 MCP 服务器和社区市场安装更多第三方技能。

## 日常使用技巧

### 快速定位代码

不要再手动翻文件夹了，直接问：

> "用户登录相关的代码在哪里？"

Claude Code 会自动使用 `Glob` 和 `Grep` 组合搜索，精确定位到相关文件和函数。

### 理解陌生代码库

接手新项目时，试试这样问：

> "梳理一下这个项目的整体架构和数据流"

Claude Code 会阅读关键文件，分析目录结构，给出清晰的架构总结。

### 批量重构

> "把 src/api 目录下所有 API 调用从 axios 迁移到 fetch"

Claude Code 会逐个文件修改，同时保持代码风格一致。

### 修复 Bug

> "用户反馈提交订单后页面白屏，帮我定位并修复"

Claude Code 会追踪调用链、检查错误日志，找到根因并给出修复方案。

### 编写测试

> "为 UserService 的 createUser 方法补充单元测试"

Claude Code 会阅读现有测试风格，生成风格一致的测试代码。

## 进阶功能

### 记忆系统

Claude Code 拥有持久化的记忆系统，存放在 `.claude/projects/<project>/memory/` 目录中。它会自动记录：

- **用户偏好** — 你的角色、技术栈偏好、编码习惯
- **反馈记录** — 你纠正过的行为模式，避免重复犯错
- **项目上下文** — 项目目标、重要决策、当前工作方向
- **外部资源** — Bug 追踪系统、监控面板等外部引用

这让每次协作都能从上次中断的地方继续，而不是从零开始。

### 工作树隔离

当需要做大规模实验性改动时，你可以让 Claude Code 在隔离的 Git 工作树中操作：

```bash
# 在对话中直接要求
"在 worktree 中尝试重构数据库层"
```

工作树不会影响当前分支，改动确认无误后再合并。

### MCP 服务器扩展

Model Context Protocol（MCP）允许你接入外部工具和数据源：

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@anthropic-ai/mcp-server-github"]
    }
  }
}
```

配置后，Claude Code 就能直接操作 GitHub Issues、PR、搜索仓库等。

### 定时任务

通过 `/loop` 命令可以让 Claude Code 定期执行任务：

```
/loop 5m 检查部署状态
```

适合需要持续监控的场景，比如等待构建完成、轮询 CI 状态等。

## 最佳实践

### 1. 写好 CLAUDE.md

项目根目录的 `CLAUDE.md` 是投入产出比最高的配置。花 10 分钟写好它，后续每次对话都会受益。

### 2. 从小任务开始

先让 Claude Code 处理一些简单明确的任务建立信任，再逐步交给它更复杂的任务。

### 3. 善用 Plan 模式

对于较大的改动，先进入 Plan 模式让 Claude Code 设计方案，确认后再执行，避免返工：

> "帮我进入 Plan 模式，设计一下用户权限系统的实现方案"

### 4. 明确你的需求

越具体的需求，得到的结果越好。与其说"优化一下性能"，不如说"首页加载用了 5 秒，帮我在 Next.js 中做代码分割和图片懒加载来优化 LCP"。

### 5. 保持代码审查习惯

虽然 Claude Code 输出质量很高，但最终决策权在你手上。重要改动前先用 `git diff` 确认，关键逻辑写测试验证。

### 6. 让它了解你

不要吝啬在对话中告诉 Claude Code 你的偏好。它的记忆系统会学习并适应你的工作方式，用得越久越顺手。

---

Claude Code 不是一个简单的代码补全插件，而是一个能理解上下文、自主执行任务的编程伙伴。从日常的代码搜索、Bug 修复，到复杂的架构重构、自动化工作流，它都能成为你的得力助手。现在就开始尝试，让它成为你开发工具箱中最强大的武器吧。