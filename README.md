# TheAIEra - 24h AI Updates Radar

聚合 8 个 AI/科技站点，实时追踪 24 小时内重要更新

[![GitHub Pages](https://img.shields.io/badge/GitHub-Pages-222?logo=github)](https://itech001.github.io/oh-my-news/)
[![Update Frequency](https://img.shields.io/badge/Update-30%20min-blue)](.github/workflows/update-news.yml)

## 特性

- **8 个数据源聚合**：TechURLs, Buzzing, Info Flow, TopHub, Zeli, AI HubToday, AIbase, NewsNow
- **24 小时滚动窗口**：只展示最近 24 小时内的 AI/科技新闻
- **智能分类**：自动按新闻/技术/工具/研究/公司分类
- **实时更新**：每 30 分钟自动抓取并部署
- **静态部署**：基于 GitHub Pages，无需服务器

## 在线访问

- **主站**：https://itech001.github.io/oh-my-news/
- **数据源**：8 个 AI/科技聚合网站
- **更新频率**：每 30 分钟

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

两个独立 workflow：

### 1. 数据更新 (`.github/workflows/update-news.yml`)
- **触发**：每 30 分钟（cron: `*/30 * * * *`）
- **任务**：抓取数据并提交到仓库
- **权限**：`contents: write`

### 2. 页面部署 (`.github/workflows/deploy.yml`)
- **触发**：推送到 `master` 分支
- **任务**：部署静态页面到 GitHub Pages
- **权限**：`pages: write`, `id-token: write`

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

支持通过 OPML 文件批量导入私人 RSS 订阅：

1. 复制模板：`cp feeds/follow.example.opml feeds/follow.opml`
2. 编辑 `feeds/follow.opml`，添加你的 RSS 订阅
3. 运行时添加参数：`--rss-opml feeds/follow.opml`

**GitHub Actions 配置**：
1. 生成 base64：`base64 < feeds/follow.opml`
2. 添加到 GitHub Secrets：`FOLLOW_OPML_B64`
3. Actions 会自动加载私人订阅

## 项目结构

```
oh-my-news/
├── .github/
│   └── workflows/
│       ├── update-news.yml    # 数据更新 workflow
│       └── deploy.yml          # 页面部署 workflow
├── data/                       # JSON 数据输出
├── feeds/                      # RSS OPML 配置
├── scripts/
│   └── update_news.py         # 数据抓取脚本
├── DESIGN.md                   # 设计规范文档
└── index.html                  # 静态页面
```

## 技术栈

- **后端**：Python 3.11, requests, BeautifulSoup4, feedparser
- **前端**：原生 HTML/CSS/JS
- **部署**：GitHub Pages
- **自动化**：GitHub Actions

## License

MIT
