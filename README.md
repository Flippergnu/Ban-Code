# Ban-Code
A VS Code Remake in HTML

Website at https://202011.my.canva.site/ban-code-editor

# 🚫 Ban Code — Extension-Powered Editor

A complete, self-contained code editor that runs from a **single `index.html`** file.
It's a modern upgrade of the classic "Ban Code" editor: an app-like workspace with
a file explorer, Monaco-powered editing, terminal, command palette, search, themes,
settings — **plus a real extension system driven by plain `.json` files**.

No build step. Just open `index.html` in a browser (internet needed on first load for Monaco).

---

## ✨ What's new / improved

- **JSON extension system** — the headline feature (see below).
- **Install & uninstall extensions from the Extensions tab.**
- **Command Palette** (`Ctrl+Shift+P`) and **Quick Open** (`Ctrl+P`).
- **Settings that actually work** — live theme, font size, tab size, word wrap,
  minimap, line numbers, bracket coloring, cursor blink, auto-save.
- **Local persistence** — settings and installed extensions survive reloads
  (`localStorage`).
- **Drag-and-drop install** — drop a `.json` extension onto the editor to install it.
- **Open a whole folder** (`Ctrl+K O`) or **load a project from JSON**.
- **Export your workspace as a ZIP**.
- Real Monaco editor with syntax highlighting, minimap, bracket matching, snippets.

---

## 🧩 The extension system

Extensions are **plain `.json` files** (VS Code-manifest style). You choose any
`.json` file to open and install it — or drag it onto the window.

### What an extension can contribute

| Contribution      | What it does                                                            |
|-------------------|-------------------------------------------------------------------------|
| `commands`        | Adds runnable commands (toast, alert, terminal output, insert text, open file) shown in the Command Palette & a "Commands" menu |
| `themes`          | Adds a color theme selectable in Settings                              |
| `snippets`        | Adds autocomplete snippets per language                                |
| `terminalCommands`| Adds commands you can type in the integrated terminal                 |
| `keybindings`     | Binds a keyboard shortcut to a contributed command                    |

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
    "commands": [
      { "id": "my.hello", "title": "Say Hello", "action": "toast", "arg": "Hello!" }
    ],
    "keybindings": [
      { "key": "Ctrl+Alt+H", "command": "my.hello", "title": "Say Hello" }
    ]
  }
}
```

### Try the included samples (`extensions/` folder)

| File                     | Adds                                                     |
| ------------------------ | -------------------------------------------------------- |
| `synthwave-theme.json`   | A neon "Synthwave '84" color theme                       |
| `console-snippets.json`  | JS/TS snippets + commands + terminal commands            |
| `git-tools.json`         | Git commands + a `Ctrl+Shift+G` keybinding               |
| `demo-project.json`      | Not an extension — load it via **Load Project JSON**     |

Install an extension:
1. Open the **Extensions** panel (🧩 in the activity bar).
2. Click **Choose extension .json** and pick a `.json` file.
3. Or just **drag the `.json` file** onto the window.

Uninstall: open the **Extensions** panel and click **Uninstall** on any card.

---

## ⌨️ Shortcuts

| Shortcut              | Action                       |
| --------------------- | ---------------------------- |
| `Ctrl+N`              | New file                     |
| `Ctrl+Shift+N`        | New folder                   |
| `Ctrl+K` / `Ctrl+K O` | Open folder                  |
| `Ctrl+S`              | Save                         |
| `Ctrl+Shift+S`        | Save As                      |
| `Ctrl+W`              | Close editor                 |
| `Ctrl+Shift+P`        | Command Palette              |
| `Ctrl+P`              | Quick Open                   |
| `Ctrl+Shift+F`        | Search                       |
| `Ctrl+Shift+X`        | Extensions panel             |
| `Ctrl+` `` ` ``       | Terminal                     |
| `Ctrl+B`              | Toggle sidebar               |
| `Ctrl+,`              | Settings                     |
| `Ctrl+=` / `Ctrl+-`   | Zoom in / out                |

---

## 📁 File layout

```
ban-code-editor/
├── index.html            ← the entire editor (open this)
├── extensions/           ← sample .json extensions & demo project
│   ├── synthwave-theme.json
│   ├── console-snippets.json
│   ├── git-tools.json
│   └── demo-project.json
└── README.md
```

> Requires internet for the Monaco editor (loaded from a CDN) and for ZIP export
> (JSZip). Everything else works offline.
