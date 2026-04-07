# 📚 Skills Repository

> 优秀 AI Skills 合集仓库 - 收录高质量、可复用的 AI 技能模块

## 📖 简介

本仓库致力于收集、归档各类优秀的 AI Skills，每个 Skill 都是一个独立、可复用的能力模块，可用于增强 AI 助手的特定领域能力。

## 🎯 已收录 Skills

### 1. [wiki-builder](wiki-builder/)

**描述**：知识库 Wiki 构建技能

将散乱的 Markdown 文件整理为结构化 Wiki 站点，支持全量和增量更新。

**核心能力**：
- ✅ 自动扫描和分析 Markdown 文件
- ✅ 智能识别关键词和主题分类
- ✅ 规划 Wiki 目录结构
- ✅ 重新组织内容生成页面（非简单搬运）
- ✅ 图片统一管理和重命名
- ✅ 生成导航首页和更新日志
- ✅ 支持 Obsidian 双链语法
- ✅ 支持增量更新

**触发场景**：`构建wiki`、`生成知识库`、`整理成wiki`、`wiki构建`

---

## 🚀 使用方法

### 单个 Skill 使用

进入对应 Skill 目录，查看 `SKILL.md` 文件了解详细使用说明和工作流程。

```bash
cd wiki-builder/
cat SKILL.md
```

### 作为 AI 技能库

将本仓库作为 AI 助手的技能库，当用户触发对应场景时，AI 会自动加载相关 Skill 并执行工作流程。

## 📂 仓库结构

```
skills/
├── README.md              # 本文件
├── wiki-builder/          # Wiki 构建技能
│   ├── SKILL.md          # Skill 描述文件
│   └── templates/        # 模板文件
│       └── page_template.md
└── ...                   # 更多 Skills 待添加
```

## 🤝 贡献指南

欢迎贡献优秀的 Skills！请遵循以下规范：

### 添加新 Skill

1. 在仓库根目录下创建新的 Skill 目录
2. 创建 `SKILL.md` 文件，包含以下内容：
   - YAML frontmatter（name、description）
   - Skill 详细描述
   - 工作流程
   - 使用示例

3. 如有需要，创建 `templates/` 目录存放模板文件

### SKILL.md 模板

```markdown
---
name: skill-name
description: |
  Skill 的简短描述
  
  触发场景：xxx
---

# Skill Name

详细说明...

## 工作流程

### Step 1: ...

## 示例调用

...
```

## 📝 规范说明

- 每个 Skill 必须是独立可复用的能力模块
- `SKILL.md` 应包含完整的使用说明和工作流程
- 使用 UTF-8 编码
- 保持清晰的目录结构
- 提供实际可用的示例

## 🔮 未来规划

- [ ] 添加更多优秀 Skills
- [ ] 建立 Skill 分类体系
- [ ] 提供 Skill 组合使用方案
- [ ] 自动化测试框架
- [ ] Skill 版本管理

## 📄 许可证

[MIT License](LICENSE)

## ⭐ 支持项目

如果这个仓库对你有帮助，欢迎 Star 和分享！

---

**注意**：本仓库持续更新中，欢迎贡献优秀的 Skills！
