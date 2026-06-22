# 📖 详细安装指南

本指南将帮助你完整配置 Obsidian Dashboard。

## 前置要求

- 已安装 [Obsidian](https://obsidian.md) 
- 对 Obsidian 基本操作有了解
- macOS / Windows / Linux 均支持

## 分步安装

### 步骤 1: 获取项目文件

**方式 A: 从 GitHub 克隆**
```bash
git clone https://github.com/your-username/obsidian-dashboard.git
cd obsidian-dashboard
```

**方式 B: 直接下载**
1. 访问 GitHub 仓库
2. 点击 Code → Download ZIP
3. 解压到任意位置

### 步骤 2: 在 Obsidian 中打开

1. 打开 Obsidian
2. 点击左下角 "打开其他库"（或 Vault 图标）
3. 点击 "打开文件夹"
4. 选择 `obsidian-dashboard` 文件夹
5. 点击 "选择" 或 "Open"

### 步骤 3: 禁用安全模式

Dashboard 使用 JavaScript，需要禁用安全模式：

1. 打开 **Obsidian 设置** (Ctrl/Cmd + ,)
2. 左侧菜单 → **社区插件**
3. 关闭 **安全模式** 开关
4. 弹出警告提示，点击 "禁用" 确认

### 步骤 4: 安装 Dataview 插件

1. 设置 → 社区插件 → 浏览
2. 搜索 "**Dataview**"（查找黑色 Logo 的官方插件）
3. 点击 "安装"
4. 点击 "启用"

### 步骤 5: 配置 Dataview

1. 返回设置 → 社区插件 → 已安装的插件
2. 找到 **Dataview** 点击设置图标
3. 向下滚动找到 **JavaScript**：
   - ✅ 启用 `Enable JavaScript Queries`
   - ✅ 启用 `Enable Inline JavaScript Queries`
   - ✅ 启用 `Enable Dataviewjs Queries`
4. 点击返回

### 步骤 6: 启用 CSS Snippet

1. 设置 → 外观 → **CSS 代码片段**
2. 右边的文件夹图标打开片段文件夹
3. 确认 `Dashboard.css` 文件在该文件夹中
4. 返回设置，点击刷新按钮（↻）
5. 启用 **Dashboard** 代码片段（开关打开）

### 步骤 7: 打开 Dashboard

1. 点击左侧文件面板
2. 找到 **Home.md**
3. 点击打开

**✨ 恭喜！Dashboard 应该正常显示了！**

## 验证安装

检查以下几点确认安装成功：

- [ ] 看到 Dashboard 的各个模块（导航、日历等）
- [ ] 日历可以点击
- [ ] 待办项可以勾选
- [ ] 没有看到代码或警告

## 如果出现问题

### 问题 1: 看到代码而不是 Dashboard

**原因**：未启用阅读模式或 JavaScript 查询未开启

**解决**：
1. 确认 Home.md 顶部有 `obsidianUIMode: preview`
2. 确认 Dataview 的 JavaScript 选项已启用
3. 重新启动 Obsidian（按下 Cmd/Ctrl + Q）

### 问题 2: 日历功能不工作

**原因**：`00 - Daily` 文件夹不存在

**解决**：
1. 在项目根目录创建 `00 - Daily` 文件夹
2. 在该文件夹中创建一个测试文件：`2026-06-22.md`
3. 返回 Home.md，刷新

### 问题 3: CSS 样式没有应用

**原因**：Snippet 未启用或被其他样式覆盖

**解决**：
1. 确认 Dashboard 代码片段已启用
2. 尝试重启 Obsidian
3. 检查是否有其他 CSS 插件冲突

### 问题 4: 数据文件看不到内容

**原因**：Dataview 查询出错或数据格式不对

**解决**：
1. 检查数据文件格式是否正确（参考 README）
2. 打开浏览器开发者工具查看错误（Cmd/Ctrl + Shift + I）
3. 在 [Discussions](../../discussions) 中提问

## 下一步

- 📚 查看 [高级自定义](./CUSTOMIZATION.md) 了解如何个性化配置
- 🎨 修改 CSS 调整配色和样式
- 📝 创建你自己的数据并开始使用

## 需要帮助？

- 💬 [讨论社区](../../discussions) - 提问和交流
- 🐛 [报告 Bug](../../issues) - 提交问题
- 📖 [贡献指南](CONTRIBUTING.md) - 了解如何贡献
