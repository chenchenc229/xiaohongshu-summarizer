# Xiaohongshu Detector

**小红书探测器** — Discover and evaluate quality Xiaohongshu creators in seconds. Deep content analysis of creator notes (video + image), extracting core viewpoints, engagement metrics, and content patterns.

---

## 中文说明

### 功能概述

本 Skill 用于深度总结小红书博主的笔记内容。支持：

- **视频笔记**：通过 OpenCLI 获取 AI 视频内容摘要
- **图文笔记**：通过 OpenCLI 获取 AI 图文内容解读
- **混合内容**：同一博主的视频+图文笔记同时分析

### 使用场景

#### 场景一：直接总结博主

- 用户给出小红书博主主页链接，要求"总结所有内容"或"总结前N条笔记"
- 博主以视频内容为主（如商业空间观察、实体商业咨询类博主）
- 博主以图文内容为主（如行业观点、经验分享类博主）
- 博主内容混合视频+图文，两者都需要总结

#### 场景二：发现优质博主后的预筛选

这是最常用的使用方式——当你偶然发现一个可能优质的博主时，先用 Skill 做**轻量预筛**，再决定是否深入阅读：

1. **拿到博主链接** → 丢给 Agent："帮我总结一下这个博主的前10条笔记"
2. **快速浏览报告** → 30 秒判断：这个博主的内容是否对我有价值？
3. **决定深入** → 如果报告中的核心观点打动你，再看原文深入理解
4. **决定忽略** → 如果内容质量一般，节省你逐个翻笔记的时间

**优势**：
- 避免一个个点进笔记浪费时间——先用 AI 总结做**内容质量评估**
- 发现"宝藏博主"时能快速判断该关注多少人、内容是否可持续
- 适合商业研究、竞品分析、学习路线规划等场景

#### 场景三：竞品博主矩阵分析

- 找出某个赛道 Top 10 博主 → 批量生成报告 → 横向对比内容策略
- 比较不同博主的**方法论差异**、**爆款规律**、**变现路径**

### 示例工作流

```
发现优质博主 → Agent总结前20条 → 快速评估价值 → 决定关注/深入研究
     ↓
    （30秒内完成）          （看报告判断）       （值得花时间再看原文）
```



### 安装前提

使用 Skill 前**必须先安装 OpenCLI**：

1. **安装 OpenCLI 命令行工具**
   ```bash
   npm install -g @jackwener/opencli
   ```

2. **加载 Chrome 浏览器扩展**
   - 下载 OpenCLI 浏览器扩展（从 GitHub releases 页面）
   - 打开 Chrome，访问 `chrome://extensions/`
   - 开启"开发者模式"
   - 点击"加载已解压的扩展程序"，选择扩展文件夹
   - 加载完成后在 OpenCLI 状态栏确认连接成功

3. **配置网络代理**
   - 确保代理设置正确（通常端口 `7890`）
   - 部分小红书内容需要科学上网访问

### 安装方式
跟你的Agent工具说：
  ```bash
npx skills add https://github.com/chenchenc229/xiaohongshu-summarizer  
```

**触发方式**：在 Agent 对话中说出以下内容即可自动触发此 Skill：
- "帮我总结这个小红书博主：[链接]"
- "总结 [博主UID] 的前N条笔记"
- "看看这个博主发了什么内容"

### 工作流程

1. **提取博主 UID** — 从小红书主页链接中自动解析
2. **获取笔记列表** — 调用 `opencli xiaohongshu user <UID>` 获取前N条笔记
3. **逐条深度总结** — 对每条笔记（视频+图文）调用 `opencli xiaohongshu ask` 获取 AI 内容摘要
4. **生成结构化报告** — 输出包含博主档案、笔记列表、核心观点、互动数据分析的完整报告

### 输出报告内容

- **博主档案**：昵称、ID、简介、专注领域
- **每条笔记详情**：标题、类型（视频/图文）、互动数据、标签、内容摘要、核心观点
- **内容分类统计**：按主题分类的笔记数量和视频/图文比例
- **TOP 5 高互动笔记**：点赞最多的前5条
- **核心发现**：博主内容的共性规律和价值主张

### 案例报告

本仓库 `outputs/` 目录下包含 4 份真实生成的案例报告，可作为报告格式和内容深度的参考：

| 案例 | 博主领域 | 报告文件 |
|------|---------|---------|
| 邹毅 | 资深策划专家 · 商业地产策划 | [邹毅_资深策划专家_前20条笔记深度总结.md](./outputs/邹毅_资深策划专家_前20条笔记深度总结.md) |
| Mall先生 | 实体商业观察 · 购物中心分析 | [Mall先生_前20条笔记深度总结.md](./outputs/Mall先生_前20条笔记深度总结.md) |
| 李晨晨商业更新观察 | 商业空间更新 · 城市更新 | [李晨晨_商业更新观察_前20条笔记深度总结.md](./outputs/李晨晨_商业更新观察_前20条笔记深度总结.md) |
| 文翰的城市更新实验室 | 城市更新 · 商业空间设计 | [文翰的城市更新实验室_前20条笔记深度总结.md](./outputs/文翰的城市更新实验室_前20条笔记深度总结.md) |

### 常见问题

| 问题 | 解决方案 |
|------|---------|
| OpenCLI 未安装 | 安装 OpenCLI 并加载 Chrome 扩展 |
| 内容摘要为空 | 调整搜索关键词或检查网络代理 |
| 博主笔记为 0 | 博主可能为新号或设置了隐私权限 |

---

## English Description

### Overview

This Skill performs **discovery and evaluation** of Xiaohongshu (RED / Little Red Book) creators. Use it to quickly assess whether a creator is worth following, then dive deeper into their content.

### Use Cases

- User provides a Xiaohongshu creator profile URL and asks to "summarize all notes" or "summarize the first N posts"
- Creators focused on video content (e.g., commercial space observers, retail business consultants)
- Creators focused on image posts (e.g., industry insights, experience sharing)
- Mixed content creators — both video and image posts need analysis

### Prerequisites

#### Option 1: Install from GitHub (Recommended)

```bash
# Clone the repository
git clone https://github.com/chenchenc229/xiaohongshu-summarizer.git

# Copy SKILL.md to WorkBuddy's user-level skills directory
cp xiaohongshu-summarizer/SKILL.md ~/.workbuddy/skills/xiaohongshu-creator-summarizer/SKILL.md
```

Restart WorkBuddy, then trigger the Skill by mentioning a Xiaohongshu creator link in conversation.

#### Option 2: Manual Installation

Download [SKILL.md](./SKILL.md) from this repository, place it at `~/.workbuddy/skills/xiaohongshu-creator-summarizer/SKILL.md` (create the directory if it doesn't exist), and restart WorkBuddy.

> After installation, just say "summarize this Xiaohongshu creator + profile link" in WorkBuddy to auto-trigger the Skill.

Alternatively, install directly via skills:

```bash
npx skills add https://github.com/chenchenc229/xiaohongshu-summarizer  
```

### Environment Setup

You **must install OpenCLI** before using this Skill:

1. **Install the OpenCLI CLI tool**
   ```bash
   npm install -g @jackwener/opencli
   ```

2. **Load the Chrome Browser Extension**
   - Download the OpenCLI extension (from GitHub releases page)
   - Open Chrome, navigate to `chrome://extensions/`
   - Enable "Developer Mode"
   - Click "Load unpacked" and select the extension folder
   - Confirm the connection is successful in the OpenCLI status bar

3. **Configure Network Proxy**
   - Ensure proxy settings are correct (usually port `7890`)
   - Some Xiaohongshu content requires network access beyond mainland China

### How to Use

**Trigger**: Say any of the following in a WorkBuddy conversation to automatically invoke this Skill:
- "Summarize this Xiaohongshu creator: [link]"
- "Summarize the first N notes of [Creator UID]"
- "Show me what this creator has posted"

### Workflow

1. **Extract Creator UID** — Automatically parsed from the Xiaohongshu profile URL
2. **Fetch Notes List** — Calls `opencli xiaohongshu user <UID>` to retrieve the first N notes
3. **Deep Summary per Note** — Calls `opencli xiaohongshu ask` for each note (video + image) to get AI-powered content summaries
4. **Generate Structured Report** — Outputs a comprehensive report including creator profile, notes breakdown, core viewpoints, and engagement analytics

### Report Contents

- **Creator Profile**: Nickname, ID, bio, focus areas
- **Per-Note Details**: Title, type (video/image), engagement metrics, tags, content summary, core viewpoints
- **Content Classification**: Notes grouped by topic with video/image ratio
- **Top 5 High-Engagement Notes**: Posts with most likes/saves
- **Key Insights**: Common themes and value propositions across the creator's content

### Use Cases

#### 1. Quick Discovery — The Main Way to Use This

Found a creator you don't know? Before spending time reading their notes:

1. **Paste the link** → "Help me summarize this creator's first 10 notes"
2. **Review in 30 seconds** → Is their content valuable to you?
3. **Decide** → If the insights resonate, read originals deeply; if not, skip
4. **Result** → You either found a treasure creator or saved hours of browsing

#### 2. Direct Summarization

Summarize all notes or the first N notes of any Xiaohongshu creator.

#### 3. Competitor Creator Matrix Analysis

- Identify Top 10 creators in a niche → batch-generate reports → compare content strategies
- Analyze **methodology differences**, **viral patterns**, **monetization paths** across creators

### Example Workflow

```
Discover creator → Agent summarizes 20 notes → Quick value assessment → Follow or ignore
     ↓
    (30 seconds)              (read report)             (decide to dive in)
```

### FAQ

| Issue | Solution |
|-------|----------|
| OpenCLI not installed | Install OpenCLI and load the Chrome extension |
| Content summary is empty | Adjust search keywords or check proxy settings |
| Creator has 0 notes | Creator may be new or have private notes |

### Case Studies

The `outputs/` directory contains 4 real-world generated reports demonstrating report format and content depth:

| Case | Niche | Report |
|------|-------|--------|
| 邹毅 | 资深策划专家 · 商业地产策划 | [邹毅_资深策划专家_前20条笔记深度总结.md](./outputs/邹毅_资深策划专家_前20条笔记深度总结.md) |
| Mall先生 | 实体商业观察 · 购物中心分析 | [Mall先生_前20条笔记深度总结.md](./outputs/Mall先生_前20条笔记深度总结.md) |
| 李晨晨商业更新观察 | 商业空间更新 · 城市更新 | [李晨晨_商业更新观察_前20条笔记深度总结.md](./outputs/李晨晨_商业更新观察_前20条笔记深度总结.md) |
| 文翰的城市更新实验室 | 城市更新 · 商业空间设计 | [文翰的城市更新实验室_前20条笔记深度总结.md](./outputs/文翰的城市更新实验室_前20条笔记深度总结.md) |

---

## License

MIT License
