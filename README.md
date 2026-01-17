[English](#english) | [中文](#中文)

---

<a name="english"></a>

# Code Review Router

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

An **Agent Skill** that intelligently routes code reviews between Gemini CLI and Codex CLI based on tech stack, complexity, and change characteristics.

## What is an Agent Skill?

Agent Skills are reusable packages of knowledge that extend what AI agents can do. A Skill is a markdown file (SKILL.md) containing instructions that the agent follows when working on specific tasks. Skills follow a **progressive disclosure** pattern:

1. **Discovery**: The agent sees available Skills with their names and descriptions
2. **Activation**: When your task matches a Skill's description, the agent loads the full instructions
3. **Execution**: The agent follows the Skill's instructions automatically

This Skill works with both **Claude Code** and **Google Antigravity**.

## Why Use a Decision Tree Skill?

### Advantages of Decision Tree-Based Routing

| Advantage | Description |
|-----------|-------------|
| **Deterministic & Predictable** | Unlike free-form prompts, decision trees follow explicit rules, ensuring consistent routing every time |
| **Transparent Logic** | Every routing decision can be traced back to specific criteria—no black-box behavior |
| **Optimized Tool Selection** | Automatically selects the best tool (Gemini vs Codex) based on change characteristics |
| **Reduced Token Usage** | Pre-defined decision paths eliminate the need for lengthy reasoning chains |
| **Extensible** | Easy to add new routing rules or modify existing ones without rewriting the entire logic |
| **Fallback Handling** | Built-in graceful degradation when the primary tool fails |

### When Decision Trees Excel

- **Multi-tool orchestration**: Choosing between different CLIs, APIs, or services
- **Complex conditional logic**: When routing depends on multiple factors
- **Audit requirements**: When you need to explain why a specific tool was selected
- **Performance optimization**: Avoiding unnecessary reasoning overhead

## Features

- 🔀 **Smart Routing** — Automatically selects Gemini or Codex based on code characteristics
- 📊 **Complexity Scoring** — Calculates a 0-10 complexity score based on multiple factors
- 🔍 **Language Detection** — Identifies TypeScript, Python, Go, Rust, and 10+ languages
- 🛡️ **Security Pattern Recognition** — Detects auth, credentials, and sensitive code patterns
- 🔄 **Automatic Fallback** — Switches to alternative CLI if the primary one fails
- ⏱️ **Timeout Protection** — 120-second safeguard prevents hanging reviews

## Installation

### For Claude Code

Place the skill file in one of these locations:
```bash
# Project-specific (shared with team)
.claude/skills/code-review-router/SKILL.md

# Personal (all projects)
~/.claude/skills/code-review-router/SKILL.md
```

### For Google Antigravity

Place the skill file in one of these locations:
```bash
# Workspace-specific
<workspace-root>/.agent/skills/code-review-router/SKILL.md

# Global (all workspaces)
~/.gemini/antigravity/skills/code-review-router/SKILL.md
```

## Prerequisites

- A Git repository with uncommitted changes
- At least one CLI installed:
  - [Gemini CLI](https://github.com/google-gemini/gemini-cli) — Google's command-line AI assistant
  - [Codex CLI](https://github.com/openai/codex) — OpenAI's code review tool

## Usage

Simply ask the agent to review your code:
```
Review my current changes
```

or
```
/code-review-router
```

The Skill automatically:
1. Verifies Git repository and CLI prerequisites
2. Analyzes your diff (files, lines, patterns)
3. Calculates complexity score
4. Routes to the optimal CLI
5. Presents formatted review results

## Routing Decision Tree

The routing follows a priority-based decision tree:

### Priority 1: Pattern-Based Rules (Hard Rules)

| Pattern | Routes To | Reason |
|---------|-----------|--------|
| Security-sensitive files/code | Codex | Requires careful security analysis |
| Files > 20 OR lines > 500 | Codex | Large changeset needs thorough review |
| Database migrations/schema | Codex | Architectural risk |
| API/service layer changes | Codex | Backend architectural changes |
| Changes span 3+ directories | Codex | Multi-service impact |
| Complex TypeScript generics | Codex | Type system complexity |
| Pure frontend (jsx/vue/css) | Gemini | Simpler, visual-focused review |
| Python ecosystem | Gemini | Strong Python support |
| Documentation only | Gemini | Simple text review |

### Priority 2: Complexity Score

| Score | Routes To | Reason |
|-------|-----------|--------|
| ≥ 6 | Codex | High complexity warrants deeper analysis |
| < 6 | Gemini | Moderate complexity, prefer speed |

### Priority 3: Default

Falls back to **Gemini** for faster feedback on unclear cases.

## Complexity Score Calculation

| Condition | Points |
|-----------|--------|
| Files changed > 10 | +2 |
| Files changed > 20 | +3 (additional) |
| Lines changed > 300 | +2 |
| Lines changed > 500 | +3 (additional) |
| Multiple directories | +1 |
| Test files included | +1 |
| Config files changed | +1 |
| Database/schema changes | +2 |
| API route changes | +2 |
| Service layer changes | +2 |

## Quick Reference

| Change Type | Route | Reason |
|-------------|-------|--------|
| React component styling | Gemini | Pure frontend |
| Django view update | Gemini | Python ecosystem |
| Single bug fix < 50 lines | Gemini | Simple change |
| New API endpoint + tests | Codex | Architectural |
| Auth system changes | Codex | Security-sensitive |
| Database migration | Codex | Schema change |
| Multi-service refactor | Codex | High complexity |

## License

MIT

---

<a name="中文"></a>

# 代码审查路由器

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

一个 **Agent Skill**，根据技术栈、复杂度和代码变更特征，智能地在 Gemini CLI 和 Codex CLI 之间路由代码审查。

## 什么是 Agent Skill？

Agent Skills 是可复用的知识包，用于扩展 AI 代理的能力。Skill 是一个包含指令的 markdown 文件（SKILL.md），代理在处理特定任务时会遵循这些指令。Skills 采用**渐进式披露**模式：

1. **发现阶段**：代理看到可用 Skills 的名称和描述
2. **激活阶段**：当你的任务匹配某个 Skill 的描述时，代理加载完整指令
3. **执行阶段**：代理自动遵循 Skill 的指令执行任务

此 Skill 同时支持 **Claude Code** 和 **Google Antigravity**。

## 为什么使用决策树 Skill？

### 基于决策树路由的优势

| 优势 | 描述 |
|------|------|
| **确定性与可预测性** | 与自由形式的提示不同，决策树遵循明确规则，确保每次路由结果一致 |
| **逻辑透明** | 每个路由决策都可以追溯到具体条件——没有黑盒行为 |
| **优化工具选择** | 根据代码变更特征自动选择最佳工具（Gemini vs Codex） |
| **减少 Token 消耗** | 预定义的决策路径消除了冗长的推理链需求 |
| **易于扩展** | 可以轻松添加新的路由规则或修改现有规则，无需重写整个逻辑 |
| **故障回退处理** | 当主要工具失败时，内置优雅降级机制 |

### 决策树的最佳使用场景

- **多工具编排**：在不同的 CLI、API 或服务之间进行选择
- **复杂条件逻辑**：当路由依赖多个因素时
- **审计需求**：当需要解释为什么选择特定工具时
- **性能优化**：避免不必要的推理开销

## 功能特性

- 🔀 **智能路由** — 根据代码特征自动选择 Gemini 或 Codex
- 📊 **复杂度评分** — 基于多个因素计算 0-10 的复杂度分数
- 🔍 **语言检测** — 识别 TypeScript、Python、Go、Rust 等 10+ 种语言
- 🛡️ **安全模式识别** — 检测认证、凭证和敏感代码模式
- 🔄 **自动故障回退** — 主 CLI 失败时自动切换到备用 CLI
- ⏱️ **超时保护** — 120 秒安全机制防止审查挂起

## 安装

### Claude Code

将 skill 文件放置在以下位置之一：
```bash
# 项目级（与团队共享）
.claude/skills/code-review-router/SKILL.md

# 个人级（所有项目）
~/.claude/skills/code-review-router/SKILL.md
```

### Google Antigravity

将 skill 文件放置在以下位置之一：
```bash
# 工作区级
<workspace-root>/.agent/skills/code-review-router/SKILL.md

# 全局级（所有工作区）
~/.gemini/antigravity/skills/code-review-router/SKILL.md
```

## 前置条件

- 包含未提交更改的 Git 仓库
- 至少安装以下 CLI 之一：
  - [Gemini CLI](https://github.com/google-gemini/gemini-cli) — Google 的命令行 AI 助手
  - [Codex CLI](https://github.com/openai/codex) — OpenAI 的代码审查工具

## 使用方法

只需让代理审查你的代码：
```
审查我当前的更改
```

或者
```
/code-review-router
```

Skill 会自动：
1. 验证 Git 仓库和 CLI 前置条件
2. 分析你的 diff（文件、行数、模式）
3. 计算复杂度分数
4. 路由到最优的 CLI
5. 呈现格式化的审查结果

## 路由决策树

路由遵循基于优先级的决策树：

### 优先级 1：模式匹配规则（硬规则）

| 模式 | 路由至 | 原因 |
|------|--------|------|
| 安全敏感文件/代码 | Codex | 需要仔细的安全分析 |
| 文件 > 20 或行数 > 500 | Codex | 大型变更集需要彻底审查 |
| 数据库迁移/Schema | Codex | 架构风险 |
| API/服务层变更 | Codex | 后端架构变更 |
| 变更跨越 3+ 目录 | Codex | 多服务影响 |
| 复杂 TypeScript 泛型 | Codex | 类型系统复杂度 |
| 纯前端 (jsx/vue/css) | Gemini | 更简单、专注视觉的审查 |
| Python 生态系统 | Gemini | 强大的 Python 支持 |
| 仅文档 | Gemini | 简单文本审查 |

### 优先级 2：复杂度分数

| 分数 | 路由至 | 原因 |
|------|--------|------|
| ≥ 6 | Codex | 高复杂度需要更深入的分析 |
| < 6 | Gemini | 中等复杂度，优先速度 |

### 优先级 3：默认

对于不明确的情况，回退到 **Gemini** 以获得更快的反馈。

## 复杂度分数计算

| 条件 | 分数 |
|------|------|
| 变更文件 > 10 | +2 |
| 变更文件 > 20 | +3（额外） |
| 变更行数 > 300 | +2 |
| 变更行数 > 500 | +3（额外） |
| 涉及多个目录 | +1 |
| 包含测试文件 | +1 |
| 配置文件变更 | +1 |
| 数据库/Schema 变更 | +2 |
| API 路由变更 | +2 |
| 服务层变更 | +2 |

## 快速参考

| 变更类型 | 路由 | 原因 |
|----------|------|------|
| React 组件样式 | Gemini | 纯前端 |
| Django 视图更新 | Gemini | Python 生态系统 |
| 单个 bug 修复 < 50 行 | Gemini | 简单变更 |
| 新 API 端点 + 测试 | Codex | 架构性 |
| 认证系统变更 | Codex | 安全敏感 |
| 数据库迁移 | Codex | Schema 变更 |
| 多服务重构 | Codex | 高复杂度 |

## 许可证

MIT
