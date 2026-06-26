# Xiaohongshu Creator Summarizer

**小红书博主总结** — Skill for deep analysis of Xiaohongshu (RED/Little Red Book) creators' notes, covering both video and image posts.

---

## 中文说明

### 功能概述

本 Skill 用于深度总结小红书博主的笔记内容。支持：

- **视频笔记**：通过 OpenCLI 获取 AI 视频内容摘要
- **图文笔记**：通过 OpenCLI 获取 AI 图文内容解读
- **混合内容**：同一博主的视频+图文笔记同时分析

### 使用场景

- 用户给出小红书博主主页链接，要求"总结所有内容"或"总结前N条笔记"
- 博主以视频内容为主（如商业空间观察、实体商业咨询类博主）
- 博主以图文内容为主（如行业观点、经验分享类博主）
- 博主内容混合视频+图文，两者都需要总结

### 安装前提

使用前**必须先安装 OpenCLI**：

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

- **博主档案**：昵称、ID、简介、专注领域
- **每条笔记详情**：标题、类型（视频/图文）、互动数据、标签、内容摘要、核心观点
- **内容分类统计**：按主题分类的笔记数量和视频/图文比例
- **TOP 5 高互动笔记**：点赞最多的前5条
- **核心发现**：博主内容的共性规律和价值主张

### 常见问题

| 问题 | 解决方案 |
|------|---------|
| OpenCLI 未安装 | 安装 OpenCLI 并加载 Chrome 扩展 |
| 内容摘要为空 | 调整搜索关键词或检查网络代理 |
| 博主笔记为 0 | 博主可能为新号或设置了隐私权限 |

---

## English Description

### Overview

This Skill performs **deep content analysis** of Xiaohongshu (RED / Little Red Book) creator profiles. It summarizes both **video notes** and **image notes**, extracting core viewpoints, engagement metrics, and content patterns.

### Use Cases

- User provides a Xiaohongshu creator profile URL and asks to "summarize all notes" or "summarize the first N posts"
- Creators focused on video content (e.g., commercial space observers, retail business consultants)
- Creators focused on image posts (e.g., industry insights, experience sharing)
- Mixed content creators — both video and image posts need analysis

### Prerequisites

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

### FAQ

| Issue | Solution |
|-------|----------|
| OpenCLI not installed | Install OpenCLI and load the Chrome extension |
| Content summary is empty | Adjust search keywords or check proxy settings |
| Creator has 0 notes | Creator may be new or have private notes |

---

## License

MIT License
