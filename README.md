# Ban-Code
A VS Code Remake in HTML

Website at https://202011.my.canva.site/ban-code-editor

# 🚫 Ban Code v3.0 — Extension-Powered Editor

A complete, self-contained code editor that runs from a **single `index.html`** file.
App-like workspace with a file explorer, Monaco-powered editing, terminal, command palette,
search, themes, settings — **plus a powerful extension system driven by plain `.json` files**.

No build step. Just open `index.html` in a browser (internet needed on first load for Monaco).

---

## ✨ What's new in v3.0

### Extensions can now restyle & extend everything 🎨
- **`ui`** — change accent color, background, sidebar, titlebar, corner radius, fonts,
  brand name/logo, inject custom CSS, or hide UI parts.
- **Themes with `chrome`** — contributed themes now paint *the whole page*, not just the editor.
- **`statusBarItems`** — add clickable items to the status bar.
- **`activityBarButtons`** — add buttons to the activity bar.
- **`views`** — custom sidebar panels with your own HTML.
- **`menus`** — contribute items into File/Edit/View/Help **or create brand-new top-level menus**.
- **`languages` & `fileIcons`** — register extensions → languages and custom file icons.
- **JavaScript commands** — `"action": "javascript"` runs code with a full `ban` API
  (read/replace text & selection, switch themes, open files, tweak settings…).
- **Multi-step commands** (`multi`), `setSetting`, `copy`, `openUrl`, `theme`, `zenMode`,
  `format`, `newFile`, `save`.
- **Startup activation** — run commands automatically when the editor boots.

### Editor features
- ☁️ **Auto-save workspace** to browser storage + restore on reload.
- 🧘 **Zen mode** (`Ctrl+Alt+Z`, Esc exits).
- 🖱️ Tab middle-click close, right-click tab menu (close others / copy path…).
- 🔍 Search results now **jump to the exact line** and highlight the match.
- 🧠 Fuzzy matching in Command Palette & Quick Open.
- ⌨️ Richer terminal: `cat`, `touch`, `mkdir`, `rm`, `open`, `wc`, `grep`, `history`, and more.
- New editor settings: font ligatures, line height, indent guides, sticky scroll,
  smooth caret, whitespace rendering, cursor blink styles.
- Smarter status bar: Ln/Col + selection size + word/char count.
- Smart drag-and-drop: dropping a project `.json` loads it as a project; extension `.json` installs.

### 🖼️ Media viewers & HTML live preview
- **Images** (`png jpg jpeg gif webp bmp ico cur avif`) open in a viewer — plus natural size info.
- **Audio** (`mp3 wav ogg m4a flac aac opus`…) and **video** (`mp4 webm mov mkv`…)
  get native playback controls right in the editor area.
- **HTML files** show a Code / Preview split bar: edit in Monaco or flip to a
  sandboxed **live preview** that refreshes as you type. `↗ Browser` opens the page in a real tab.
  Toggle via **View → Toggle HTML Preview**, the Command Palette, or right-click an `.html` file.
- Media files are imported & saved as data URLs inside project JSON (SVG stays editable text),
  they persist across reloads, and search/word-count skip them automatically.

### Theme layering (fixed)
- Switching from an extension theme back to a **built-in theme now applies fully**:
  every inline CSS variable is wiped on each switch, so no colors leak between themes.
- Extension `ui` **color** contributions only overlay when a *contributed* theme is active —
  picking Ban Dark / Ban Light / High Contrast always looks like the real built-in theme.
  Non-color reskinning (radius, fonts, brand, custom CSS) still applies everywhere.

---

## 🧩 The extension system

Extensions are **plain `.json` files**. Install via the Extensions panel (🧩) or drag onto the window.

### Contribution points

| Contribution          | What it does                                                                     |
|-----------------------|----------------------------------------------------------------------------------|
| `commands`            | Runnable commands shown in palette/menus — actions: `toast`, `alert`, `terminal`, `insert`, `open`, `javascript`, `multi`, `setSetting`, `copy`, `openUrl`, `theme`, `toggleTerminal`, `toggleSidebar`, `zenMode`, `format`, `newFile`, `save` |
| `ui`                  | Live UI theming: `accent`, `bg`, `sidebar`, `titlebar`, `card`, `radius`, `fontFamily`, `brandName`, `logo`, `css`, `hide[]` — color keys overlay when a contributed theme is active; radius/fonts/css/brand apply always |
| `themes`              | Color themes with Monaco `colors`/`rules` **plus `chrome` page colors**            |
| `snippets`            | Autocomplete snippets per language                                                |
| `terminalCommands`    | Terminal commands: `"cmd": "output"` or `{description, output}`                   |
| `keybindings`         | Keyboard shortcuts bound to commands                                              |
| `statusBarItems`      | `{text, tooltip, alignment, command}` items in the status bar                     |
| `activityBarButtons`  | `{icon, label, command \| view}` buttons in the activity bar                      |
| `views`               | Custom sidebar panels: `{id, title, icon, html}`                                  |
| `menus`               | `{menu:"file\|edit\|view\|help", items[]}` merge into built-ins; `{label, items[]}` creates a new menu |
| `languages`           | Map file extensions to languages                                                  |
| `fileIcons`           | Custom icons per file extension                                                   |
| `settingsDefaults`    | Settings applied when the extension is installed                                  |
| `activationEvents` + `startupCommands` | Run commands at startup                                          |

### The `ban` JavaScript API (for `javascript` commands)

```js
ban.toast(msg)           ban.insert(text)        ban.getText()      ban.setText(t)
ban.getSelection()       ban.activeFile()        ban.openFile(name) ban.files()
ban.setting(key[, val])  ban.theme(id)           ban.terminal(msg)  ban.runCommand(id)
ban.zen()                ban.format()            ban.saveWorkspace() ban.state()
```

### Minimal example

```json
{
  "id": "my.hello",
  "name": "My Extension",
  "version": "1.0.0",
  "publisher": "you",
  "description": "Says hello.",
  "icon": "👋",
  "contributes": {
    "ui": { "accent": "#ff5f56", "radius": 10 },
    "commands": [
      { "id": "my.hello", "title": "Say Hello", "action": "toast", "arg": "Hello!" },
      { "id": "my.js", "title": "Hi from JS", "action": "javascript", "arg": "ban.toast('Hi!')" }
    ],
    "statusBarItems": [
      { "text": "👋 Hi", "alignment": "right", "command": "my.hello" }
    ],
    "keybindings": [
      { "key": "Ctrl+Alt+H", "command": "my.hello", "title": "Say Hello" }
    ]
  }
}
```

> There's a full reference inside the app: **Help → Extension Guide**, plus a live
> manifest template in the Extensions panel.

### Try the included samples (`extensions/` folder)

| File                       | Adds                                                                          |
| -------------------------- | ----------------------------------------------------------------------------- |
| `ui-studio.json`           | Full UI reskin + Synthwave chrome theme, side panel, status bar & activity bar items, custom menu, JS commands |
| `productivity-pack.json`   | Snippets, text transforms, timestamps, extra languages/icons, terminal helpers |
| `console-snippets.json`    | JS/TS snippets + commands + terminal commands                                  |
| `git-tools.json`           | Git commands + a `Ctrl+Shift+G` keybinding                                     |
| `demo-project.json`        | Not an extension — load via **Open JSON**                                      |

Install an extension:
1. Open the **Extensions** panel (🧩 in the activity bar).
2. Click **Choose extension .json** and pick a `.json` file.
3. Or just **drag the `.json` file** onto the window.

Uninstall: open the **Extensions** panel and click **Uninstall** on any card.

---

## ⌨️ Shortcuts

| Shortcut              | Action                        |
| --------------------- | ----------------------------- |
| `Ctrl+N`              | New project                   |
| `Ctrl+S`              | Save file                     |
| `Ctrl+Shift+S`        | Download workspace JSON       |
| `Ctrl+W`              | Close editor tab              |
| `Ctrl+Shift+P`        | Command Palette               |
| `Ctrl+P`              | Quick Open                    |
| `Ctrl+Shift+F`        | Search everywhere             |
| `Ctrl+Shift+X`        | Extensions panel              |
| `Ctrl+` `` ` ``       | Terminal                      |
| `Ctrl+B`              | Toggle sidebar                |
| `Ctrl+,`              | Settings                      |
| `Ctrl+=` / `Ctrl+-`   | Zoom in / out                 |
| `Ctrl+Alt+Z`          | Zen mode (Esc exits)          |
| `Shift+Alt+F`         | Format document               |
| Middle-click tab      | Close tab                     |
| Right-click tab       | Tab menu (close others…)      |

---

## 📁 File layout

```
ban-code-editor/
├── index.html            ← the entire editor (open this)
├── extensions/           ← sample .json extensions & demo project
│   ├── ui-studio.json
│   ├── productivity-pack.json
│   ├── console-snippets.json
│   ├── git-tools.json
│   └── demo-project.json
└── README.md
```

> Requires internet for the Monaco editor and for ZIP export.
> Everything else works offline.
