# Claude Code 开发套件

[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Changelog](https://img.shields.io/badge/changelog-v2.1.0-orange.svg)](CHANGELOG.md)

一个集成系统，通过自动化文档管理、多代理工作流和外部AI专业知识，将Claude Code转变为一个协调统一的开发环境。

> **项目来源**: 基于 [Claude Code Development Kit](https://github.com/peterkrueck/Claude-Code-Development-Kit) 的中文本地化版本 | MIT License

## 为什么选择 Claude Code？

Claude Code的子代理功能使得这种高度自动化、集成的方法成为可能。虽然其他AI工具可能可以使用文档结构（参见FAQ）和一些命令，但目前只有Claude Code能够协调并行代理并充分发挥此开发套件的潜力。

## 🎯 为什么需要这个套件？

> *是否曾经尝试用AI辅助构建大型项目，却发现随着代码库增长，AI开始吃力？*

Claude Code的输出质量直接取决于它对你项目的了解程度。随着AI辅助开发的规模化，出现了三个关键挑战：

---

### 挑战1：上下文管理

**问题：**
```
❌ 失去对架构模式和设计决策的跟踪
❌ 忘记编码标准和团队约定
❌ 在大型代码库中缺乏关于在哪里找到正确上下文的指导
```

**解决方案：**
✅ **自动化上下文传递**通过两个集成系统：
- **3层文档系统** - 在正确时间自动加载正确的文档
- **带子代理的自定义命令** - 编排已经了解你项目的专业代理
- 结果：无需手动上下文加载，所有代理间的知识保持一致

---

### 挑战2：AI可靠性

**问题：**
```
❌ 过时的库文档
❌ 幻觉API方法
❌ 不一致的架构决策
```

**解决方案：**
✅ **"四眼原则"**通过MCP集成：

| 服务 | 目的 | 益处 |
|---------|---------|---------|
| **Context7** | 实时库文档 | 当前API，而非训练数据 |
| **Gemini** | 架构咨询 | 交叉验证和最佳实践 |

*结果：更少错误，更好代码，当前标准*

---

### 挑战3：自动化而不复杂化

**问题：**
```
❌ 每个会话需要手动上下文加载
❌ 重复的命令序列
❌ 任务完成时无反馈
```

**解决方案：**
✅ **智能自动化**通过钩子和命令：
- 通过自定义命令自动更新文档
- 为所有子代理和Gemini MCP调用注入上下文
- 任务完成的音频通知（可选）
- 复杂任务的一键工作流

---

### 🎉 结果

> **Claude Code从一个有用的工具转变为可靠的开发伙伴，它记住你的项目上下文，验证自己的工作，并自动处理繁琐的事情。**

[![Demo-Video auf YouTube](https://img.youtube.com/vi/kChalBbMs4g/0.jpg)](https://youtu.be/kChalBbMs4g)

## 快速开始

### 前置要求

- **必需**: [Claude Code](https://github.com/anthropics/claude-code)
- **推荐**: MCP服务器如 [Context7](https://github.com/upstash/context7) 和 [Gemini Assistant](https://github.com/peterkrueck/mcp-gemini-assistant)

#### 平台支持

- **Windows**: ❌ (已报告错误 - 使用风险自负)

### 📦 文件分类指南

本套件包含不同类型的文件,需要区别对待：

#### 🔵 需要复制并填充的文件 (优先级 ⭐⭐⭐)

这些文件需要复制到你的项目并填充实际内容：

| 文件 | 位置 | 说明 | 优先级 |
|------|------|------|--------|
| `CLAUDE.md` | 项目根目录 | 主AI上下文和编码标准 | ⭐⭐⭐ 必需 |
| `project-structure.md` | `docs/ai-context/` | 完整技术栈和文件树 | ⭐⭐⭐ 必需 |
| `docs-overview.md` | `docs/ai-context/` | 文档路由和导航地图 | ⭐⭐⭐ 必需 |
| `MCP-ASSISTANT-RULES.md` | 项目根目录 | Gemini咨询规则 | ⭐⭐ 推荐 |
| `handoff.md` | `docs/ai-context/` | 任务交接模板 | ⭐ 可选 |

#### 🟡 供AI参考的模板 (不需要修改)

这些文件是给AI看的结构模板,**保持原样即可**：

- `docs/CONTEXT-tier2-component.md` - AI生成组件文档时参考的模板
- `docs/CONTEXT-tier3-feature.md` - AI生成功能文档时参考的模板

#### 🟢 示例文件 (可删除)

这些是给你看的例子,了解后可以删除：

- `docs/specs/example-*.md` - 功能规范示例
- `docs/open-issues/example-*.md` - 问题跟踪示例

### 🚀 三步快速开始

#### 第一步：安装到你的项目

```bash
# 克隆本套件
git clone https://github.com/你的用户名/Claude-Code-Development-Kit-CN.git

# 复制核心文件到你的项目
cd your-project/

# 复制命令和钩子
cp -r /path/to/Claude-Code-Development-Kit-CN/commands .claude/
cp -r /path/to/Claude-Code-Development-Kit-CN/hooks .claude/

# 复制文档模板
cp -r /path/to/Claude-Code-Development-Kit-CN/docs ./
cp /path/to/Claude-Code-Development-Kit-CN/docs/CLAUDE.md ./
```

**结果**:
```
your-project/
├── .claude/
│   ├── commands/          # ✅ AI编排命令
│   └── hooks/             # ✅ 自动化钩子
├── docs/                  # ✅ 文档模板
├── CLAUDE.md              # ⚠️ 需要填充
└── (你的现有代码)
```

#### 第二步：填充核心文档

使用Claude Code自动生成项目文档：

```bash
cd your-project/
claude

# 在 Claude Code 中执行以下命令
/create-docs CLAUDE.md
/create-docs docs/ai-context/project-structure.md
/create-docs docs/ai-context/docs-overview.md
```

AI会：
- 📖 分析你的代码库
- 📝 自动填充项目信息
- 🎯 生成准确的技术栈说明

**或者手动编辑**:
```bash
# 替换所有 [占位符] 内容
vim CLAUDE.md
vim docs/ai-context/project-structure.md
vim docs/ai-context/docs-overview.md
```

#### 第三步：生成组件文档

为各个组件生成CONTEXT.md文件：

```bash
# 在 Claude Code 中
/create-docs backend/CONTEXT.md
/create-docs frontend/CONTEXT.md
/create-docs backend/src/api/CONTEXT.md
```

AI会参考模板结构,为你的实际组件生成文档。

**完成！** 现在你可以使用高级命令：

```bash
# 全面上下文分析
/full-context "implement user authentication"

# 多视角代码审查
/code-review "review authentication implementation"

# 自动更新文档
/update-docs "document authentication changes"
```

---

### 💡 最简单的开始方式

如果你想最快速地开始，只需这3个命令：

```bash
cd your-project/
claude

# 只需执行这3个命令！
/create-docs CLAUDE.md
/create-docs docs/ai-context/project-structure.md
/create-docs docs/ai-context/docs-overview.md
```

AI会分析你的代码并自动生成所有必要的文档。**就这么简单！** 🎉

---

### ⚙️ 可选配置

#### 配置MCP服务器

如果使用Gemini或Context7：

1. **复制MCP规则**:
   ```bash
   cp docs/MCP-ASSISTANT-RULES.md ./
   ```

2. **配置Claude Code设置**:
   编辑 `.claude/settings.json` 添加MCP服务器配置

3. **测试MCP集成**:
   ```bash
   /gemini-consult "analyze my architecture"
   ```

#### 配置钩子

钩子默认已配置,如需自定义：

```bash
# 修改安全扫描模式
vim .claude/hooks/config/sensitive-patterns.json

# 启用/禁用通知
vim .claude/hooks/notify.sh
```

## 术语

- **CLAUDE.md** - 包含项目特定AI指令、编码标准和集成模式的主要上下文文件
- **CONTEXT.md** - 组件和功能级别的文档文件（第2层和第3层），提供具体的实现细节和模式
- **MCP (Model Context Protocol)** - 用于将外部AI服务与Claude Code集成的标准
- **子代理** - 由Claude Code生成的专业AI代理，并行工作于任务的特定方面
- **3层文档** - 分层组织（基础/组件/功能），最小化维护的同时最大化AI效果
- **自动加载** - 在命令执行时自动包含相关文档
- **钩子** - 在Claude Code生命周期特定时点执行的Shell脚本，用于安全、自动化和UX增强

## 架构

### 集成智能循环

```
                        CLAUDE CODE
                   ┌─────────────────┐
                   │                 │
                   │      命令        │
                   │                 │
                   └────────┬────────┘
                  多代理    │编排
                   并行     │执行
                   动态     │扩展
                           ╱│╲
                          ╱ │ ╲
          将代理路由到   ╱  │  ╲  利用
          正确的文档    ╱   │   ╲ 专业知识
                       ╱    │    ╲
                      ▼     │     ▼
         ┌─────────────────┐│┌─────────────────┐
         │                 │││                 │
         │       文档       │││   MCP 服务器   │
         │                 │││                 │
         └─────────────────┘│└─────────────────┘
          3层结构           │  Context7 + Gemini
          自动加载          │  实时更新
          上下文路由        │  AI咨询
                      ╲     │     ╱
                       ╲    │    ╱
        为咨询提供项目  ╲   │   ╱  通过当前最佳
        上下文          ╲  │  ╱   实践增强
                         ╲ │ ╱   
                           ╲│╱
                            ▼
                    集成工作流
```

### 自动加载机制

每个命令执行都会自动加载关键文档：

```
@/CLAUDE.md                              # 主要AI上下文和编码标准
@/docs/ai-context/project-structure.md   # 完整技术栈和文件树
@/docs/ai-context/docs-overview.md       # 文档路由图
```

`subagent-context-injector.sh` 钩子将自动加载扩展到所有子代理：
- 通过Task工具生成的子代理自动接收相同的核心文档
- Task提示中无需手动包含上下文
- 确保多代理工作流中所有代理的知识一致性

这确保了：
- 所有会话和子代理间AI行为的一致性
- 任何级别都无需手动上下文管理

### 组件集成

**命令 ↔️ 文档**
- 命令根据任务复杂性确定要加载哪些文档层
- 文档结构指导代理生成模式
- 命令更新文档以维护当前上下文

**命令 ↔️ MCP服务器**
- Context7提供最新的库文档
- Gemini为复杂问题提供架构咨询
- 集成在命令工作流中无缝进行

**文档 ↔️ MCP服务器**
- 项目结构和MCP助手规则自动附加到Gemini咨询
- 确保外部AI理解特定架构和编码标准
- 使所有建议项目相关且符合标准

### 钩子集成

该套件包括经过实战测试的钩子，增强了Claude Code的能力：

- **安全扫描器** - 防止在使用MCP服务器时意外暴露机密
- **Gemini上下文注入器** - 自动在Gemini咨询中包含项目结构
- **子代理上下文注入器** - 确保所有子代理自动接收核心文档
- **通知系统** - 为任务完成和输入请求提供非阻塞音频反馈（可选）

这些钩子与命令和MCP服务器工作流无缝集成，提供：
- 对所有外部AI调用的预执行安全检查
- 为外部AI和子代理自动增强上下文
- 多代理工作流中所有代理的一致知识
- 通过愉悦、非阻塞的音频通知提高开发者意识

## 常见任务

### 开始新功能开发

```bash
/full-context "implement user authentication across backend and frontend"
```

系统将：
1. 自动加载项目文档
2. 生成专业代理（安全、后端、前端）
3. 咨询Context7获取认证框架文档
4. 向Gemini 2.5 pro请求反馈和改进建议
5. 提供全面的分析和实现计划

### 多视角代码审查

```bash
/code-review "review authentication implementation"
```

多个代理分析：
- 安全漏洞
- 性能影响
- 架构对齐
- 集成影响

### 维护文档时效性

```bash
/update-docs "document authentication changes"
```

自动：
- 更新所有层受影响的CONTEXT.md文件
- 保持project-structure.md和docs-overview.md最新
- 为未来AI会话维护上下文
- 确保文档匹配实现

## 项目结构说明

安装套件后，你的项目结构将如下：

```
your-project/
├── .claude/
│   ├── commands/              # AI编排命令模板
│   │   ├── full-context.md    # 全面上下文分析
│   │   ├── code-review.md     # 多视角代码审查
│   │   ├── create-docs.md     # 创建文档
│   │   ├── update-docs.md     # 更新文档
│   │   ├── gemini-consult.md  # Gemini咨询
│   │   └── ...
│   ├── hooks/                 # 自动化钩子
│   │   ├── config/            # 钩子配置
│   │   ├── sounds/            # 通知音频
│   │   ├── subagent-context-injector.sh     # 子代理上下文注入
│   │   ├── gemini-context-injector.sh       # Gemini上下文注入
│   │   ├── mcp-security-scan.sh            # MCP安全扫描
│   │   └── notify.sh                        # 任务通知
│   └── settings.json          # Claude Code配置
├── docs/
│   ├── ai-context/            # 第1层：基础文档
│   │   ├── project-structure.md       # ⚠️ 需填充：技术栈和文件树
│   │   ├── docs-overview.md           # ⚠️ 需填充：文档导航地图
│   │   ├── system-integration.md      # 可选：跨组件模式
│   │   ├── deployment-infrastructure.md # 可选：基础设施
│   │   └── handoff.md                 # 可选：任务交接
│   ├── CONTEXT-tier2-component.md     # 📋 模板：AI参考用
│   ├── CONTEXT-tier3-feature.md       # 📋 模板：AI参考用
│   ├── open-issues/           # 📝 示例：可删除
│   ├── specs/                 # 📝 示例：可删除
│   └── README.md              # 📖 说明：文档系统指南
├── CLAUDE.md                  # ⚠️ 需填充：主AI上下文（第1层）
├── MCP-ASSISTANT-RULES.md     # 可选：MCP编码标准
│
├── backend/                   # 你的后端代码
│   ├── CONTEXT.md            # 🔴 待生成：后端上下文（第2层）
│   └── src/api/
│       └── CONTEXT.md        # 🔴 待生成：API上下文（第3层）
│
└── frontend/                  # 你的前端代码
    ├── CONTEXT.md            # 🔴 待生成：前端上下文（第2层）
    └── src/components/
        └── CONTEXT.md        # 🔴 待生成：组件上下文（第3层）
```

**图例**:
- ⚠️ 需填充 - 使用 `/create-docs` 自动生成或手动编辑
- 🔴 待生成 - 使用 `/create-docs [path]` 为各组件生成
- 📋 模板 - AI参考用，保持原样
- 📝 示例 - 了解后可删除
- 📖 说明 - 保留参考

## 配置

该套件设计为可适应的：

- **命令** - 在 `.claude/commands/` 中修改编排模式
- **文档** - 调整你架构的层结构
- **MCP集成** - 为专业知识添加额外服务器
- **钩子** - 在 `.claude/hooks/` 中自定义安全模式、添加新钩子或修改通知
- **MCP助手规则** - 复制 `docs/MCP-ASSISTANT-RULES.md` 模板到项目根目录并为项目特定标准自定义

## 最佳实践

1. **让文档指导开发** - 3层结构反映自然边界
2. **立即更新文档** - 重大更改后使用 `/update-docs`
3. **信任自动加载** - 避免手动上下文管理
4. **自然扩展复杂性** - 简单任务保持简单，复杂任务获得复杂分析

## 文档

- [文档系统指南](docs/) - 理解3层架构
- [命令参考](commands/) - 详细命令用法
- [MCP集成](docs/CLAUDE.md) - 配置外部服务
- [钩子系统](hooks/) - 安全扫描、上下文注入和通知
- [更新日志](CHANGELOG.md) - 版本历史和迁移指南

## 常见问题

**问：安装是否会覆盖我现有的文件？**

**答：** 不会，安装器会检测现有文件并提示你跳过或覆盖每个文件。为了安全起见，我强烈推荐在新的Git分支上安装。安全第一。

**问：我可以将此与其他AI编码工具如Cursor、Cline或Gemini CLI一起使用吗？**

**答：** 部分可以。文档结构适用于任何工具（重命名CLAUDE.md以匹配你工具的约定）。然而，命令针对子代理使用进行了高度优化，钩子是Claude Code特定的。其他工具需要对编排功能进行重大适应。

**问：这会在令牌方面花费多少？**

**答：** 由于全面的上下文加载和子代理使用，此框架大量使用令牌。我强烈推荐Claude Code Max 20x订阅，而不是按令牌付费的API使用。Claude 4 Opus模型目前在复杂指令遵循方面表现最佳。

**问：我可以使用其他编码顾问MCP如Zen代替Gemini咨询吗？**

**答：** 虽然技术上可能，但模板和钩子专门配置并优化了我的Gemini MCP服务器（通过安装期间提供的链接可用）。使用替代编码顾问MCP需要调整模板、钩子，以及可能的命令结构以匹配其特定接口和能力。

**问：我可以将此框架与现有项目一起使用吗？**

**答：** 可以！该框架与现有项目配合良好。安装时，检查是否已有项目结构或CLAUDE.md文件，并在设置提示期间相应调整。要开始使用现有代码库，使用带有子代理的Claude Code理解你的项目并创建初始project-structure.md：

```
"Read and understand the project_structure.md template in docs/ai-context/project_structure.md. Your task is to fill out this template with our project's details. For this send out sub agents in parallel across the whole code base. Once the sub agents get back, ultrathink and create the markdown file."
```

创建项目结构后，使用框架的文档生成系统创建组件级和功能级上下文文件：

```
/create-docs "[your-main-component-path]/CONTEXT.md"
```

这种方法让框架学习你现有的架构，并系统地创建与你当前项目结构匹配的适当文档。

---

**注意**: 这是中文本地化版本。如有问题或建议,请在项目仓库提交 Issue。