# 贡献指南 (CONTRIBUTING)

感谢你有兴趣为本项目做出贡献！🎉

## 📋 行为准则

本项目遵循 [贡献者行为准则](CODE_OF_CONDUCT.md)。参与本项目，即表示你同意遵守该准则。

## 🚀 如何贡献

### 报告 Bug

在报告 bug 前，请先检查 [Issues](../../issues) 列表，确保问题还未被报告。

提交 bug 报告时，请包含以下信息：

- **清晰的标题和描述**
- **操作系统和 Obsidian 版本**
- **Dataview 插件版本**
- **复现步骤**
- **实际行为 vs 预期行为**
- **截图或屏幕录制**（如果适用）

### 建议功能改进

我们欢迎功能建议！提交建议时，请：

- 使用清晰的标题
- 详细描述建议的功能
- 解释为什么这会很有用
- 列出现有产品中的类似示例（如果有）

### 提交 Pull Request

1. **Fork 此仓库** 并从 `main` 分支创建新分支
2. **按照项目结构组织代码** - CSS 文件在 `.obsidian/snippets/`，Markdown 文件在根目录
3. **更新 README.md** - 如果有必要的话
4. **更新 CHANGELOG.md** - 记录你的改动
5. **添加/修改说明** - 代码注释清晰，CSS 选择器有说明

### 代码风格指南

- **CSS**: 保持现有风格，使用有意义的选择器名称
- **Markdown**: 遵循现有格式，使用中文编写中文注释
- **提交信息**: 使用英文，格式为 `feat: 功能名` 或 `fix: bug修复描述`

## 📝 提交工作流

```bash
# 1. Fork 并 clone
git clone https://github.com/YOUR_USERNAME/obsidian-dashboard.git
cd obsidian-dashboard

# 2. 创建特性分支
git checkout -b feat/your-feature-name

# 3. 做出改动并提交
git add .
git commit -m "feat: add your feature description"

# 4. Push 到你的 fork
git push origin feat/your-feature-name

# 5. 打开 Pull Request
```

## ✅ PR 审核标准

我们会检查以下内容：

- 代码质量和一致性
- 是否有完整的说明文档
- 是否包含测试或截图验证
- 是否与项目的目标一致

## 📚 开发环境设置

1. 克隆仓库：`git clone https://github.com/your-username/obsidian-dashboard.git`
2. 在 Obsidian 中打开该目录作为 Vault
3. 安装 Dataview 社区插件
4. 启用 JavaScript 查询
5. 打开 `Home.md` 查看 Dashboard

## 📞 获取帮助

- **讨论功能想法** - 开启 [Discussion](../../discussions)
- **需要支持** - 提交 [Issue](../../issues)
- **想聊天** - 在 Discussions 中加入讨论

## ✨ 致谢

感谢所有做出贡献的人！你们让这个项目变得更好。❤️

---

**Happy contributing! 🚀**
