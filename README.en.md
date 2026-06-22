# 🏠 obsidian-dashboard

[English](README.en.md) | [中文](README.md)

A **personal workspace Dashboard** built on Obsidian + the Dataview plugin. No extra plugins required (besides Dataview) — pure CSS + DataviewJS, works out of the box.

> **Feature-complete** • **Highly customizable** • **Performance-optimized** • **English & Chinese support**

## Preview

![Dataview Dashboard preview](dashboard.png)

The Dashboard includes the following modules:

- **🧭 Domain Navigation** — auto-counts notes per folder; click to expand sub-folders
- **📆 Calendar** — interactive month view; click a date to create/open a daily note
- **✅ Todo Aggregation** — manage to-dos in one place; add / complete / delete
- **📝 Quick Notes** — jot down ideas, auto-saved
- **📌 Upcoming** — auto-extracts future events from daily notes
- **📋 Project Progress** — visual progress bars + status tags
- **🎬🔗🐙 Bookmarks (three columns)** — bookmark videos / blogs / GitHub projects (maintained in hidden data files)
- **✔️ Recently Completed** — review of finished to-dos
- **🕒 Recently Modified** — list of recently edited notes

## Installation

### 1. Install the required plugin

Install the community plugin **Dataview** in Obsidian and enable these in its settings:
- ✅ Enable JavaScript Queries (enables DataviewJS)
- ✅ Enable Inline JavaScript Queries

### 2. Copy the files into your Vault

Drop the files from this project into the root of your Obsidian Vault, following this structure:

```
your-vault/
├── .obsidian/
│   └── snippets/
│       └── Dashboard.css          ← CSS styles
├── 00 - Daily/                    ← daily-notes folder (used by the Calendar)
├── Home.md                        ← Dashboard main file
├── ★★Todo.md                      ← to-do data
├── ★★Quick Notes.md               ← quick-notes data
├── ★★Project Progress.md          ← project-progress data
├── ★★Video Bookmarks.md           ← video bookmarks data
├── ★★Blog Bookmarks.md            ← blog bookmarks data
└── ★★GitHub Bookmarks.md          ← GitHub bookmarks data
```

### 3. Turn off Safe Mode & install Dataview

1. Open Obsidian Settings → Community plugins
2. **Turn off "Safe Mode"** (otherwise community plugins and DataviewJS won't work)
3. Click "Browse", search for and install **Dataview**
4. In Dataview settings, enable **Enable JavaScript Queries**

### 4. Enable the CSS snippet

1. Open Obsidian Settings → Appearance → CSS snippets
2. Click the refresh button
3. Enable the `Dashboard` snippet

### 5. Customize domain navigation

Edit the `domains` logic in `Home.md`. The dashboard auto-scans root-level folders as domains; excluded folders are listed in the `excludeFolders` set near the top of the script. Rename or add folders to match your own vault.

### 6. Set the default reading view

The Dashboard relies on Reading View to render DataviewJS; otherwise the code is shown when opened.

- **Option A (recommended)**: `Home.md` already includes `obsidianUIMode: preview` in its frontmatter, so it opens in Reading View automatically — no extra setup.
- **Option B (global)**: Settings → Editor → Default editing mode → choose "Reading View". All notes then open in Reading View; press `Cmd/Ctrl + E` to switch when you need to edit.

### 7. Set as homepage (optional)

Install the community plugin **Homepage** and set the startup page to `Home`.

## ✨ Features

| Feature | This project | Other dashboard solutions |
|---------|:------------:|:--:|
| Domain navigation | ✅ | - |
| Calendar system | ✅ | - |
| Todo management | ✅ | ✅ |
| Progress visualization | ✅ | - |
| Pure CSS implementation | ✅ | ❌ |
| No extra plugins needed | ✅ | ❌ |
| English & Chinese support | ✅ | - |

## 🔧 Dependencies

**Required:**
- [Obsidian](https://obsidian.md) v1.0+
- [Dataview plugin](https://github.com/blacksmithgu/obsidian-dataview) v0.5+

**Optional / recommended:**
- [Homepage](https://github.com/miracles-dev/obsidian-homepage) — set a home page
- [Editor Width Slider](https://github.com/MugishoMp/obsidian-editor-width-slider) — adjust the editing width

## 🆘 FAQ

### Q: The Dashboard shows no content / no styling?
**A:** Make sure:
1. Dataview is installed and JavaScript is enabled
2. `Home.md` frontmatter has `obsidianUIMode: preview`
3. **The `Dashboard` snippet is enabled under Settings → Appearance → CSS snippets** (the most commonly missed step)
4. No other styles are conflicting

### Q: Clicking the calendar doesn't create a daily note?
**A:** Check that the `00 - Daily/` folder exists and that the date-path config in `Home.md` is correct.

### Q: How do I change the theme colors?
**A:** Edit the CSS variables in `.obsidian/snippets/Dashboard.css`:
```css
:root {
  --color-primary: #5e81ac;  /* primary color */
  --color-accent: #bf616a;   /* accent color */
}
```

## 🚀 Quick start

**Don't want to configure manually?** Follow this quick flow:

> ⚠️ **Important:** Besides installing Dataview, you **must additionally** enable the `Dashboard` snippet under **Settings → Appearance → CSS snippets**, otherwise the Dashboard will be unstyled (raw code only).

```bash
# 1. Clone the project
git clone https://github.com/your-username/obsidian-dashboard.git

# 2. Open that directory as a Vault in Obsidian

# 3. Install the Dataview plugin and enable JavaScript

# 4. Settings → Appearance → CSS snippets → click refresh → enable "Dashboard"

# 5. Open Home.md — the Dashboard is ready to use!
```

## 📚 Docs & resources

- 📖 [Detailed install guide](./INSTALL.md) — step by step
- 🔧 [Advanced customization](./CUSTOMIZATION.md) — deep-dive tutorial
- 💬 [Discussions](../../discussions) — share ideas and ask questions
- 🐛 [Report a bug](../../issues) — submit an issue

## 🤝 Contributing

Contributions of all kinds are welcome! See the [contributing guide](CONTRIBUTING.md).

## ☕ Support the project

If this project helps you, you can support us in the following ways:

### 💝 Donate

**Alipay** — scan the QR code below to donate

![Alipay QR code](docs/alipay-qrcode.png)

> **⚠️ Disclaimer**
>
> - This project is free open-source software. Donations are **entirely voluntary** — all features are fully available whether or not you donate.
> - A donation is only encouragement and support for the author and **does not constitute** any commercial service, technical commitment, or after-sales guarantee.
> - Please donate responsibly within your means; any amount is deeply appreciated.

Your support helps us:
- ✨ Develop new features
- 🐛 Fix more bugs
- 📚 Improve documentation
- 💪 Keep maintaining the project

### ⭐ Other ways to support

- **Star this repo** — help more people discover the project
- **Share** — recommend it to friends
- **Contribute** — submit code or documentation improvements
- **Feedback** — open Issues and suggestions

## 📜 License

MIT License — see [LICENSE](LICENSE)

## ❤️ Acknowledgements

Thanks to everyone who uses, gives feedback on, and contributes to this project!

---

<div align="center">

**[⬆ Back to top](#-obsidian-dashboard)**

Made with ❤️ for Obsidian users

</div>
