# gsdd

gsdd 是一套用于 Claude Code 的规格驱动开发（SDD）流程：先把需求写成规格，再按规格设计、拆任务、实现和验收，规格是贯穿整个开发过程的唯一依据。

整套流程以技能（skill）的形式提供，每个技能自带它需要的规则和文档模板。作为 git 子模块挂到项目的 `.claude/skills` 即可直接使用，无需安装步骤。

项目基础来自 [cc-sdd](https://github.com/gotalab/cc-sdd) 3.1.0。

## 流程概览

```
新项目：需求梳理（discovery）── 第一轮需求分析；确认的产品、技术、结构决策由 steering 技能记入项目记忆
已有项目：项目记忆（steering，从代码初建）→ 需求梳理（discovery）
      ↓
规格：需求 → 设计 → 任务（每一阶段都有评审关和人工审批）
      ↓
实现：每个任务派实现子代理（测试先行）→ 独立评审 → 新鲜证据确认 → 提交
      ↓
功能级验收（GO / NO-GO）
```

## 目录结构

```
gsdd/                       ← 挂载后即项目的 .claude/skills/
  gsdd-*/                   各个技能，每个是一个目录
    SKILL.md                技能的流程说明
    rules/                  该技能用到的规则（如 EARS 需求写法、各评审关标准）
    templates/              该技能用到的文档模板（如 requirements / design / tasks）
  GSDD.md                   流程速查 + 中文输出指引，供项目 CLAUDE.md 导入
  README.md
  LICENSE
```

## 技能清单

| 类别 | 技能 | 作用 |
| --- | --- | --- |
| 项目记忆 | `gsdd-steering` | 维护项目级背景（产品、技术、结构）：从代码初建、与代码同步；`custom <专题>` 写专项文档（如测试、API 约定）；`record <决策>` 记录其他技能确认的决策 |
| 需求梳理 | `gsdd-discovery` | 判断需求的处理方式，必要时拆成多份规格并排好依赖顺序；新项目下即第一轮需求分析，确认的项目级决策交由 `gsdd-steering` 记录 |
| 规格 | `gsdd-spec-init` | 建立规格目录与状态文件 |
| | `gsdd-spec-requirements` | 生成 EARS 格式的需求 |
| | `gsdd-spec-design` | 生成技术设计与调研记录 |
| | `gsdd-spec-tasks` | 生成任务清单 |
| | `gsdd-spec-quick` | 一次跑完上面四步 |
| | `gsdd-spec-batch` | 按 roadmap 依赖顺序批量并行生成多份规格 |
| 实现 | `gsdd-impl` | 自动或手动实现任务 |
| | `gsdd-review` | 评审子代理遵循的评审规程 |
| | `gsdd-debug` | 排错子代理遵循的规程（先找根因） |
| | `gsdd-verify-completion` | 宣称完成前必须拿出新鲜证据 |
| 质量关口 | `gsdd-validate-gap` | 比对需求与现有代码 |
| | `gsdd-validate-design` | 设计评审 |
| | `gsdd-validate-impl` | 功能级验收 |
| 状态 | `gsdd-spec-status` | 查看规格进度 |

## 在项目中使用

**1. 添加子模块**（项目里不能已经存在 `.claude/skills` 目录）：

```bash
git submodule add https://github.com/gycherish/gsdd.git .claude/skills
```

别人克隆你的项目时，需要 `git clone --recurse-submodules`，或克隆后执行 `git submodule update --init`。

**2. 在项目根目录的 `CLAUDE.md` 里加一行导入**（推荐）：

```markdown
@.claude/skills/GSDD.md
```

它提供流程速查，便于平时的对话了解整套流程。技能本身自带语言和格式规则，不导入也能正确工作。

**3. 开始使用**，起步方式分两种：

- **新项目**：直接运行 `/gsdd-discovery <想法或需求文档>`。它做第一轮需求分析，过程中确认的产品、技术、结构决策会交由 `gsdd-steering` 写入项目记忆，不需要先单独建立项目记忆。写出代码后，可再运行 `/gsdd-steering` 让项目记忆与代码保持一致。
- **已有项目**：先运行 `/gsdd-steering` 从代码建立项目记忆，再用 `/gsdd-discovery` 或 `/gsdd-spec-init` 开始新需求。

之后的常用命令：

| 步骤 | 命令 |
| --- | --- |
| 判断需求怎么拆 | `/gsdd-discovery <想法>` |
| 单份规格逐步生成 | `/gsdd-spec-init` → `/gsdd-spec-requirements` → `/gsdd-spec-design` → `/gsdd-spec-tasks` |
| 单份规格一次生成 | `/gsdd-spec-quick <功能>` |
| 按 roadmap 批量生成 | `/gsdd-spec-batch` |
| 实现 | `/gsdd-impl <功能> [任务编号]` |
| 功能级验收 | `/gsdd-validate-impl <功能>` |
| 查看进度 | `/gsdd-spec-status <功能>` |

规格产出写在项目的 `docs/` 下：`docs/steering/` 存放项目记忆，只由 `gsdd-steering` 维护；`docs/specs/` 存放开发计划 `roadmap.md` 和各功能的规格（`docs/specs/<功能>/`）。它们属于项目自己的内容，应提交到项目仓库。

**4. 更新到 gsdd 最新版本**：

```bash
git submodule update --remote .claude/skills
git add .claude/skills
git commit -m "chore: update gsdd"
```

## 使用须知

- **子模块占用整个 `.claude/skills` 目录**。项目自己的其他技能不能再放在这里，可以放到个人目录 `~/.claude/skills/`。
- **`/gsdd-discovery` 和 `/gsdd-impl` 只能手动输入命令触发**：它们声明了禁止模型自动调用，不会出现在模型的可用技能列表里。
- **`/gsdd-impl` 会自动提交**：每完成一个任务提交一次，提交信息格式为 `feat(<功能名>): <任务描述>`，只暂存本任务改动的文件。
- **只适用于 Claude Code**。Codex 等工具读的是 `.agents/skills/`，技能头部格式也不同。

## 修改与扩展

| 想改的内容 | 改哪里 |
| --- | --- |
| 某个阶段的流程步骤 | `gsdd-*/SKILL.md` |
| 需求、设计、任务的文档格式 | 对应技能下的 `templates/` |
| EARS 写法、各评审关的判定标准 | 对应技能下的 `rules/` |
| 实现、评审、排错子代理的行为 | `gsdd-impl/templates/`、`gsdd-review/SKILL.md`、`gsdd-debug/SKILL.md` |
| 新增技能 | 新建一个含 `SKILL.md` 的目录 |
| 流程速查 | `GSDD.md` |
| 所有技能共用的语言、格式规则 | 各技能开头的 Language / Format precedence 区块 |

修改时注意：

- **技能开头的通用规则区块保持一致**：每个技能开头都有一段相同的 Language 和 Format precedence 规则，让技能单独调用时也能正确工作。修改时要在所有技能中一起替换。
- **每个技能保持自包含**：技能引用的规则、模板放在它自己的目录下，用"本技能目录下的 `rules/…`、`templates/…`"这种写法引用。同一份规则被多个技能使用时，各存一份，修改时一并更新。
- **子代理交接格式不能改名**：`gsdd-impl` 只认固定的报告块和字段，改名后父流程会解析失败、卡住。

  | 报告块 | 字段 |
  | --- | --- |
  | `## Status Report` | `- STATUS:` |
  | `## Review Verdict` | `- VERDICT:` |
  | `## Debug Report` | `- NEXT_ACTION:` |
  | `## Validation Report` | `- DECISION:` |

- **技能之间按名字引用**：`gsdd-spec-batch`、`gsdd-spec-quick`、`gsdd-impl` 通过技能名调用其他技能（如 `gsdd-spec-init`、`gsdd-review`）。改技能名时要同步修改这些引用。
- **影响大的技能建议禁止模型自动调用**：在头部声明 `disable-model-invocation: true`，只允许手动触发。
- **改完要实测**：技能是给模型读的提示词，效果只能靠实际运行验证。建议准备一个样例项目，挂上修改后的 gsdd，跑一个小功能走完全流程，对比修改前后的产出。

## 许可

gsdd 基于 cc-sdd 修改，按其 MIT 许可证要求保留原版权声明，见 [LICENSE](LICENSE)。
