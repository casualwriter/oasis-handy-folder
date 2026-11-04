# One HTML file, one exe: shipping Windows tools with Amoy Oasis

**English** | [繁體中文](oasis.zh-TW.md)

*How Handy Folder is built and shipped as a single Oasis app, and how to do the same with your own tool.*

## The problem: small tools are expensive

Every company has jobs that need a small tool: rename and file the monthly reports, look up a customer in the database, check a folder before it goes to the client, run the weekly script without opening a console. The tool is small. Getting it onto people's PCs is not:

| Option | What it costs |
|---|---|
| HTA / VBScript | Still runs on the old Internet Explorer engine; Microsoft is retiring VBScript. |
| Electron | 100 MB or more per app, a Node toolchain, an installer. |
| A web app | A server, logins, hosting, and an IT approval for every change. |
| Python or PowerShell scripts | No real user interface; Python must be installed on every PC. |

So the tool is often never built, or it lives on one person's PC.

## The idea: the runtime is one exe, the app is one HTML file

**Amoy Oasis** is a small Windows runtime (`oasis.exe`, about 37 MB, Go + WebView2). Put it next to an `index.html` and double-click it. The page opens in its own window and gets a JavaScript API for files, commands, databases, HTTP and AI, inside a sandbox.

```
my-tool\
├─ oasis.exe      the runtime (the same file for every app)
└─ index.html     your app
```

That is a complete, portable Windows app. No installer, no admin rights, nothing written outside the folder.

## Hello in two minutes

Make a folder, copy `oasis.exe` into it, and save this as `index.html`:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>My Files</title>
  <link rel="stylesheet" href="/_oasis/oasis.css">
</head>
<body style="padding:16px">
  <h1>Files in this folder</h1>
  <ul id="list"></ul>
  <script src="/_oasis/oasis.js"></script>
  <script>
    oasis.ready().then(async () => {
      for (const f of await oasis.fs.list('.')) {
        const li = document.createElement('li');
        li.textContent = (f.dir ? '📁 ' : '📄 ') + f.name;
        document.getElementById('list').append(li);
      }
    });
  </script>
</body>
</html>
```

Double-click `oasis.exe`. A window titled *My Files* lists the folder. The `/_oasis/…` files (the API, the styles, the UI components) are built into `oasis.exe`; you ship nothing else. Change `index.html` and save: the window reloads by itself.

## Add an app.json

Without `app.json`, at the default security level, an app may use files, databases, HTTP and AI. Anything riskier is declared, and the user confirms it once:

```json
{
  "title": "My Files",
  "icon": "📁",
  "permissions": ["fs", "shell"]
}
```

`shell` lets the app run commands; Oasis asks the user the first time. An `.ico` or `.png` icon also becomes the window and taskbar icon, and `"theme": "light"` or `"dark"` sets the default theme.

## What an app can use

| Area | Examples |
|---|---|
| Files | `oasis.fs.list / read / write / rename / remove`; every write keeps recent versions, removed files go to a trash |
| Commands | `oasis.shell.run` (live output), `oasis.shell.open` (the Windows default app) |
| Databases | `oasis.db`: SQLite, MySQL, SQL Server, Oracle, ODBC (MS Access) |
| Network | `oasis.http` for web APIs |
| AI | `oasis.ai.chat` with the company's own model keys |
| Extensions | `oasis.ext.call`: any program that reads JSON and writes JSON (PowerShell, Python …) |
| UI | ready components: app frame, split view, sortable table, forms, dialogs, toasts, Markdown with math |

## Safety by design

- **Sandbox.** A single app may use its own folder and its sub-folders. Other folders need the user's approval (saved in `oasis.json`), or are allowed for one run when they are named on the command line, for example a folder dropped onto `oasis.exe`.
- **Reserved.** `.oasis\` (log, permissions, backups, trash, WebView data) and `oasis.json` belong to Oasis; the app cannot touch them.
- **Permissions** are declared in `app.json`; risky ones are confirmed by the user once.
- **Nothing is hidden.** By default every API call is logged in `.oasis\oasis.log`.
- **Stricter levels.** An administrator can set `oasis.json` to level `strict` (pages may load only their own files) or switch off whole modules, such as `shell`.

## Case study: Handy Folder

Handy Folder is a file browser with previews (Markdown, PDF, CSV, images, code), ► Run for scripts, folder tools, a Markdown editor, a home page of shortcuts, and English → Chinese translation of documents.

- **The whole app:** `index.html`, about 800 lines (52 KB), and a short `app.json`.
- **Plain HTML and JavaScript.** No framework and no build step; open the file and read it.
- **The package:** `oasis.exe` + `index.html` + `app.json` + its manual. Zip the folder and it is ready to send.
- **Built with an AI assistant.** Because an app is one readable file on a documented API, an AI assistant can write it, explain it and fix it. Most of Handy Folder was written and improved this way, and each change was checked in a real window.

## Shipping and updating

| Task | How |
|---|---|
| Ship | Zip the folder, or copy it to a shared drive or a USB stick. |
| Start | Double-click `oasis.exe`, or make a shortcut to it. Drop a folder on it to give the app that folder. |
| Update the app | Replace `index.html` (and `app.json`). The user's settings and data stay. |
| Update Oasis | Replace `oasis.exe`. The app does not change. |
| Several apps | Each app folder has its own copy of `oasis.exe`, so apps do not depend on each other. |
| Remove | Delete the folder. Nothing else was installed. |

## Honest limits

- Windows 10 and 11 only (64-bit).
- `oasis.exe` is not code-signed yet, so Windows SmartScreen may warn the first time.
- About 37 MB per app folder, because each app carries its own runtime.
- No automatic updates yet: you replace the files.
- One window per app.

## Work with us

Amoy Technology uses Oasis to build and deploy tools for companies: database front ends, report and file workflows, team utilities. The result is a folder your staff can run on their own PCs, and your IT team can review in one file. If you have a tool that never got built, write to **info@amoy-tech.com** or visit https://amoy-tech.com.

*Try it: download this repository, double-click `oasis.exe`, and press F1 for the manual.*
