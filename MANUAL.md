# 🗂️ Handy Folder — user manual

**English** | [繁體中文](MANUAL.zh-TW.md)

Handy Folder is a file browser for any folder on Windows. The left side lists the folder; the right side shows the file you click. **Clicking only shows a file. Nothing runs until you press ► Run.**

It is an Amoy Oasis app: one `index.html` plus an `app.json`. It runs inside the full Oasis (as a built-in app) or alone: copy `oasis.exe` next to `index.html` and double-click it. MIT license.

Press **F1** (or the 📖 button at the top right) to open this manual inside Handy Folder.

## Contents

1. [Start](#1-start)
2. [The screen](#2-the-screen)
3. [Browse folders](#3-browse-folders)
4. [Preview files](#4-preview-files)
5. [Buttons and the right-click menu](#5-buttons-and-the-right-click-menu)
6. [Create and edit files](#6-create-and-edit-files)
7. [Run files and folder tools](#7-run-files-and-folder-tools)
8. [Translate a Markdown page](#8-translate-a-markdown-page)
9. [The home page and shortcuts](#9-the-home-page-and-shortcuts)
10. [Settings](#10-settings)
11. [Where the settings are kept](#11-where-the-settings-are-kept)
12. [Folders outside Oasis](#12-folders-outside-oasis)
13. [Trash, backups and recovery](#13-trash-backups-and-recovery)
14. [Keys](#14-keys)
15. [Permissions and safety](#15-permissions-and-safety)
16. [Limits](#16-limits)
17. [Troubleshooting](#17-troubleshooting)

## 1. Start

| How you have it | How to start |
|---|---|
| Inside the full Oasis | Launcher › **Handy Folder**, or `oasis --app "sys/handyfolder"` |
| As a single app (`oasis.exe` next to `index.html`) | Double-click `oasis.exe` |

Open a folder directly:

```
oasis --app "sys/handyfolder?dir=D:\Projects"      (full Oasis: the folder is added to your root list)
oasis.exe D:\Projects                              (single app: the folder is opened for this run only)
```

As a single app you can also **drop a folder (or a file) onto `oasis.exe`** in Explorer.

The first start creates `settings.json` and, when you first open Home, `home.md` (see [§11](#11-where-the-settings-are-kept)).

### Run it on its own (portable)

The whole program is a few files in one folder. Copy the folder anywhere (a USB stick is fine) and double-click `oasis.exe`. No install, no admin rights, no Node, no Python.

```
handy-folder\
├─ oasis.exe        Amoy Oasis, the runtime (Go + WebView2); its libraries are built in
├─ index.html       the whole app: HTML, CSS and JavaScript
├─ app.json         title, icon, permissions
├─ MANUAL.md        this manual (MANUAL.zh-TW.md in Chinese)
├─ README.md        about the project; oasis.md: how a single Oasis app ships
├─ LICENSE, THIRD_PARTY.md
├─ settings.json, home.md    made on the first run: your settings
└─ .oasis\          made on the first run: the Oasis log, permissions, backups, trash, WebView data
```

- **This is how any Oasis app ships:** `oasis.exe` next to an `index.html` runs it as a single app, and the app may use its folder and all sub-folders (not `.oasis\` or `oasis.json`). Update Oasis by replacing `oasis.exe`; the app does not change.
- **Needs:** Windows 10 or 11 (64-bit) with the WebView2 Runtime, which Windows already has.
- `oasis.exe` is not code-signed, so Windows may warn about an unknown publisher: **More info › Run anyway** if you trust the copy.
- The first time you press ► Run or a folder tool, Oasis asks once whether this app may run commands.

## 2. The screen

| Part | What it is |
|---|---|
| Top bar | **Home**, ⚙ settings, 📖 this manual |
| Left, top | root list, ↑ up, ⟳ refresh, sort, new file, new folder, filter box |
| Left, path | the path inside the root (breadcrumbs); click a part to go there |
| Left, list | folders first, then files; the right column shows the size, or the date when you sort by date |
| Right | the viewer: a folder's page, a file preview, the editor or the home page; its buttons are in its header |
| Output panel | appears under the viewer when a command runs (§7) |

Drag the bar between the two sides to resize them. The width, the root, the folder and the sort order are remembered.

## 3. Browse folders

- **Root list** (top left): the folders you work in. Inside the full Oasis the first one is `Oasis folder` (the folder that holds `oasis.exe`). A single app starts with its own folder. Choose **＋ Add a folder…** to add one (§12).
- **Click a folder** to enter it. **↑** or **Backspace** goes up (not above the root). The path line under the bar shows where you are.
- **Folder page:** when you enter a folder, the right side shows its `README.md` (or `README.txt`), the folder tools (§7), and buttons: Explorer, + shortcut, Print README, Copy this folder, Paste. A folder without a README shows how many folders and files it has.
- **Sort:** name A→Z or Z→A (numbers in order: `b2` before `b10`), date (newest or oldest), size (largest or smallest), or type (by extension). Folders always stay on top; equal items are ordered by name.
- **Filter:** type in the box to show only names that contain the text.
- **Hidden names:** `.git`, `node_modules`, `__pycache__` and `.venv` are not listed. Change this with `hidden` in the settings (§10).
- **⟳ Refresh** reads the folder again (after you change files outside Handy Folder).
- **Right-click a file or folder in the list** for Open, Copy, Paste (into that folder, when you right-click a folder), Copy path, Rename and Move to trash. Right-click the empty part of the list for Paste here, New file and New folder.
- **Drop files** from Explorer onto the list to **copy** them into the current folder (100 MB per file at most). A file with the same name is replaced; the old one is kept as a backup (§13).
- **Copy and paste files:** **Ctrl+C** or the copy button copies the file shown; on a folder page the copy button copies the whole folder. **Ctrl+V** or the **Paste** button copies them into the current folder. It uses the Windows clipboard, so you can also paste in Explorer, or copy in Explorer and paste here. A name that is taken gets ` - Copy`, like in Explorer; nothing is replaced.

## 4. Preview files

Click a file to preview it:

| Type | Preview |
|---|---|
| Markdown (`.md`, `.markdown`) | formatted, with math; links work (see below); images next to the file are shown |
| HTML (`.htm`, `.html`) | in a safe frame: its scripts run but cannot use Oasis |
| Images (`.png .jpg .jpeg .gif .webp .bmp .ico .svg`) | the picture (up to 25 MB) |
| PDF | the built-in PDF viewer (up to 40 MB) |
| CSV, TSV | a table (first 5000 rows); the separator `,` `;` or tab is guessed |
| JSON | a table when it is a list of objects, else pretty text |
| Excel (`.xlsx`, `.xlsm`) | the first 2000 rows of the first sheet; needs the Oasis `excel` extension (Python + openpyxl) |
| Text and code (`.txt .log .js .py .go .json .css .sql .bat .ps1 …`) | text with line numbers (up to 2 MB) |
| Anything else | name, size and date; use **Open** or **► Run** |

**Links in Markdown:**

| Link | What happens |
|---|---|
| `notes.md`, `sub/pic.png`, `../x.md` | opens the file, relative to the Markdown file |
| `/projects` | a path inside the current root |
| `C:\full\path` | that file or folder |
| `https://…` | opens your web browser |
| `#section` | jumps inside the page |
| `run:notepad.exe` | runs the command (in its own window) |

Images in Markdown (`![x](pic.png)`) are read from the document's own folder, also when you print. A missing image shows `[missing: name]`.

## 5. Buttons and the right-click menu

The buttons of a file are icons in the viewer header. Hold the mouse over one to see what it does. **Right-click the preview** to get the same actions as a menu with names, plus a few that only live there.

| Action | Where | What it does |
|---|---|---|
| Edit | button | open the text editor (text files) |
| Print | button | print a Markdown file, or save it as PDF |
| Open | button | open the file with its Windows app |
| ► Run | button | run the file with its command (§7); only for types listed in `run` |
| Launch app | button | open an Oasis app (`apps\…\index.html`; full Oasis only) |
| Reveal in Explorer | button | open Explorer with the file selected |
| Copy file | button | copy the file, to paste here or in Explorer (Ctrl+C) |
| Move to trash | button | move the file to the Oasis trash (§13) |
| Open in VS Code / Open in editor | menu | open the file in your editor (`editor` in §10) |
| Copy path | menu | copy the full path |
| Paste here | menu | paste copied files into this folder (Ctrl+V) |
| Rename | menu | give the file a new name |
| Translate | menu | Markdown only: English, 繁體中文, or both side by side (§8) |
| Copy text | menu | when you have selected some text |

**Printing:** the Windows print preview of the WebView has a fixed size. On a small screen press **Ctrl+Shift+P** inside it to get the normal Windows print dialog.

## 6. Create and edit files

- **New file** (left bar): suggests `YYYYMMDD-notes.md` with today's date. A new `.md` file starts with a heading. The file opens in the editor.
- **New folder** (left bar): asks for a name.
- **Editor:** a plain text editor. For Markdown it shows a live preview on the right that scrolls with the text; the ▥ button hides or shows the preview (the choice is remembered).
  - **Ctrl+S** saves. Oasis keeps the previous version first (§13).
  - **Ctrl+F** finds text (not case-sensitive): the match is marked in orange and the box shows `3 / 12`. **Enter**, **F3** or **↓** goes to the next one; **Shift+Enter**, **Shift+F3** or **↑** to the previous one; **Esc** closes the box and leaves the match selected.
  - **Tab** inserts two spaces.
  - **Close** goes back to the preview; it asks first if you have unsaved changes.
- **Rename** is in the right-click menu.
- **Find in a preview:** **Ctrl+F** also works when you only view a file (Markdown, text, code, tables): every match is marked in yellow, the current one in orange. HTML and PDF are shown in their own frame; click into a PDF and use its own Ctrl+F.
- **Search in files:** on a folder page, **Search in files** (or **Ctrl+Shift+F**, or right-click the empty list) finds words in the documents of the folder and its sub-folders: Markdown, text, HTML, PDF (text only), CSV, JSON, XML, YAML, INI. Each match shows the file, the heading it is under, and the words in context; click it to open the file with the word marked. Chinese works, also short words. The first search reads every document; later ones read only the files that changed. The index is kept in a hidden `.kb` folder inside that folder (delete it any time). Needs an Oasis with `kb.js`; with an older `oasis.exe` the button is not shown.

## 7. Run files and folder tools

**► Run** appears for the file types that have a command in `run` (§10). Defaults:

| Extension | Command |
|---|---|
| `py` | `python "{file}"` |
| `bat`, `cmd` | `"{file}"` |
| `ps1` | `powershell -NoProfile -ExecutionPolicy Bypass -File "{file}"` |
| `js` | `node "{file}"` |
| `go` | `go run "{file}"` |
| `exe msi lnk url hta` | `open` (the Windows default app) |

The command runs in the file's folder. Its output streams into the **output panel** under the viewer, with the exit code and the time at the end. **Stop** ends it, ✕ hides the panel. One command runs at a time.

**Folder tools** are the buttons on every folder page. They run in that folder. Defaults: CMD here, PowerShell here, Explorer, Git status, List (dir). A command that starts with `start` or `explorer` opens its own window; any other command shows its output in the panel.

Python, Node, Go, Git and VS Code are not part of Oasis. Install them if you want those commands.

## 8. Translate a Markdown page

Right-click a Markdown preview and choose **繁體中文** or **English | 中文** (both side by side, block by block). **English (original)** goes back.

- It uses Google's free web translation (no key, not an official API). It may be slow or refuse many requests; then wait and try again.
- **The text of the page is sent to Google.** Do not translate private documents.
- Each heading, paragraph, list item and table cell becomes plain translated text, so bold and links inside it are lost in the copy. Code blocks stay as they are.
- The translation is kept while the file stays open.

## 9. The home page and shortcuts

Click **Home**. It is your own Markdown page, `home.md` (where it is kept: §11). Press **Edit** to change it.

- A list in which **every item is only a link** becomes a row of **tiles**: 📁 folder, 📝 file, 🌐 web link, ⚙️ `run:` command.
- Other Markdown is shown as usual.
- **+ shortcut** (on Home and on every folder page) asks for a name and adds the current folder to the `## Shortcuts` list.

Example `home.md`:

```markdown
# My home page

## Shortcuts

- [Projects](D:\Projects)
- [Notes](/docs/notes.md)
- [Wails docs](https://wails.io)
- [Notepad](run:notepad.exe)

## Notes

Anything else is normal Markdown.
```

A link that starts with `/` is a path inside the current root.

## 10. Settings

Click ⚙ to edit `settings.json`. Save with **Ctrl+S**, then press **F5** to use the new settings.

| Key | Meaning | Default |
|---|---|---|
| `roots` | the root list (full paths; `~` = the Oasis folder) | `["~"]`; a single app: its own folder |
| `editor` | command for **Open in editor**; empty = VS Code (`code "{file}"`) | `""` |
| `hidden` | names to leave out of the list (a regular expression) | `^(\.git\|node_modules\|__pycache__\|\.venv)$` |
| `run` | extension(s) → command for **► Run**; `open` = the Windows default app | see §7 |
| `tools` | button name → command, shown on every folder page | see §7 |

In a command, `{file}` is the full path, `{name}` the file name, `{dir}` the file's folder. Several extensions can share one command: `"bat,cmd"`.

Example:

```json
{
  "roots": ["~", "D:\\Projects"],
  "editor": "notepad++ \"{file}\"",
  "hidden": "^(\\.git|node_modules|__pycache__|\\.venv|dist)$",
  "run": { "py": "C:\\Python313\\python.exe \"{file}\"", "txt": "open" },
  "tools": { "Git log": "git log --oneline -20", "Terminal": "start \"\" wt -d ." }
}
```

JSON needs `\\` for each `\` in a path. A key you leave out keeps its default; new `run` and `tools` entries are added to the defaults.

## 11. Where the settings are kept

`settings.json` and `home.md` are kept **next to `index.html`** (this app's own folder), unless `app.json` says otherwise with `"dataDir"`:

| `dataDir` in `app.json` | Folder used |
|---|---|
| not set, or `"."` | the app folder (the default) |
| `"data"` | a sub-folder of the app folder |
| `"~/shared/handyfolder"` | inside the Oasis folder |
| `"D:\\MyData\\hf"` | that full path |

The built-in copy of the full Oasis uses `"dataDir": "~/shared/handyfolder"`. Then your settings stay when the app folder is replaced by a new version, and they are never published with the app.

The browser part also remembers the last root, folder, sort order and panel width (WebView storage, per Windows user).

## 12. Folders outside Oasis

Oasis lets apps use only the Oasis folder and the folders you allow (the sandbox). A single app next to `oasis.exe` may use its whole folder and all sub-folders, except `.oasis\` and `oasis.json`; one started as `oasis.exe <folder>` may use that folder.

- **＋ Add a folder…** asks for a folder. If it is outside Oasis, Handy Folder asks you first; when you agree it is added to Oasis *Settings › Security › Extra folders* (in `oasis.json`, `security.extra_paths`). You can remove it there later.
- A link to a file outside the allowed folders asks the same question.
- **Single app, folder on the command line** (`oasis.exe D:\Projects`, or a folder dropped on `oasis.exe`): Oasis allows that folder **for this run only**. It is shown in the root list but not saved. To keep it, use **＋ Add a folder…**.

## 13. Trash, backups and recovery

Nothing is deleted at once:

| What | Where it goes |
|---|---|
| **Move to trash** | `.oasis\trash\` in the Oasis folder |
| The old version, when you save, replace or rename over a file | `.oasis\backup\` in the Oasis folder |

In the full Oasis, **System › Recovery** lists them and puts them back. As a single app there is no Recovery app: copy the file back from those folders with Explorer.

## 14. Keys

| Key | Where | Action |
|---|---|---|
| Backspace | the list | up one folder |
| F1 | anywhere | this manual |
| F5 | anywhere | reload (also applies new settings) |
| Ctrl+S | editor | save |
| Tab | editor | insert two spaces |
| Ctrl+F | editor and preview | find text |
| Ctrl+C | a file is shown, no text selected | copy the file |
| Ctrl+V | outside the editor | paste files into this folder |
| Ctrl+Shift+F | outside the editor | search in files (this folder and below) |
| Enter / F3 / ↓, Shift+Enter / Shift+F3 / ↑ | find box | next / previous match |
| Esc | menu | close the right-click menu |
| Alt+Home | full Oasis | back to the launcher |

## 15. Permissions and safety

Handy Folder asks Oasis for:

| Permission | Used for |
|---|---|
| `fs` | reading and writing files |
| `shell` | ► Run, folder tools, Open, Reveal, `run:` links. **Oasis asks you once.** |
| `ext` | the Excel preview |
| `system` | adding extra folders to the Oasis settings |

- Clicking a file never runs it. **► Run**, the folder tools and `run:` links do.
- Commands run with your Windows rights, like any program you start.
- HTML files are shown in a sandboxed frame: their scripts cannot reach Oasis or your files.
- **Translate** sends the page text to Google (§8). Nothing else leaves your PC.

## 16. Limits

- Windows only (Oasis uses WebView2, which is part of Windows 10 and 11).
- Text files are read as UTF-8.
- One command at a time in the output panel. A command that waits for keyboard input will not get any; use a `start …` tool for interactive programs.
- The preview limits: text 2 MB, image 25 MB, PDF 40 MB, CSV 5000 rows, Excel 2000 rows, dropped files 100 MB each.
- No OCR, no search inside files.

## 17. Troubleshooting

| Problem | What to do |
|---|---|
| "outside the sandbox" | use **＋ Add a folder…** and allow it (§12) |
| ► Run does nothing, or "not recognized" | the program (python, node, …) is not installed or not on PATH; set a full path in `run` (§10) |
| The Excel preview shows an error | the `excel` extension (Python + openpyxl) is missing; use **Open** instead |
| Settings changes do nothing | save with Ctrl+S, then press F5; check that the JSON is valid |
| The print preview does not fit the screen | press Ctrl+Shift+P in it for the Windows print dialog |
| A saved or deleted file is gone | look in `.oasis\backup\` or `.oasis\trash\` in the Oasis folder (§13) |
| Oasis asks about `shell` | that is the one-time permission for running commands; choose Allow to use ► Run and the tools |
