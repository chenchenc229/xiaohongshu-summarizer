# Xiaohongshu Detector

**小红书探测器** — Discover and evaluate quality Xiaohongshu creators in seconds. Deep content analysis of creator notes (video + image), extracting core viewpoints, engagement metrics, and content patterns.

---

## 中文说明

### 功能概述

本 Skill 用于深度总结小红书博主的笔记内容。支持：

- **视频笔记**：通过 OpenCLI 获取 AI 视频内容摘要
- **图文笔记**：通过 OpenCLI 获取 AI 图文内容解读
- **混合内容**：同一博主的视频+图文笔记同时分析
- **进度追踪**：实时记录每条笔记的获取状态，成功率低于阈值时主动提醒
- **多关键词搜索**：每条笔记自动尝试多组关键词，最大化获取率
- **批量搜索优化**：5-8条笔记合并为一次搜索，节省 30-50% 的 API 调用
- **时间线分析**：自动梳理博主的内容演变脉络
- **跨笔记关联**：识别同主题笔记之间的方法论递进关系

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

### 进阶用法

#### 批量分析多个博主

当需要调研某个行业的头部博主时：

```bash
# 依次分析多个博主
# 例：分析商业空间领域的 Top 5 博主
1. 建筑师黄伟 → 2. 文翰的城市更新实验室 → 3. 邹毅 → 4. Mall先生 → 5. 李晨晨
```

每个博主生成一份独立报告，放在 `outputs/` 目录下，可横向对比。

#### 从单条笔记反向查找博主

如果用户只提供了单条笔记链接（而非博主主页链接）：

1. 用 AI 搜索提取博主名
2. 基于博主名多角度搜索其他笔记
3. 逐条获取详细内容
4. 自动生成报告（标注数据来源）

### 安装方式

#### 方式一：从 GitHub 仓库安装（推荐）

```bash
# 克隆仓库到本地
git clone https://github.com/chenchenc229/xiaohongshu-summarizer.git

# 将 SKILL.md 复制到 WorkBuddy 用户级 skills 目录
cp xiaohongshu-summarizer/SKILL.md ~/.workbuddy/skills/xiaohongshu-detector/SKILL.md
```

安装完成后，重启 WorkBuddy 或在对话中触发关键词即可使用。

#### 方式二：直接复制 SKILL.md

从本仓库下载 [SKILL.md](./SKILL.md)，将其放入 `~/.workbuddy/skills/xiaohongshu-detector/` 目录下（目录不存在则手动创建），然后重启 WorkBuddy。

> 安装此 Skill 后，在 WorkBuddy 对话中输入"总结小红书博主 + 主页链接"即可自动触发。

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

### 使用方法

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

- **博主档案**：昵称、ID、简介、专注领域、粉丝数
- **每条笔记详情**：标题、类型（视频/图文）、互动数据、标签、内容摘要、核心观点
- **内容分类统计**：按主题分类的笔记数量和视频/图文比例
- **TOP 5 高互动笔记**：点赞最多的前5条
- **核心发现**：博主内容的共性规律和价值主张
- **内容演变时间线**：按时间顺序梳理博主话题变化
- **跨笔记关联分析**：识别同主题笔记之间的方法论递进关系
- **详细解读覆盖率**：标注成功获取/部分获取/无法获取的笔记数量

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
| 批量搜索结果混乱 | 退回单条搜索，逐条获取 |
| 成功率 < 50% | 检查网络连接，确认代理设置，重试 3 次后仍失败则告知用户 |

---

## English Description

### Overview

This Skill performs **discovery and evaluation** of Xiaohongshu (RED / Little Red Book) creators. Use it to quickly assess whether a creator is worth following, then dive deeper into their content.

**Features**:
- Deep content analysis of video + image notes
- Progress tracking with success rate monitoring
- Multi-keyword search strategy for maximum coverage
- Batch search optimization (saves 30-50% API calls)
- Timeline analysis + cross-note correlation + methodology extraction

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

### Advanced Usage

#### Batch Analysis of Multiple Creators

When researching top creators in a specific industry:

```bash
# Analyze Top 5 creators in a niche one by one
# Example: commercial space领域
1. 建筑师黄伟 → 2. 文翰的城市更新实验室 → 3. 邹毅 → 4. Mall先生 → 5. 李晨晨
```

Each creator generates an independent report in the `outputs/` directory for cross-comparison.

#### Reverse Lookup from Single Note Link

If you only have a single note link (not a creator profile URL):

1. Use AI search to extract the creator name
2. Search for other notes based on the creator name
3. Get detailed content for each note
4. Auto-generate report (with data source annotations)

### Prerequisites

#### Option 1: Install from GitHub (Recommended)

```bash
# Clone the repository
git clone https://github.com/chenchenc229/xiaohongshu-summarizer.git

# Copy SKILL.md to WorkBuddy's user-level skills directory
cp xiaohongshu-summarizer/SKILL.md ~/.workbuddy/skills/xiaohongshu-detector/SKILL.md
```

Restart WorkBuddy, then trigger the Skill by mentioning a Xiaohongshu creator link in conversation.

#### Option 2: Manual Installation

Download [SKILL.md](./SKILL.md) from this repository, place it at `~/.workbuddy/skills/xiaohongshu-detector/SKILL.md` (create the directory if it doesn't exist), and restart WorkBuddy.

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

- **Creator Profile**: Nickname, ID, bio, focus areas, follower count
- **Per-Note Details**: Title, type (video/image), engagement metrics, tags, content summary, core viewpoints
- **Content Classification**: Notes grouped by topic with video/image ratio
- **Top 5 High-Engagement Notes**: Posts with most likes/saves
- **Key Insights**: Common themes and value propositions across the creator's content
- **Timeline Analysis**: Chronological evolution of creator's topics
- **Cross-Note Correlation**: Identifies methodological connections between related notes
- **Coverage Report**: Marks successfully obtained / partially obtained / unobtainable notes

### FAQ

| Issue | Solution |
|-------|----------|
| OpenCLI not installed | Install OpenCLI and load the Chrome extension |
| Content summary is empty | Adjust search keywords or check proxy settings |
| Creator has 0 notes | Creator may be new or have private notes |
| Batch search results are messy | Fall back to individual note searches |
| Success rate < 50% | Check network, retry 3 times, or inform user |

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
