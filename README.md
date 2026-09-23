# Project-Guidelines

## 简介

本仓库收录项目通用的代码规范、开发规范与文档规范。

一方面用于统一各个项目的基本约定，避免同类问题各写一套；另一方面这些规范以纯文本维护，便于 AI（LLMs）抓取和学习，用来约束代码生成工具的产出风格。

本仓库作为规范总入口，具体内容以**独立仓库**形式维护，并通过 Git submodule 挂载到对应目录下：

| 目录 | 内容形态 | 说明 |
| :-----: | :------ | :------ |
| `skills/` | [Agent Skill](https://agentskills.io/) | 可被 AI 工具主动调用的能力包，含触发描述、审查流程与参考资料 |
| `rules/` | 常驻提示词 | Agent 工作时默认持续生效的规则，用于约束代码风格、开发流程和行为边界 |
| `prompts/` | 按需提示词 | 需用户主动调用的单次任务提示词（斜杠命令），执行完即结束，与常驻的 `rules/` 区分。目前为预留目录，暂无内容 |

## 导航

### Skills（`skills/`）

| Skill | 说明 | 独立仓库 | 国内镜像 | 本地路径 |
| :-----: | :------ | :-----: | :-----: | :-----: |
| c-code-review | C 代码风格审查 | [Luyi365/c-code-review](https://github.com/Luyi365/c-code-review) | [Gitee](https://gitee.com/Luyi365/c-code-review) | [`skills/c-code-review/`](skills/c-code-review/) |
| git-doctor | Git 配置诊断与修正 | [Luyi365/git-doctor](https://github.com/Luyi365/git-doctor) | [Gitee](https://gitee.com/Luyi365/git-doctor) | [`skills/git-doctor/`](skills/git-doctor/) |
| editorconfig-doctor | EditorConfig 配置诊断与修正 | [Luyi365/editorconfig-doctor](https://github.com/Luyi365/editorconfig-doctor) | [Gitee](https://gitee.com/Luyi365/editorconfig-doctor) | [`skills/editorconfig-doctor/`](skills/editorconfig-doctor/) |

### Rules（`rules/`）

`rules/` 用于存放常驻提示词。Agent 在相关工作中应持续遵循其中的规则；它与 `prompts/` 不同，后者是需要主动调用的单次任务提示词。

| Rule | 说明 | 独立仓库 | 国内镜像 | 本地路径 |
| :-----: | :------ | :-----: | :-----: | :-----: |
| code-comment | 代码注释规范 | [Luyi365/code-comment](https://github.com/Luyi365/code-comment) | [Gitee](https://gitee.com/Luyi365/code-comment) | [`rules/code-comment/`](rules/code-comment/) |

### Prompts（`prompts/`）

按需提示词集中在一个仓库内，每个 `.prompt.md` 文件对应一个可调用的单次任务：[Luyi365/prompts](https://github.com/Luyi365/prompts) ｜ [Gitee 镜像](https://gitee.com/Luyi365/prompts) ｜ 本地路径 [`prompts/`](prompts/)

目前暂无内容。原先的 Git 配置诊断提示词已转为 Skill，见上方 `git-doctor`。

## 组合使用原则

各已发布的规范子仓库都包含完成自身职责所需的规则与资源，可以单独克隆使用；组合使用时只在配置交界处对齐，不把另一个子模块作为运行前置：

- `c-code-review`负责 C 风格合规，`c-style.md`是唯一评分基准；`.clang-format`只处理可机械化部分，冲突时以规范正文为准。
- `git-doctor`负责 Git 子模块与仓库行尾，`.gitattributes`决定版本库实际保存的行尾。
- `editorconfig-doctor`负责编辑器通用行为；项目存在`.gitattributes`或格式化工具配置时与其对齐，不存在时仍可独立建立基线。
- `code-comment`负责注释内容质量，不改变语言专属规范的语法与评分标准。

发生交叉冲突时，统一按「项目自身强制规范与 CI → 语义性要求 → 专用工具配置 → 通用基线」处理。换言之：`c-style.md`高于`.clang-format`，`.gitattributes`的行尾规则高于`.editorconfig`，项目自己的明确约定高于各 Skill 的默认值。

## 获取仓库

本仓库地址：[GitHub](https://github.com/Luyi365/Project-Guidelines) ｜ [Gitee 镜像](https://gitee.com/Luyi365/Project-Guidelines)

由于 `skills/`、`rules/` 与 `prompts/` 的内容均以 submodule 形式引入，克隆时需要一并拉取子模块：

```bash
# GitHub
git clone --recurse-submodules git@github.com:Luyi365/Project-Guidelines.git

# Gitee 镜像（国内网络）
git clone --recurse-submodules https://gitee.com/Luyi365/Project-Guidelines.git

# 已克隆过则补拉
git submodule update --init --recursive

# 更新子模块到各自远端最新
git submodule update --remote --merge
```

`.gitmodules` 中的子模块统一使用**相对地址**（`../<仓库名>.git`），会按父仓库的 remote 自动解析：从 GitHub 克隆则走 GitHub，从 Gitee 克隆则走 Gitee，两条链路都无需切换配置。新增子模块时请沿用这一写法。

若只需要其中某一项，也可以单独克隆对应仓库，无需本仓库。仓库名见上方导航表：

```bash
git clone git@github.com:Luyi365/<仓库名>.git        # GitHub
git clone https://gitee.com/Luyi365/<仓库名>.git     # Gitee 镜像
```

## 使用 Skill

各 Skill 以 [Agent Skill](https://agentskills.io/) 形式提供，适用于支持该标准的 AI 工具（如 GitHub Copilot）。把 Skill 目录放进工具约定的 skills 位置后，输入 `/` 加 Skill 名即可调用；也可以直接用自然语言描述需求，工具会按 `SKILL.md` 里的触发描述自动匹配。

### c-code-review

- **触发**：`/c-code-review`，或「帮我审查这段 C 代码是否符合规范」
- **范围**：`.c` / `.h` / `.inc` 文件或代码片段
- **审查依据**：[C语言代码风格精简版](https://github.com/Luyi365/c-code-review/blob/main/references/c-style.md)（唯一基准，不额外发明规则）
- **附带资源**：[`.clang-format`](https://github.com/Luyi365/c-code-review/blob/main/assets/.clang-format) 提供规范中可机械化部分的配置基线；`switch`、`while(1)`、`inline`、元方法及`case`整体条件等不可完整表达的规则仍需按`c-style.md`复核
- **输出**：分级审查报告（🔴 违规 / 🟡 警告 / 🟢 通过）+ 修改建议与优先修复清单

### git-doctor

- **触发**：`/git-doctor`，或「检查一下这个仓库的 Git 配置」
- **范围**：`.gitmodules` 的子模块配置、`.gitattributes` 的行尾配置
- **行为**：未指定范围时做全量体检；也可只查单项，或按规范接入新子模块。可安全修正的当场修正，带取舍或涉及远端的只警告并给出建议命令
- **附带资源**：[`.gitattributes`](https://github.com/Luyi365/git-doctor/blob/main/assets/.gitattributes) 基线配置，复制到目标项目根目录即可生效
- **输出**：已修正 / 需人工处理 / 通过 三段汇总 + 优先处理清单

### editorconfig-doctor

- **触发**：`/editorconfig-doctor`，或「检查一下这个项目的 EditorConfig」
- **范围**：`.editorconfig` 的根声明、缩进、字符集、空白处理、文件类型覆盖，以及与项目既有版本控制和格式化配置的一致性
- **行为**：已有配置时执行体检，没有时按项目实际文件类型建立基线；其他配置存在时对齐，不存在时不阻塞，也不代为创建。可安全修正的配置项当场修正，批量重排既有文件等操作须用户确认
- **附带资源**：[`.editorconfig`](https://github.com/Luyi365/editorconfig-doctor/blob/main/assets/.editorconfig) 基线配置，可裁剪后放到目标项目根目录
- **输出**：已修正 / 需人工处理 / 通过 三段汇总 + 优先处理清单

## 快速使用

供 AI 工具直接爬取的规范原文链接：

| 语言 | 类型 | 爬取链接 | 国内镜像 |
| :-----: | :------: | :-----: | :-----: |
| C | 代码规范 | [C语言代码风格精简版](https://raw.githubusercontent.com/Luyi365/c-code-review/refs/heads/main/references/c-style.md) | [链接](https://gitee.com/Luyi365/c-code-review/raw/main/references/c-style.md) |
| C | 格式化配置 | [.clang-format](https://raw.githubusercontent.com/Luyi365/c-code-review/refs/heads/main/assets/.clang-format) | [链接](https://gitee.com/Luyi365/c-code-review/raw/main/assets/.clang-format) |
| 通用 | 代码注释规则 | [AGENTS.md](https://raw.githubusercontent.com/Luyi365/code-comment/refs/heads/main/AGENTS.md) | [链接](https://gitee.com/Luyi365/code-comment/raw/main/AGENTS.md) |
| Git | 配置诊断规则 | [SKILL.md](https://raw.githubusercontent.com/Luyi365/git-doctor/refs/heads/main/SKILL.md) | [链接](https://gitee.com/Luyi365/git-doctor/raw/main/SKILL.md) |
| Git | 行尾基线配置 | [.gitattributes](https://raw.githubusercontent.com/Luyi365/git-doctor/refs/heads/main/assets/.gitattributes) | [链接](https://gitee.com/Luyi365/git-doctor/raw/main/assets/.gitattributes) |
| 通用 | EditorConfig 诊断规则 | [SKILL.md](https://raw.githubusercontent.com/Luyi365/editorconfig-doctor/refs/heads/main/SKILL.md) | [链接](https://gitee.com/Luyi365/editorconfig-doctor/raw/main/SKILL.md) |
| 通用 | EditorConfig 基线配置 | [.editorconfig](https://raw.githubusercontent.com/Luyi365/editorconfig-doctor/refs/heads/main/assets/.editorconfig) | [链接](https://gitee.com/Luyi365/editorconfig-doctor/raw/main/assets/.editorconfig) |

`rules/code-comment` 的常驻提示词正文位于 `AGENTS.md`，Skill 的规则正文位于各自的 `SKILL.md`，上表提供了 GitHub 和 Gitee 的 raw 链接，便于 AI 工具直接读取。

> 注意：规范原文位于各自的独立仓库，请勿使用 `Project-Guidelines/skills/...`、`Project-Guidelines/rules/...` 或 `Project-Guidelines/prompts/...` 路径下的 raw 链接（submodule 内容不随父仓库分发，该类链接一律无效）。Gitee 镜像为单向同步，内容可能略滞后于 GitHub。
