# Claude Code 钩子系统

本目录包含经过实战测试的钩子，通过自动化安全扫描、智能上下文注入和愉悦的音频反馈来增强您的Claude Code开发体验。

## 架构

```
Claude Code 生命周期
        │
        ├── PreToolUse ──────► 安全扫描器
        │                      ├── 上下文注入器 (Gemini)
        │                      └── 上下文注入器 (子代理)
        │
        ├── 工具执行
        │
        ├── PostToolUse
        │
        ├── Notification ────────► 音频反馈
        │
        └── Stop/SubagentStop ───► 完成声音
```

这些钩子在Claude Code生命周期的特定点执行，提供对AI行为的确定性控制。

## 可用钩子

### 1. Gemini上下文注入器 (`gemini-context-injector.sh`)

**目的**: 在启动新的Gemini咨询会话时自动包含您的项目文档和助手规则，确保AI具有关于您代码库和项目标准的完整上下文。

**触发器**: `PreToolUse` 用于 `mcp__gemini__consult_gemini`

**功能**:
- 检测新的Gemini咨询会话（无session_id）
- 自动附加两个关键文件：
  - `docs/ai-context/project-structure.md` - 完整的项目结构和技术栈
  - `MCP-ASSISTANT-RULES.md` - 项目特定的编码标准和指南
- 保留现有文件附件
- 会话感知（仅在新会话时注入）
- 记录所有注入事件用于调试
- 如果任一文件缺失会优雅失败
- 处理部分可用性（将附加存在的文件）

**自定义**:
- 将`docs/MCP-ASSISTANT-RULES.md`模板复制到您的项目根目录
- 使用您项目特定的标准、原则和约束进行自定义
- 钩子将自动在Gemini咨询中包含它

### 2. MCP安全扫描器 (`mcp-security-scan.sh`)

**目的**: 在使用Gemini或Context7等MCP服务器时防止意外暴露密钥、API密钥和敏感数据。

**触发器**: `PreToolUse` 用于所有MCP工具 (`mcp__.*`)

**功能**:
- 基于模式的API密钥、密码和密钥检测
- 扫描代码上下文、问题描述和附加文件
- 带大小限制的文件内容扫描
- 通过`config/sensitive-patterns.json`可配置模式匹配
- 占位符值白名单
- Context7命令注入保护
- 全面的安全事件日志记录到`.claude/logs/`

**自定义**: 编辑`config/sensitive-patterns.json`以：
- 添加自定义API密钥模式
- 修改凭据检测规则
- 更新敏感文件模式
- 扩展占位符白名单

### 3. 子代理上下文注入器 (`subagent-context-injector.sh`)

**目的**: 在所有子代理Task提示中自动包含核心项目文档，确保多代理工作流程中的一致上下文。

**触发器**: `PreToolUse` 用于 `Task` 工具

**功能**:
- 在执行前拦截所有Task工具调用
- 为三个核心文档文件添加引用前缀：
  - `docs/CLAUDE.md` - 项目概览、编码标准、AI指令
  - `docs/ai-context/project-structure.md` - 完整文件树和技术栈
  - `docs/ai-context/docs-overview.md` - 文档架构
- 对非Task工具透明传递
- 通过在原始任务提示前添加上下文来保留它
- 在所有子代理间启用一致的知识
- 消除在Task提示中手动包含上下文的需要

**优势**:
- 每个子代理都从相同的基础知识开始
- Task提示中无需手动指定上下文
- 通过@引用而非内容重复实现Token效率
- 在一个地方更新上下文，影响所有子代理
- 对非Task工具的简洁透明传递操作

### 4. 通知系统 (`notify.sh`)

**目的**: 当Claude Code需要您的注意或完成任务时提供愉悦的音频反馈。

**触发器**:
- `Notification` 事件（包括需要输入的所有通知）
- `Stop` 事件（主任务完成）

**功能**:
- 跨平台音频支持（macOS、Linux、Windows）
- 非阻塞音频播放（后台运行）
- 多种音频播放回退方案
- 愉悦的通知声音
- 两种通知类型：
  - `input`: 当Claude需要用户输入时
  - `complete`: 当Claude完成任务时

## 安装

1. **将钩子复制到您的项目**:
   ```bash
   cp -r hooks your-project/.claude/
   ```

2. **在您的项目中配置钩子**:
   ```bash
   cp hooks/setup/settings.json.template your-project/.claude/settings.json
   ```
   然后编辑设置文件中的WORKSPACE路径。

3. **测试钩子**:
   ```bash
   # 测试通知
   .claude/hooks/notify.sh input
   .claude/hooks/notify.sh complete

   # 查看日志
   tail -f .claude/logs/context-injection.log
   tail -f .claude/logs/security-scan.log
   ```

## 钩子配置

添加到您的Claude Code `settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "mcp__gemini__consult_gemini",
        "hooks": [
          {
            "type": "command",
            "command": "${WORKSPACE}/.claude/hooks/gemini-context-injector.sh"
          }
        ]
      },
      {
        "matcher": "mcp__.*",
        "hooks": [
          {
            "type": "command",
            "command": "${WORKSPACE}/.claude/hooks/mcp-security-scan.sh"
          }
        ]
      },
      {
        "matcher": "Task",
        "hooks": [
          {
            "type": "command",
            "command": "${WORKSPACE}/.claude/hooks/subagent-context-injector.sh"
          }
        ]
      }
    ],
    "Notification": [
      {
        "matcher": ".*",
        "hooks": [
          {
            "type": "command",
            "command": "${WORKSPACE}/.claude/hooks/notify.sh input"
          }
        ]
      }
    ],
    "Stop": [
      {
        "matcher": ".*",
        "hooks": [
          {
            "type": "command",
            "command": "${WORKSPACE}/.claude/hooks/notify.sh complete"
          }
        ]
      }
    ]
  }
}
```

查看`hooks/setup/settings.json.template`获取包含所有钩子和MCP服务器的完整配置。

## 安全模型

1. **执行上下文**: 钩子以完整用户权限运行
2. **阻塞行为**: 退出代码2会阻止工具执行
3. **数据流**: 钩子可以通过JSON转换修改工具输入
4. **隔离**: 每个钩子在自己的进程中运行
5. **日志记录**: 所有安全事件记录到`.claude/logs/`

## 与MCP服务器集成

钩子系统补充MCP服务器集成：

- **Gemini咨询**: 上下文注入器确保项目结构和MCP助手规则都被包含
- **Context7文档**: 安全扫描器保护库ID输入
- **所有MCP工具**: 外部调用前的通用安全扫描

## 最佳实践

1. **钩子设计**:
   - 优雅失败 - 永远不要破坏主工作流程
   - 记录重要事件用于调试
   - 适当使用退出代码（0=成功，2=阻止）
   - 保持执行时间最小

2. **安全**:
   - 定期更新敏感模式
   - 定期查看安全日志
   - 首先在安全环境中测试钩子
   - 永远不要在钩子中记录敏感数据

3. **配置**:
   - 使用`${WORKSPACE}`变量以实现可移植性
   - 保持钩子可执行（`chmod +x`）
   - 版本控制钩子配置
   - 记录自定义修改

## 故障排除

### 钩子未执行
- 检查文件权限：`chmod +x *.sh`
- 验证settings.json中的路径
- 检查Claude Code日志中的错误

### 安全扫描器过于严格
- 查看`config/sensitive-patterns.json`中的模式
- 将合法模式添加到白名单
- 检查日志以了解触发阻止的原因

### 没有声音播放
- 验证`sounds/`目录中存在声音文件
- 测试音频播放：`.claude/hooks/notify.sh input`
- 检查系统音频设置
- 确保您安装了音频播放器（macOS上的afplay，Linux上的paplay、aplay、pw-play、play、ffplay，或Windows上的PowerShell）

## 钩子设置命令

要进行全面的设置验证和测试，使用：

```
/hook-setup
```

此命令使用多代理编排来验证安装、检查配置并运行全面测试。详情请参阅[hook-setup.md](setup/hook-setup.md)。

## 扩展点

该工具包设计为可扩展：

1. **自定义钩子**: 按照现有模式添加新脚本
2. **事件处理器**: 为任何Claude Code事件配置钩子
3. **模式更新**: 根据您的需要修改安全模式
4. **声音自定义**: 用您的首选项替换音频文件