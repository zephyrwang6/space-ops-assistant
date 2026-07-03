# Space Ops Assistant

`space-ops-assistant.skill`

「你想追的下一个热点，何必靠刷信息流」

License: MIT · Agent Skills · Multi-Runtime

Space Ops Assistant 是一套给内容创作者、运营、增长和自媒体作者用的运营情报 Skill 集合。

它帮你从小红书、公众号、B站等内容平台里搜索热点、挖爆款、看评论、查竞品、找选题，把“今天写什么”从玄学变成一套可复用的数据工作流。

基于开放的 Agent Skills 协议，可在 Claude Code、Codex、Cursor、OpenClaw、Hermes Agent、CodeBuddy、Workbuddy、Gemini CLI、OpenCode 等兼容 runtime 中运行。

不是让 AI 凭感觉给你编选题。

是先把平台上的真实内容、互动数据、标题结构和传播信号拉回来，再让 Agent 帮你判断：

- 哪些话题正在涨
- 哪些标题值得拆
- 哪些内容形式在被收藏
- 哪些爆款只是噪声
- 你的下一篇文章、小红书笔记、B站视频该往哪里打

看效果 · 安装 · 它能搜索什么 · 工作原理 · 诚实边界

## 效果示例

```text
用户      ❯ 帮我看看最近 Codex 在小红书上有什么爆款

助手      ❯ 已搜索小红书热门笔记，关键词：Codex / OpenAI Codex / AI编程 / Claude Code

            近 7 天返回 39 条，筛选出 10 条高相关结果。

            最高互动方向：
            1. 低成本玩转 Claude Code：6w+ 互动
            2. Codex 写文献综述：1w+ 互动
            3. 5 分钟安装 Codex：1w+ 互动
            4. Codex 小白教程：5000+ 互动
            5. Codex 磁盘日志避坑：5000+ 互动

            结论：
            小红书用户不关心“模型多强”，更关心：
            - 怎么安装
            - 怎么省钱
            - 怎么避坑
            - 普通人能不能用
            - 能不能马上做 PPT、论文、剪视频
```

再看公众号：

```text
用户      ❯ 搜一下公众号里 Codex 相关爆款

助手      ❯ 已生成公众号赛道报告。

            强相关爆款包括：
            - 阿里版 Codex 正式发布：7w+ 阅读，3075 分享
            - 你的 Codex 可能只解锁了 1%：3w+ 阅读，2012 分享
            - 用 Codex 搭建 Karpathy 同款知识库：1w+ 阅读，2527 分享
            - 往 Codex 里塞块无限画布：1w+ 阅读，2173 分享

            公众号更吃“Codex + 具体生产场景”：
            知识库、PPT、视频、画布改图、国产替代。
```

再看跨平台搜索：

```text
用户      ❯ B站搜一下 AI 编程，看哪些视频在起量

助手      ❯ 已通过 B站公开搜索 API 返回结果：

            1. 一口气学会 AI 编程
               播放 17w+，收藏 1.7w+

            2. 黑马 Vibe Coding 零基础入门
               播放 6w+，覆盖 Claude Code / Cursor / Codex / Agent

            3. Trae 保姆级教程
               播放 95w+，收藏 2.9w+

            结论：
            B站教程内容更吃“长课 + 工具合集 + 零基础 + 项目实操”。
```

这不是泛泛的“帮我想 10 个选题”。  
它先搜索，再筛选，再把平台语境还原出来。

## 安装

Space Ops Assistant 基于开放的 Agent Skills 协议，可在任何 skills-compatible 的 AI agent runtime 中运行。

### 方式一：一行命令（推荐，跨 runtime）

打开你正在用的 agent（Claude Code、Codex、Cursor、OpenClaw、Hermes、CodeBuddy、Workbuddy、Gemini CLI、OpenCode 等），告诉它：

```text
帮我安装这个 skill：https://github.com/zephyrwang6/space-ops-assistant
```

或者用通用 CLI 安装器（vercel-labs/skills，支持多 runtime）：

```bash
npx skills add zephyrwang6/space-ops-assistant
```

它会自动识别你当前的 runtime 并把 skill 放到正确目录。需要指定时可加 runtime 参数，例如 `-a codex` / `-a claude-code` / `-a cursor`。

### 方式二：手动安装

克隆仓库后，把需要的 skill 目录复制到你的 runtime skills 目录：

```bash
git clone https://github.com/zephyrwang6/space-ops-assistant.git
```

仓库结构：

```text
skills/
  xhs-hotnotes/
  global-content-search/
  gzh-explosive-content-detector/
  baokuan-article-analysis/
```

每个子目录都是一个独立 Skill。

### 方式三：作为参考资料使用

即使你的 runtime 不支持 Agent Skills 自动加载，也可以直接打开对应目录里的 `SKILL.md`，把内容粘贴进对话。  
它本质是一份 markdown + YAML frontmatter + 可运行脚本。

## 使用

装好后，可以直接告诉 agent：

```text
帮我搜一下小红书最近 Codex 的热门笔记
```

```text
查一下公众号里 AI Agent 相关爆款
```

```text
帮我看 B站上 AI 编程教程最近哪些视频起量
```

```text
找一下小红书某个博主最近发了什么，评论区在关心什么
```

如果你想手动运行脚本，也可以：

### 小红书热门笔记

```bash
export REDFOX_API_KEY="your_api_key_here"

python3 skills/xhs-hotnotes/scripts/fetch_xhs_hot_articles.py \
  --keyword "Codex,AI编程" \
  --start-date 2026-06-23
```

### 小红书 / B站 / 抖音内容分析

```bash
node skills/global-content-search/src/xiaohongshu/search-cli.js \
  --platform xiaohongshu \
  --keyword "AI编程" \
  --limit 20
```

```bash
node skills/global-content-search/src/xiaohongshu/search-cli.js \
  --platform bilibili \
  --keyword "AI编程" \
  --limit 10
```

抖音目前是扩展入口：

```bash
export DOUYIN_COMMAND="/path/to/douyin-readonly-cli"

node skills/global-content-search/src/xiaohongshu/search-cli.js \
  --platform douyin \
  --keyword "AI工具"
```

### 公众号爆款搜索

```bash
python3 skills/gzh-explosive-content-detector/scripts/fetch_gzh_trends.py \
  --keyword "AI编程,Codex,Claude Code" \
  --start-date 2026-06-23
```

### 公众号赛道分析

```bash
python3 skills/baokuan-article-analysis/scripts/daily_sector_trends.py \
  --sector "AI Coding=Codex,Claude Code,AI编程" \
  --days 7 \
  --output-dir ./reports
```

## 它能搜索什么

| Skill | 解决的问题 | 输出 |
| --- | --- | --- |
| `xhs-hotnotes` | 小红书热门笔记搜索、互动排序、相关性评分 | HTML 报告 + JSON |
| `global-content-search` | 全域内容搜索：小红书/B站/抖音扩展入口，查关键词、详情、评论、创作者作品；小红书支持 Guaikei API 兜底 | JSON / raw 输出 + logs |
| `gzh-explosive-content-detector` | 公众号关键词爆款搜索 | HTML 报告 |
| `baokuan-article-analysis` | 公众号赛道级爆款聚合、去重、排名、风格分析 | HTML 报告 + `data.json` |

## 工作原理

Space Ops Assistant 不是一个单一爬虫，而是一组内容运营 Skill。

它把运营调研拆成四层：

| 层次 | 说明 |
| --- | --- |
| 平台访问 | 通过 Redfox、Agent Reach、OpenCLI、bili-cli、公开 API 等后端读取公开内容 |
| 数据整理 | 去重、排序、评分、截断、生成结构化 JSON |
| 报告生成 | 输出 HTML 报告、排名卡片、互动指标、标题样本 |
| 运营判断 | 让 Agent 基于数据提炼选题方向、标题机制、内容形式和传播原因 |

其中 `global-content-search` 的访问顺序是 Agent Reach 优先，Guaikei API 最后兜底：

- 小红书：优先走 Agent Reach 检测到的 `OpenCLI` / `xiaohongshu-mcp` / `xhs-cli`
- 小红书兜底：如果 Agent Reach 搜不了，配置 `GUAIKEI_API_TOKEN` 后使用 Guaikei API
- B站：优先走 `bili-cli` / `opencli bilibili`，否则走公开搜索/详情 API
- 抖音：预留 `DOUYIN_COMMAND` 自定义只读 CLI

## 适合谁

- 自媒体作者：找选题、拆标题、看评论区需求
- 公众号作者：找近期爆款、判断文章方向
- 小红书运营：查热点笔记、竞品账号、评论反馈
- B站创作者：看视频选题、教程形态和播放收藏信号
- 产品/增长团队：监控用户讨论、内容趋势和传播路径

## 诚实边界

每个运营工具都应该说明自己做不到什么。

- 不是实时后台数据：平台数据来自公开页面、第三方数据源或入库快照。
- 不绕过平台限制：不做发帖、点赞、评论等写操作，不承诺绕过验证码或风控。
- 小红书受 `xsec_token` 机制影响：详情页通常需要使用搜索结果返回的完整 URL。
- 抖音当前不是内置可用后端：仓库只提供扩展入口，需你接入本地只读 CLI。
- 热门不等于适合你：爆款数据只能说明平台上什么在传播，不能替代你的定位判断。
- 公开表达不等于真实需求：评论和互动是信号，不是用户内心的完整答案。

一个不告诉你边界在哪的运营工具，不值得信任。

## 许可证

MIT
