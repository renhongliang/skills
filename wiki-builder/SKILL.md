---
name: wiki-builder
description: |
  知识库 Wiki 构建技能。将散乱的 Markdown 文件整理为结构化 Wiki 站点，支持全量和增量更新。
  
  触发场景：用户说"构建wiki"、"生成知识库"、"整理成wiki"、"wiki构建"等。
  
  核心能力：
  (1) 扫描源目录的 Markdown 和图片文件
  (2) 自动识别核心关键词和主题分类
  (3) 规划 Wiki 目录结构并确认
  (4) 基于源文件重新组织生成 Wiki 页面（非简单归类汇总）
  (5) 图片统一复制到各分类的 images/ 目录并重命名
  (6) 生成 Home.md 导航页和 changelog.md
  (7) 支持 Obsidian 双链语法
---

# Wiki Builder

将散乱的 Markdown 文件整理为结构化 Wiki 站点，支持全量和增量更新。

---

## 核心原则

1. **分类从内容来，不预设框架** — 先扫描分析，再根据实际内容确定分类
2. **重新组织而非简单归类** — 每个页面基于多个源文件重新整合生成，保持连贯性
3. **图片是核心知识载体** — 图片统一管理，重命名为有意义的名称
4. **增量友好** — 支持基于 changelog 的增量更新

---

## 完整工作流程

### Step 1：扫描源目录

扫描源目录，排除：
- 配置文件（package.json、.gitignore 等）
- 临时文件（*.tmp、*.bak 等）
- 隐藏文件（以 . 开头）
- 非 Markdown/图片格式的文件
- 以 . 开头的文件夹

**扫描命令**（PowerShell）：
```powershell
Get-ChildItem -Path "源目录" -Recurse -Include "*.md","*.png","*.jpg","*.jpeg","*.gif","*.webp","*.svg" | 
  Where-Object { $_.FullName -notmatch '\\wiki\\' -and $_.Name -notmatch '^\.' -and $_.DirectoryName -notmatch '\\\.' } | 
  Select-Object FullName, Length, LastWriteTime
```

### Step 2：分析内容，提取关键词

读取核心 Markdown 文件的前 30-50 行，分析：
- 文章主题和核心概念
- 技术栈/工具名称
- 领域分类

**输出**：核心关键词列表（如：AI Agent、Claude Code、OpenClaw、Harness、认知成长）

### Step 3：规划目录结构

根据关键词和内容分析，规划 Wiki 目录结构：

```
目标目录/
├── Home.md                    # 知识库首页导航
├── changelog.md               # 更新记录
├── 01-分类名称/
│   ├── images/               # 该分类的图片
│   ├── 页面1.md
│   └── 页面2.md
├── 02-分类名称/
│   ├── images/
│   └── ...
└── .obsidian/                # Obsidian 配置
```

**重要**：向用户展示目录结构，确认后再执行。

### Step 4：复制图片

为每个分类创建 `images/` 目录，从源目录复制相关图片并重命名：

```powershell
# 示例：复制并重命名图片
Copy-Item -Path "源图片路径" -Destination "目标wiki分类/images/新名称.png" -Force
```

**命名规则**：`{主题关键词}-{描述}.{扩展名}`
- 例：`claude-agent-loop.png`、`openclaw-memory-system.png`

### Step 5：生成 Wiki 页面

**核心原则**：页面是**重新组织**生成，不是简单搬运归类。

每个 Wiki 页面：
1. 读取相关的多个源文件
2. 提取核心内容，重新组织结构
3. 按照 `page_template.md` 格式生成
4. 使用 `write_file.py` 脚本写入（确保 UTF-8 编码）

**页面模板结构**：
```markdown
# 页面标题

> **一句话摘要**：...

---

## 核心要点

- **要点1**：...
- **要点2**：...
- **要点3**：...

---

## 详解

### 背景与动机
### 核心原理
### 示例

---

## 实践指南

---

## 优缺点与局限

### 优点
### 局限
### 与其他概念的关系

---

## 参考资源

---

## 关联条目

- [[相关页面A]]
- [[相关页面B]]

---

## 待补充

- [ ] 待验证的问题
- [ ] 待补充的案例
```

### Step 6：写入文件

**必须使用 `write_file.py` 脚本**写入，确保跨平台编码兼容：

```powershell
# 1. 先用 write 工具写入临时文件
# 2. 再用脚本写入目标文件

python "D:\Program Files\QClaw\resources\openclaw\config\skills\qclaw-text-file\scripts\write_file.py" `
  --path "目标路径.md" `
  --content-file "临时文件路径.md" `
  --encoding utf-8
```

### Step 7：生成首页和 changelog

**Home.md**：知识库导航首页，包含：
- 各分类的页面列表（使用 `[[双链]]` 语法）
- 知识库统计信息
- 快速搜索说明

**changelog.md**：更新记录，包含：
- Last-Update 时间戳
- 新增/修改/删除的文件列表
- 各分类的变更详情

### Step 8：创建 Obsidian 配置

```powershell
New-Item -ItemType Directory -Path "目标目录\.obsidian" -Force
# 创建 app.json 配置文件
```

---

## 增量更新流程

如果目标目录已存在且非空，执行增量更新：

1. 读取 `changelog.md` 获取上次更新时间
2. 扫描源目录，对比文件的 `LastWriteTime`
3. 列出新增/修改的文件
4. 向用户展示变更预览，确认后执行
5. 仅更新变更的页面和图片
6. 追加 changelog 记录

---

## 并行构建策略

对于大型知识库，使用子代理并行构建：

```
spawn 子代理1 → 处理分类 01-02
spawn 子代理2 → 处理分类 03-04
spawn 子代理3 → 处理分类 05-07
等待所有子代理完成
汇总结果，生成 Home.md 和 changelog.md
```

**子代理示例**：
```javascript
sessions_spawn({
  task: "构建分类 01-AI-Agent-系统 的 Wiki 页面...",
  label: "wiki-builder-agent",
  mode: "run",
  runTimeoutSeconds: 600
})
```

---

## 页面组织原则

### 单主题页面

一个 Wiki 页面围绕**一个核心概念**展开，整合多个相关源文件的内容。

例：`OpenClaw-深度解析.md` 整合了：
- 安装指南
- 记忆系统解析
- 技能系统解析
- 日志系统指南
- 多 Agent 协作指南

### 横向对比页面

当有多个同类主题时，额外生成一个横向对比页面。

例：`Agent-平台横向对比.md` 对比了：
- Claude Code
- OpenClaw
- CoPaw
- Molili

### 关联条目

每个页面底部添加 Obsidian 双链关联：

```markdown
## 关联条目

- [[相关页面A]]
- [[相关页面B]]
- [[相关页面C]]
```

---

## 图片处理规范

1. **位置**：每个分类独立的 `images/` 目录
2. **命名**：`{主题关键词}-{描述}.{扩展名}`
3. **引用**：使用相对路径 `images/xxx.png`
4. **格式**：优先保留原格式，必要时统一转为 PNG

---

## 脚本说明

| 脚本 | 作用 |
|------|------|
| `scripts/build_wiki.py` | 全量构建入口（可选） |
| `templates/page_template.md` | 页面标准模板 |

---

## 常见问题

### Q: 如何处理源文件中的图片引用？

A: 解析源文件中的图片引用路径，找到对应图片，复制到目标 `images/` 目录并重命名，然后更新 Wiki 页面中的引用路径。

### Q: 如何处理重复内容？

A: 多个源文件提到同一概念时，在 Wiki 页面中整合为一段完整阐述，避免重复。

### Q: 分类数量如何确定？

A: 根据内容主题自然聚类，通常 5-10 个分类为宜。太少则杂乱，太多则分散。

---

## 示例调用

```
用户：将 F:\web 的页面进行 wiki 本地知识库构建，目标地址是 F:\web\wiki

AI：
1. 扫描 F:\web 目录
2. 分析内容，识别关键词：AI Agent、Harness、认知成长...
3. 规划 7 个分类目录
4. [展示目录结构，等待确认]
5. 并行构建各分类页面
6. 生成 Home.md 和 changelog.md
7. 完成：23 篇页面 + 88 张图片
```
