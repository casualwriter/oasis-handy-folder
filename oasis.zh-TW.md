# 一個 HTML 檔，一個 exe：用 Amoy Oasis 發布 Windows 工具

[English](oasis.md) | **繁體中文**

*Handy Folder 怎樣做成並發布成一個單一 Oasis app，以及怎樣用同樣的方法做出你自己的工具。*

## 問題：小工具，代價卻不小

每家公司都有需要小工具的工作：把每月報表改名歸檔、在資料庫裡查客戶、交給客戶前先檢查資料夾、不開命令視窗就跑每週的腳本。工具本身很小，但要把它裝到同事的電腦上卻不容易：

| 做法 | 代價 |
|---|---|
| HTA / VBScript | 還跑在舊的 Internet Explorer 引擎上；微軟正在淘汰 VBScript。 |
| Electron | 每個 app 100 MB 以上，要 Node 開發環境，還要安裝程式。 |
| 網頁系統 | 要伺服器、登入、主機，而且每次修改都要 IT 核准。 |
| Python 或 PowerShell 腳本 | 沒有像樣的操作介面；每台電腦都要先裝 Python。 |

結果工具常常沒做出來，或者只留在某一個人的電腦上。

## 構想：執行環境是一個 exe，app 是一個 HTML 檔

**Amoy Oasis** 是一個小型的 Windows 執行環境（`oasis.exe`，約 37 MB，Go + WebView2）。把它放在 `index.html` 旁邊，雙擊。頁面就會在自己的視窗裡打開，並且得到一套 JavaScript API，可以在沙盒裡存取檔案、執行指令、連資料庫、HTTP 和 AI。

```
my-tool\
├─ oasis.exe      執行環境（每個 app 都是同一個檔案）
└─ index.html     你的 app
```

這就是一個完整、可攜的 Windows app。沒有安裝程式、不用系統管理員權限，也不會寫到資料夾以外的地方。

## 兩分鐘做出 Hello

建一個資料夾，把 `oasis.exe` 複製進去，再把下面這段存成 `index.html`：

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

雙擊 `oasis.exe`。一個標題為 *My Files* 的視窗會列出資料夾裡的檔案。`/_oasis/…` 這些檔案（API、樣式、UI 元件）都已經包在 `oasis.exe` 裡，你不用另外附上任何東西。修改 `index.html` 並存檔，視窗會自動重新載入。

## 加上 app.json

在預設的安全等級下，沒有 `app.json` 的 app 可以使用檔案、資料庫、HTTP 和 AI。風險較高的權限要先宣告，並由使用者確認一次：

```json
{
  "title": "My Files",
  "icon": "📁",
  "permissions": ["fs", "shell"]
}
```

`shell` 讓 app 可以執行指令；第一次使用時 Oasis 會詢問使用者。`.ico` 或 `.png` 圖示也會成為視窗和工作列的圖示；`"theme": "light"` 或 `"dark"` 可以設定預設主題。

## app 可以用什麼

| 範圍 | 例子 |
|---|---|
| 檔案 | `oasis.fs.list / read / write / rename / remove`；每次寫入都會保留最近的版本，刪除的檔案會進垃圾桶 |
| 指令 | `oasis.shell.run`（即時輸出）、`oasis.shell.open`（用 Windows 預設程式打開） |
| 資料庫 | `oasis.db`：SQLite、MySQL、SQL Server、Oracle、ODBC（MS Access） |
| 網路 | `oasis.http` 呼叫網路 API |
| AI | `oasis.ai.chat`，使用公司自己的模型金鑰 |
| 擴充 | `oasis.ext.call`：任何讀 JSON、寫 JSON 的程式（PowerShell、Python 等） |
| 介面 | 現成的元件：app 外框、左右分割、可排序表格、表單、對話框、提示訊息、含數學式的 Markdown |

## 從設計就講究安全

- **沙盒。** 單一 app 可以使用它自己的資料夾和子資料夾。其他資料夾要經過使用者核准（存在 `oasis.json`），或者在命令列上指定時只在那次執行中可用，例如拖到 `oasis.exe` 上的資料夾。
- **保留區。** `.oasis\`（記錄、權限、備份、垃圾桶、WebView 資料）和 `oasis.json` 屬於 Oasis；app 碰不到它們。
- **權限**宣告在 `app.json`；有風險的權限由使用者確認一次。
- **全程留紀錄。** 預設會把每一次 API 呼叫記錄在 `.oasis\oasis.log`。
- **更嚴格的等級。** 系統管理員可以把 `oasis.json` 設成 `strict` 等級（頁面只能載入自己的檔案），或整個關掉某些模組，例如 `shell`。

## 實例：Handy Folder

Handy Folder 是一個檔案總管，可以預覽 Markdown、PDF、CSV、圖片和程式碼，用 ► Run 執行腳本，有資料夾工具、Markdown 編輯器、捷徑首頁，還能把英文文件翻成中文。

- **整個 app：** 一個 `index.html`，約 800 行（52 KB），加上一個簡短的 `app.json`。
- **純 HTML 和 JavaScript。** 沒有框架、不用建置；打開檔案就能讀。
- **發布內容：** `oasis.exe` + `index.html` + `app.json` + 使用手冊。把資料夾壓縮起來就能寄出去。
- **用 AI 助理開發。** 因為 app 是一個看得懂的檔案，API 又有完整文件，AI 助理可以寫它、解釋它、修正它。Handy Folder 大部分就是這樣寫出來並一路改進的，每次修改都在真實視窗裡驗證過。

## 發布與更新

| 工作 | 做法 |
|---|---|
| 發布 | 把資料夾壓縮，或複製到共用磁碟或隨身碟。 |
| 啟動 | 雙擊 `oasis.exe`，或為它建一個捷徑。把資料夾拖到它上面，就會把那個資料夾交給 app。 |
| 更新 app | 取代 `index.html`（和 `app.json`）。使用者的設定和資料都會保留。 |
| 更新 Oasis | 取代 `oasis.exe`。app 本身不用改。 |
| 多個 app | 每個 app 資料夾都有自己的一份 `oasis.exe`，所以 app 之間互不影響。 |
| 移除 | 刪掉資料夾。沒有安裝任何其他東西。 |

## 坦白說的限制

- 只支援 Windows 10 和 11（64 位元）。
- `oasis.exe` 目前還沒有程式碼簽章，所以第一次執行時 Windows SmartScreen 可能會警告。
- 每個 app 資料夾約 37 MB，因為每個 app 都帶著自己的執行環境。
- 還沒有自動更新：要自己取代檔案。
- 每個 app 只有一個視窗。

## 與我們合作

Amoy Technology 用 Oasis 為企業製作和部署工具：資料庫前端、報表和檔案流程、團隊小工具。成果是一個資料夾，員工可以在自己的電腦上執行，你的 IT 團隊只要看一個檔案就能審查。如果你有一個一直沒做出來的工具，歡迎來信 **info@amoy-tech.com**，或參觀 https://amoy-tech.com。

*試試看：下載這個 repository，雙擊 `oasis.exe`，按 F1 看使用手冊。*
