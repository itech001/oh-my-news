# TheAIEra - 24h AI Updates Radar

聚合 8 个 AI/科技站点,实时追踪 24 小时内重要更新

[![GitHub Pages](https://img.shields.io/badge/GitHub-Pages-222?logo=github)](https://itech001.github.io/the-ai-era/)
[![Vercel](https://img.shields.io/badge/Vercel-000000?logo=vercel)](https://the-ai-era.vercel.app)
[![Update Frequency](https://img.shields.io/badge/Update-Daily%206:30%20AM-blue)](.github/workflows/update-news.yml)

## 特性

- **8 个数据源聚合**:TechURLs, Buzzing, Info Flow, TopHub, Zeli, AI HubToday, AIbase, NewsNow
- **24 小时滚动窗口**:只展示最近 24 小时内的 AI/科技新闻
- **智能分类**:自动按新闻/技术/工具/研究/公司分类
- **实时更新**:每天自动抓取并部署
- **双平台部署**:同时支持 GitHub Pages 和 Vercel CDN
- **全球加速**:Vercel 提供 70+ 全球节点 CDN

## 在线访问

- **GitHub Pages**:https://itech001.github.io/the-ai-era/
- **Vercel CDN**:https://the-ai-era.vercel.app
- **国内访问**:https://www.theaiera.cn
- **数据源**:8 个 AI/科技聚合网站 + Karpathy 推荐博客
- **更新频率**:每天北京时间 6:30

## 本地运行

```bash
# 安装依赖
pip install -r requirements.txt

# 抓取最新数据（24小时窗口）
python scripts/update_news.py --output-dir data --window-hours 24

# 启动本地服务器
python -m http.server 8080
```

访问 `http://localhost:8080` 查看结果

## 数据输出

| 文件 | 说明 |
|------|------|
| `data/latest-24h.json` | 最近 24 小时新闻数据 |
| `data/title-zh-cache.json` | 中文标题翻译缓存 |

## GitHub Actions 自动化

三个独立 workflow:

### 1. 数据更新 (`.github/workflows/update-news.yml`)
- **触发**:每天北京时间 6:30 (cron: `30 22 * * *`)
- **任务**:抓取数据并提交到仓库
- **权限**:`contents: write`

### 2. GitHub Pages 部署 (`.github/workflows/deploy-github-pages.yml`)
- **触发**:
  - 每天北京时间 7:00 (数据更新后)
  - 推送到 master 分支时(数据更新后自动触发)
  - 手动触发
- **任务**:部署静态页面到 GitHub Pages
- **权限**:`pages: write`, `id-token: write`

### 3. Vercel 部署 (`.github/workflows/deploy-vercel.yml`)
- **触发**:
  - 每天北京时间 7:00 (数据更新后)
  - 推送到 master 分支时(数据更新后自动触发)
  - 手动触发
- **任务**:部署到 Vercel 全球 CDN
- **权限**:需要 `VERCEL_TOKEN`, `VERCEL_ORG_ID`, `VERCEL_PROJECT_ID`

## 数据源详情

| 站点 | 说明 | 状态 |
|------|------|------|
| TechURLs | AI/技术新闻链接 | ✅ 正常 |
| Buzzing | 社交媒体热点 | ✅ 正常 |
| Info Flow | 信息流聚合 | ✅ 正常 |
| TopHub | 热榜聚合 | ✅ 正常 |
| Zeli | AI 专题聚合 | ✅ 正常 |
| AI HubToday | AI 工具导航 | ✅ 正常 |
| AIbase | AI 产品数据库 | ✅ 正常 |
| NewsNow | 新闻聚合 | ✅ 正常 |

## 配置 RSS 订阅（可选）

支持通过 OPML 文件批量导入私人 RSS 订阅。

### 默认 RSS 订阅

项目已包含以下优质 RSS 订阅列表 (`feeds/follow.opml`)：

**1. Karpathy 推荐博客** (100+ 源)
- Simon Willison、Mitchell Hashimoto 等技术大牛的博客
- Hacker News 热门博客
- AI/技术领域优质内容源

**2. AI 公司官方 RSS** (66 源)
- 国际科技巨头: OpenAI, Google, Microsoft, NVIDIA, Meta, Apple, Amazon 等
- 国内头部企业: 阿里巴巴、腾讯、百度、华为、字节跳动、小米等
- AI 独角兽: Anthropic, Perplexity AI, Stability AI 等
- 顶尖高校: MIT, UC Berkeley, Stanford, CMU, arXiv 等
- 技术媒体: Machine Learning Mastery, Towards Data Science 等

**来源**:
- [HN Popular Blogs 2025](https://gist.github.com/emschwartz/e6d2bf860ccc367fe37ff953ba6de66b#file-hn-popular-blogs-2025-opml)
- AI Company RSS (66 个全球 AI 大公司及顶尖 AI 技术平台)

### 自定义 RSS 订阅

**方式 1: 覆盖默认订阅（推荐）**
1. 准备你的 OPML 文件
2. 生成 base64：`base64 < your-feeds.opml`
3. 添加到 GitHub Secrets：`FOLLOW_OPML_B64`
4. Actions 会自动使用你的订阅覆盖默认文件

**方式 2: 本地测试**
```bash
# 复制模板
cp feeds/follow.example.opml feeds/follow.opml

# 编辑添加你的 RSS 订阅
# vim feeds/follow.opml

# 运行脚本
python scripts/update_news.py --output-dir data --window-hours 24 --rss-opml feeds/follow.opml
```

## 项目结构

```
the-ai-era/
├── .github/
│   └── workflows/
│       ├── update-news.yml           # 数据更新 workflow
│       ├── deploy-github-pages.yml   # GitHub Pages 部署
│       └── deploy-vercel.yml         # Vercel 部署
├── data/                             # JSON 数据输出
├── feeds/                            # RSS OPML 配置
├── scripts/
│   └── update_news.py                # 数据抓取脚本
├── DESIGN.md                         # 设计规范文档
├── README.md                         # 项目说明
└── index.html                        # 静态页面
```

## 技术栈

- **后端**:Python 3.11, requests, BeautifulSoup4, feedparser
- **前端**:原生 HTML/CSS/JS
- **部署**:
  - GitHub Pages(全球访问)
  - Vercel CDN(70+ 全球节点)
- **自动化**:GitHub Actions
- **域名**:www.theaiera.cn(自定义域名)

## License

MIT
