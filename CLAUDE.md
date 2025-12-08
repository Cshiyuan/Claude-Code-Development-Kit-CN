# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 1. 项目概述

**Claude Code Development Kit - 中文版** 是一个为 Claude Code 提供的开发工具套件模板仓库,所有文档和命令均为中文版本。

- **项目性质**: 文档和模板仓库,非可执行应用
- **核心内容**: Claude Code 命令模板、钩子脚本、文档结构示例
- **目标用户**: 使用 Claude Code 进行 AI 辅助开发的中文开发者
- **主分支**: `main`

## 2. 仓库架构

### 核心目录结构

```
Claude-Code-Development-Kit-CN/
├── commands/           # 命令模板目录
│   ├── code-review.md
│   ├── create-docs.md
│   ├── full-context.md
│   ├── gemini-consult.md
│   ├── handoff.md
│   ├── refactor.md
│   ├── update-docs.md
│   └── README.md
├── hooks/              # 钩子脚本及配置
│   ├── config/         # 钩子配置文件
│   ├── setup/          # 设置说明
│   ├── sounds/         # 通知音频文件
│   ├── gemini-context-injector.sh
│   ├── mcp-security-scan.sh
│   ├── notify.sh
│   ├── subagent-context-injector.sh
│   └── README.md
├── docs/               # 文档模板和示例
│   ├── ai-context/     # 基础 AI 上下文文档
│   ├── open-issues/    # 问题跟踪示例
│   ├── specs/          # 规格示例
│   ├── CLAUDE.md       # 项目 AI 上下文模板
│   ├── CONTEXT-tier2-component.md  # 第2层文档模板
│   ├── CONTEXT-tier3-feature.md    # 第3层文档模板
│   ├── MCP-ASSISTANT-RULES.md      # MCP 助手规则
│   └── README.md
├── mcp-gemini-assistant/  # Gemini MCP 服务器
├── LICENSE
└── README.md           # 主要说明文档
```

### 文件类型分布

- **Markdown 文档**: 命令、文档和 README 文件(中文内容)
- **Shell 脚本**: 5 个可执行钩子脚本 (`.sh`)
- **配置文件**: JSON 格式,位于 `hooks/config/` 和 `hooks/setup/`
- **音频文件**: 位于 `hooks/sounds/`,用于通知系统

## 3. 命令系统 (commands/)

所有命令文件都是 Claude Code 的提示词模板,包含:

### 核心命令

1. **full-context.md** - 全面上下文收集
   - 自适应子代理策略
   - 智能分析和综合
   - 支持 0-N 个并行子代理

2. **code-review.md** - 代码审查
   - 多专家视角分析
   - 安全、性能、架构关注点

3. **gemini-consult.md** - Gemini 咨询
   - 与 Gemini 2.5 Pro 的深度对话
   - 持久会话支持

4. **update-docs.md** - 文档更新
   - 同步代码变更到文档
   - 维护 3 层文档结构

5. **create-docs.md** - 创建文档
   - 为现有项目生成文档结构
   - 初始化 3 层架构

6. **refactor.md** - 重构
   - 智能代码重构
   - 依赖关系更新

7. **handoff.md** - 会话交接
   - 保存会话状态
   - 上下文连续性

### 命令文件特征

- 使用 `$ARGUMENTS` 变量接收用户输入
- 自动加载核心文档引用 (`@/CLAUDE.md`, `@/docs/ai-context/project-structure.md` 等)
- 包含详细的步骤化执行指令
- 提供子代理设计模板和示例

## 4. 钩子系统 (hooks/)

### 可执行脚本

所有 `.sh` 文件都是可执行的 bash 脚本:

1. **subagent-context-injector.sh**
   - PreToolUse 钩子,监听 Task 工具
   - 自动为所有子代理注入核心文档引用
   - 确保多代理工作流中的上下文一致性

2. **gemini-context-injector.sh**
   - PreToolUse 钩子,监听 `mcp__gemini__consult_gemini`
   - 自动附加项目结构和助手规则
   - 仅在新会话时注入

3. **mcp-security-scan.sh**
   - PreToolUse 钩子,监听所有 `mcp__.*` 工具
   - 扫描敏感数据(API 密钥、密码等)
   - 可配置模式匹配 (`config/sensitive-patterns.json`)

4. **notify.sh**
   - Notification 和 Stop 钩子
   - 播放音频通知
   - 支持跨平台 (macOS/Linux/Windows)

### 配置和设置

- `config/sensitive-patterns.json` - 安全扫描模式配置
- `setup/hook-setup.md` - 钩子设置验证命令说明
- `setup/settings.json.template` - Claude Code 设置模板
- `sounds/` - 通知音频文件

## 5. 文档系统 (docs/)

### 3 层文档架构

框架实现了分层文档系统:

**第 1 层 (基础)** - `CLAUDE.md`
- 项目范围的编码标准
- AI 指令和约定
- 很少变更

**第 2 层 (组件)** - `CONTEXT-tier2-component.md`
- 组件级架构模式
- 集成点和边界
- 偶尔变更

**第 3 层 (功能)** - `CONTEXT-tier3-feature.md`
- 功能特定实现细节
- 技术决策
- 频繁变更

### AI 上下文文档 (ai-context/)

- `project-structure.md` - 项目结构和技术栈模板
- `docs-overview.md` - 文档路由和导航指南
- `system-integration.md` - 跨组件集成模式
- `deployment-infrastructure.md` - 基础设施上下文
- `handoff.md` - 会话连续性模板

### 示例文档

- `open-issues/example-api-performance-issue.md` - 问题跟踪示例
- `specs/example-feature-specification.md` - 功能规范示例
- `specs/example-api-integration-spec.md` - API 集成规范示例

## 6. 开发和维护指导

### 文档编辑原则

1. **保持一致性** - 所有文档应使用统一的术语和风格
2. **遵循 3 层架构** - 文档更新要符合层级边界
3. **避免膨胀** - 保持文档简洁,仅记录必要信息
4. **模板性质** - 记住这些是模板,不是特定项目的文档

### Shell 脚本规范

- **可执行权限**: 所有 `.sh` 文件必须有执行权限 (`chmod +x`)
- **Bash 兼容**: 使用 `#!/bin/bash` shebang
- **错误处理**: 使用 `set -euo pipefail` 确保错误被捕获
- **退出代码**: 0=成功, 2=阻止工具执行
- **日志记录**: 记录到 `.claude/logs/` (在用户项目中)

### 命令模板开发

- **变量使用**: 使用 `$ARGUMENTS` 接收用户输入
- **上下文引用**: 使用 `@/path/to/file.md` 引用文档
- **步骤化**: 提供清晰的步骤化指令
- **示例**: 包含具体的使用示例

## 7. 关键技术概念

### 自动上下文注入机制

通过 `subagent-context-injector.sh` 实现:
- 拦截所有 Task 工具调用
- 在原始提示前添加文档引用
- 对非 Task 工具透明传递
- 确保所有子代理获得一致的基础上下文

### MCP 服务器集成

- **Gemini Assistant**: 架构咨询和代码分析
- **Context7**: 实时库文档访问
- 通过钩子自动注入项目上下文
- 支持会话持久化

### 多代理编排模式

命令模板支持:
- 自适应代理生成 (0-N 个代理)
- 并行执行(通过单个消息的多个 Task 调用)
- 任务专业化和自定义范围
- 综合多视角发现

## 8. 与原始项目的关系

- **原始项目**: [Claude Code Development Kit by peterkrueck](https://github.com/peterkrueck/Claude-Code-Development-Kit)
- **本项目**: 中文本地化版本
- **许可证**: MIT License (2025 peterkrueck)
- **主要区别**:
  - 所有文档翻译为中文
  - 服务中文用户群体

## 9. 常用操作

### 浏览文档结构

```bash
# 查看所有命令模板
ls -l commands/

# 查看钩子脚本
ls -l hooks/*.sh

# 查看文档模板
ls -l docs/

# 查看 AI 上下文文档
ls -l docs/ai-context/
```

### 验证脚本权限

```bash
# 确保钩子脚本可执行
chmod +x hooks/*.sh

# 验证脚本语法
bash -n hooks/*.sh
```

### Git 工作流

```bash
# 查看状态
git status

# 查看未追踪文件
git ls-files --others --exclude-standard

# 添加所有新文件
git add .

# 提交
git commit -m "描述性提交信息"
```

## 10. 使用场景

### 作为模板使用

开发者可以:
1. 复制整个仓库到自己的项目
2. 根据项目需求自定义命令和文档模板
3. 配置钩子脚本
4. 填充项目特定的信息

### 作为参考使用

开发者可以:
1. 阅读命令模板了解提示词工程最佳实践
2. 学习钩子系统的实现模式
3. 理解 3 层文档架构的设计理念
4. 参考示例文档的格式

### 作为学习资源

适合学习:
- Claude Code 的高级特性(钩子、命令)
- 多代理编排模式
- AI 上下文管理策略
- MCP 服务器集成

## 11. 重要注意事项

1. **模板性质**: 这是一个模板仓库,需要用户根据自己的项目进行定制
2. **路径假设**: 钩子脚本假设特定的目录结构 (`.claude/hooks/`, `docs/` 等)
3. **依赖环境**: 需要 Claude Code CLI 工具才能使用
4. **平台兼容性**: Shell 脚本主要针对 Unix-like 系统,Windows 支持有限
5. **MCP 可选**: Gemini 和 Context7 MCP 服务器是可选的增强功能

## 12. 扩展和贡献

### 添加新命令模板

1. 在 `commands/` 创建新的 `.md` 文件
2. 遵循现有命令的结构和风格
3. 包含必要的自动加载上下文引用
4. 更新 `commands/README.md`

### 修改钩子脚本

1. 编辑对应的 `.sh` 文件
2. 遵循 bash 最佳实践
3. 测试脚本功能
4. 更新 `hooks/README.md`

### 改进文档模板

1. 编辑 `docs/` 下的相应模板文件
2. 保持 3 层架构的一致性
3. 更新 `docs/README.md`
4. 确保模板的通用性

## 13. 相关资源

- **Claude Code 官方仓库**: https://github.com/anthropics/claude-code
- **原始英文版**: https://github.com/peterkrueck/Claude-Code-Development-Kit
- **MCP 协议**: Model Context Protocol
- **相关项目**: Freigeist (AI 编码平台)

---

*此文件为 Claude Code Development Kit 中文版提供 AI 上下文指导。在此仓库中工作时,请遵循上述指导原则并保持模板的通用性和可重用性。*
