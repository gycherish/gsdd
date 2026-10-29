# gsdd

gsdd 是一套用于 Claude Code 的规格驱动开发（SDD）流程：先把需求写成规格，再按规格设计、拆任务、实现和验收，规格是贯穿整个开发过程的唯一依据。

整套流程以技能（skill）的形式提供，每个技能自带它需要的规则和文档模板。作为 git 子模块挂到项目的 `.claude/skills` 即可直接使用，无需安装步骤。

项目基础来自 [cc-sdd](https://github.com/gotalab/cc-sdd) 3.1.0。

## 流程概览

```
项目记忆（steering）
      ↓
需求梳理（discovery）── 判断是扩展已有规格、直接实现、写一份规格，还是拆成多份
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
  kiro-*/                   各个技能，每个是一个目录
    SKILL.md                技能的流程说明
    rules/                  该技能用到的规则（如 EARS 需求写法、各评审关标准）
    templates/              该技能用到的文档模板（如 requirements / design / tasks）
  sdd.md                    流程速查 + 中文输出指引，供项目 CLAUDE.md 导入
  README.md
  LICENSE
```

## 技能清单

| 类别 | 技能 | 作用 |
| --- | --- | --- |
| 项目记忆 | `kiro-steering` | 建立或更新项目级背景（产品、技术、结构） |
| | `kiro-steering-custom` | 增加领域专项文档（API 规范、安全、测试等） |
| 需求梳理 | `kiro-discovery` | 判断需求的处理方式，必要时拆成多份规格并排好依赖顺序 |
| 规格 | `kiro-spec-init` | 建立规格目录与状态文件 |
| | `kiro-spec-requirements` | 生成 EARS 格式的需求 |
| | `kiro-spec-design` | 生成技术设计与调研记录 |
| | `kiro-spec-tasks` | 生成任务清单 |
| | `kiro-spec-quick` | 一次跑完上面四步 |
| | `kiro-spec-batch` | 按 roadmap 依赖顺序批量并行生成多份规格 |
| 实现 | `kiro-impl` | 自动或手动实现任务 |
| | `kiro-review` | 评审子代理遵循的评审规程 |
| | `kiro-debug` | 排错子代理遵循的规程（先找根因） |
| | `kiro-verify-completion` | 宣称完成前必须拿出新鲜证据 |
| 质量关口 | `kiro-validate-gap` | 比对需求与现有代码 |
| | `kiro-validate-design` | 设计评审 |
| | `kiro-validate-impl` | 功能级验收 |
| 状态 | `kiro-spec-status` | 查看规格进度 |

## 在项目中使用

**1. 添加子模块**（项目里不能已经存在 `.claude/skills` 目录）：

```bash
git submodule add https://github.com/gycherish/gsdd.git .claude/skills
```

别人克隆你的项目时，需要 `git clone --recurse-submodules`，或克隆后执行 `git submodule update --init`。

**2. 在项目根目录的 `CLAUDE.md` 里加一行导入**（推荐）：

```markdown
@.claude/skills/sdd.md
```

它提供流程速查，并指示 Claude 用简体中文回复、用规格设定的语言写规格文档。不导入也能用技能，只是少了这份背景。

**3. 开始使用**，常用顺序：

| 步骤 | 命令 |
| --- | --- |
| 建立项目记忆（可选） | `/kiro-steering` |
| 判断需求怎么拆 | `/kiro-discovery <想法>` |
| 单份规格逐步生成 | `/kiro-spec-init` → `/kiro-spec-requirements` → `/kiro-spec-design` → `/kiro-spec-tasks` |
| 单份规格一次生成 | `/kiro-spec-quick <功能>` |
| 按 roadmap 批量生成 | `/kiro-spec-batch` |
| 实现 | `/kiro-impl <功能> [任务编号]` |
| 功能级验收 | `/kiro-validate-impl <功能>` |
| 查看进度 | `/kiro-spec-status <功能>` |

规格产出写在项目的 `.kiro/` 下（`steering/`、`specs/<功能>/`），属于项目自己的内容，应提交到项目仓库。

**4. 更新到 gsdd 最新版本**：

```bash
git submodule update --remote .claude/skills
git add .claude/skills
git commit -m "chore: update gsdd"
```

## 使用须知

- **子模块占用整个 `.claude/skills` 目录**。项目自己的其他技能不能再放在这里，可以放到个人目录 `~/.claude/skills/`。
- **`/kiro-discovery` 和 `/kiro-impl` 只能手动输入命令触发**：它们声明了禁止模型自动调用，不会出现在模型的可用技能列表里。
- **`/kiro-impl` 会自动提交**：每完成一个任务提交一次，提交信息格式为 `feat(<功能名>): <任务描述>`，只暂存本任务改动的文件。
- **只适用于 Claude Code**。Codex 等工具读的是 `.agents/skills/`，技能头部格式也不同。

## 修改与扩展

| 想改的内容 | 改哪里 |
| --- | --- |
| 某个阶段的流程步骤 | `kiro-*/SKILL.md` |
| 需求、设计、任务的文档格式 | 对应技能下的 `templates/` |
| EARS 写法、各评审关的判定标准 | 对应技能下的 `rules/` |
| 实现、评审、排错子代理的行为 | `kiro-impl/templates/`、`kiro-review/SKILL.md`、`kiro-debug/SKILL.md` |
| 新增技能 | 新建一个含 `SKILL.md` 的目录 |
| 项目说明与输出语言 | `sdd.md` |

修改时注意：

- **每个技能保持自包含**：技能引用的规则、模板放在它自己的目录下，用"本技能目录下的 `rules/…`、`templates/…`"这种写法引用。同一份规则被多个技能使用时，各存一份，修改时一并更新。
- **子代理交接格式不能改名**：`kiro-impl` 只认固定的报告块和字段，改名后父流程会解析失败、卡住。

  | 报告块 | 字段 |
  | --- | --- |
  | `## Status Report` | `- STATUS:` |
  | `## Review Verdict` | `- VERDICT:` |
  | `## Debug Report` | `- NEXT_ACTION:` |
  | `## Validation Report` | `- DECISION:` |

- **注意技能之间的路径引用**：
  - `kiro-spec-batch` 写死了 `.claude/skills/<技能>/SKILL.md`。
  - `kiro-impl` 用 `../kiro-review/SKILL.md` 这类相对路径定位协议文件。
  - 改技能目录名时要同步修改这些引用，否则会悄悄断链。
- **影响大的技能建议禁止模型自动调用**：在头部声明 `disable-model-invocation: true`，只允许手动触发。
- **改完要实测**：技能是给模型读的提示词，效果只能靠实际运行验证。建议准备一个样例项目，挂上修改后的 gsdd，跑一个小功能走完全流程，对比修改前后的产出。

## 许可

gsdd 基于 cc-sdd 修改，按其 MIT 许可证要求保留原版权声明，见 [LICENSE](LICENSE)。
