# 🔧 GitHub 配置说明

本文档说明如何在 GitHub 上配置仓库以获得最佳体验。

## 1. 仓库基本设置

### 仓库名称和描述

**推荐名称**:
```
obsidian-dashboard
obsidian-personal-dashboard
Obsidian-Dashboard-Home
```

**推荐描述**:
```
🏠 Personal dashboard for Obsidian with Dataview. 
Features domain navigation, calendar, todos, and project tracking.
```

### Topics（标签）

添加这些 Topics 便于用户发现：

```
obsidian
obsidian-theme
dashboard
dataview
productivity
note-taking
css
```

## 2. GitHub Pages（可选）

如果你想创建项目文档网站：

1. 设置 → Pages
2. Build and deployment 选择 "GitHub Actions"
3. 创建 `.github/workflows/pages.yml`

## 3. Discussions 设置

启用社区讨论：

1. 设置 → Discussions → ✅ 启用
2. 选择讨论类别：
   - **Ideas** - 功能建议
   - **Polls** - 投票
   - **Q&A** - 问答
   - **Show and Tell** - 分享用例

## 4. Security 设置

### 启用 Branch Protection

1. 设置 → Branches
2. Add branch ruleset
3. 保护 `main` 分支：
   - [x] Require a pull request before merging
   - [x] Dismiss stale pull request approvals when new commits are pushed
   - [x] Require status checks to pass

### 启用 Dependabot（可选）

用于依赖更新提醒。

## 5. Actions 工作流（可选）

### 创建自动检查工作流

`.github/workflows/checks.yml`:

```yaml
name: Checks

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Check markdown
        run: |
          # 检查 markdown 文件
          find . -name "*.md" -type f | head -20
```

## 6. 协作者管理

### 添加协作者

1. 设置 → Collaborators and teams
2. 添加团队成员（可选）

### 权限级别

- **Maintain**: 可以管理仓库，但不能删除
- **Push**: 可以推送代码
- **Triage**: 可以管理 Issues 但不能推送
- **Pull**: 只读

## 7. 模板文件位置

GitHub 会自动检测并使用：

```
.github/
├── ISSUE_TEMPLATE/
│   ├── bug_report.md
│   ├── feature_request.md
│   └── config.yml
├── pull_request_template.md
└── workflows/
    └── checks.yml
```

## 8. 社区标准

### README.md - ✅ 已包含
- 项目描述
- 功能列表
- 安装说明
- 使用示例

### CONTRIBUTING.md - ✅ 已包含
- 贡献方式
- 代码风格
- Pull Request 流程

### LICENSE - ✅ 已包含
- MIT 许可

### Code of Conduct - ✅ 已包含
- 社区行为准则

### CHANGELOG.md - ✅ 已包含
- 版本更新记录

## 9. README 徽章（可选）

在 README 顶部添加徽章：

```markdown
# 🏠 Obsidian Dashboard

![GitHub](https://img.shields.io/github/license/your-username/obsidian-dashboard)
![GitHub Stars](https://img.shields.io/github/stars/your-username/obsidian-dashboard)
![GitHub Release](https://img.shields.io/github/v/release/your-username/obsidian-dashboard)
```

生成工具: https://shields.io/

## 10. 社区宣传

### 发布到官方社区

- **Obsidian Community**: https://forum.obsidian.md/
- **Awesome Obsidian**: https://github.com/obsidianmd/obsidian-sample-plugin#community-plugins

### 社交媒体

- Twitter: 使用 #obsidian #productivity #dashboard
- Reddit: r/obsidian, r/productivity
- Product Hunt: https://www.producthunt.com/

## 11. 维护计划

### 每周任务

- [ ] 检查新 Issues
- [ ] 回复 Discussions
- [ ] 合并 Pull Requests

### 每月任务

- [ ] 更新 CHANGELOG.md
- [ ] 发布新版本（如有重大更新）
- [ ] 总结社区反馈

### 每季度任务

- [ ] 审查路线图
- [ ] 规划大版本更新
- [ ] 撰写月度总结

## 12. 常用 GitHub 操作

### 创建 Release

```bash
# 1. 创建 tag
git tag -a v1.0.0 -m "Release v1.0.0"

# 2. Push tag
git push origin v1.0.0

# 3. 在 GitHub 上创建 Release
# 访问: https://github.com/your-username/repo/releases/new
```

### 管理 Discussions

1. 每周检查新讨论
2. 标记已解答的问题
3. 分类贡献的想法

### 处理 Pull Requests

1. 自动化检查通过后才能合并
2. 至少一次代码审查
3. 合并时添加清晰的提交信息

## 13. 监控和分析

### GitHub Insights

1. 设置 → Insights → Traffic
2. 查看：
   - Clones 数量
   - Unique clones
   - Views 数量

### 参与度跟踪

- Forks 数量
- Stars 趋势
- Issues/PRs 活动
- Discussions 参与

---

**所有设置完成后，你的项目就可以发布了！** 🚀
