# 文档系统指南

> **项目来源**: 基于 [Claude Code Development Kit](https://github.com/peterkrueck/Claude-Code-Development-Kit) 的中文版 | MIT License

本指南解释了 3 层文档架构如何为 Claude Code 开发套件提供支持,以及为什么它能提供比传统文档方法更优越的结果。

## 快速开始 🚀

**最简单的开始方式(推荐):**

```bash
# 在你的项目中,只需这 3 个命令!
/create-docs CLAUDE.md
/create-docs docs/ai-context/project-structure.md
/create-docs docs/ai-context/docs-overview.md
```

AI 会自动分析你的项目并填充这些文件!

---

## 文件分类指南 📂

### 🔵 类型 A:需要复制到项目并填充内容的文件

这些文件要复制到你的项目中,然后填充实际项目信息:

#### 1. CLAUDE.md ⭐⭐⭐ (必需)

- **作用**: 主 AI 上下文文件
- **位置**: 复制到项目根目录 `your-project/CLAUDE.md`
- **需要修改**: 是,必须填充!

**需要替换的内容:**
```
[PROJECT NAME] → "My Awesome App"
[Vision] → "Build the best todo app"
[Current Phase] → "MVP Development"
[Key Architecture] → "React + FastAPI + PostgreSQL"
...所有 [...] 占位符
```

**填充方式:**
- 手动编辑
- 或运行 `/create-docs CLAUDE.md` 自动生成

---

#### 2. ai-context/project-structure.md ⭐⭐⭐ (必需)

- **作用**: 项目结构和技术栈
- **位置**: 复制到 `your-project/docs/ai-context/project-structure.md`
- **需要修改**: 是,必须填充!

**需要替换的内容:**
```
[Language] [Version] → Python 3.11+
[Web Framework] [Version] → FastAPI 0.115.0+
[DATABASE] [Version] → PostgreSQL 15
[PROJECT-NAME]/ → my-awesome-app/
├── [BACKEND-DIR]/ → backend/
│   ├── [BUILD-FILE] → pyproject.toml
...整个文件树结构
```

**填充方式:**
- 手动编辑
- 或运行 `/create-docs docs/ai-context/project-structure.md`

---

#### 3. ai-context/docs-overview.md ⭐⭐⭐ (必需)

- **作用**: 文档架构地图
- **位置**: 复制到 `your-project/docs/ai-context/docs-overview.md`
- **需要修改**: 是,必须填充!

**需要替换的内容:**
```markdown
# 默认的示例路径需要替换为实际路径

## Tier 2: Component-Level Documentation
- **[Backend Context](/backend/CONTEXT.md)** → 改成你的实际路径
- **[Frontend Context](/frontend/CONTEXT.md)** → 改成你的实际路径

## Tier 3: Feature-Specific Documentation
- **[API Layer](/backend/src/api/CONTEXT.md)** → 改成你的实际路径
```

**填充方式:**
- 手动编辑添加/删除条目
- 或 `/create-docs` 和 `/update-docs` 会自动维护

---

#### 4. MCP-ASSISTANT-RULES.md ⭐⭐ (推荐)

- **作用**: Gemini 咨询时的项目规则
- **位置**: 复制到项目根目录 `your-project/MCP-ASSISTANT-RULES.md`
- **需要修改**: 是,建议填充

**需要替换的内容:**
```
[Project Name] → "My Awesome App"
[Describe your project's vision] → "Build the best todo app"
...
```

**填充方式:**
- 手动编辑
- 这个文件主要给 Gemini 使用

---

#### 5. ai-context/handoff.md ⭐ (可选)

- **作用**: 任务交接和会话连续性
- **位置**: 复制到 `your-project/docs/ai-context/handoff.md`
- **需要修改**: 是,使用时填充

**内容性质:**
- 这是一个动态文件
- 记录当前任务、待办事项、已完成工作
- 需要在工作过程中持续更新

**填充方式:**
- 使用 `/handoff` 命令自动更新
- 或手动编辑

---

#### 6. ai-context/system-integration.md ⭐ (可选)

- **作用**: 跨组件集成模式
- **位置**: 复制到 `your-project/docs/ai-context/system-integration.md`
- **需要修改**: 是,但可选

**是否需要:**
- 如果项目有多个组件需要集成 → 填充
- 如果是简单单体应用 → 可以不用

---

#### 7. ai-context/deployment-infrastructure.md ⭐ (可选)

- **作用**: 部署和基础设施
- **位置**: 复制到 `your-project/docs/ai-context/deployment-infrastructure.md`
- **需要修改**: 是,但可选

**是否需要:**
- 如果有复杂的部署流程 → 填充
- 如果是简单本地开发 → 可以不用

---

### 🟡 类型 B:供 Command 参考的模板(不需要修改)

这些文件是给 AI 看的"示例",告诉 AI 应该生成什么样的文档:

#### 8. CONTEXT-tier2-component.md 📋

- **作用**: 第2层(组件级)文档的模板示例
- **是否需要修改**: 否,保持原样
- **用途**: command 参考这个结构生成实际文档

**实际使用:**
```bash
# 当你运行
/create-docs backend/CONTEXT.md

# AI 会:
# 1. 读取 CONTEXT-tier2-component.md 了解结构
# 2. 分析 backend/ 目录
# 3. 在 your-project/backend/CONTEXT.md 生成实际文档
```

**生成的文件位置:**
```
your-project/
├── backend/
│   └── CONTEXT.md  ← 新生成的实际文档
└── docs/
    └── CONTEXT-tier2-component.md  ← 模板,不动
```

---

#### 9. CONTEXT-tier3-feature.md 📋

- **作用**: 第3层(功能级)文档的模板示例
- **是否需要修改**: 否,保持原样
- **用途**: command 参考这个结构生成实际文档

**实际使用:**
```bash
# 当你运行
/create-docs backend/src/api/CONTEXT.md

# AI 会:
# 1. 读取 CONTEXT-tier3-feature.md 了解结构
# 2. 分析 backend/src/api/ 目录
# 3. 在 your-project/backend/src/api/CONTEXT.md 生成实际文档
```

**生成的文件位置:**
```
your-project/
├── backend/
│   └── src/
│       └── api/
│           └── CONTEXT.md  ← 新生成的实际文档
└── docs/
    └── CONTEXT-tier3-feature.md  ← 模板,不动
```

---

### 🟢 类型 C:纯示例文件(可以删除)

这些是给你看的例子,告诉你可以如何使用某些功能:

#### 10. open-issues/example-api-performance-issue.md 📝

- **作用**: 展示如何记录待解决问题
- **是否需要修改**: 否,可以删除
- **用途**: 纯示例,给你参考

**你可以:**
- 删除这个示例
- 创建自己的 issue 文件:`docs/open-issues/my-issue.md`

---

#### 11. specs/example-feature-specification.md 📝

- **作用**: 展示如何编写功能规范
- **是否需要修改**: 否,可以删除
- **用途**: 纯示例,给你参考

---

#### 12. specs/example-api-integration-spec.md 📝

- **作用**: 展示如何编写 API 集成规范
- **是否需要修改**: 否,可以删除
- **用途**: 纯示例,给你参考

---

### 📘 类型 D:说明文档(保留参考)

#### 13. README.md 📖

- **作用**: docs/ 目录的说明文档
- **是否需要修改**: 否,保留参考
- **用途**: 解释 3 层文档系统

---

## 📊 完整分类表

| 文件 | 分类 | 需要修改 | 修改方式 | 优先级 |
|------|------|----------|----------|--------|
| CLAUDE.md | 项目文档 | ✅ 是 | 复制到项目根目录并填充 | ⭐⭐⭐ 必需 |
| ai-context/project-structure.md | 项目文档 | ✅ 是 | 复制并填充 | ⭐⭐⭐ 必需 |
| ai-context/docs-overview.md | 项目文档 | ✅ 是 | 复制并填充 | ⭐⭐⭐ 必需 |
| MCP-ASSISTANT-RULES.md | 项目文档 | ✅ 是 | 复制到项目根目录并填充 | ⭐⭐ 推荐 |
| ai-context/handoff.md | 项目文档 | ✅ 是 | 复制并使用时填充 | ⭐ 可选 |
| ai-context/system-integration.md | 项目文档 | ✅ 是 | 复制并填充(可选) | ⭐ 可选 |
| ai-context/deployment-infrastructure.md | 项目文档 | ✅ 是 | 复制并填充(可选) | ⭐ 可选 |
| CONTEXT-tier2-component.md | Command 模板 | ❌ 否 | 保持原样,AI 参考 | 📋 模板 |
| CONTEXT-tier3-feature.md | Command 模板 | ❌ 否 | 保持原样,AI 参考 | 📋 模板 |
| open-issues/example-*.md | 示例 | ❌ 否 | 可以删除 | 📝 示例 |
| specs/example-*.md | 示例 | ❌ 否 | 可以删除 | 📝 示例 |
| README.md | 说明 | ❌ 否 | 保留参考 | 📖 说明 |

---

## 🎯 具体操作流程

### 第一步:复制必需文件到你的项目

```bash
cd your-project/

# 复制核心文档
cp /path/to/Claude-Code-Development-Kit-CN/CLAUDE.md ./
cp -r /path/to/Claude-Code-Development-Kit-CN/docs/ai-context ./docs/

# 复制 MCP 规则(可选)
cp /path/to/Claude-Code-Development-Kit-CN/docs/MCP-ASSISTANT-RULES.md ./
```

**结果:**
```
your-project/
├── CLAUDE.md                          ← 需要填充
├── MCP-ASSISTANT-RULES.md             ← 需要填充(可选)
└── docs/
    └── ai-context/
        ├── project-structure.md       ← 需要填充
        ├── docs-overview.md           ← 需要填充
        ├── handoff.md                 ← 使用时填充
        ├── system-integration.md      ← 可选
        └── deployment-infrastructure.md ← 可选
```

---

### 第二步:填充内容

#### 方法 A:自动生成(推荐)

```bash
cd your-project/
claude

# 在 Claude Code 中
/create-docs CLAUDE.md
/create-docs docs/ai-context/project-structure.md
/create-docs docs/ai-context/docs-overview.md
```

**AI 会:**
- 分析你的代码
- 自动填充这些文件
- 生成真实的项目信息

#### 方法 B:手动编辑

```bash
# 编辑 CLAUDE.md
vim CLAUDE.md
# 替换所有 [PROJECT NAME], [Vision] 等占位符

# 编辑 project-structure.md
vim docs/ai-context/project-structure.md
# 替换技术栈和文件树
```

---

### 第三步:生成实际的 CONTEXT.md 文件

```bash
# 在 Claude Code 中为各个组件生成文档
/create-docs backend/CONTEXT.md
/create-docs frontend/CONTEXT.md
/create-docs backend/src/api/CONTEXT.md
```

**这些命令会:**
1. 参考 `CONTEXT-tier2-component.md` 或 `CONTEXT-tier3-feature.md` 的结构
2. 分析实际代码
3. 在你的项目中生成实际的 CONTEXT.md 文件

**生成的位置:**
```
your-project/
├── backend/
│   ├── CONTEXT.md              ← 新生成(基于 tier2 模板)
│   └── src/
│       └── api/
│           └── CONTEXT.md      ← 新生成(基于 tier3 模板)
└── frontend/
    └── CONTEXT.md              ← 新生成(基于 tier2 模板)
```

---

### 第四步:可选的示例文件处理

```bash
# 如果你想使用 specs/ 和 open-issues/
mkdir -p docs/specs docs/open-issues

# 复制示例(可选)
cp /path/to/Claude-Code-Development-Kit-CN/docs/specs/* docs/specs/
cp /path/to/Claude-Code-Development-Kit-CN/docs/open-issues/* docs/open-issues/

# 或者删除示例,创建自己的
rm docs/specs/example-*.md
rm docs/open-issues/example-*.md
```

---

## 为什么使用 3 层系统

### 传统文档问题

标准文档方法为 AI 辅助开发创造了摩擦:

- **上下文过载** - AI 代理必须处理整个文档集才能完成简单任务
- **维护负担** - 每个代码变更都会级联到多个文档位置
- **过时内容** - 文档与实现现实偏离
- **无 AI 优化** - 人类可读格式缺乏机器处理的结构

### 3 层解决方案

该套件通过分层组织解决这些问题:

**第 1 层:基础(很少变更)**
- 项目范围的标准、架构决策、技术栈
- 每个 AI 会话自动加载
- 提供一致的基线,避免冗余
- 使用 CLAUDE.md 作为主上下文文件

**第 2 层:组件(偶尔变更)**
- 组件边界、架构模式、集成点
- 仅在特定组件内工作时加载
- 将架构决策与实现细节隔离
- 在组件根目录使用 CONTEXT.md 文件

**第 3 层:功能(频繁变更)**
- 实现细节、技术详情、本地模式
- 与代码就近放置以便立即更新
- 代码变更时最小化文档级联
- 在功能目录内使用 CONTEXT.md 文件

---

## 与传统系统相比的优势

### 1. 智能上下文加载

**传统**:AI 加载整个文档语料库,无论任务如何
**3 层**:命令根据复杂性仅加载相关层级

**示例:**
- 简单查询 → 仅第 1 层(最少令牌)
- 组件工作 → 第 1 层 + 相关第 2 层
- 深度实现 → 所有相关层级

### 2. 维护效率

**传统**:每个变更需要更新多个文档
**3 层**:更新隔离到适当层级

**示例:**
- API 端点变更 → 仅更新第 3 层 API 文档
- 新组件 → 添加第 2 层文档,第 1 层不变
- 编码标准 → 仅更新第 1 层,适用于所有地方

### 3. AI 性能优化

**传统**:AI 难以找到相关信息
**3 层**:结构化层次引导 AI 到精确上下文

**系统提供:**
- 代理导航的清晰路由逻辑
- 可预测的文档位置
- 通过目标加载实现高效令牌使用

---

## 与套件组件的集成

### 命令集成

命令利用 3 层结构进行智能操作:

```
命令执行 → 分析任务复杂性 → 加载适当层级
                                     ↓
                            简单:仅第 1 层
                            组件:第 1-2 层
                            复杂:所有相关层级
```

### MCP 服务器集成

外部 AI 服务通过层级系统接收适当上下文:

- **Gemini 咨询** - 自动附加 `project-structure.md`(第 1 层)
- **Context7 查找** - 在既定项目上下文内进行
- **建议** - 与记录的架构对齐

### 多代理路由

文档结构决定代理行为:

- 根据涉及的层级生成代理数量
- 每个代理接收目标文档子集
- 没有上下文重叠的并行分析

---

## 🔑 关键理解

### 模板的两种用途

#### 1. 直接填充的模板(需要修改)
```
CLAUDE.md
ai-context/*.md
MCP-ASSISTANT-RULES.md
```
→ 复制到项目 → 填充实际内容 → 被 AI 直接使用

#### 2. 结构参考的模板(不需要修改)
```
CONTEXT-tier2-component.md
CONTEXT-tier3-feature.md
```
→ 留在原处 → AI 参考结构 → 生成新的 CONTEXT.md 文件

### 工作流程

```
1. 复制并填充核心文档
   CLAUDE.md, project-structure.md, docs-overview.md
   ↓
2. 这些文档被自动加载到所有 command 和 hook 中
   ↓
3. 使用 /create-docs 为各个组件生成 CONTEXT.md
   ↓
4. AI 参考 CONTEXT-tier*.md 的结构生成实际文档
   ↓
5. 生成的 CONTEXT.md 文件在各个组件目录中
```

---

## 📝 总结

### 需要你处理的文件(优先级)

**1. 必需(3 个):**
- ✅ CLAUDE.md - 复制到项目根目录并填充
- ✅ ai-context/project-structure.md - 复制并填充
- ✅ ai-context/docs-overview.md - 复制并填充

**2. 推荐(1 个):**
- 📋 MCP-ASSISTANT-RULES.md - 如果使用 Gemini 咨询

**3. 可选(3 个):**
- 📋 ai-context/handoff.md - 如果需要任务交接
- 📋 ai-context/system-integration.md - 如果有复杂集成
- 📋 ai-context/deployment-infrastructure.md - 如果有复杂部署

### 不需要修改的文件

- ❌ CONTEXT-tier2-component.md - AI 参考模板
- ❌ CONTEXT-tier3-feature.md - AI 参考模板
- ❌ specs/example-*.md - 可以删除的示例
- ❌ open-issues/example-*.md - 可以删除的示例
- 📖 README.md - 保留参考

---

## 衡量成功

3 层系统在以下情况下成功:

1. **AI 代理快速找到上下文** - 无需搜索无关文档
2. **更新保持本地化** - 变更不会不必要地级联
3. **文档保持最新** - 就近放置确保更新发生
4. **命令高效工作** - 适当上下文自动加载
5. **MCP 服务器提供相关建议** - 外部 AI 理解你的项目

---

*Claude Code 开发套件的一部分 - 有关完整系统概述,请参见[主文档](../README.md)。*
