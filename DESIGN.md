# TheAIEra 网页设计规范

## 品牌标识

- **名称**：TheAIEra
- **标题**：TheAIEra - 24h AI Updates Radar
- **副标题**：聚合 8 个 AI/科技站点，实时追踪 24 小时内重要更新
- **Live 标签**：实时状态指示器

## 设计系统

### 配色方案

```css
:root {
  --primary: #DC2626;        /* 主红色 - 强调色 */
  --bg-light: #FEF2F2;       /* 浅粉背景 */
  --bg-white: #FFFFFF;       /* 白色卡片 */
  --text-dark: #1F2937;      /* 深灰文字 */
  --text-muted: #6B7280;     /* 次要文字 */
  --border: #E5E7EB;         /* 边框灰 */
}
```

### 字体系统

- **主字体**：Inter (Google Fonts)
- **字重**：400 (Regular), 500 (Medium), 600 (Semibold), 700 (Bold)
- **字号层级**：
  - 标题：1.25rem / 800 weight / -0.025em letter-spacing
  - 副标题：0.8125rem / 400 weight
  - 正文：0.875rem / 500 weight
  - 次要文字：0.8125rem / 400 weight
  - 小字：0.6875rem / 600 weight

### 间距系统

- **Header**：0.75rem 1.5rem
- **Controls**：0.5rem 1.5rem
- **Subtitle**：0.5rem 1.5rem
- **Category Tabs**：0.5rem 1.5rem
- **Main Content**：0.75rem 1.5rem
- **Card Padding**：0.625rem
- **Grid Gap**：0.5rem

## 布局结构

### Header (固定顶部)

```
┌─────────────────────────────────────────────────────┐
│ TheAIEra [Live]  │  1065 更新  │  8 源  │  14:30  │
└─────────────────────────────────────────────────────┘
```

- **Sticky 定位**：`position: sticky; top: 0; z-index: 100`
- **Flex 布局**：左右对齐
- **响应式**：移动端隐藏统计信息

### Subtitle (居中说明)

```
┌─────────────────────────────────────────────────────┐
│     聚合 8 个 AI/科技站点，实时追踪 24 小时内重要更新   │
└─────────────────────────────────────────────────────┘
```

- **居中对齐**：`text-align: center`
- **背景色**：白色
- **下边框**：1px solid

### Controls (搜索+筛选)

```
┌─────────────────────────────────────────────────────┐
│ [🔍 搜索...]  [▼ 全部站点]                          │
└─────────────────────────────────────────────────────┘
```

- **搜索框**：flex: 1, 占据剩余空间
- **下拉菜单**：只显示有数据的站点
- **响应式**：移动端搜索框占满整行

### Category Tabs (分类标签)

```
┌─────────────────────────────────────────────────────┐
│ [全部] [新闻] [技术] [工具] [研究] [公司]            │
└─────────────────────────────────────────────────────┘
```

- **水平滚动**：`overflow-x: auto`
- **激活状态**：底部红色边框 + 红色文字
- **标签大小**：0.375rem 0.875rem

## 组件设计

### News Card (新闻卡片)

```
┌────────────────────────────────────────────┐
│ [Buzzing] [36氪 · 24小时热榜]          2h  │
│ Anthropic发布最新模型，性能提升显著        │
│ Anthropic releases new model with...      │
└────────────────────────────────────────────┘
```

**尺寸规格**：
- **圆角**：0.375rem
- **内边距**：0.625rem
- **间距**：0.375rem (元素间), 0.5rem (网格间)
- **最小宽度**：320px
- **最大宽度**：1fr (自适应)

**交互状态**：
- **默认**：白色背景，浅灰边框
- **悬停**：
  - 边框变红
  - 轻微阴影：`0 1px 4px rgba(220, 38, 38, 0.1)`
  - 标题变红

**层级结构**：
1. Meta 行（站点徽章 + 来源 + 时间）
2. 主标题（2 行截断）
3. 英文标题（1 行截断，可选）

### Site Badge (站点徽章)

- **背景色**：`--bg-light`
- **文字色**：`--primary`
- **内边距**：0.0625rem 0.375rem
- **圆角**：0.25rem
- **字号**：0.6875rem
- **字重**：500

### Time Badge (时间徽章)

- **位置**：Meta 行右侧
- **对齐**：`margin-left: auto`
- **格式**：
  - < 1h：`5m`, `30m`
  - < 24h：`2h`, `12h`
  - ≥ 24h：`3月5日`

## 响应式设计

### 断点

**桌面端** (> 768px)：
- 网格布局：`repeat(auto-fill, minmax(320px, 1fr))`
- Header 统计显示
- 搜索框和下拉框并排

**移动端** (≤ 768px)：
- 网格布局：单列 `1fr`
- Header 统计隐藏
- 搜索框占满宽度
- Padding 减半

### 移动端优化

```css
@media (max-width: 768px) {
  .header {
    padding: 0.5rem 1rem;
  }

  .header-stats {
    display: none;
  }

  .controls {
    padding: 0.5rem 1rem;
  }

  .search-box {
    width: 100%;
    flex: none;
  }

  .main-content {
    padding: 0.5rem 1rem;
  }

  .news-grid {
    grid-template-columns: 1fr;
  }
}
```

## 数据加载

### 加载策略

1. **API**：`fetch('./data/latest-24h.json')`
2. **错误处理**：console.log + 空状态提示
3. **降级显示**：加载失败时显示友好提示

### 时间戳处理

```javascript
// 优先使用 published_at，回退到 first_seen_at
const timestamp = item.published_at || item.first_seen_at;

// 相对时间格式化
< 1h:  "5m", "30m"
< 24h: "2h", "12h"
≥ 24h: "3月5日"
```

## 分类逻辑

### 关键词匹配

```javascript
const categoryKeywords = {
  news: ['新闻', '发布', '宣布', '推出', '上线', 'release', 'launch', 'announce'],
  tech: ['技术', '模型', '算法', '训练', '推理', 'model', 'algorithm', 'training'],
  tools: ['工具', '软件', '应用', '平台', 'API', 'tool', 'software', 'app', 'platform'],
  research: ['研究', '论文', '实验', '发现', 'research', 'paper', 'study'],
  company: ['公司', '融资', '收购', '合作', 'OpenAI', 'Google', 'funding', 'acquisition']
};
```

### 默认分类

- 未匹配到任何关键词 → `news` 分类

## 性能优化

### CSS

- **Transform 动画**：使用 `transform` 而非 `width/height`
- **过渡时间**：0.15s 快速响应
- **减少重排**：使用 `will-change` 提示

### JavaScript

- **事件委托**：单个监听器管理所有卡片
- **防抖搜索**：输入即过滤，无需额外优化（数据量小）
- **懒加载**：暂不需要（全部数据已在内存）

## 可访问性

- **颜色对比度**：4.5:1 (WCAG AA)
- **焦点状态**：所有交互元素有 `:focus` 样式
- **语义化标签**：`<header>`, `<nav>`, `<main>`, `<article>`
- **ARIA 标签**：可按需添加
- **键盘导航**：Tab 键顺序正确

## 浏览器兼容性

- **现代浏览器**：Chrome 90+, Firefox 88+, Safari 14+, Edge 90+
- **移动浏览器**：iOS Safari 14+, Chrome Android 90+
- **不支持的特性**：无需降级处理

## 设计决策记录

### 为什么选择 Inter 字体？

- 现代几何无衬线
- 优秀的屏幕可读性
- 多语言支持（中文、英文）
- Google Fonts 免费托管

### 为什么使用 CSS 变量？

- 主题统一管理
- 便于维护和调整
- 符合现代 CSS 最佳实践

### 为什么卡片如此紧凑？

- 信息密度优先
- 快速浏览体验
- 移动端友好
- 减少滚动操作

### 为什么去掉了中英文切换？

- 单一视图更简洁
- 自动显示双语（中主英辅）
- 降低用户选择成本

## 未来改进方向

- [ ] 添加深色模式支持
- [ ] 支持自定义分类关键词
- [ ] 添加收藏功能
- [ ] 支持导出为 RSS/JSON
- [ ] 添加 PWA 支持（离线访问）
- [ ] 优化首屏加载速度
