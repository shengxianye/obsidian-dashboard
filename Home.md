---
editor-width: 100
obsidianUIMode: preview
cssclasses:
  - hide-properties
---

<div class="dash-section-title">🧭 领域导航</div>

```dataviewjs
// === 动态扫描领域导航卡片（含二级目录展开）===
// 自动获取 Vault 根目录下的文件夹作为领域，排除特殊目录
const excludeFolders = new Set(["00 - Daily", "模板", "templates", "附件", "assets"]);

const allFoldersRaw = app.vault.getAllLoadedFiles().filter(f => f.children);
const rootFolders = allFoldersRaw
  .filter(f => f.parent && (f.parent.path === "/" || f.parent.path === "") && !f.name.startsWith(".") && !excludeFolders.has(f.name))
  .sort((a, b) => a.name.localeCompare(b.name, "zh"));

// 为每个文件夹自动分配图标（可通过文件夹名首字匹配或使用默认）
const iconMap = {
  "工作": "💼", "学习": "📚", "项目": "🚀", "技术": "💻",
  "生活": "🌱", "读书": "📖", "笔记": "📝", "设计": "🎨",
  "研究": "🔬", "运维": "🔧", "开发": "⚡", "产品": "📦",
};
function getIcon(name) {
  for (const [key, icon] of Object.entries(iconMap)) {
    if (name.includes(key)) return icon;
  }
  return "📂";
}

const domains = rootFolders.map(f => ({
  name: f.name,
  icon: getIcon(f.name),
  folder: f.path
}));

const allFiles = app.vault.getFiles();
const allFolders = app.vault.getAllLoadedFiles().filter(f => f.children);

const container = dv.container.createEl("div");
const grid = container.createEl("div", { cls: "dash-grid" });

// 二级目录展开区域（在卡片网格下方）
const subPanel = container.createEl("div", { cls: "dash-sub-panel" });
subPanel.style.display = "none";

let activeFolder = null;

for (const d of domains) {
  const prefix = d.folder + "/";
  const files = allFiles.filter(f => f.path.startsWith(prefix));
  const notes = files.filter(f => f.extension === "md").length;

  const card = grid.createEl("div", { cls: "dash-card" });
  card.createEl("div", { text: d.icon, cls: "dash-icon" });
  card.createEl("div", { text: d.name, cls: "dash-name" });
  card.createEl("div", { text: `${notes} 笔记 · ${files.length} 文件`, cls: "dash-meta" });

  card.addEventListener("click", () => {
    // 点击同一个卡片则折叠
    if (activeFolder === d.folder) {
      subPanel.style.display = "none";
      activeFolder = null;
      grid.querySelectorAll(".dash-card").forEach(c => c.classList.remove("dash-card-active"));
      return;
    }
    activeFolder = d.folder;
    grid.querySelectorAll(".dash-card").forEach(c => c.classList.remove("dash-card-active"));
    card.classList.add("dash-card-active");

    // 获取二级子目录
    const subFolders = allFolders.filter(f => {
      if (!f.parent) return false;
      return f.parent.path === d.folder;
    }).sort((a, b) => a.name.localeCompare(b.name, "zh"));

    subPanel.innerHTML = "";
    subPanel.style.display = "block";

    const header = subPanel.createEl("div", { cls: "sub-panel-header" });
    header.createEl("span", { text: `${d.icon} ${d.name} 的子目录` });
    const searchAll = header.createEl("a", { text: "搜索全部 →", cls: "sub-panel-search-all" });
    searchAll.addEventListener("click", () => {
      app.internalPlugins.getPluginById("global-search").instance.openGlobalSearch(`path:"${d.folder}"`);
    });

    if (subFolders.length === 0) {
      subPanel.createEl("div", { text: "无子目录，点击上方「搜索全部」查看所有笔记", cls: "sub-panel-empty" });
    } else {
      const subGrid = subPanel.createEl("div", { cls: "dash-sub-grid" });
      for (const sf of subFolders) {
        const sfFiles = allFiles.filter(f => f.path.startsWith(sf.path + "/"));
        const sfNotes = sfFiles.filter(f => f.extension === "md").length;
        const chip = subGrid.createEl("div", { cls: "dash-sub-chip" });
        chip.createEl("span", { text: `📂 ${sf.name}`, cls: "sub-chip-name" });
        chip.createEl("span", { text: `${sfNotes} 篇`, cls: "sub-chip-count" });
        chip.addEventListener("click", () => {
          app.internalPlugins.getPluginById("global-search").instance.openGlobalSearch(`path:"${sf.path}"`);
        });
      }
    }
  });
}
```

```dataviewjs
// === 三栏布局：日历 + 待办 + 速记 ===
const { DateTime } = dv.luxon;
const now = DateTime.now();
const todayStr = now.toFormat("yyyy-MM-dd");
const dailyFile = `00 - Daily/${todayStr}.md`;
const dailyFiles = app.vault.getMarkdownFiles().filter(f => f.path.startsWith("00 - Daily/"));
const dateSet = new Set(dailyFiles.map(f => f.basename));
const weekLabels = ["一", "二", "三", "四", "五", "六", "日"];

let currentYear = now.year;
let currentMonth = now.month;

// ===== 外层三栏 =====
const wrapper = dv.container.createEl("div", { cls: "dash-panel" });
const leftCol = wrapper.createEl("div", { cls: "dash-panel-left" });   // 日历
const rightCol = wrapper.createEl("div", { cls: "dash-panel-mid" });   // 待办
const noteCol = wrapper.createEl("div", { cls: "dash-panel-right" });  // 速记

// ===== 中栏：待办聚合 =====
rightCol.createEl("div", { text: "✅ 待办聚合", cls: "dash-section-title" });

const todoFile = "★★待办.md";

// 解析待办文件
async function loadTodos() {
  const f = app.vault.getAbstractFileByPath(todoFile);
  if (!f) return [];
  try {
    const content = await app.vault.read(f);
    const lines = content.split("\n");
    const todos = [];
    lines.forEach((line, idx) => {
      const trimmed = line.trim();
      if (trimmed.startsWith("- [ ] ")) {
        todos.push({ text: trimmed.replace("- [ ] ", ""), done: false, line: idx });
      } else if (trimmed.startsWith("- [x] ")) {
        todos.push({ text: trimmed.replace("- [x] ", ""), done: true, line: idx });
      }
    });
    return todos;
  } catch (e) {
    return [];
  }
}

// 保存待办文件
async function saveTodos(todos) {
  const lines = todos.map(t => t.done ? `- [x] ${t.text}` : `- [ ] ${t.text}`);
  const content = lines.join("\n");
  let file = app.vault.getAbstractFileByPath(todoFile);
  if (!file) {
    await app.vault.create(todoFile, content);
  } else {
    await app.vault.modify(file, content);
  }
}

// 加载并显示待办
let todos = await loadTodos();

// 新增按钮
const btnWrap = rightCol.createEl("div", { cls: "todo-btn-wrap" });
const addBtn = btnWrap.createEl("button", { text: "+ 新增待办", cls: "todo-action-btn todo-add-btn" });
addBtn.addEventListener("click", async () => {
  const modal = new obsidian.Modal(app);
  modal.titleEl.setText("新增待办");
  const inputEl = modal.contentEl.createEl("input", {
    attr: { type: "text", placeholder: "输入待办内容…", style: "width:100%;padding:10px;font-size:1rem;border-radius:8px;border:1.5px solid var(--background-modifier-border);background:var(--background-primary);color:var(--text-normal);" }
  });
  const confirmBtn = modal.contentEl.createEl("button", {
    text: "确认添加",
    attr: { style: "margin-top:12px;padding:8px 20px;border-radius:8px;background:var(--color-accent);color:var(--text-on-accent,#fff);border:none;cursor:pointer;font-size:0.9rem;" }
  });
  const doAdd = async () => {
    const text = inputEl.value.trim();
    if (!text) return;
    todos.push({ text, done: false });
    await saveTodos(todos);
    new Notice("✅ 已添加待办");
    modal.close();
    const leaf = app.workspace.activeLeaf;
    if (leaf) { leaf.rebuildView(); }
  };
  confirmBtn.addEventListener("click", doAdd);
  inputEl.addEventListener("keydown", (e) => { if (e.key === "Enter") doAdd(); });
  modal.open();
  setTimeout(() => inputEl.focus(), 50);
});

// 只显示未完成的待办
const pendingTodos = todos.filter(t => !t.done).slice(0, 15);

if (pendingTodos.length === 0) {
  rightCol.createEl("div", { text: "🎉 暂无待办，太棒了！", cls: "todo-empty" });
} else {
  const list = rightCol.createEl("div", { cls: "todo-list" });
  for (let i = 0; i < pendingTodos.length; i++) {
    const t = pendingTodos[i];
    const row = list.createEl("div", { cls: "todo-row" });
    const checkbox = row.createEl("input", { attr: { type: "checkbox" }, cls: "todo-checkbox" });
    checkbox.addEventListener("change", async () => {
      const origIdx = todos.findIndex(item => item.text === t.text && !item.done);
      if (origIdx !== -1) {
        todos[origIdx].done = true;
        await saveTodos(todos);
        row.style.opacity = "0.4";
        row.style.textDecoration = "line-through";
        new Notice("✅ 已完成");
      }
    });
    row.createEl("span", { text: t.text, cls: "todo-text" });
    const delBtn = row.createEl("button", { text: "×", cls: "todo-action-btn todo-del-btn" });
    delBtn.addEventListener("click", async () => {
      const origIdx = todos.findIndex(item => item.text === t.text && !item.done);
      if (origIdx !== -1) {
        todos.splice(origIdx, 1);
        await saveTodos(todos);
        row.remove();
        new Notice("🗑 已删除");
      }
    });
  }
}

// 打开完整待办文件链接
rightCol.createEl("a", { text: "在编辑器中打开 →", cls: "internal-link scratch-open", href: todoFile });

// ===== 左栏：日历 =====
leftCol.createEl("div", { text: "📆 日历", cls: "dash-section-title" });
const calMain = leftCol.createEl("div", { cls: "cal-main" });

function renderCalendar() {
  calMain.innerHTML = "";
  const nav = calMain.createEl("div", { cls: "cal-nav" });
  nav.createEl("button", { text: "‹", cls: "cal-nav-btn" }).addEventListener("click", () => {
    currentMonth--;
    if (currentMonth < 1) { currentMonth = 12; currentYear--; }
    renderCalendar();
  });
  nav.createEl("span", { text: `${currentYear}.${String(currentMonth).padStart(2,"0")}`, cls: "cal-nav-title" });
  nav.createEl("button", { text: "›", cls: "cal-nav-btn" }).addEventListener("click", () => {
    currentMonth++;
    if (currentMonth > 12) { currentMonth = 1; currentYear++; }
    renderCalendar();
  });


  const grid = calMain.createEl("div", { cls: "cal-grid" });
  for (const w of weekLabels) {
    grid.createEl("div", { text: w, cls: "cal-weekday" });
  }
  const firstDay = DateTime.local(currentYear, currentMonth, 1);
  const daysInMonth = firstDay.daysInMonth;
  const startWeekday = firstDay.weekday;
  for (let i = 1; i < startWeekday; i++) {
    grid.createEl("div", { cls: "cal-empty" });
  }
  for (let d = 1; d <= daysInMonth; d++) {
    const dateStr = `${currentYear}-${String(currentMonth).padStart(2,"0")}-${String(d).padStart(2,"0")}`;
    const hasNote = dateSet.has(dateStr);
    const isToday = (currentYear === now.year && currentMonth === now.month && d === now.day);
    let cls = "cal-day cal-clickable";
    if (isToday) cls += " cal-today";
    if (hasNote) cls += " cal-has-note";
    if (hasNote) {
      grid.createEl("a", { text: String(d), cls: cls + " internal-link", href: `00 - Daily/${dateStr}` });
    } else {
      const dayEl = grid.createEl("div", { text: String(d), cls: cls });
      dayEl.addEventListener("click", async () => {
        const filePath = `00 - Daily/${dateStr}.md`;
        let file = app.vault.getAbstractFileByPath(filePath);
        if (!file) {
          const weekday = DateTime.local(currentYear, currentMonth, d).toFormat("cccc");
          await app.vault.create(filePath, `# 📅 ${dateStr} ${weekday}\n\n## 🎯 今日目标\n- [ ] \n\n## 📝 工作记录\n\n\n## 📌 事件记录\n\n\n## 💡 灵感 & 思考\n\n\n## ✅ 今日复盘\n\n`);
          dateSet.add(dateStr);
          new Notice(`📅 已创建 ${dateStr} 日记`);
        }
        const created = app.vault.getAbstractFileByPath(filePath);
        if (created) {
          await app.workspace.getLeaf(false).openFile(created);
        }
      });
    }
  }
}
renderCalendar();

// ===== 日历下方：未来事件清单 =====
leftCol.createEl("div", { text: "📌 upcoming", cls: "dash-section-title upcoming-title" });
const upcomingBox = leftCol.createEl("div", { cls: "upcoming-list" });

// 从未来日记中读取实际事件内容
const futureDays = 30;
let upcomingItems = [];

for (let i = 0; i <= futureDays; i++) {
  const dt = now.plus({ days: i });
  const dateStr = dt.toFormat("yyyy-MM-dd");
  const file = dailyFiles.find(f => f.basename === dateStr);
  if (!file) continue;

  try {
    const content = await app.vault.read(file);
    const lines = content.split("\n");

    // 提取"事件记录"区块下的内容
    let inEventSection = false;
    for (const line of lines) {
      if (/^##\s.*事件记录/.test(line)) { inEventSection = true; continue; }
      if (/^##\s/.test(line) && inEventSection) break;
      if (inEventSection) {
        const text = line.replace(/^[-*>\s]+/, "").trim();
        if (text && !text.startsWith("#")) {
          upcomingItems.push({ date: dt.toFormat("MM-dd"), weekday: dt.toFormat("ccc"), text, path: file.path, type: "event" });
        }
      }
    }
  } catch (e) {
    console.warn("读取日记文件失败:", dateStr, e);
  }
}

if (upcomingItems.length === 0) {
  upcomingBox.createEl("div", { text: "未来 30 天暂无事件", cls: "scratch-empty" });
} else {
  for (const ev of upcomingItems.slice(0, 10)) {
    const row = upcomingBox.createEl("div", { cls: "upcoming-row" });
    row.createEl("span", { text: `${ev.date}`, cls: "upcoming-date" });
    const icon = ev.type === "todo" ? "☐" : "•";
    row.createEl("span", { text: icon, cls: "upcoming-icon" });
    const link = row.createEl("a", { text: ev.text, cls: "internal-link upcoming-text", href: ev.path });
  }
}

// ===== 右栏：速记白板 =====
const scratchFile = "★★速记.md";
noteCol.createEl("div", { text: "📝 速记", cls: "dash-section-title" });

const textarea = noteCol.createEl("textarea", {
  cls: "scratch-textarea",
  attr: { placeholder: "随手记录任何想法…\n内容自动保存到「★★速记.md」" }
});

try {
  const sf = app.vault.getAbstractFileByPath(scratchFile);
  if (sf) {
    const content = await app.vault.read(sf);
    textarea.value = content;
  }
} catch (e) {
  console.warn("读取速记文件失败:", e);
}

let saveTimer = null;
const doSave = async () => {
  let file = app.vault.getAbstractFileByPath(scratchFile);
  if (!file) {
    await app.vault.create(scratchFile, textarea.value);
  } else {
    await app.vault.modify(file, textarea.value);
  }
};
textarea.addEventListener("input", () => {
  clearTimeout(saveTimer);
  saveTimer = setTimeout(doSave, 1000);
});
textarea.addEventListener("blur", doSave);

noteCol.createEl("a", { text: "在编辑器中打开 →", cls: "internal-link scratch-open", href: scratchFile });
```

```dataviewjs
// === 收藏三栏：视频 + 博客 + GitHub ===
const videoFile = "★★收藏视频.md";
const blogFile = "★★收藏博客.md";
const githubFile = "★★收藏GitHub.md";

// 解析 Markdown 表格
async function parseTable(filePath) {
  const f = app.vault.getAbstractFileByPath(filePath);
  if (!f) return [];
  const content = await app.vault.read(f);
  const lines = content.split("\n").map(l => l.trim());
  return lines.filter(l =>
    l.startsWith("|") &&
    !l.match(/^\|[\s-|]+\|$/) &&
    !l.toLowerCase().includes("标题") &&
    !l.startsWith(">")
  ).map(row => {
    const cells = row.split("|").map(c => c.trim()).filter((_, i, arr) => i > 0 && i < arr.length - 1);
    return { title: cells[0] || "", url: cells[1] || "", tag: cells[2] || "" };
  }).filter(item => item.title && item.url);
}

const videos = await parseTable(videoFile);
const blogs = await parseTable(blogFile);
const githubs = await parseTable(githubFile);

const wrapper = dv.container.createEl("div", { cls: "fav-two-col" });

// --- 左栏：收藏视频 ---
const leftCol = wrapper.createEl("div", { cls: "fav-col" });
const leftHeader = leftCol.createEl("div", { cls: "fav-col-header" });
leftHeader.createEl("span", { text: "🎬 收藏视频", cls: "fav-col-title" });
const leftEdit = leftHeader.createEl("a", { text: "编辑 →", cls: "fav-col-edit internal-link", href: videoFile });

if (videos.length === 0) {
  leftCol.createEl("div", { text: "暂无收藏视频", cls: "fav-empty" });
} else {
  const leftList = leftCol.createEl("div", { cls: "fav-item-list" });
  for (const v of videos) {
    const item = leftList.createEl("a", { cls: "fav-item", href: v.url, attr: { target: "_blank" } });
    item.createEl("span", { text: "▶️", cls: "fav-item-icon" });
    item.createEl("span", { text: v.title, cls: "fav-item-title" });
    if (v.tag) item.createEl("span", { text: v.tag, cls: "fav-item-tag" });
  }
}

// --- 中栏：收藏博客 ---
const midCol = wrapper.createEl("div", { cls: "fav-col" });
const midHeader = midCol.createEl("div", { cls: "fav-col-header" });
midHeader.createEl("span", { text: "🔗 收藏博客", cls: "fav-col-title" });
const midEdit = midHeader.createEl("a", { text: "编辑 →", cls: "fav-col-edit internal-link", href: blogFile });

if (blogs.length === 0) {
  midCol.createEl("div", { text: "暂无收藏博客", cls: "fav-empty" });
} else {
  const midList = midCol.createEl("div", { cls: "fav-item-list" });
  for (const b of blogs) {
    const item = midList.createEl("a", { cls: "fav-item", href: b.url, attr: { target: "_blank" } });
    item.createEl("span", { text: "📄", cls: "fav-item-icon" });
    item.createEl("span", { text: b.title, cls: "fav-item-title" });
    if (b.tag) item.createEl("span", { text: b.tag, cls: "fav-item-tag" });
  }
}

// --- 右栏：GitHub 项目 ---
const rightCol = wrapper.createEl("div", { cls: "fav-col" });
const rightHeader = rightCol.createEl("div", { cls: "fav-col-header" });
rightHeader.createEl("span", { text: "🐙 GitHub 项目", cls: "fav-col-title" });
const rightEdit = rightHeader.createEl("a", { text: "编辑 →", cls: "fav-col-edit internal-link", href: githubFile });

if (githubs.length === 0) {
  rightCol.createEl("div", { text: "暂无 GitHub 收藏", cls: "fav-empty" });
} else {
  const rightList = rightCol.createEl("div", { cls: "fav-item-list" });
  for (const g of githubs) {
    const item = rightList.createEl("a", { cls: "fav-item", href: g.url, attr: { target: "_blank" } });
    item.createEl("span", { text: "⭐", cls: "fav-item-icon" });
    item.createEl("span", { text: g.title, cls: "fav-item-title" });
    if (g.tag) item.createEl("span", { text: g.tag, cls: "fav-item-tag" });
  }
}
```

<div class="dash-section-title">📋 项目进度及跟进</div>

```dataviewjs
// === 项目进度及跟进 ===
const projFile = "★★项目进度.md";
const file = app.vault.getAbstractFileByPath(projFile);

if (!file) {
  dv.container.createEl("div", { text: "未找到项目进度文件「★★项目进度.md」，请先创建。", cls: "todo-empty" });
} else {
  try {
    const content = await app.vault.read(file);
    const lines = content.split("\n").map(l => l.trim());

    // 解析 Markdown 表格行（跳过表头、分隔行和空行）
    const dataRows = lines.filter(l =>
      l.startsWith("|") &&
      !l.match(/^\|[\s-|]+\|$/) &&
      !l.toLowerCase().includes("项目名") &&
      !l.startsWith(">") &&
      l.replace(/[\s|]/g, "").length > 0
    );

    if (dataRows.length === 0) {
      dv.container.createEl("div", { text: "暂无项目记录，请在「★★项目进度.md」中添加。", cls: "todo-empty" });
    } else {
      const list = dv.container.createEl("div", { cls: "proj-list" });

      for (const row of dataRows) {
        const cells = row.split("|").map(c => c.trim()).filter((_, i, arr) => i > 0 && i < arr.length - 1);
        if (cells.length < 4) continue;

        const name = cells[0] || "";
        const status = cells[1] || "进行中";
        let progress = parseInt(cells[2], 10);
        if (isNaN(progress)) progress = 0;
        progress = Math.max(0, Math.min(100, progress));
        const owner = cells[3] || "-";
        const linkRaw = cells[4] || "";

        // 解析 wikilink [[...]] 或标准链接 [text](path)
        let targetPath = null;
        const wikiMatch = linkRaw.match(/\[\[([^\]]+)\]\]/);
        const mdMatch = linkRaw.match(/\[.*?\]\(([^)]+)\)/);
        if (wikiMatch) {
          targetPath = wikiMatch[1].trim();
        } else if (mdMatch) {
          targetPath = decodeURIComponent(mdMatch[1].trim());
        }

        // 状态标签样式
        const statusMap = {
          "进行中": { cls: "proj-status-active", label: "🟢 进行中" },
          "已完成": { cls: "proj-status-done", label: "✅ 已完成" },
          "阻塞": { cls: "proj-status-blocked", label: "🔴 阻塞" },
          "未启动": { cls: "proj-status-pending", label: "⚪ 未启动" },
        };
        const st = statusMap[status] || statusMap["进行中"];

        // 进度条颜色
        let barCls = "proj-progress-bar-low";
        if (progress >= 100) barCls = "proj-progress-bar-done";
        else if (progress >= 71) barCls = "proj-progress-bar-high";
        else if (progress >= 31) barCls = "proj-progress-bar-mid";

        const rowEl = list.createEl("div", { cls: "proj-row" + (targetPath ? " proj-row-clickable" : "") });

        rowEl.createEl("div", { text: name, cls: "proj-name" });
        rowEl.createEl("span", { text: st.label, cls: "proj-status " + st.cls });

        const wrap = rowEl.createEl("div", { cls: "proj-progress-wrap" });
        const track = wrap.createEl("div", { cls: "proj-progress-track" });
        const bar = track.createEl("div", { cls: "proj-progress-bar " + barCls });
        bar.style.width = progress + "%";
        wrap.createEl("span", { text: progress + "%", cls: "proj-progress-text" });

        rowEl.createEl("span", { text: owner, cls: "proj-owner" });

        if (targetPath) {
          rowEl.addEventListener("click", async () => {
            let f = app.vault.getAbstractFileByPath(targetPath);
            if (!f) {
              f = app.vault.getAbstractFileByPath(targetPath + ".md");
            }
            if (f) {
              await app.workspace.getLeaf(false).openFile(f);
            } else {
              new Notice("⚠ 未找到文件：" + targetPath);
            }
          });
        }
      }

      // 底部打开项目进度文件
      const openLink = dv.container.createEl("a", {
        text: "在编辑器中打开「★★项目进度.md」维护 →",
        cls: "proj-open-link internal-link",
        href: projFile
      });
    }
  } catch (e) {
    dv.container.createEl("div", { text: "读取项目进度失败，请检查文件格式", cls: "todo-empty" });
  }
}
```

<div class="dash-section-title">✔️ 最近完成</div>

```dataviewjs
// === 最近完成的待办 ===
const todoFile = "★★待办.md";
const file = app.vault.getAbstractFileByPath(todoFile);

let doneTasks = [];
if (file) {
  try {
    const content = await app.vault.read(file);
    const lines = content.split("\n");
    let idx = 0;
    for (const line of lines) {
      const trimmed = line.trim();
      if (trimmed.startsWith("- [x] ")) {
        doneTasks.push({ text: trimmed.replace("- [x] ", ""), line: idx++ });
      }
    }
  } catch (e) {
    console.warn("读取待办文件失败:", e);
  }
}
// 只显示最近100条
doneTasks = doneTasks.slice(0, 100);

const box = dv.container.createEl("div", { cls: "done-list", attr: { style: "max-height: 600px; overflow-y: auto;" } });
if (doneTasks.length === 0) {
  box.createEl("div", { text: "暂无已完成待办", cls: "todo-empty" });
} else {
  for (const t of doneTasks) {
    const row = box.createEl("div", { cls: "recent-row" });
    row.createEl("span", { text: "✓", cls: "done-check" });
    row.createEl("span", { text: t.text, cls: "done-text" });
  }
}
```

<div class="dash-section-title">🕒 最近修改</div>

```dataviewjs
// === 最近修改的笔记 ===
const exclude = ["Home.md"];
const files = app.vault.getMarkdownFiles()
  .filter(f => !exclude.includes(f.path) && !f.path.startsWith("模板"))
  .sort((a, b) => b.stat.mtime - a.stat.mtime)
  .slice(0, 100);

const box = dv.el("div", "", { cls: "dash-recent", attr: { style: "max-height: 600px; overflow-y: auto;" } });
for (const f of files) {
  const row = box.createEl("div", { cls: "recent-row" });
  const left = row.createEl("div", { cls: "recent-link-wrap" });
  left.createEl("a", { text: f.basename, cls: "internal-link recent-link", href: f.path });
  const folder = f.parent && f.parent.path !== "/" ? f.parent.name : "";
  if (folder) left.createEl("span", { text: folder, cls: "recent-folder" });
  const dt = dv.luxon.DateTime.fromMillis(f.stat.mtime);
  row.createEl("span", { text: dt.toFormat("MM-dd HH:mm"), cls: "recent-date" });
}
```
