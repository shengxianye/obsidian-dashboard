# 🎨 高级自定义指南

本指南讲解如何深度定制 Dashboard 以满足你的个性化需求。

## 目录

1. [修改主题色](#修改主题色)
2. [自定义领域导航](#自定义领域导航)
3. [调整布局和间距](#调整布局和间距)
4. [添加新模块](#添加新模块)
5. [修改数据源](#修改数据源)

## 修改主题色

所有主题色都通过 CSS 变量定义，易于修改。

### 打开 CSS 文件

1. 设置 → 外观 → CSS 代码片段 → 打开文件夹
2. 用文本编辑器打开 `Dashboard.css`

### CSS 变量列表

```css
:root {
  /* 主色调 */
  --color-primary: #5e81ac;
  --color-accent: #bf616a;
  --color-success: #a3be8c;
  --color-warning: #ebcb8b;
  --color-error: #bf616a;
  
  /* 背景色 */
  --bg-primary: #2e3440;
  --bg-secondary: #3b4252;
  --bg-tertiary: #434c5e;
  
  /* 文字色 */
  --text-primary: #eceff4;
  --text-secondary: #d8dee9;
  
  /* 间距 */
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
  --spacing-xl: 32px;
}
```

### 示例：改为浅色主题

```css
:root {
  --color-primary: #2e7d32;
  --bg-primary: #ffffff;
  --bg-secondary: #f5f5f5;
  --text-primary: #212121;
  --text-secondary: #616161;
}
```

## 自定义领域导航

### 编辑 Home.md 中的领域列表

在 `Home.md` 顶部找到 `domains` 变量：

```javascript
const domains = [
  { name: "Tech Learning", icon: "📚", folder: "Tech Learning" },
  { name: "Work", icon: "💼", folder: "Work" },
  { name: "Life", icon: "🌟", folder: "Life" },
  // 添加更多...
];
```

### 参数说明

| 参数 | 说明 | 示例 |
|------|------|------|
| `name` | 显示名称 | "我的项目" |
| `icon` | Emoji 图标 | "🚀" |
| `folder` | 实际文件夹名 | "projects" |

### 常用图标列表

```
📚 📖 ✍️ 📝 📄 📃 📋 📊 📈 📉 💻 ⌨️ 🖥️ 📱 
🎨 🎭 🎪 🎬 🎤 🎧 🎵 🎶 🎸 🎹 📷 📹 🎥
🚀 🔧 🛠️ ⚙️ 🔩 🔗 📡 💡 🔦 🕯️ 💡
🌍 🌎 🌏 🗺️ 🧭 ⛺ 🏔️ 🏕️ 🏠 🏡 🏢 🏬
```

## 调整布局和间距

### 修改间距

```css
:root {
  --spacing-md: 24px;  /* 从 16px 改为 24px，增加间距 */
}
```

### 修改卡片宽度

在 `Dashboard.css` 中找到：

```css
.dashboard-card {
  max-width: 500px;  /* 修改此值调整卡片宽度 */
}
```

### 修改列数

```css
.dashboard-grid {
  grid-template-columns: repeat(2, 1fr);  /* 改为 3 列或其他 */
}
```

## 添加新模块

### 1. 创建新的 Dataview 查询

在 `Home.md` 中添加新的代码块：

```dataviewjs
// 新模块代码
dv.header(2, "📌 新模块");

// 你的查询逻辑
const results = dv.pages().where(p => p.file.folder === "你的文件夹");
dv.table(["标题", "修改时间"], results);
```

### 2. 为模块添加样式

在 `Dashboard.css` 中添加：

```css
.custom-module {
  background: var(--bg-secondary);
  padding: var(--spacing-md);
  border-radius: 8px;
}
```

### 3. 常见模块示例

**列出所有标签**
```dataviewjs
const pages = dv.pages();
const tags = new Set();
pages.forEach(p => {
  if (p.tags) p.tags.forEach(t => tags.add(t));
});
dv.list(Array.from(tags));
```

**显示最新笔记**
```dataviewjs
dv.header(2, "📝 最新笔记");
dv.table(
  ["标题", "创建时间"],
  dv.pages()
    .sort(p => p.file.ctime, "desc")
    .limit(10)
    .map(p => [p.file.link, p.file.ctime])
);
```

## 修改数据源

### 数据文件位置

默认数据文件位置可在 `Home.md` 中修改：

```javascript
const TODO_FILE = "★★Todo.md";
const PROGRESS_FILE = "★★Project Progress.md";
const COLLECTION_FILES = {
  videos: "★★Video Bookmarks.md",
  blogs: "★★Blog Bookmarks.md",
  github: "★★GitHub Bookmarks.md"
};
```

### 修改为自己的路径

```javascript
const TODO_FILE = "日记/待办事项.md";
const PROGRESS_FILE = "项目/进度.md";
```

## 性能优化

### 1. 限制查询结果

```javascript
// 只显示最近 10 项
dv.table(["标题"], results.limit(10));
```

### 2. 缓存查询

```javascript
// 避免重复查询
const cache = {};
function getCachedData(key) {
  if (!cache[key]) {
    cache[key] = /* 查询 */;
  }
  return cache[key];
}
```

### 3. 延迟加载

在 CSS 中添加：

```css
.dashboard-module {
  loading: lazy;  /* 使用浏览器原生懒加载 */
}
```

## 高级配置示例

### 多列布局

```css
.dashboard-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: var(--spacing-lg);
}
```

### 响应式设计

```css
@media (max-width: 768px) {
  .dashboard-grid {
    grid-template-columns: 1fr;
  }
}
```

### 深色/浅色模式

```css
/* Obsidian 主题感知 */
body.theme-light {
  --color-primary: #0066cc;
  --bg-primary: #ffffff;
}

body.theme-dark {
  --color-primary: #66b3ff;
  --bg-primary: #1a1a1a;
}
```

## 常见问题

**Q: 修改后样式没有生效？**
A: 修改 CSS 后：
1. 保存文件
2. 返回 Obsidian
3. 按 Cmd/Ctrl + Shift + L 刷新样式（或重启 Obsidian）

**Q: 如何添加新的 emoji？**
A: 在任何 emoji 支持的地方直接粘贴：`{ icon: "🚀" }`

**Q: 能否关闭某个模块？**
A: 在 `Home.md` 中注释掉对应的代码块：
```javascript
/* dataviewjs
// 你的代码
*/
```

## 导出你的配置

1. 进行定制后，备份你的文件：
```bash
cp Dashboard.css Dashboard-custom.css
cp Home.md Home-custom.md
```

2. 分享给其他人，他们可以快速应用相同的样式

## 获取帮助

- 📖 [返回 README](README.md)
- 💬 [讨论社区](../../discussions) 分享你的定制
- 🐛 [报告问题](../../issues) 
- 📝 [贡献指南](CONTRIBUTING.md) 贡献代码
