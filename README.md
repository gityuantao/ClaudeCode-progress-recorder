# Progress Recorder - Claude Code 项目记忆管理系统

<div align="center">

![Progress Recorder](https://img.shields.io/badge/Progress-Recorder-blue?style=for-the-badge)
![Claude Code](https://img.shields.io/badge/Claude-Code-orange?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

**为 Claude Code 项目提供持久记忆能力的智能管理系统**

[快速开始](#-快速开始) • [功能特性](#-功能特性) • [使用示例](#-使用示例) • [文档](#-文档) • [贡献](#-贡献)

</div>

---

## 🎯 项目简介

Progress Recorder 专门解决 Claude Code 开发项目时信息散落的问题。在长期项目开发中，重要的决策、任务进度、技术约束等信息会散落在对话历史中，随着对话轮次增加容易被遗忘或难以查找。

这个系统通过语义识别自动捕获对话中的关键信息，并持续维护一个结构化的项目记忆文件，让 Claude Code 具备了持久记忆能力。

### ✨ 核心价值

- 🧠 **持久记忆** - 替代对话历史查找，提供结构化项目记忆
- 🤖 **语义识别** - 基于语义理解自动分类信息，无需手工整理
- 🔒 **分级保护** - 关键决策和约束永不丢失
- 📈 **生命周期管理** - 完整的任务状态追踪
- 📁 **智能归档** - 自动管理文档容量，保持清爽可读

## 🚀 快速开始

### 第一步：下载配置文件

下载并复制以下文件到你的 Claude Code 项目根目录：

```bash
your-project/
├── CLAUDE.md                    # 项目规则文件
├── .claude/
│   ├── agents/
│   │   ├── record.md           # 记录 agent
│   │   ├── recap.md            # 回顾 agent
│   │   └── archive.md          # 归档 agent
│   └── commands/
│       ├── record.md           # /record 指令
│       ├── recap.md            # /recap 指令
│       └── archive.md          # /archive 指令
└── progress.md                 # 项目记忆文件（自动生成）
```

### 第二步：开始使用

**自动记录（推荐）**：
```
你: "决定使用 Next.js 开发前端，必须支持移动端"
系统: 自动识别并记录决策和约束
```

**手动记录**：
```
你: "/record"
系统: 分析当前对话并记录关键信息
```

**项目回顾**：
```
你: "/recap"
系统: 生成完整的项目状态报告
```

**历史归档**：
```
你: "/archive"
系统: 整理历史记录，保持主文件清爽
```

## 🎨 功能特性

### 🧠 语义识别系统

**自动分类**：
- 📌 **Pinned（关键约束）** - "必须支持..."、"不能使用..."
- 🎯 **Decisions（历史决策）** - "决定使用..."、"选择..."
- 📝 **TODO（待办任务）** - "需要实现..."、"要开发..."
- ✅ **Done（完成事项）** - "完成了..."、"搞定了..."
- 💡 **Notes（临时笔记）** - 其他重要信息

**置信度判定**：
- 🔒 **高置信度** - 强承诺语言写入受保护区块
- ⚠️ **低置信度** - 弱化表达降级到 Notes 并标注确认

### 🔒 分级保护机制

```
🎯 永不修改（分级保护）
├── 📌 Pinned - 关键约束一旦记录永不自动修改
└── 🎯 Decisions - 历史决策按时间追加，旧决策永远保留

📁 智能管理
├── 📝 TODO - 完整生命周期：OPEN→DOING→DONE
├── ✅ Done - 包含完成日期和证据链接
└── 💡 Notes - 临时信息，可归档整理
```

### 📊 任务生命周期管理

- **唯一 ID** - #1, #2, #3... 自动分配，跨状态保持
- **优先级** - P0(紧急), P1(重要), P2(一般)
- **状态流转** - OPEN → DOING → DONE（支持回退）
- **证据链接** - 关联 commit、PR、文件路径等证据

### 📁 智能归档系统

- **自动触发** - Notes + Done 超过 100 条时
- **保留策略** - 最近 50 条，其余按月份归档
- **分级保护** - Pinned 和 Decisions 永不归档
- **只增不删** - 完整的项目历史可追溯

## 💡 使用示例

### 项目启动阶段

```
你: "我们决定开发一个电商网站，必须支持移动端和微信支付，不能使用 jQuery。需要实现用户注册、商品展示和购物车功能。"

系统自动记录：
📌 Pinned: 必须支持移动端、必须支持微信支付、不能使用 jQuery
🎯 Decisions: [2024-09-24] 决定开发一个电商网站
📝 TODO: #1 [P1] [OPEN] 实现用户注册功能
📝 TODO: #2 [P1] [OPEN] 实现商品展示功能
📝 TODO: #3 [P1] [OPEN] 实现购物车功能
```

### 技术选型记录

```
你: "经过讨论，确定使用 Next.js 作为前端框架，选择 MongoDB 作为数据库。"

系统自动记录：
🎯 Decisions: [2024-09-24] 确定使用 Next.js 作为前端框架
🎯 Decisions: [2024-09-24] 选择 MongoDB 作为数据库
```

### 项目进展更新

```
你: "用户注册功能已经完成了，现在开始做商品展示模块。"

系统自动更新：
✅ Done: #1 [P1] [DONE] 实现用户注册功能 [2024-09-24]
📝 TODO: #2 [P1] [DOING] 实现商品展示功能（状态更新）
```

### 状态回顾

```
你: "/recap"

系统生成报告：
📊 Progress Recorder 项目状态回顾 [2024-09-24]

🎯 核心约束 (3 条)
- 必须支持移动端
- 必须支持微信支付
- 不能使用 jQuery

📋 决策历程 (共 3 项)
- 最新：[2024-09-24] 选择 MongoDB 作为数据库
- [2024-09-24] 确定使用 Next.js 作为前端框架
- [2024-09-24] 决定开发一个电商网站

📝 任务状态
├── 总计：3 个任务
├── OPEN：1 个 (P1: 1)
├── DOING：1 个 (P1: 1)
└── DONE：1 个 (P1: 1)
完成率：33%

🔥 当前焦点
- #2 [P1] [DOING] 实现商品展示功能
- #3 [P1] [OPEN] 实现购物车功能

✅ 最近完成
- #1 [P1] 用户注册功能 [2024-09-24]
```

## 📁 项目结构

```
Progress Recorder/
├── README.md                   # 项目说明
├── LICENSE                     # MIT 开源协议
├── CLAUDE.md                   # 完整方案文档和配置规则
├── progress.md                 # 项目记忆文件（示例）
├── .claude/
│   ├── agents/                 # 三个核心 Agent
│   │   ├── record.md          # 记录 Agent - 语义识别和信息记录
│   │   ├── recap.md           # 回顾 Agent - 状态分析和报告生成
│   │   └── archive.md         # 归档 Agent - 历史管理和容量控制
│   └── commands/              # 三个核心指令
│       ├── record.md          # /record - 触发记录
│       ├── recap.md           # /recap - 状态回顾
│       └── archive.md         # /archive - 历史归档
├── docs/                      # 文档目录
│   ├── examples/              # 使用示例
│   └── guide/                 # 详细指南
└── CONTRIBUTING.md            # 贡献指南
```

## 🎯 系统架构

```
┌──────────────────┐
│    CLAUDE.md     │ ← 规则层：定义触发条件和指令集
├──────────────────┤
│ record/recap/    │ ← 处理层：三个专门的 agent
│ archive agents   │   负责记录、回顾、归档
├──────────────────┤
│   progress.md    │ ← 工作记忆：当前项目状态
│progress.archive  │ ← 历史记忆：完整项目历程
└──────────────────┘
```

**工作流程**：
1. **对话监听** - 主代理识别关键语言模式
2. **触发判断** - 根据规则决定是否需要记录
3. **语义抽取** - 分析对话内容，提取结构化信息
4. **智能合并** - 与现有记录对比，执行去重或更新
5. **分级存储** - 根据信息类型写入对应区块
6. **容量管理** - 超过阈值时自动归档

## 🔧 高级配置

### 自定义触发规则

在 `CLAUDE.md` 中可以自定义语义识别规则：

```markdown
## 自动触发规则

### 决策类语言（→ Decisions）
- "决定使用..."、"确定采用..."
- "最终选择..."、"批准..."

### 约束类语言（→ Pinned）
- "必须支持..."、"不能使用..."
- "法规要求..."、"安全限制..."
```

### 调整归档阈值

修改 archive agent 中的触发条件：

```markdown
## 归档触发条件
- 主要条件：Notes + Done 条目总数 ≥ 100 条
- 备用条件：progress.md 文件大小 > 50KB
```

### 自定义优先级

调整 record agent 中的优先级判定规则：

```markdown
## 优先级判定
- P0 (紧急)：阻塞性任务，影响发布
- P1 (重要)：核心功能，重要特性
- P2 (一般)：优化改进，非关键功能
```

## 🤝 贡献

我们欢迎各种形式的贡献！请查看 [CONTRIBUTING.md](CONTRIBUTING.md) 了解如何参与。

### 贡献方式

- 🐛 **Bug 报告** - 发现问题请提交 Issue
- 💡 **功能建议** - 有好想法请分享
- 📖 **文档改进** - 帮助完善文档
- 🛠️ **代码贡献** - 提交 Pull Request
- 🌍 **本地化** - 支持多语言

### 开发指南

1. Fork 本项目
2. 创建功能分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送分支 (`git push origin feature/AmazingFeature`)
5. 提交 Pull Request

## 📄 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情。

## 🙏 致谢

- [Claude](https://claude.ai) - 提供强大的 AI 能力
- [Claude Code](https://docs.claude.com/docs/claude-code) - 优秀的 AI 开发工具
- 所有贡献者和用户的支持

## 📞 支持

- 📧 **问题反馈** - 提交 GitHub Issue
- 💬 **功能讨论** - 参与 Discussions
- 📖 **文档** - 查看项目 Wiki
- 🎯 **示例** - 参考 examples 目录

---

<div align="center">

**如果这个项目对你有帮助，请给个 ⭐️ Star！**

Made with ❤️ for Claude Code Community

</div>