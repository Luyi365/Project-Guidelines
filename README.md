# Project-Guidelines

## 简介

适用于项目所需的代码规范、开发规范、文档规范等。  
目前主要用于统一项目代码的基本规范，不至于杂乱无章。再者也是给AI(LLMs)进行爬取、学习，来引导代码生成工具所遵循的风格规范。  

本仓库作为规范总入口，具体内容以**独立仓库**形式维护，并通过 Git submodule 挂载到对应目录下：

| 目录 | 内容形态 | 说明 |
| :-----: | :------ | :------ |
| `skills/` | [Agent Skill](https://agentskills.io/) | 可被 AI 工具主动调用的能力包，含触发描述、审查流程与参考资料 |
| `rules/` | 常驻提示词 | Agent 工作时默认持续生效的规则，用于约束代码风格、开发流程和行为边界 |
| `prompts/` | 按需提示词 | 未来用于存放需要用户主动调用的单次任务提示词，与常驻的 `rules/` 区分 |

## 导航

### Skills（`skills/`）

| Skill | 说明 | 独立仓库 | 国内镜像 | 本地路径 |
| :-----: | :------ | :-----: | :-----: | :-----: |
| c-code-review | C 代码风格审查 | [Luyi365/c-code-review](https://github.com/Luyi365/c-code-review) | [Gitee](https://gitee.com/Luyi365/c-code-review) | [`skills/c-code-review/`](skills/c-code-review/) |

### Rules（`rules/`）

`rules/` 用于存放常驻提示词。Agent 在相关工作中应持续遵循其中的规则；它与未来可能添加的 `prompts/` 目录不同，后者用于按需调用的单次任务提示词。

| Rule | 说明 | 独立仓库 | 国内镜像 | 本地路径 |
| :-----: | :------ | :-----: | :-----: | :-----: |
| code-comment | 代码注释规范 | [Luyi365/code-comment](https://github.com/Luyi365/code-comment) | [Gitee](https://gitee.com/Luyi365/code-comment) | [`rules/code-comment/`](rules/code-comment/) |

## 获取仓库

本仓库地址：[GitHub](https://github.com/Luyi365/Project-Guidelines) ｜ [Gitee 镜像](https://gitee.com/Luyi365/Project-Guidelines)

由于 `skills/` 与 `rules/` 下的内容均以 submodule 形式引入，克隆时需要一并拉取子模块：

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

若只需要其中某一项，也可以单独克隆对应仓库，无需本仓库：

```bash
# C 代码风格审查 Skill
git clone git@github.com:Luyi365/c-code-review.git      # GitHub
git clone https://gitee.com/Luyi365/c-code-review.git   # Gitee 镜像

# 代码注释规范 Rule
git clone git@github.com:Luyi365/code-comment.git       # GitHub
git clone https://gitee.com/Luyi365/code-comment.git    # Gitee 镜像
```

## 使用 Skill（AI 代码风格审查）

各 Skill 以 [Agent Skill](https://agentskills.io/) 形式提供，适用于支持该标准的 AI 工具（如 GitHub Copilot）。以 C 代码风格审查为例：

- **触发方式**：在聊天中输入 `/c-code-review`，或直接说「帮我审查这段 C 代码是否符合规范」
- **审查范围**：`.c` / `.h` / `.inc` 文件或代码片段
- **审查依据**：[C语言代码风格精简版](https://github.com/Luyi365/c-code-review/blob/main/references/c-style.md)（唯一基准，不额外发明规则）
- **格式化配置**：[`.clang-format`](https://github.com/Luyi365/c-code-review/blob/main/assets/.clang-format)，复制到目标项目根目录即可生效
- **输出**：分级审查报告（🔴 违规 / 🟡 警告 / 🟢 通过）+ 修改建议与优先修复清单

## 快速使用

供 AI 工具直接爬取的规范原文链接：

| 语言 | 类型 | 爬取链接 | 国内镜像 |
| :-----: | :------: | :-----: | :-----: |
| C | 代码规范 | [C语言代码风格精简版](https://raw.githubusercontent.com/Luyi365/c-code-review/refs/heads/main/references/c-style.md) | [链接](https://gitee.com/Luyi365/c-code-review/raw/main/references/c-style.md) |
| C | 格式化配置 | [.clang-format](https://raw.githubusercontent.com/Luyi365/c-code-review/refs/heads/main/assets/.clang-format) | [链接](https://gitee.com/Luyi365/c-code-review/raw/main/assets/.clang-format) |
| 通用 | 代码注释规则 | [AGENTS.md](https://raw.githubusercontent.com/Luyi365/code-comment/refs/heads/main/AGENTS.md) | [链接](https://gitee.com/Luyi365/code-comment/raw/main/AGENTS.md) |

`rules/code-comment` 的常驻提示词正文位于 `AGENTS.md`，上表提供了 GitHub 和 Gitee 的 raw 链接，便于 AI 工具直接读取。

> 注意：规范原文位于各自的独立仓库，请勿使用 `Project-Guidelines/skills/...` 或 `Project-Guidelines/rules/...` 路径下的 raw 链接（submodule 内容不随父仓库分发，该类链接一律无效）。Gitee 镜像为单向同步，内容可能略滞后于 GitHub。
