---
layout: post
title: "从 Vibe Coding 到 Agentic Engineering：一文读懂 AI 编程的工程化革命"
date: 2026-03-21 09:32:25 +0800
categories: [与AI共生]
tags: [Claude Code, Agentic Engineering, Vibe Coding, Context Engineering, ECC, Agent Harness]
summary: "Vibe Coding 让你上路，Agentic Engineering 让你到达目的地。这篇文章从'为什么需要工程化'出发，以 everything-claude-code 为核心案例，完整拆解 AI 编程工程化的五大支柱：Context Engineering、Agent 编排、Hook 治理、持续学习与安全审计。"
pillars:
  - 与AI共生
---

## 写在开头：一个正在发生的范式交接

2025 年 2 月，Andrej Karpathy 在 X 上写道：

> "There's a new kind of coding I call **vibe coding**... you fully give in to the vibes, embrace exponentials, and forget that the code even exists."

这条帖子引爆了 AI 编程讨论。一年后的 2026 年初，Karpathy 自己承认——"**Vibe coding might be becoming passé**"——取而代之的是更结构化的 **Agentic Engineering**。

这不是概念游戏。它反映了一个真实的工程问题：

**当你从"让 AI 帮我写个功能"走向"让 AI 帮我持续交付一个系统"时，一切都会崩塌——除非你认真对待工程化。**

本文以开源项目 [everything-claude-code](https://github.com/affaan-m/everything-claude-code)（**50K+ stars，30 位贡献者**，Anthropic Hackathon 获奖作品）为核心案例，拆解 AI 编程工程化的五个关键支柱，并给出可落地的实践路径。

---

## 第一章：Vibe Coding 的天花板在哪里？

### 1.1 Vibe Coding 并不是贬义词

先正视它的价值：

- **创意探索**：想到什么立刻让 AI 实现，快速验证直觉
- **原型速度**：一个下午做出可交互的 MVP
- **降低门槛**：非程序员也能参与产品构建
- **学习工具**：通过对话理解代码，比看文档高效

很多优秀产品的第一版就是"vibe"出来的——包括 Karpathy 自己提到的"throwaway weekend projects"。

### 1.2 问题出在哪里？

当一个 vibe coding 项目需要**持续维护、多人协作、上线运行**时，你会遇到四类系统性问题：

```
┌─────────────────────────────────────────────────────────┐
│                AI 编程的四面高墙                          │
├──────────────┬──────────────────────────────────────────┤
│ 上下文预算   │ 200K token 窗口 ≠ 200K 可用              │
│              │ MCP 工具描述、系统提示、历史挤占有效空间    │
│              │ 实际可用可能只剩 ~70K                     │
├──────────────┼──────────────────────────────────────────┤
│ 跨会话记忆   │ 开新对话 → 之前所有约定归零               │
│              │ 调试心得、架构决策、项目约定全部丢失        │
├──────────────┼──────────────────────────────────────────┤
│ 质量稳定性   │ pass@1 = 70%（一次做对的概率）            │
│              │ pass^3 = 34%（连续三次都对的概率）         │
│              │ "随机做对"和"持续做对"是完全不同的事        │
├──────────────┼──────────────────────────────────────────┤
│ 多实例协作   │ 多个 Claude 实例并行 → 容易冲突           │
│              │ 没有明确作用域 → 互相覆盖修改              │
└──────────────┴──────────────────────────────────────────┘
```

这四个问题不是"用更好的提示词"就能解决的。它们是**系统设计问题**，需要系统性的工程方案。

### 1.3 Agentic Engineering 的本质

Anthropic 在 2024 年底发布了影响力巨大的《Building Effective Agents》指南，区分了两个层次：

- **Workflows**：LLM 调用沿预定义路径执行（确定性的）
- **Agents**：LLM 动态决定下一步行动，自主迭代直到目标达成（自主性的）

Agentic Engineering 就是**设计、构建和治理这些 Agent 系统的工程学科**。它的核心能力可以浓缩为一个等式：

**Agentic Engineering = Context Engineering + Agent 编排 + 生命周期治理 + 持续学习 + 安全审计**

这正好对应了 ECC 的五个子系统。但在展开 ECC 之前，我们先理解一个更底层的概念。

---

## 第二章：Context Engineering — 所有 Agent 问题的根源

### 2.1 从 Prompt Engineering 到 Context Engineering

2025 年，行业开始用 **Context Engineering** 取代 Prompt Engineering 这个词。区别在于：

| | Prompt Engineering | Context Engineering |
|---|---|---|
| 关注点 | 单条消息的措辞 | 整个信息生态的设计 |
| 范围 | 一次交互 | 跨会话、跨 Agent 的信息流 |
| 核心问题 | "怎么说才能让 AI 理解" | "在正确的时刻给 AI 正确的信息" |
| 工程化程度 | 手工技艺 | 系统架构 |

**Context Engineering 的核心原则**：

> 在有限的注意力预算（context window）里，找到**最小的高信号 token 集合**，最大化期望输出的概率。

这就引出了一个著名的设计模式——

### 2.2 Thick Scripts, Thin Instructions

这是 ECC 整体架构的设计哲学，也是 Anthropic 推荐的 Agent 设计范式：

- **Thin Instructions**（薄指令）：给 LLM 的直接提示要精简、高层、切中要害
- **Thick Scripts**（厚脚本）：复杂逻辑放在实际脚本、工具和外部资源中，按需加载

```
┌─────────────────────────────────────────────────────┐
│ 传统方式：在 CLAUDE.md 里塞满所有规则                  │
│                                                     │
│   → 每个会话都加载全量指令                            │
│   → 窗口占用大，信噪比低                             │
│   → 越长越不被遵守                                  │
└─────────────────────────────────────────────────────┘
                       VS
┌─────────────────────────────────────────────────────┐
│ Context Engineering 方式：渐进式信息披露               │
│                                                     │
│   CLAUDE.md → 最小核心规则 + "去哪里找更多信息"        │
│   Skills    → 按需触发加载                           │
│   Hooks     → 事件驱动，精确注入上下文                 │
│   CLI flags → claude --system-prompt "$(cat ctx.md)" │
│                                                     │
│   → 每个时刻只加载高信号信息                          │
│   → 上下文利用率最大化                               │
└─────────────────────────────────────────────────────┘
```

**这就是 ECC 的第一层设计智慧：不是去塞更多配置，而是设计一套信息流系统，让正确的知识在正确的时刻流入 Agent。**

### 2.3 实践：Context 管理的三板斧

**第一板斧：MCP 瘦身**

ECC longform guide 的第一条建议就是：去掉不必要的 MCP。

```bash
# 反模式：同时启用 GitHub MCP、Supabase MCP、Vercel MCP
# 每个 MCP 工具描述消耗 token → 200K 窗口可能只剩 70K

# 正确方式：用 CLI + Skill 替代 MCP
# 比如用 /gh-pr 命令封装 gh pr create，不需要加载整个 GitHub MCP
```

**规则：每个项目最多 10 个 MCP，80 个活跃工具。**

**第二板斧：战略性 Compact**

什么时候压缩上下文是一门学问：

| ✅ 合适的压缩时机 | ❌ 禁止压缩的时机 |
|---|---|
| 研究/探索结束，准备进入实现 | 实现进行中（会丢失变量名、路径、中间状态） |
| 完成一个里程碑，准备下一个 | 正在调试一个多步骤 bug |
| 调试完成，继续功能开发 | — |
| 一个方案失败，准备尝试新方案 | — |

```json
// ~/.claude/settings.json — 推荐的基础配置
{
  "model": "sonnet",
  "env": {
    "MAX_THINKING_TOKENS": "10000",
    "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "50"
  }
}
```

**第三板斧：System Prompt 动态注入**

```bash
# 按场景切换上下文，比在 CLAUDE.md 里塞所有内容更精确
alias claude-dev='claude --system-prompt "$(cat ~/.claude/contexts/dev.md)"'
alias claude-review='claude --system-prompt "$(cat ~/.claude/contexts/review.md)"'
alias claude-research='claude --system-prompt "$(cat ~/.claude/contexts/research.md)"'
```

System prompt 的优先级高于用户消息，高于工具结果——这使得它成为"外科精准注入"的最佳入口。

---

## 第三章：ECC 架构全景 — 不是配置包，是控制平面

### 3.1 重新理解 ECC

[everything-claude-code](https://github.com/affaan-m/everything-claude-code) 从 v1.8 开始，将自己明确定位为 **"Agent Harness Performance Optimization System"**——Agent 容器性能优化系统。

它不是"一堆配置文件"，而是一个有完整生命周期管理能力的控制平面：

```
ECC 架构模型
═══════════════════════════════════════════════════════════

                  ┌─ Rules     (行为基线：12 语言 × 通用规则)
                  │
                  ├─ Agents    (28 个专职子智能体)
  📦 内容层       │
  "装什么"       ├─ Skills    (50+ 可复用工作流)
                  │
                  ├─ Commands  (命令入口)
                  │
                  └─ Hooks     (6 类生命周期钩子)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

                  ┌─ install-plan.js   (清单驱动，目标感知)
                  │
  🎛️ 控制层      ├─ install-apply.js  (执行安装，可 dry-run)
  "怎么装、      │
   怎么管、      ├─ install-state.json (持久化安装合约)
   怎么修"      │
                  ├─ doctor / repair   (诊断与修复)
                  │
                  └─ uninstall         (完整卸载)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

                  ┌─ AgentShield       (安全审计 + 红队蓝队)
                  │
  🛡️ 生态层      ├─ Plankton          (写时代码质量执法)
  "附加能力"     │
                  ├─ Continuous Learning v2 (持续学习)
                  │
                  └─ Skill Creator     (从 Git 历史提炼技能)
```

它的关键属性是**可计划、可审计、可修复、可卸载**——每一个组件的安装都有状态记录，可以诊断、可以回滚。

### 3.2 跨平台统一：一套系统，四个 Harness

ECC 是业界第一个认真做跨编码 Agent 平台支持的开源项目：

| 平台 | 核心机制 | Hooks | Skills | 安装方式 |
|---|---|---|---|---|
| **Claude Code** | Plugin 原生 | ✅ 6 类完整 | ✅ SKILL.md | `claude plugin add` |
| **Cursor IDE** | DRY 适配器 | ✅ 复用脚本 | ✅ | 适配层 |
| **OpenCode** | 插件系统 | ✅ 插件化 | ✅ | Plugin Install |
| **Codex** | AGENTS.md | ❌ 暂无 | ✅ | 手动/自动 |

**核心架构创新**：`AGENTS.md` 放在项目根目录是"万能跨工具文件"——四个平台都能读取。Cursor 通过 DRY 适配器模式复用 Claude Code 的 hook 脚本，避免了维护两份逻辑的问题。

---

## 第四章：五大支柱逐一拆解

### 支柱一：Rules — Agent 的行为宪法

Rules 是 Agent 的"永远在线的守则"。ECC 将规则组织成两级结构：

```
rules/
├── common/        # 语言无关的通用原则（始终安装）
│   └── 代码风格、Git 规范、错误处理...
├── typescript/    # TypeScript/JavaScript 模式
├── python/        # Python 模式
├── golang/        # Go 模式
├── java/          # v1.9 新增
├── kotlin/        # Kotlin/Android/KMP
├── rust/          # 系统级编程
├── swift/         # iOS 开发
├── php/           # Web 后端
├── cpp/           # C++
└── perl/          # Perl
```

**v1.9 已支持 12 种语言生态。** `common/` 是通用工程原则（代码风格、测试规范、安全基线），语言目录处理特定工具链和最佳实践。

**设计智慧**：ECC 的 Rules 不是"把所有规则塞进一个 CLAUDE.md"，而是模块化按需加载——你用 TypeScript 就只加载 TypeScript 规则，不会浪费上下文给 Rust 规则。

### 支柱二：Agents — 28 个专职子智能体

ECC 定义了 28 个子智能体，涵盖从代码审查到架构设计的完整光谱：

```markdown
# Agent 定义格式（以 code-reviewer 为例）
---
name: code-reviewer
description: 审查代码质量、安全性和可维护性
tools: [Read, Grep, Glob, Bash]   # 只给必要的工具
model: opus                        # 选择适合的推理深度
---
你是一位资深代码审查者...
```

**核心原则：每个 Agent 的工具集必须受限。** 一个审查 Agent 不需要 Write 权限，一个搜索 Agent 不需要 Edit 权限。这不只是安全考虑——受限的工具集让 Agent 更专注于它的单一职责。

**Agent 全景表（部分）**：

| 类别 | Agent | 职责 | 推荐模型 |
|---|---|---|---|
| **核心流程** | planner | 任务规划和分解 | Sonnet |
| | tdd-guide | 测试驱动开发 | Sonnet |
| | build-error-resolver | 构建错误排查 | Opus |
| | architect | 架构设计 | Opus |
| **代码审查** | code-reviewer | 通用审查 | Opus |
| | typescript-reviewer | TS 专项审查 | Sonnet |
| | python-reviewer | Python 专项审查 | Sonnet |
| | security-reviewer | 安全审查 | Opus |
| | database-reviewer | 数据库审查 | Sonnet |
| **运维** | loop-operator | 循环执行自动化 | Sonnet |
| | harness-optimizer | Agent 容器优化 | Sonnet |
| | chief-of-staff | 任务编排协调 | Opus |
| | refactor-cleaner | 代码重构 | Sonnet |

**Agent 编排的阶段模式**（来自 ECC longform guide）：

```
Phase 1: RESEARCH  → Explore Agent → research-summary.md
Phase 2: PLAN      → Planner Agent → plan.md
Phase 3: IMPLEMENT → TDD-Guide Agent → 代码变更
Phase 4: REVIEW    → Code-Reviewer Agent → review-comments.md
Phase 5: VERIFY    → Build-Error-Resolver → 完成或返回 Phase 3
```

每个 Agent 进一个清晰的输入，产出一个清晰的输出。输出成为下一阶段的输入。**阶段之间用 `/clear` 清洁上下文，中间产物存入文件。**

### 支柱三：Hooks — 6 类生命周期钩子

Hooks 是 ECC 中最有深度的机制。理解 Hooks，就理解了 ECC 的"治理"理念。

**Hook 执行时序**：

```
用户请求 → Claude 选择工具 → [PreToolUse Hook] → 工具执行 → [PostToolUse Hook]
                                                                     ↓
                                                              Claude 响应
                                                                     ↓
                                                              [Stop Hook]
```

**6 类 Hook 及其典型用途**：

| Hook 类型 | 触发时机 | 能力 | 典型用途 |
|---|---|---|---|
| **PreToolUse** | 工具执行前 | 阻止(exit 2)或警告(stderr) | 防止危险操作、格式检查 |
| **PostToolUse** | 工具执行后 | 分析输出（不能阻止） | 自动格式化、类型检查 |
| **SessionStart** | 新会话开始 | 初始化上下文 | 加载上次记忆 |
| **SessionEnd** | 会话结束 | 清理与保存 | 保存会话总结 |
| **Stop** | 每次 Claude 响应后 | 后处理 | 持续学习、进度保存 |
| **PreCompact** | 上下文压缩前 | 状态保存 | 保存关键中间状态 |

**四个实战 Hook 配方**（来自 ECC 仓库）：

**配方一：阻止超大文件创建**
```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "...检查文件行数，超过 800 行则 exit 2 阻止..."
  }],
  "description": "防止创建超过 800 行的超大文件，强制拆分"
}
```

**配方二：新建源文件时提醒写测试**
```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "...检测 src/ 目录下新文件，若无对应 .test 文件则警告..."
  }],
  "description": "新建源文件时提醒创建测试"
}
```

**配方三：Python 文件编辑后自动 ruff format**
```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "...检测 .py 文件编辑后自动运行 ruff format..."
  }],
  "description": "编辑 Python 文件后自动格式化"
}
```

**配方四：编辑时检测 console.log 残留**
```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "...grep console.log 并通过 stderr 警告..."
  }],
  "description": "发现 console.log 时提醒清理"
}
```

**运行时控制（v1.8+ 核心特性）**：

```bash
# 无需修改文件，通过环境变量控制 Hook 行为
export ECC_HOOK_PROFILE=minimal    # minimal | standard | strict
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"
```

三档 Profile：
- `minimal`：只保留生命周期和安全 Hook
- `standard`：平衡质量检查和安全检查（默认）
- `strict`：启用额外提醒和更严格的护栏

**写自己的 Hook**：

```javascript
// my-custom-hook.js — Hook 基础模板
let data = '';
process.stdin.on('data', chunk => data += chunk);
process.stdin.on('end', () => {
  const input = JSON.parse(data);
  
  // 可访问的信息
  const toolName  = input.tool_name;   // "Edit", "Bash", "Write" 等
  const toolInput = input.tool_input;  // 工具特定参数
  
  // 警告（不阻断）：写入 stderr
  console.error('[Hook] 你的警告信息');
  
  // 阻断（仅 PreToolUse）：exit code 2
  // process.exit(2);
  
  // 始终将原始数据输出到 stdout
  console.log(data);
});
```

### 支柱四：持续学习 — 让 Agent 从自身经验中进化

这是 ECC 中最前沿的能力。

**问题**：你在 Claude Code 中反复调试一个问题，终于找到了解法。下次遇到类似问题——它已经忘了。你为它浪费了 token，它也为你浪费了时间。

**ECC 的解决方案**：Continuous Learning v2——一个基于"instinct"（本能）的学习系统。

**运作原理**：

```
你的正常工作会话
      ↓
Claude 发现非平凡的知识（调试技巧、项目模式、解决方案）
      ↓
Stop Hook 在会话结束时自动保存为 "instinct"
      ↓
下次遇到类似问题 → 对应 instinct 自动加载
      ↓
/evolve 命令：将相关 instinct 聚合成新的 Skill
```

```bash
/instinct-status           # 查看已学本能及置信度
/instinct-import <file>    # 从他人处导入本能
/instinct-export           # 导出供团队分享
/evolve                    # 将相关本能聚合为技能
```

**为什么用 Stop Hook 而非 UserPromptSubmit？**

UserPromptSubmit 在**每条消息**上触发——增加全程延迟。Stop Hook 在**会话结束时**运行一次——轻量，不影响交互体验。ECC 作者明确表示这是经过权衡的关键设计决策。

**Skill Creator 的进阶应用**：

```bash
/skill-create              # 分析当前仓库的 Git 历史
/skill-create --instincts  # 同时生成持续学习 instincts
```

它从你的 commit 历史中提炼项目特定的模式和实践，这意味着 ECC 可以从**你的工程决策**中学习，而非依赖通用最佳实践。

**跨会话记忆的最佳实践**（来自 ECC longform guide）：

每个会话结束时，Claude 创建一个总结文件。这个文件应包含：
- ✅ 哪些方法**有效**（附证据）
- ❌ 哪些方法**尝试过但失败**
- 🔲 哪些方法**尚未尝试**，以及还剩什么要做

下个会话只需给出文件路径即可恢复上下文。**每个会话一个文件，避免旧上下文污染新工作。**

### 支柱五：安全审计 — AgentShield

当你让 AI Agent 读写你的文件系统、执行 Shell 命令时，安全问题不是可选项。

**AgentShield** 来自 Anthropic x Cerebral Valley Hackathon（2026 年 2 月），**1282 个测试，98% 覆盖率，102 条静态分析规则**。

```bash
# 快速扫描（无需安装）
npx ecc-agentshield scan

# 自动修复安全问题
npx ecc-agentshield scan --fix

# 终极武器：三 Opus Agent 红队/蓝队/审计管线
npx ecc-agentshield scan --opus --stream
```

**`--opus` 模式**是 AgentShield 最惊艳的设计——它启动三个 Claude Opus Agent：

```
┌──────────────┐   ┌──────────────┐   ┌──────────────┐
│  🔴 攻击者   │   │  🔵 防守者   │   │  🟡 审计者   │
│  Red Team    │   │  Blue Team   │   │  Auditor     │
│  寻找漏洞链  │   │  评估保护    │   │  综合评估    │
│  和攻击路径  │   │  措施有效性  │   │  给出评级    │
└──────┬───────┘   └──────┬───────┘   └──────┬───────┘
       │                   │                   │
       └───────────────────┴───────────────────┘
                   ↓
         优先级排序的风险报告
         A-F 色阶评级
```

这是**对抗性推理**，不是简单的规则匹配。

**扫描覆盖范围**：CLAUDE.md、settings.json、MCP 配置、hooks、agent 定义、skills——涵盖 secrets 检测（14 种模式）、权限审计、hook 注入分析、MCP 风险画像。

**CI 集成**：发现 Critical 问题时 exit code = 2，可直接用作构建门控。

---

## 第五章：Token 经济学 — 降本不降质的系统方法

AI 编程成本高的根本原因不是模型贵——而是**架构设计对 token 不敏感**。

### 5.1 模型路由策略

**ECC 的核心建议：90% 的编程任务用 Sonnet，升级到 Opus 的触发条件要明确。**

| 任务类型 | 推荐模型 | 原因 |
|---|---|---|
| 文件探索/搜索 | **Haiku** | 快速，成本低，"够用就行" |
| 简单单文件修改 | **Haiku** | 指令清晰场景不需要重型模型 |
| 多文件实现 | **Sonnet** | 编程任务最佳性价比 |
| 复杂架构设计 | **Opus** | 需要深度推理能力 |
| PR 审查 | **Sonnet** | 理解上下文同时捕捉细节 |
| 安全分析 | **Opus** | 不能有遗漏 |
| 文档写作 | **Haiku** | 结构简单的文本任务 |
| 调试复杂 Bug | **Opus** | 需要在脑中维持整个系统状态 |

**Opus 升级触发条件**：
1. 首次尝试失败
2. 任务跨越 5+ 文件
3. 涉及架构决策
4. 安全关键代码

### 5.2 MCP 替换策略

> "Most MCPs are just wrappers around CLIs that already exist." — ECC Longform Guide

```bash
# 代替 GitHub MCP → 节省 token
/gh-pr            # 封装 gh pr create
/gh-issue         # 封装 gh issue create

# 代替 Supabase MCP → 节省更多
# 将 Supabase CLI 操作包装为 Skill
```

即使 MCP 现在支持 lazy loading，CLI + Skills 方案在 **token 消耗**上仍然更优。

### 5.3 代码库的模块化红利

ECC 的 longform guide 指出了一个常被忽略的事实：

> **模块化的代码库（主文件几百行而非几千行）不仅降低 token 开销，还显著提高 Agent 首次成功的概率。**

原因很简单——Agent 读取一个 200 行的文件，理解和修改它的成功率远高于一个 2000 行的文件。

---

## 第六章：并行化 — 多实例工作的工程纪律

### 6.1 最小可行并行度

ECC longform guide 的并行化哲学并不是"开更多实例"：

> "Your goal should be: **how much can you get done with the minimum viable amount of parallelization.**"

**起始模式**：左终端写代码，右终端提问题。用 `/rename` 命名，用 `/fork` 分叉。

### 6.2 Git Worktree 隔离模式

当你确实需要多个 Claude 实例并行修改代码时：

```bash
git worktree add ../project-feature-a feature-a
git worktree add ../project-feature-b feature-b

# 每个 worktree 运行独立的 Claude 实例
cd ../project-feature-a && claude
```

**关键：每个实例必须有明确的、不重叠的作用域定义。**

### 6.3 瀑布级联管理法

当运行多个实例时：
1. 新任务在右侧新标签页打开
2. 从左到右扫视（最老到最新）
3. 同时专注最多 3-4 个任务

### 6.4 双实例启动模式

ECC 作者的项目启动习惯：

| 实例 1：Scaffolding Agent | 实例 2：Deep Research Agent |
|---|---|
| 创建项目结构 | 连接服务、网页搜索 |
| 设置配置（CLAUDE.md、rules、agents） | 编写详细 PRD |
| 搭建基础框架 | 绘制架构 mermaid 图 |

**Pro Tip**：在文档页面 URL 后附加 `/llms.txt` 可以获取 LLM 优化版本的文档——很多平台已经支持这个约定。

---

## 第七章：评测框架 — 你以为在衡量什么 vs 你真正需要什么

### 7.1 pass@k vs pass^k

这是 ECC longform guide 中最值得每个做 AI 编程的人理解的框架：

```
pass@k：k 次尝试中至少 1 次成功
  k=1: 70%  │  k=3: 91%  │  k=5: 97%

pass^k：k 次尝试全部成功
  k=1: 70%  │  k=3: 34%  │  k=5: 17%
```

**pass@k ≈ "它能不能做到"（能力上限）**  
**pass^k ≈ "它能不能持续做到"（可靠性）**  

**生产场景的苦难：你以为在衡量 pass@k，实际你需要的是 pass^k，而两者之间差距巨大。**

这正是 Rules + Hooks + Skills 存在的根本原因——它们不是在提高 Agent 的"天花板"，而是在提高"地板"。

### 7.2 验证循环模式

- **检查点验证**：设定明确检查点，对照标准验证，失败则修复后继续
- **持续验证**：每隔 N 分钟或重大变更后，自动运行完整测试套件 + lint

**A/B 验证法**：

```
1. Fork 对话
2. 一个分支使用某个 Skill，另一个不使用
3. 两者执行相同任务
4. 比较 diff、token 消耗、成功率、返工次数
```

---

## 第八章：安装系统 — 从"复制粘贴"到"声明式管理"

### 8.1 选择性安装的三步走

ECC v1.9 引入了清单驱动的安装管线：

```bash
# Step 1：查看支持的安装画像
node scripts/install-plan.js --list-profiles --json
# → 返回 5 个 profile: core / developer / security / research / full

# Step 2：生成安装计划（只做规划，不改文件）
node scripts/install-plan.js --profile developer --target codex --json
# → 请求 9 个模块，实际选中 4 个，跳过不兼容的 5 个
# → 这不是全量复制，是目标感知的选择性安装

# Step 3：预览 → 执行
node scripts/install-apply.js --profile developer --target codex --dry-run
node scripts/install-apply.js --profile developer --target codex
```

### 8.2 完整 CLI

```bash
node scripts/ecc.js --help

# 控制面命令
install          安装 ECC 组件
plan             生成安装计划
doctor           诊断安装状态
repair           修复异常组件
uninstall        完整卸载
sessions         会话管理
list-installed   查看已安装组件
```

安装状态持久化在 `ecc-install-state.json`——这是它支持"可计划、可审计、可修复、可卸载"的技术基础。

---

## 第九章：Plankton — 一个值得单独讨论的写时质量系统

Plankton（由社区贡献者 @alxfazio 构建）值得独立讨论，因为它代表了一种全新的代码质量理念：**不是事后审查，是写时执法。**

**三阶段架构**：

```
Agent 编辑文件 → PostToolUse Hook 触发
        ↓
Phase 1: 静默自动格式化（修复 40-50% 的问题）
        ↓
Phase 2: 收集剩余违规为结构化 JSON
        ↓
Phase 3: 按违规复杂度路由到子进程修复
         ├─ 简单违规 → Haiku 修复（省钱）
         ├─ 中等违规 → Sonnet 修复
         └─ 复杂违规 → Opus 修复（精准）
```

支持 Python、TypeScript、Shell、YAML、JSON、TOML、Markdown 和 Dockerfile。

**最妙的设计**：Plankton 包含**配置保护钩子**——防止 Agent 通过修改 linter 配置来"通过检查"而非"修复代码"（是的，AI 真的会这么做）。

---

## 第十章：给不同读者的行动路线

### 🧑‍💻 个人开发者：最小可行路径

**第一周**：
1. 先不要安装 ECC。用原生 Claude Code，在自己项目里建 `CLAUDE.md`
2. 记录一周的痛点：哪些错误反复出现？哪些上下文丢失最让你痛苦？

**第二周**：
```bash
# 安装 ECC core profile
node scripts/install-plan.js --profile core --target claude-code
node scripts/install-apply.js --profile core --target claude-code

# 运行 doctor 确认状态
node scripts/ecc.js doctor
```
3. 用 AgentShield 扫描你的配置：`npx ecc-agentshield scan`
4. 每周回顾：哪些 Skills 命中率高？哪些 Hooks 只在产生噪音？

**第三周起**：按痛点增量添加模块。

### 👥 团队工程师：协作落地路径

1. **CLAUDE.md vs AGENTS.md 分工**：共享指令放 AGENTS.md，Claude 专属补充放 CLAUDE.md
2. AgentShield 接入 CI，Critical findings 阻断构建
3. 用 Skill Creator 从团队 Git 历史提炼项目专属 Skills
4. Continuous Learning v2 的 instincts 定期导出共享给全团队
5. 用 `/rename` 和 `/fork` 建立团队的并行工作纪律

### 🌱 Vibe Coding 入门者：学习路径

如果你刚开始探索 AI 编程，ECC 可以作为**工程师视角**的学习教材：

1. 读 `rules/common/` → 理解"工程级的规范长什么样"
2. 读 `agents/code-reviewer.md` → 理解"受限的 AI 为什么比全能的 AI 更有用"
3. 读 `hooks/README.md` → 理解"事件驱动治理"
4. 试用 `npx ecc-agentshield scan` → 理解"AI 安全"
5. 读 `the-longform-guide.md` → 理解"10 个月日常使用的实战心得"

---

## 结语：构建你自己的 Agent 工程哲学

ECC 的真正价值不在于它有 50+ Skills 或 28 个 Agents。

真正值得学的是一套**可迁移的工程思维**：

> 1. **Context Engineering** 比 Prompt Engineering 重要一个数量级
> 2. **受限的 Agent** 比全能的 Agent 更可靠
> 3. **Hooks 和 Rules** 是在提高"地板"（pass^k），不是"天花板"（pass@k）
> 4. **持续学习** 让每一次使用都在为下一次积累优势
> 5. **安全审计** 是 Agent 系统不可协商的基线

Vibe Coding 让你快速上路。Agentic Engineering 让你到达目的地。

两者不是对立关系——它们是同一段旅程的不同阶段。当你准备好从"it works on my machine"走向"it works in production"时，这篇文章里提到的五个支柱就是你的起点。

---

**参考与延伸阅读**

- [everything-claude-code 仓库](https://github.com/affaan-m/everything-claude-code) — 50K+ stars，完整源码
- [The Longform Guide](https://github.com/affaan-m/everything-claude-code/blob/main/the-longform-guide.md) — 必读的实战指南
- [The Shortform Guide](https://github.com/affaan-m/everything-claude-code/blob/main/the-shortform-guide.md) — 基础设置指南
- [AgentShield](https://github.com/affaan-m/agentshield) — AI Agent 安全审计
- [Anthropic: Building Effective Agents](https://www.anthropic.com/research/building-effective-agents) — Agent 设计模式圣经
- [Anthropic: Claude Code 官方文档](https://docs.anthropic.com/en/docs/claude-code/overview)
- [Karpathy on Vibe Coding](https://x.com/karpathy/status/1886192184808149383) — 概念起源
