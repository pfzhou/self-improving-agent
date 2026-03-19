---
name: self-improvement
description: "自我改进与记忆系统，解决'同类错误反复犯、用户纠正不长记性'的痛点。用于捕获错误、用户纠正、最佳实践，并转化为长期记忆以实现持续改进。适用于以下情况：(1) 命令或操作意外失败 (2) 用户纠正，如‘不，那是错的……’、‘实际上……’ (3) 用户请求不存在的功能 (4) 调用外部 API 或工具失败 (5) 意识到其知识已陈旧或错误 (6) 为经常性任务找到了更好的方法。注意：在执行重大任务前，也请回顾这些记录。"
version: 1.0.0
metadata:
  # 中文触发关键词
  triggers:
    - 自我进化
    - 自我提升
    - 总结经验
    - 总结教训
    - 分析错误
    - 记录心得
    - 学习到了
    - 改进建议
    - 今天学到了什么
---

# Self-Improvement Skill

将学习心得和错误记录到 Markdown 文件中，以实现持续改进。Agent稍后可以将这些记录处理为修复方案，重要的心得将被提升到“项目记忆”中。

## Quick Reference

| **场景**             | **行动**                                                     |
| -------------------- | ------------------------------------------------------------ |
| 命令/操作失败        | 记录至 `.learnings/ERRORS.md`                                |
| 用户纠正了你         | 记录至 `.learnings/LEARNINGS.md`，类别设为 `correction`      |
| 用户需要缺失的功能   | 记录至 `.learnings/FEATURE_REQUESTS.md`                      |
| API/外部工具失败     | 记录至 `.learnings/ERRORS.md` 并包含集成详情                 |
| 知识已陈旧           | 记录至 `.learnings/LEARNINGS.md`，类别设为 `knowledge_gap`   |
| 发现更好的方法       | 记录至 `.learnings/LEARNINGS.md`，类别设为 `best_practice`   |
| 简化/强化重复模式    | 记录/更新 `.learnings/LEARNINGS.md`，标注 `Source: simplify-and-harden` 和稳定的 `Pattern-Key` |
| 与现有条目相似       | 使用 `**See Also**` 进行关联，考虑提高优先级                 |
| 具有普遍适用性的心得 | 提升至 `CLAUDE.md`、`AGENTS.md` 或 `.github/copilot-instructions.md` |
| 工作流改进           | 提升至 `AGENTS.md` (OpenClaw workspace)                         |
| 工具陷阱/注意点      | 提升至 `TOOLS.md` (OpenClaw workspace)                          |
| 行为模式/沟通风格    | 提升至 `SOUL.md` (OpenClaw workspace)                           |


## OpenClaw 设置（推荐）

OpenClaw 是此技能的主要平台。它使用基于工作区（workspace）的提示词注入（Prompt Injection）并支持自动加载技能（automatic skill loading）。

### 安装（Installation）

**手动安装：**

```Bash
git clone https://github.com/pfzhou/self-improving-agent.git ~/.openclaw/skills/self-improving-agent
```

**注意：**

本SKILL是基于原始仓库汉化并为 OpenClaw 优化重制：[peterskoett/self-improving-agent — GitHub](https://github.com/peterskoett/self-improving-agent)，或 [self-improving-agent — ClawHub](https://clawhub.ai/pskoett/self-improving-agent)，并不会跟随升级。


### 工作区结构（Workspace Structure）

OpenClaw 会将以下文件注入到每个会话中：

```
~/.openclaw/workspace/
├── AGENTS.md          # Multi-agent workflows, delegation patterns (多智能体工作流、委派模式)
├── SOUL.md            # Behavioral guidelines, personality, principles (行为准则、性格、原则)
├── TOOLS.md           # Tool capabilities, integration gotchas (工具能力、集成陷阱)
├── MEMORY.md          # Long-term memory (main session only) (长期记忆，仅限主会话)
├── memory/            # Daily memory files (每日记忆文件)
│   └── YYYY-MM-DD.md
└── .learnings/        # This skill's log files (此技能的日志文件)
    ├── LEARNINGS.md
    ├── ERRORS.md
    └── FEATURE_REQUESTS.md
```


### 创建学习记录文件（Create Learning Files）

```Bash
mkdir -p ~/.openclaw/workspace/.learnings
```

然后创建日志文件（或从 `.learnings/` 目录复制）：

- `LEARNINGS.md` — 捕获到的经验教训（Captured learnings）, 纠正更正（corrections）, 知识缺口（knowledge gaps）, 最佳实践（best practices）, 探索（discoveries）, 主要任务前的审核（Review before major tasks）
- `ERRORS.md` — 命令失败(Command failures), 异常(exceptions), and 意外的行为(unexpected behaviors)
- `FEATURE_REQUESTS.md` — 用户请求的功能（user-requested capabilities）


### 提升目标（Promotion Targets）

当学习心得证明具有广泛适用性时，将其提升至工作区（workspace）文件：

| **心得类型** | **提升至**  | **示例**                   |
| ------------ | ----------- | -------------------------- |
| 行为模式     | `SOUL.md`   | “简洁明了，避免免责声明式措辞”   |
| 工作流改进   | `AGENTS.md` | “为长时间任务生成子智能体（sub-agents）” |
| 工具陷阱     | `TOOLS.md`  | “git push 之前需要先配置认证”  |


### 会话间通信(Inter-Session Communication)

OpenClaw 提供了在不同会话间共享学习记录的工具：

- **sessions_list** — View active/recent sessions (查看活跃/最近的会话)
- **sessions_history** — Read another session's transcript (阅读其他会话的对话记录)
- **sessions_send** — Send a learning to another session (将记录发送到另一个会话)
- **sessions_spawn** — Spawn a sub-agent for background work (生成子智能体sub-agent进行后台工作)

### 可选：启用钩子（Hook）

用于在会话开始时自动提醒：

```bash
# Copy hook to OpenClaw hooks directory
cp -r hooks/openclaw ~/.openclaw/hooks/self-improvement

# Enable it
openclaw hooks enable self-improvement
```

详见 `references/openclaw-integration.md` 以获取完整细节。


## 通用设置（其他Agents）

对于 Claude Code、Codex、Copilot 或其他智能体，请在项目中创建 `.learnings/` 目录：

```Bash
mkdir -p .learnings
```

从 `.learnings/` 复制模板或创建带有标题的文件。


### 在智能体文件（如 AGENTS.md、CLAUDE.md 或 .github/copilot-instructions.md）中添加引用，以提醒自己记录心得。（这是hook提醒的替代方案）

#### Self-Improvement Workflow

当发生错误或更正时：

1. 记录到 `.learnings/ERRORS.md`、`LEARNINGS.md` 或 `FEATURE_REQUESTS.md`

2. 审查并将具有广泛适用性的心得提升至：

   - `CLAUDE.md` - project facts and conventions (项目事实与规范)
   - `AGENTS.md` - workflows and automation (工作流与自动化)
   - `.github/copilot-instructions.md` - Copilot context (Copilot 上下文)


## 记录格式(Logging Format)

### 学习记录条目(Learning Entry)

追加至 `.learnings/LEARNINGS.md`：

```markdown
## [LRN-YYYYMMDD-XXX] category

**Logged**: ISO-8601 timestamp
**Priority**: low | medium | high | critical
**Status**: pending
**Area**: frontend | backend | infra | tests | docs | config

### Summary
One-line description of what was learned

### Details
Full context: what happened, what was wrong, what's correct

### Suggested Action
Specific fix or improvement to make

### Metadata
- Source: conversation | error | user_feedback
- Related Files: path/to/file.ext
- Tags: tag1, tag2
- See Also: LRN-20250110-001 (if related to existing entry)
- Pattern-Key: simplify.dead_code | harden.input_validation (optional)
- Recurrence-Count: 1 (optional)
- First-Seen: 2025-01-15 (optional)
- Last-Seen: 2025-01-15 (optional)

---
```

### 错误条目(Error Entry)

追加至 `.learnings/ERRORS.md`：

```markdown
## [ERR-YYYYMMDD-XXX] skill_or_command_name

**Logged**: ISO-8601 timestamp
**Priority**: high
**Status**: pending
**Area**: frontend | backend | infra | tests | docs | config

### Summary
Brief description of what failed

### Error
```

Actual error message or output

```markdown
### Context
- Command/operation attempted
- Input or parameters used
- Environment details if relevant

### Suggested Fix
If identifiable, what might resolve this

### Metadata
- Reproducible: yes | no | unknown
- Related Files: path/to/file.ext
- See Also: ERR-20250110-001 (if recurring)

---
```

### 功能请求条目(Feature Request Entry)

追加至 `.learnings/FEATURE_REQUESTS.md`：

```markdown
## [FEAT-YYYYMMDD-XXX] capability_name

**Logged**: ISO-8601 timestamp
**Priority**: medium
**Status**: pending
**Area**: frontend | backend | infra | tests | docs | config

### Requested Capability
What the user wanted to do

### User Context
Why they needed it, what problem they're solving

### Complexity Estimate
simple | medium | complex

### Suggested Implementation
How this could be built, what it might extend

### Metadata
- Frequency: first_time | recurring
- Related Features: existing_feature_name

---
```

## ID 生成(ID Generation)

格式：`TYPE-YYYYMMDD-XXX`

- TYPE: `LRN` (learning), `ERR` (error), `FEAT` (feature)
- YYYYMMDD: Current date
- XXX: Sequential number or random 3 chars (e.g., `001`, `A7B`)

示例：`LRN-20250115-001`, `ERR-20250115-A3F`, `FEAT-20250115-002`

## 解决条目(Resolving Entries)

当问题被修复时，更新该条目：

1. 将 `**Status**: pending` 修改为 `**Status**: resolved`
2. 在元数据Metadata后添加解决块( resolution block)：

```markdown
### Resolution
- **Resolved**: 2025-01-16T09:00:00Z
- **Commit/PR**: abc123 or #42
- **Notes**: Brief description of what was done
```

其他状态值：

- `in_progress` - Actively being worked on (正在处理中)
- `wont_fix` - Decided not to address (决定不予处理，需在 Resolution 备注中写明原因)
- `promoted` - Elevated to CLAUDE.md, AGENTS.md, or .github/copilot-instructions.md (已提升至更高层级的文件)

## 提升至项目记忆

当一个学习心得具有广泛适用性（而非一次性修复）时，将其提升为永久的项目记忆。

### 何时提升

- Learning applies across multiple files/features (心得适用于多个文件/功能)
- Knowledge any contributor (human or AI) should know (任何贡献者（人或 AI）都应该知道的知识)
- Prevents recurring mistakes (防止重复犯错)
- Documents project-specific conventions (记录项目特定的约定)

### 提升目标

| **目标文件**                          | **适用内容**                            |
| --------------------------------- | ----------------------------------- |
| `CLAUDE.md`                       | 项目事实、规范、所有 Claude 交互的易错点            |
| `AGENTS.md`                       | 智能体特定的工作流、工具使用模式、自动化规则              |
| `.github/copilot-instructions.md` | GitHub Copilot 的项目上下文和规范            |
| `SOUL.md`                         | 行为准则、沟通风格、原则 (OpenClaw workspace)   |
| `TOOLS.md`                        | 工具能力、使用模式、集成陷阱 (OpenClaw workspace) |

### 如何提升

1. **提炼**学习心得为简洁的规则或事实
2. **添加**到目标文件的相应章节（如果文件不存在则创建）
3. **更新**原始条目：

   - 将 `**Status**: pending` 改为 `**Status**: promoted`
   - 添加 `**Promoted**: CLAUDE.md`、`AGENTS.md` 或 `.github/copilot-instructions.md`

### 提升示例

**学习记录Learning**（详细版verbose）：

> 项目使用 pnpm workspaces。尝试使用 `npm install` 失败。
> 锁定文件是 `pnpm-lock.yaml`。必须使用 `pnpm install`。

**提升写入 CLAUDE.md**（精简版concise）：

```Markdown
## Build & Dependencies
- Package manager: pnpm (not npm) - use `pnpm install`
```

**学习记录**（详细版verbose）：

> 修改 API endpoints 后，必须重新生成 TypeScript client。
> 忘记做这一步会导致运行时类型不匹配。

**写入 AGENTS.md**（可操作版actionable）：

```markdown
## After API Changes
1. 重新生成 client：`pnpm run generate:api`
2. 检查类型错误：`pnpm tsc --noEmit`
```

## 重复模式检测(Recurring Pattern Detection)

如果记录的内容与现有条目相似：

1. **先搜索**: `grep -r "keyword" .learnings/`

2. **关联条目**：在 Metadata 中添加 `**See Also**: ERR-20250110-001`

3. **提高优先级**, 如果问题不断重复

4. **考虑系统性修复**：重复出现的问题通常预示着：

   - 文档缺失（→ 提升到 CLAUDE.md 或 .github/copilot-instructions.md）
   - 自动化缺失（→ 添加到 AGENTS.md）
   - 架构问题（→ 创建技术债tech debt 工单）

## 简化与强化反馈信息流

 使用此工作流，把来自 `simplify-and-harden` 技能中的重复模式“摄入”（ingest）并转化为更耐用的提示指导（prompt guidance）。

### 摄取工作流(Ingestion Workflow)

1. 从任务总结中读取简化/强化的候选模式(`simplify_and_harden.learning_loop.candidates`)。

2. 对于每个候选模式，使用 `pattern_key` 作为稳定的去重键（dedupe key）。

3. 在 `.learnings/LEARNINGS.md` 中搜索具有该键的现有条目。

4. 如果找到：
- 增加“重复计数”
- 更新“最后看到时间”  `Last-Seen`
- 为相关条目/任务添加 `See Also` 链接

5. 如果未找到：
- 创建一个新的 `LRN-...` 条目
- 设置 `Source: simplify-and-harden`
- 设置 `Pattern-Key`、`Recurrence-Count: 1` 以及 `First-Seen`/`Last-Seen`

### 提升规则（系统提示词反馈）

当满足以下所有条件时，将重复模式提升至智能体上下文/系统提示词文件：

- `Recurrence-Count >= 3` (重复次数大于等于 3)
- Seen across at least 2 distinct tasks (在至少 2 个不同的任务中出现)
- Occurred within a 30-day window (发生在 30 天窗口期内)

提升目标：

- `CLAUDE.md`
- `AGENTS.md`
- `.github/copilot-instructions.md`
- 如适用（OpenClaw worksoace level 指导）：`SOUL.md` / `TOOLS.md`

将提升后的规则写成简短的预防规则（在编码前/编码中该做什么），而不是冗长的事故复盘（incident write-up）。

## 定期回顾

在自然中断点处回顾 `.learnings/` 目录：

### 何时回顾

- Before starting a new major task (开始新的重大任务前)
- After completing a feature (完成一个功能后)
- When working in an area with past learnings (在处理曾经有相关 learnings 的领域时)
- Weekly during active development (活跃开发期间每周回顾一次)

### 快速状态检查

```Bash
# Count pending items (计算待处理项目数量)
grep -h "Status\*\*: pending" .learnings/*.md | wc -l

# List pending high-priority items (列出待处理的高优先级项目)
grep -B5 "Priority\*\*: high" .learnings/*.md | grep "^## \["
```

## 回顾动作
- Resolve fixed items(将已修复条目标记为 resolved)
- Promote applicable learnings(提升适用的学习记录)
- Link related entries(关联相关条目)
- Escalate recurring issues(对重复问题升级处理,例如提高优先级/推动系统性修复)

## 触发条件

当你注意到以下情况时，自动记录：

**纠正（Corrections）**（→ 记录为 learning，类别 `correction`）：

- “不对，这不对……”
- “其实应该是……”
- “你关于……说错了”
- “这已经过时了……”
- "纠正一下..."
- "错了，正确的是..."

 **功能请求（Feature Requests）**（→ 记录为 feature request）：

- "能不能加个..."
- "我希望你能..."
- "有没有办法..."
- "为什么不能..."
- "建议增加..."
- "能不能优化一下..."

**知识缺口（Knowledge Gaps）**（→ 记录为 learning，类别 `knowledge_gap`）：

- 用户提供了你不知道的信息
- 你引用的文档已过时
- API 行为与你的理解不一致
- "告诉你一个新知识..."
- "最新的情况是..."

**错误（Errors）**（→ 记录为 error entry）：

- 命令返回非零退出码
- 异常或堆栈跟踪（stack trace）
- 意外输出或行为
- 超时或连接失败

**主动触发（中文指令）**：
- "自我进化"
- "自我提升"
- "总结经验"
- "总结教训"
- "分析错误"
- "记录心得"
- "学习到了"
- "改进建议"
- "今天学到了什么"

## 主动反思机制
每次完成用户请求后，自动在后台执行以下反思逻辑（不影响主会话响应速度）：
1. 回顾本次交互的完整过程，判断是否有值得记录的经验或教训
2. 如果有可沉淀的内容，自动分类写入对应的`.learnings/`文件
3. 重大经验教训会主动向用户提出改进建议（可选）

## 优先级指南

| **优先级**        | **适用场景**             |
| -------------- | -------------------- |
| `critical`（紧急） | 阻塞核心功能、有数据丢失风险、安全问题  |
| `high`（高）      | 影响显著、影响常用工作流、重复出现的问题 |
| `medium`（中）    | 影响中等、有替代方案/变通方法      |
| `low`（低）       | 轻微不便、边缘情况、可选改进       |

## 领域标签(Area Tags)

| **领域**   | **范围**                    |
| ---------- | --------------------------- |
| `frontend` | UI、组件、客户端代码        |
| `backend`  | API、服务、服务端代码       |
| `infra`    | CI/CD、部署、Docker、云服务 |
| `tests`    | 测试文件、测试工具、覆盖率  |
| `docs`     | 文档、注释、README          |
| `config`   | 配置文件、环境、设置        |


## 最佳实践


1. **立刻记录** - 问题发生后上下文最清晰
2. **具体明确** - 未来的agents能快速理解
3. **包含复现步骤** - 特别是错误类条目
4. **链接相关文件** - 更容易修复
5. **提出具体修复建议** - 不只是写“去调查”
6. **使用一致的分类** - 便于筛选与统计
7. **积极提升** - 拿不准就加到 CLAUDE.md 或 MEMORY.md
8. **定期回顾** - 过时的学习记录价值会下降


## Gitignore 选项

 **把 learnings 保持在本地**（每个开发者各自维护）：

```gitignore
.learnings/
```

**在仓库中跟踪 learnings**（团队共享）： 
不要加入 .gitignore —— learnings 会成为共享知识。

```gitignore
.learnings/*.md
!.learnings/.gitkeep
```


## Hook 集成

通过代理 hooks 启用自动提醒。这是**可选开启（opt-in）**的——你必须显式配置 hooks。

### Quick Setup (Claude Code / Codex)

Create `.claude/settings.json` in your project:

```json
{
  "hooks": {
    "UserPromptSubmit": [{
      "matcher": "",
      "hooks": [{
        "type": "command",
        "command": "./skills/self-improvement/scripts/activator.sh"
      }]
    }]
  }
}
```

This injects a learning evaluation reminder after each prompt (~50-100 tokens overhead).

### Full Setup (With Error Detection)

```json
{
  "hooks": {
    "UserPromptSubmit": [{
      "matcher": "",
      "hooks": [{
        "type": "command",
        "command": "./skills/self-improvement/scripts/activator.sh"
      }]
    }],
    "PostToolUse": [{
      "matcher": "Bash",
      "hooks": [{
        "type": "command",
        "command": "./skills/self-improvement/scripts/error-detector.sh"
      }]
    }]
  }
}
```

### 可用 Hook 脚本

| **脚本**                    | **钩子类型**       | **用途**                 |
| --------------------------- | ------------------ | ------------------------ |
| `scripts/activator.sh`      | UserPromptSubmit   | 在任务完成后提醒评估心得 |
| `scripts/error-detector.sh` | PostToolUse (Bash) | 在命令出错时触发         |
 详细配置与排障请参见 `references/hooks-setup.md`。
 

## 自动技能提取


当某条学习足够有价值、值得变成可复用技能时，使用提供的辅助工具（helper）进行提取（extract）。


### 技能提取标准

 满足以下任一条件时，该学习条目适合提取为技能：

- **Recurring**: Has `See Also` links to 2+ similar issues (重复出现：关联了 2 个以上类似问题`See Also` )
- **Verified**: Status is `resolved` with working fix (已验证：状态为已解决且有现成方案)
- **Non-obvious**: Required actual debugging/investigation (非显而易见：需要实际调试或调查)
- **Broadly applicable**: Useful across codebases (广泛适用：跨代码库通用)
- **User-flagged**: User says "save this as a skill" (用户标记：用户要求保存为技能)


### 提取工作流（Extraction Workflow）


1. **确定候选项**：learning 满足提取标准

2. 运行辅助脚本（或手动创建）：

   ```bash
   ./skills/self-improvement/scripts/extract-skill.sh skill-name --dry-run
   ./skills/self-improvement/scripts/extract-skill.sh skill-name
   ```

3. **定制 SKILL.md**：用学习内容填充模板

4. **更新 learning**：将状态设为 `promoted_to_skill`，并添加 `Skill-Path`

5. **验证**：在新的会话中读取该技能，确保其是自包含（self-contained）的

### 手动提取（Manual Extraction）

 如果你更倾向于手动创建：

1. 创建 `skills/<skill-name>/SKILL.md`

2. 使用 `assets/SKILL-TEMPLATE.md` 的模板

3. 遵循 Agent Skills 规范:

   - 需要包含带 `name` 与 `description` 的 YAML frontmatter
   - 名称必须与文件夹名一致
   - skill 文件夹内不应包含 README.md

### 提取触发信号（Extraction Detection Triggers）

 关注以下信号，表示某条 learning 可能应该变成 skill：

 **在对话中：**

- “把这个保存成技能”
- “我总是遇到这个问题”
- “这对其他项目也有用”
- “记住这个模式”

 **在 learning 条目中：**

- 多个 `See Also` 链接（说明是重复问题）
- 高优先级 + resolved 状态
- 类别为 `best_practice` 且适用面广
- 用户反馈称赞该解决方案

### 技能质量门槛（Skill Quality Gates）

 提取前请确认：

- [ ] Solution is tested and working(方案已测试并可用)
- [ ] Description is clear without original context(描述在脱离原始上下文时仍清晰)
- [ ] Code examples are self-contained(代码示例自包含)
- [ ] No project-specific hardcoded values(不包含项目特有的硬编码值)
- [ ] Follows skill naming conventions (lowercase, hyphens)(遵循技能命名约定（小写、用连字符）)

## 多智能体支持

### Claude Code


**启用方式**：Hooks（UserPromptSubmit、PostToolUse）
**配置**：在 `.claude/settings.json` 中配置 hooks
**检测**：通过 hook 脚本自动检测

### Codex CLI

**启用方式**：Hooks（与 Claude Code 同模式）
**配置**：在 `.codex/settings.json` 中配置 hooks
**检测**：通过 hook 脚本自动检测

### GitHub Copilot

**启用方式**：手动（不支持hook）

**设置**：添加到 `.github/copilot-instructions.md`：

```Markdown
## Self-Improvement

After solving non-obvious issues, consider logging to `.learnings/`:
1. Use format from self-improvement skill
2. Link related entries with See Also
3. Promote high-value learnings to skills
```

### OpenClaw

**启用方式**：工作区（workspace）注入 + 代理间消息（inter-agent messaging）
**配置**：参见上文 “OpenClaw Setup” 章节
**检测**：通过会话工具与工作区文件进行


### 与代理无关的通用指导（Agent-Agnostic Guidance）

无论使用哪种代理，遇到以下情况都应该应用自我改进（self-improvement）：

1. **发现非显而易见的内容** - 解决方案并非立刻就能想到
2. **纠正自己** - 起初的方法是错误的
3. **学到项目约定** - 发现了未文档化的模式
4. **遇到意外错误** - 尤其是诊断过程较困难时
5. **发现更好的方法** - 相比原方案有改进


