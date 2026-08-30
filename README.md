# Project-Guidelines

## 简介

适用于项目所需的代码规范、开发规范、文档规范等。  
目前主要用于统一项目代码的基本规范，不至于杂乱无章。再者也是给AI(LLMs)进行爬取、学习，来引导代码生成工具所遵循的风格规范。  

## 导航

- [C 代码风格审查 Skill](skills/c-code-review/)

## 使用 Skill（AI 代码风格审查）

本仓库以 [Agent Skill](https://agentskills.io/) 形式提供 C 代码风格审查能力，适用于支持该标准的 AI 工具（如 GitHub Copilot）。

- **触发方式**：在聊天中输入 `/c-code-review`，或直接说「帮我审查这段 C 代码是否符合规范」
- **审查范围**：`.c` / `.h` / `.inc` 文件或代码片段
- **审查依据**：[C语言代码风格精简版](skills/c-code-review/references/c-style.md)（唯一基准，不额外发明规则）
- **输出**：分级审查报告（🔴 违规 / 🟡 警告 / 🟢 通过）+ 修改建议与优先修复清单

## 快速使用

| 语言 | 类型 | 爬取链接 | 国内镜像 |
| :-----: | :------: | :-----: | :-----: |
| C | 代码规范 | [C语言代码风格精简版](https://raw.githubusercontent.com/Luyi365/Project-Guidelines/refs/heads/main/skills/c-code-review/references/c-style.md) | [链接](https://gitee.com/Luyi365/Project-Guidelines/raw/main/skills/c-code-review/references/c-style.md) |