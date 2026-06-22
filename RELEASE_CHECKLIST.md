# 🚀 发布前清单

完整的 GitHub 开源发布前检查清单。

## ✅ 项目文件检查

- [x] README.md - 详细的项目说明
- [x] LICENSE - MIT 协议（已配置）
- [x] CONTRIBUTING.md - 贡献指南
- [x] CODE_OF_CONDUCT.md - 行为准则
- [x] CHANGELOG.md - 版本更新日志
- [x] INSTALL.md - 安装指南
- [x] CUSTOMIZATION.md - 自定义指南
- [x] .github/ISSUE_TEMPLATE/ - Issue 模板
- [x] .github/pull_request_template.md - PR 模板

## ✅ 内容清理

- [x] 移除个人工作内容（周报、会议记录等）
- [x] 清理个人生活信息
- [x] 用示例数据替换个人项目信息
- [x] 检查所有文件中没有敏感信息
- [x] 验证示例文件夹结构清晰

## ✅ 代码质量

- [x] CSS 代码已检查，注释清晰
- [x] JavaScript 代码（DataviewJS）可读
- [x] 没有硬编码的 API Key 或凭证
- [x] 文件结构符合 Obsidian 规范

## ✅ 文档完整性

- [ ] README 包含所有关键信息
  - [ ] 功能列表
  - [ ] 安装步骤（详细）
  - [ ] 数据格式说明
  - [ ] 常见问题解答
  
- [ ] 代码注释充分
  - [ ] CSS 选择器有说明
  - [ ] 复杂逻辑有注释
  
- [ ] 示例文件完整
  - [ ] 示例数据格式正确
  - [ ] 所有提到的文件都存在

## 📝 GitHub 仓库配置

### 在创建仓库时设置

- [ ] **仓库名称**: `obsidian-dashboard` (或你的命名)
- [ ] **描述**: 一行简洁的项目说明
- [ ] **Public**: 公开仓库
- [ ] **Initialize with**: 
  - [x] Add a README file - 否（我们已有 README.md）
  - [x] Add .gitignore - 否（我们已有）
  - [x] Choose a license - 否（我们已有 MIT）

### 仓库描述建议

```
🏠 A personal dashboard for Obsidian using Dataview. 
Features: domain navigation, calendar, todos, projects tracking.
Pure CSS + DataviewJS implementation. 中文支持。
```

### Topics（标签）建议

添加以下 Topics 帮助用户发现：

```
obsidian
obsidian-plugin
obsidian-theme
dashboard
dataview
productivity
note-taking
css
javascript
```

## 🔐 安全检查

- [ ] 没有 `.env` 或 `.env.local` 文件
- [ ] 没有私密 API 密钥
- [ ] 没有个人信息（地址、电话等）
- [ ] 没有工作敏感内容

### 检查命令

```bash
# 检查是否有敏感信息
grep -r "api_key\|password\|token\|secret" .
grep -r "TODO.*个人\|FIXME.*隐私" .
```

## 📊 .gitignore 验证

```bash
# 查看会被上传的文件
git status

# 模拟 clone 后的效果
git ls-files
```

## 🏷️ 版本和标签

### 创建首个发布版本

```bash
# 创建 tag
git tag -a v1.0.0 -m "Initial release"

# 推送 tag 到 GitHub
git push origin v1.0.0
```

### GitHub Release 信息

**标题**: `v1.0.0 - Initial Release`

**描述**:
```markdown
# 🎉 首个发布版本

## ✨ 功能
- 完整的 Dashboard 系统
- 领域导航、日历、待办等 10+ 模块
- 纯 CSS + DataviewJS 实现

## 🚀 快速开始
1. Clone 此仓库
2. 在 Obsidian 中打开
3. 安装 Dataview 插件
4. 完成！

详见 [安装指南](INSTALL.md)

## 📝 已知问题
- [ ] 移动端支持有限

## 感谢
感谢所有贡献者和用户的支持！
```

## 🔔 发布后

- [ ] 分享到 Obsidian 社区论坛
- [ ] 发送到 [Awesome Obsidian](https://github.com/obsidianmd/obsidian-sample-plugin) 列表
- [ ] 发布到 Reddit (r/obsidian, r/productivity)
- [ ] Twitter/X 分享
- [ ] 邀请用户反馈和贡献

## 📋 发布命令参考

```bash
# 1. 本地最后检查
git status
git diff

# 2. 创建发布 commit
git commit -m "chore: prepare for v1.0.0 release"

# 3. 创建 tag
git tag -a v1.0.0 -m "Initial release"

# 4. Push 代码和 tag
git push origin main
git push origin v1.0.0

# 5. 在 GitHub 上创建 Release
# 访问 https://github.com/your-username/obsidian-dashboard/releases/new
```

## 🎯 发布目标

**第一周**: 
- 50+ Stars ⭐
- 10+ Clones
- 2-3 Issues/讨论

**第一个月**:
- 300+ Stars
- 100+ Clones
- 活跃社区讨论

## 📞 反馈收集

- 💬 启用 Discussions 收集想法
- 🐛 跟踪 Issues
- ⭐ 监控 Stars 增长
- 👥 邀请贡献者

---

**准备好了吗？开始发布吧！🚀**
