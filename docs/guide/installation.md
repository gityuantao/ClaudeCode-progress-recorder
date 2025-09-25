# 安装和配置指南

本指南将帮助您在 Claude Code 项目中安装和配置 Progress Recorder 系统。

## 📋 前置要求

- [Claude Code](https://docs.claude.com/docs/claude-code) 已安装并正常工作
- 项目使用 Git 版本控制（推荐）
- 基本的 Markdown 和 YAML 语法知识

## 🚀 快速安装

### 方法一：下载发布包（推荐）

1. **下载最新版本**
```bash
# 访问 GitHub Releases 页面下载最新版本
# 或使用 git clone
git clone https://github.com/your-username/ClaudeCode-progress-recorder.git
```

2. **复制配置文件**
```bash
# 将以下文件复制到你的项目根目录
cp ClaudeCode-progress-recorder/CLAUDE.md your-project/
cp -r ClaudeCode-progress-recorder/.claude your-project/
```

3. **验证安装**
在 Claude Code 中输入 `/recap`，如果显示项目状态回顾，说明安装成功。

### 方法二：手动配置

#### 步骤1：创建项目结构

```bash
cd your-project
mkdir -p .claude/agents .claude/commands
```

#### 步骤2：配置文件

创建以下文件：

**CLAUDE.md**（项目根目录）
```markdown
# Progress Recorder 项目记忆管理系统

## 核心指令集

### /record
触发记录当前对话内容中的关键信息

### /recap
快速回顾项目当前状态

### /archive
执行历史记录归档操作

## 自动触发规则

当对话中出现以下语言模式时，系统会自动调用 record agent：

### 决策类语言（→ Decisions）
- "决定使用..."、"确定采用..."
- "最终选择..."、"批准..."

### 约束类语言（→ Pinned）
- "必须支持..."、"不能使用..."
- "法规要求..."、"安全限制..."

### 任务类语言（→ TODO）
- "需要实现..."、"要开发..."
- "计划..."、"下一步..."

### 完成类语言（→ Done）
- "完成了..."、"搞定了..."
- "已经..."、"上线了..."

## 核心设计原则

1. **分级保护** - Pinned 和 Decisions 永不自动修改
2. **语义识别** - 基于语义理解而非关键词匹配
3. **增量合并** - 智能去重和冲突处理
4. **历史归档** - 自动管理文档容量
```

**三个 Agent 文件**

从项目中复制 `.claude/agents/` 目录下的三个文件：
- `record.md` - 记录 agent
- `recap.md` - 回顾 agent
- `archive.md` - 归档 agent

**三个 Command 文件**

从项目中复制 `.claude/commands/` 目录下的三个文件：
- `record.md` - /record 指令
- `recap.md` - /recap 指令
- `archive.md` - /archive 指令

#### 步骤3：初始化记忆文件

系统会自动创建 `progress.md`，或者您可以手动创建：

```markdown
# 项目进度记录

## 📌 Pinned（关键约束）
<!-- 不可变的核心要求和限制 -->

## 🎯 Decisions（历史决策）
<!-- 按时间顺序的重要决策记录 -->

## 📝 TODO（待办任务）
<!--
格式：#ID [优先级] [状态] 任务描述
优先级：P0(紧急) P1(重要) P2(一般)
状态：OPEN, DOING, DONE
-->

## ✅ Done（完成事项）
<!-- 已完成任务，包含完成日期 -->

## 💡 Notes（临时笔记）
<!-- 其他信息，讨论记录，提醒事项 -->
```

## 🔧 配置验证

### 验证安装

1. **检查文件结构**
```bash
your-project/
├── CLAUDE.md                    ✅
├── .claude/
│   ├── agents/
│   │   ├── record.md           ✅
│   │   ├── recap.md            ✅
│   │   └── archive.md          ✅
│   └── commands/
│       ├── record.md           ✅
│       ├── recap.md            ✅
│       └── archive.md          ✅
└── progress.md                  ✅
```

2. **功能测试**

**测试 recap**：
```
输入: /recap
期望: 显示项目状态回顾
```

**测试自动记录**：
```
输入: "决定使用 React 开发前端"
期望: 自动记录到 Decisions
```

**测试手动记录**：
```
输入: /record
期望: 分析对话并记录信息
```

### 常见问题排查

#### 问题1：指令不生效

**症状**：输入 `/recap` 没有反应

**解决方案**：
1. 检查 `.claude/commands/recap.md` 文件是否存在
2. 确认文件格式正确（YAML frontmatter）
3. 重启 Claude Code

#### 问题2：自动记录不工作

**症状**：说"决定使用 React"没有自动记录

**解决方案**：
1. 检查 `CLAUDE.md` 中的自动触发规则配置
2. 确认 agent 文件格式正确
3. 尝试手动触发 `/record`

#### 问题3：progress.md 格式错误

**症状**：记录的信息格式混乱

**解决方案**：
1. 检查 agent 文件中的格式规范
2. 手动修正 progress.md 格式
3. 重新测试记录功能

## ⚙️ 高级配置

### 自定义触发规则

在 `CLAUDE.md` 中添加您项目特定的触发词汇：

```markdown
### 项目特定语言
- "集成测试完成..." → Done
- "部署到生产环境..." → Done
- "客户要求..." → Pinned
- "技术评审通过..." → Decisions
```

### 调整归档阈值

修改 `.claude/agents/archive.md` 中的阈值：

```markdown
## 归档触发条件
- 主要条件：Notes + Done 条目总数 ≥ 50 条  # 从100改为50
- 备用条件：progress.md 文件大小 > 30KB    # 从50KB改为30KB
```

### 自定义优先级规则

修改 `.claude/agents/record.md` 中的优先级判定：

```markdown
## 优先级判定
- P0 (紧急)：影响上线、阻塞其他任务
- P1 (重要)：核心功能、主要特性
- P2 (一般)：优化改进、非关键功能
- P3 (低)：文档、清理、重构
```

### 添加证据链接模板

自定义 Done 项目的证据链接格式：

```markdown
## 证据链接格式
- Git 提交: (commit: abc123)
- Pull Request: (PR: #45)
- 部署记录: (deployed to prod)
- 测试报告: (test: passed)
```

## 🔒 安全和隐私

### 数据安全

- **本地存储** - 所有数据保存在本地项目中
- **版本控制** - 建议将 progress.md 加入 Git 管理
- **备份策略** - 定期备份项目文件
- **敏感信息** - 避免记录密码、API密钥等敏感信息

### 隐私保护

- **团队共享** - progress.md 会被团队成员看到
- **信息分类** - 区分公开信息和内部信息
- **访问控制** - 通过 Git 权限管理访问

## 📊 性能优化

### 大项目优化

对于大型项目，建议：

1. **定期归档** - 每月执行 `/archive`
2. **分模块记录** - 不同模块使用不同的 progress.md
3. **精简记录** - 只记录关键决策和重要任务

### 响应速度优化

- **文件大小控制** - 保持 progress.md 在合理大小
- **格式标准化** - 遵循标准格式避免解析错误
- **并发处理** - 避免同时操作多个记录指令

## 🔄 升级和维护

### 版本升级

1. **备份现有配置**
```bash
cp -r .claude .claude.backup
cp CLAUDE.md CLAUDE.md.backup
```

2. **下载新版本**
```bash
git pull origin main
```

3. **合并配置**
```bash
# 比较并合并配置文件的变更
diff CLAUDE.md.backup CLAUDE.md
```

4. **测试功能**
```bash
# 验证所有功能正常
/recap
/record
```

### 维护最佳实践

- **定期检查** - 每月检查系统运行状态
- **清理归档** - 定期执行归档操作
- **配置更新** - 根据项目需求调整配置
- **团队培训** - 确保团队成员了解使用方法

## 📚 下一步

安装完成后，建议：

1. **阅读完整文档** - 了解所有功能特性
2. **查看使用示例** - 学习最佳实践
3. **开始项目记录** - 从当前项目状态开始
4. **定期回顾** - 使用 `/recap` 了解项目进展

---

如果在安装过程中遇到问题，请查看 [常见问题解答](faq.md) 或提交 [GitHub Issue](https://github.com/your-username/ClaudeCode-progress-recorder/issues)。