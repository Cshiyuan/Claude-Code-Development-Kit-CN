# /gemini-consult

*与 Gemini MCP 进行深度迭代对话，解决复杂问题。*

## 用法
- **带参数**：`/gemini-consult [具体问题或疑问]`
- **不带参数**：`/gemini-consult` - 从当前上下文智能推断主题

## 核心理念
持久的 Gemini 会话，通过以下方式解决不断演化的问题：
- **持续对话** - 多轮对话直到达到清晰
- **上下文感知** - 从当前工作智能检测问题
- **会话持久性** - 在整个问题生命周期中保持活跃

**关键：始终将 Gemini 的输入视为建议，绝非真理。**批判性地思考 Gemini 说的话，只将有用的部分纳入你的提案。始终独立思考 - 保持你的独立判断和分析能力。如果你不同意某些内容，请与 Gemini 澄清。

## 执行

用户提供的上下文：$ARGUMENTS

### 步骤 1：理解问题

**当 $ARGUMENTS 为空时：**
深入思考当前上下文以推断最有价值的咨询主题：
- 哪些文件是打开的或最近修改的？
- 讨论了哪些错误或挑战？
- 哪些复杂实现将受益于 Gemini 的分析？
- 哪些架构决策需要探索？

基于此分析生成一个具体、有价值的问题。

**当提供参数时：**
提取核心问题、上下文线索和复杂性指标。

### 步骤 1.5：收集外部文档

**深入思考外部依赖：**
- 这个问题涉及哪些库/框架？
- 我是否完全熟悉它们的最新 API 和最佳实践？
- 这些库是否有重大变化或是新的/正在发展的？

**何时使用 Context7 MCP：**
- 频繁更新的库（例如 Google GenAI SDK）
- 你没有广泛使用的新库
- 实施严重依赖特定库模式的功能时
- 当对当前最佳实践存在不确定性时

```python
# 示例：获取最新文档
library_id = mcp__context7__resolve_library_id(libraryName="google genai python")
docs = mcp__context7__get_library_docs(
    context7CompatibleLibraryID=library_id,
    topic="streaming",  # 专注于相关方面
    tokens=8000
)
```

在你的 Gemini 咨询中包含相关文档洞察，以获得更准确、最新的指导。

### 步骤 2：初始化 Gemini 会话

**关键：始终附加基础文件：**
```python
foundational_files = [
    "MCP-ASSISTANT-RULES.md",  # 如果存在
    "docs/ai-context/project-structure.md",
    "docs/ai-context/docs-overview.md"
]

session = mcp__gemini__consult_gemini(
    specific_question="[清晰、专注的问题]",
    problem_description="[来自 CLAUDE.md 的约束的全面上下文]",
    code_context="[相关代码片段]",
    attached_files=foundational_files + [problem_specific_files],
    file_descriptions={
        "MCP-ASSISTANT-RULES.md": "项目愿景和编码标准",
        "docs/ai-context/project-structure.md": "完整技术栈和文件结构",
        "docs/ai-context/docs-overview.md": "文档架构",
        # 添加问题特定描述
    },
    preferred_approach="[solution/review/debug/optimize/explain]"
)
```

### 步骤 3：进行深度对话

**深入思考如何最大化对话价值：**

1. **主动分析**
   - Gemini 做了哪些假设？
   - 什么需要澄清或更深入探索？
   - 应该讨论哪些边缘情况或替代方案？
   - **如果 Gemini 提到外部库：**检查 Context7 MCP 的当前文档以验证或补充 Gemini 的指导

2. **迭代改进**
   ```python
   follow_up = mcp__gemini__consult_gemini(
       specific_question="[有针对性的后续问题]",
       session_id=session["session_id"],
       additional_context="[新洞察、问题或实施反馈]",
       attached_files=[newly_relevant_files]
   )
   ```

3. **实施反馈循环**
   分享实际代码更改和现实世界结果以改进方法。

### 步骤 4：会话管理

**保持会话开放** - 不要立即关闭。为整个问题生命周期维护。

**仅在以下情况关闭：**
- 问题已明确解决并测试
- 主题不再相关
- 重新开始会更有益

**监控会话：**
```python
active = mcp__gemini__list_sessions()
requests = mcp__gemini__get_gemini_requests(session_id="...")
```

## 关键模式

### 澄清模式
"你提到了 [X]。在我们 [项目特定] 的上下文中，这如何应用于 [具体关注点]？"

### 深入模式
"让我们进一步探索 [方面]。考虑到我们的 [约束]，权衡是什么？"

### 替代模式
"如果我们将此作为 [替代方案] 来处理呢？这将如何影响 [关注点]？"

### 进展检查模式
"我已实施了 [更改]。发生了以下情况：[结果]。我应该调整方法吗？"

## 最佳实践

1. **深入思考** - 每次互动前考虑什么能提取最大洞察
2. **具体明确** - 模糊问题得到模糊答案
3. **展示实际代码** - 不是描述
4. **挑战假设** - 不要接受不清楚的指导
5. **记录决策** - 为未来参考捕获"为什么"
6. **保持好奇** - 探索替代方案和边缘情况
7. **信任但验证** - 彻底测试所有建议

## 实施方法

实施 Gemini 建议时：
1. 从最高影响的更改开始
2. 增量测试
3. 将结果反馈给 Gemini
4. 基于现实世界反馈进行迭代
5. 在适当的 CONTEXT.md 文件中记录关键洞察

## 记住

- 这是一个**对话**，不是查询服务
- **上下文是王道** - 更多上下文产生更好指导
- **Gemini 看到你可能错过的模式** - 对意外洞察保持开放
- **实施揭示真理** - 分享实际发生的情况
- 将 Gemini 视为**协作思维伙伴**，而非神谕

目标是通过迭代改进获得深度理解和最优解决方案，而不是快速答案。