# 🗂️ Handy Folder — 使用手冊

[English](MANUAL.md) | **繁體中文**

Handy Folder 是 Windows 上可以瀏覽任何資料夾的檔案總管。左邊列出資料夾內容，右邊顯示你點的檔案。**點一下檔案只會顯示它；要按 ► Run 才會執行。**

它是一個 Amoy Oasis app：一個 `index.html` 加一個 `app.json`。它可以在完整的 Oasis 裡當內建 app 用，也可以單獨執行：把 `oasis.exe` 複製到 `index.html` 旁邊，再雙擊它。授權：MIT。

按 **F1**（或右上角的 📖 按鈕），就會在 Handy Folder 裡打開這份手冊。

## 目錄

1. [啟動](#1-啟動)
2. [畫面](#2-畫面)
3. [瀏覽資料夾](#3-瀏覽資料夾)
4. [預覽檔案](#4-預覽檔案)
5. [按鈕與右鍵選單](#5-按鈕與右鍵選單)
6. [新增與編輯檔案](#6-新增與編輯檔案)
7. [執行檔案與資料夾工具](#7-執行檔案與資料夾工具)
8. [翻譯 Markdown 頁面](#8-翻譯-markdown-頁面)
9. [首頁與捷徑](#9-首頁與捷徑)
10. [設定](#10-設定)
11. [設定存在哪裡](#11-設定存在哪裡)
12. [Oasis 以外的資料夾](#12-oasis-以外的資料夾)
13. [垃圾桶、備份與復原](#13-垃圾桶備份與復原)
14. [按鍵](#14-按鍵)
15. [權限與安全](#15-權限與安全)
16. [限制](#16-限制)
17. [疑難排解](#17-疑難排解)

## 1. 啟動

| 你的安裝方式 | 啟動方法 |
|---|---|
| 在完整的 Oasis 裡 | 啟動器 › **Handy Folder**，或 `oasis --app "sys/handyfolder"` |
| 單一 app（`oasis.exe` 放在 `index.html` 旁邊） | 雙擊 `oasis.exe` |

直接打開某個資料夾：

```
oasis --app "sys/handyfolder?dir=D:\Projects"      （完整 Oasis：這個資料夾會加入你的根目錄清單）
oasis.exe D:\Projects                              （單一 app：只在這次執行中打開這個資料夾）
```

單一 app 也可以在檔案總管裡**把資料夾（或檔案）拖到 `oasis.exe` 上**。

第一次啟動會建立 `settings.json`；第一次打開首頁時會建立 `home.md`（見 [§11](#11-設定存在哪裡)）。

### 單獨執行（可攜）

整個程式就是一個資料夾裡的幾個檔案。把資料夾複製到任何地方（隨身碟也可以），雙擊 `oasis.exe` 就能用。不用安裝、不用系統管理員權限、不用 Node、不用 Python。

```
handy-folder\
├─ oasis.exe        Amoy Oasis，執行環境（Go + WebView2）；程式庫已內建
├─ index.html       整個 app：HTML、CSS 和 JavaScript
├─ app.json         標題、圖示、權限
├─ MANUAL.zh-TW.md  這份手冊（英文版是 MANUAL.md）
├─ README.zh-TW.md  專案介紹；oasis.zh-TW.md：單一 Oasis app 怎麼發布
├─ LICENSE、THIRD_PARTY.md
├─ settings.json、home.md    第一次執行時建立：你的設定
└─ .oasis\          第一次執行時建立：Oasis 的記錄、權限、備份、垃圾桶、WebView 資料
```

- **任何 Oasis app 都是這樣發布的：** `oasis.exe` 放在 `index.html` 旁邊，就會以單一 app 執行它，而且 app 可以使用這個資料夾和所有子資料夾（`.oasis\` 和 `oasis.json` 除外）。更新 Oasis 只要換掉 `oasis.exe`，app 本身不用改。
- **需求：** Windows 10 或 11（64 位元），並有 WebView2 Runtime（Windows 已內建）。
- `oasis.exe` 沒有程式碼簽章，Windows 可能會警告發行者不明：如果你信任這份檔案，選 **其他資訊 › 仍要執行**。
- 第一次按 ► Run 或資料夾工具時，Oasis 會問一次是否允許這個 app 執行指令。

## 2. 畫面

| 位置 | 內容 |
|---|---|
| 最上方 | **Home**、⚙ 設定、📖 這份手冊 |
| 左上 | 根目錄清單、↑ 上一層、⟳ 重新整理、排序、新增檔案、新增資料夾、過濾框 |
| 左邊的路徑列 | 目前在根目錄裡的位置（麵包屑）；點其中一段就會跳過去 |
| 左邊的清單 | 資料夾在前，檔案在後；右欄顯示大小，依日期排序時顯示日期 |
| 右邊 | 檢視區：資料夾頁、檔案預覽、編輯器或首頁；它的按鈕在標題列 |
| 輸出面板 | 執行指令時出現在檢視區下方（§7） |

拖動兩邊之間的分隔線可以調整寬度。寬度、根目錄、資料夾和排序方式都會被記住。

## 3. 瀏覽資料夾

- **根目錄清單**（左上）：你常用的資料夾。在完整的 Oasis 裡，第一個是 `Oasis folder`（放 `oasis.exe` 的資料夾）。單一 app 則從它自己的資料夾開始。選 **＋ Add a folder…** 可以加一個（§12）。
- **點資料夾**就會進去。**↑** 或 **Backspace** 回上一層（不會超出根目錄）。按鈕下方的路徑列顯示目前位置。
- **資料夾頁：** 進入資料夾時，右邊會顯示它的 `README.md`（或 `README.txt`）、資料夾工具（§7），以及這些按鈕：Explorer、+ shortcut、Print README、Copy path。沒有 README 的資料夾會顯示裡面有幾個資料夾和檔案。
- **排序：** 名稱 A→Z 或 Z→A（數字依大小排，`b2` 在 `b10` 前面）、日期（新到舊或舊到新）、大小（大到小或小到大）、類型（依副檔名）。資料夾永遠排在最上面；相同的項目再依名稱排。
- **過濾：** 在框裡輸入文字，就只顯示名稱含有這段文字的項目。
- **隱藏的名稱：** `.git`、`node_modules`、`__pycache__` 和 `.venv` 不會列出。可以在設定的 `hidden` 修改（§10）。
- **⟳ 重新整理：** 重新讀取資料夾（在 Handy Folder 以外改了檔案之後用）。
- **拖放檔案：** 從檔案總管把檔案拖到清單上，就會**複製**到目前的資料夾（每個檔案最大 100 MB）。同名的檔案會被取代，舊的會留一份備份（§13）。

## 4. 預覽檔案

點一下檔案就會預覽：

| 類型 | 預覽方式 |
|---|---|
| Markdown（`.md`、`.markdown`） | 排版後顯示，含數學式；連結可以點（見下表）；檔案旁邊的圖片會顯示出來 |
| HTML（`.htm`、`.html`） | 放在安全的框架裡：它的程式會執行，但不能使用 Oasis |
| 圖片（`.png .jpg .jpeg .gif .webp .bmp .ico .svg`） | 顯示圖片（最大 25 MB） |
| PDF | 內建的 PDF 檢視器（最大 40 MB） |
| CSV、TSV | 表格（前 5000 列）；分隔符號 `,`、`;` 或 Tab 會自動判斷 |
| JSON | 如果是物件陣列就顯示成表格，否則顯示排好格式的文字 |
| Excel（`.xlsx`、`.xlsm`） | 第一個工作表的前 2000 列；需要 Oasis 的 `excel` 擴充（Python + openpyxl） |
| 文字和程式碼（`.txt .log .js .py .go .json .css .sql .bat .ps1 …`） | 有行號的文字（最大 2 MB） |
| 其他 | 顯示名稱、大小和日期；請用 **Open** 或 **► Run** |

**Markdown 裡的連結：**

| 連結 | 結果 |
|---|---|
| `notes.md`、`sub/pic.png`、`../x.md` | 打開這個檔案，路徑相對於這份 Markdown 檔 |
| `/projects` | 目前根目錄裡的路徑 |
| `C:\full\path` | 這個檔案或資料夾 |
| `https://…` | 用網頁瀏覽器打開 |
| `#section` | 跳到這一頁的某一節 |
| `run:notepad.exe` | 執行這個指令（在它自己的視窗） |

Markdown 裡的圖片（`![x](pic.png)`）會從文件所在的資料夾讀取，列印時也一樣。找不到的圖片會顯示 `[missing: 檔名]`。

## 5. 按鈕與右鍵選單

檔案的按鈕是檢視區標題列上的圖示，滑鼠停在上面就會顯示說明。**在預覽上按右鍵**，會出現同樣的動作，用文字列成選單，另外還有幾個只在選單裡的動作。

| 動作 | 位置 | 作用 |
|---|---|---|
| Edit | 按鈕 | 打開文字編輯器（文字檔） |
| Print | 按鈕 | 列印 Markdown 檔，或存成 PDF |
| Open | 按鈕 | 用 Windows 對應的程式打開 |
| ► Run | 按鈕 | 用它的指令執行（§7）；只有 `run` 裡列出的類型才有 |
| Launch app | 按鈕 | 打開一個 Oasis app（`apps\…\index.html`；只在完整 Oasis） |
| Reveal in Explorer | 按鈕 | 打開檔案總管並選取這個檔案 |
| Copy path | 按鈕 | 複製完整路徑 |
| Move to trash | 按鈕 | 移到 Oasis 的垃圾桶（§13） |
| Open in VS Code / Open in editor | 選單 | 用你的編輯器打開（§10 的 `editor`） |
| Rename | 選單 | 改名 |
| Translate | 選單 | 只限 Markdown：英文、繁體中文，或兩者並排（§8） |
| Copy text | 選單 | 有選取文字時才出現 |

**列印：** WebView 的列印預覽大小是固定的。螢幕小的時候，在預覽裡按 **Ctrl+Shift+P**，就會改開一般的 Windows 列印對話框。

## 6. 新增與編輯檔案

- **新增檔案**（左邊按鈕列）：預設名稱是今天日期的 `YYYYMMDD-notes.md`。新的 `.md` 檔會先有一個標題。建立後會直接用編輯器打開。
- **新增資料夾**（左邊按鈕列）：會問你名稱。
- **編輯器：** 純文字編輯器。編輯 Markdown 時，右邊有即時預覽，會跟著文字一起捲動；按 ▥ 按鈕可以收起或打開預覽（會記住你的選擇）。
  - **Ctrl+S** 存檔。Oasis 會先把舊版本留下來（§13）。
  - **Tab** 會插入兩個空格。
  - **Close** 回到預覽；有沒存的修改時會先問你。
- **改名**在右鍵選單裡。

## 7. 執行檔案與資料夾工具

**► Run** 只會出現在 `run` 裡有指令的檔案類型（§10）。預設值：

| 副檔名 | 指令 |
|---|---|
| `py` | `python "{file}"` |
| `bat`、`cmd` | `"{file}"` |
| `ps1` | `powershell -NoProfile -ExecutionPolicy Bypass -File "{file}"` |
| `js` | `node "{file}"` |
| `go` | `go run "{file}"` |
| `exe msi lnk url hta` | `open`（Windows 預設程式） |

指令會在檔案所在的資料夾執行。輸出會即時顯示在檢視區下方的**輸出面板**，最後附上結束代碼和花費時間。**Stop** 停止執行，✕ 關閉面板。一次只能執行一個指令。

**資料夾工具**是每個資料夾頁上的按鈕，會在那個資料夾裡執行。預設有：CMD here、PowerShell here、Explorer、Git status、List (dir)。以 `start` 或 `explorer` 開頭的指令會開自己的視窗；其他指令的輸出會顯示在面板裡。

Python、Node、Go、Git 和 VS Code 都不是 Oasis 的一部分。要用這些指令，請自己安裝。

## 8. 翻譯 Markdown 頁面

在 Markdown 預覽上按右鍵，選 **繁體中文** 或 **English | 中文**（兩者逐段並排）。選 **English (original)** 回到原文。

- 它用的是 Google 免費的網頁翻譯（不用金鑰，不是正式的 API）。可能會慢，或在請求太多時被拒絕；這時請稍等再試。
- **頁面的文字會送到 Google。** 不要翻譯機密文件。
- 每個標題、段落、清單項目和表格儲存格都會變成翻譯後的純文字，所以裡面的粗體和連結在譯文中會消失。程式碼區塊維持原樣。
- 檔案開著的期間，翻譯結果會保留。

## 9. 首頁與捷徑

按 **Home**。這是你自己的 Markdown 頁面 `home.md`（存放位置見 §11）。按 **Edit** 修改它。

- 如果一個清單裡**每一項都只有一個連結**，它就會變成一排**磚塊**：📁 資料夾、📝 檔案、🌐 網頁連結、⚙️ `run:` 指令。
- 其他 Markdown 照一般方式顯示。
- **+ shortcut**（首頁和每個資料夾頁都有）會問你名稱，再把目前的資料夾加到 `## Shortcuts` 清單。

`home.md` 範例：

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

以 `/` 開頭的連結是目前根目錄裡的路徑。

## 10. 設定

按 ⚙ 編輯 `settings.json`。用 **Ctrl+S** 存檔，再按 **F5** 讓新設定生效。

| 鍵 | 意思 | 預設值 |
|---|---|---|
| `roots` | 根目錄清單（完整路徑；`~` = Oasis 資料夾） | `["~"]`；單一 app：它自己的資料夾 |
| `editor` | **Open in editor** 用的指令；空白 = VS Code（`code "{file}"`） | `""` |
| `hidden` | 不要列出的名稱（正規表示式） | `^(\.git\|node_modules\|__pycache__\|\.venv)$` |
| `run` | 副檔名 → **► Run** 的指令；`open` = Windows 預設程式 | 見 §7 |
| `tools` | 按鈕名稱 → 指令，顯示在每個資料夾頁 | 見 §7 |

指令裡的 `{file}` 是完整路徑，`{name}` 是檔名，`{dir}` 是檔案所在的資料夾。多個副檔名可以共用一個指令：`"bat,cmd"`。

範例：

```json
{
  "roots": ["~", "D:\\Projects"],
  "editor": "notepad++ \"{file}\"",
  "hidden": "^(\\.git|node_modules|__pycache__|\\.venv|dist)$",
  "run": { "py": "C:\\Python313\\python.exe \"{file}\"", "txt": "open" },
  "tools": { "Git log": "git log --oneline -20", "Terminal": "start \"\" wt -d ." }
}
```

JSON 裡路徑的每個 `\` 都要寫成 `\\`。沒寫的鍵會用預設值；新的 `run` 和 `tools` 項目會加到預設值上。

## 11. 設定存在哪裡

`settings.json` 和 `home.md` 預設存在 **`index.html` 旁邊**（也就是這個 app 自己的資料夾），除非 `app.json` 用 `"dataDir"` 指定別的位置：

| `app.json` 的 `dataDir` | 使用的資料夾 |
|---|---|
| 沒寫，或 `"."` | app 資料夾（預設） |
| `"data"` | app 資料夾下的子資料夾 |
| `"~/shared/handyfolder"` | Oasis 資料夾裡 |
| `"D:\\MyData\\hf"` | 這個完整路徑 |

完整 Oasis 裡的內建版本用的是 `"dataDir": "~/shared/handyfolder"`。這樣換上新版的 app 資料夾時，你的設定不會被蓋掉，也不會跟著 app 一起被發布出去。

瀏覽器那一層另外會記住上次的根目錄、資料夾、排序和面板寬度（存在 WebView 裡，每個 Windows 使用者各自一份）。

## 12. Oasis 以外的資料夾

Oasis 只讓 app 使用 Oasis 資料夾和你允許的資料夾（沙盒）。放在 `oasis.exe` 旁邊的單一 app，可以使用整個資料夾和所有子資料夾，但 `.oasis\` 和 `oasis.json` 除外；用 `oasis.exe <資料夾>` 啟動的 app 則可以使用那個資料夾。

- **＋ Add a folder…** 會請你選一個資料夾。如果它在 Oasis 以外，Handy Folder 會先問你；你同意後，它會加入 Oasis 的「Settings › Security › Extra folders」（存在 `oasis.json` 的 `security.extra_paths`）。之後可以在那裡移除。
- 連結指向允許範圍以外的檔案時，也會問同樣的問題。
- **單一 app 在命令列指定資料夾**（`oasis.exe D:\Projects`，或把資料夾拖到 `oasis.exe` 上）：Oasis **只在這次執行中**允許這個資料夾。它會出現在根目錄清單，但不會存起來。要保留它，請用 **＋ Add a folder…**。

## 13. 垃圾桶、備份與復原

沒有任何東西會立刻被刪掉：

| 動作 | 去處 |
|---|---|
| **Move to trash** | Oasis 資料夾裡的 `.oasis\trash\` |
| 存檔、取代或改名蓋過某個檔案時的舊版本 | Oasis 資料夾裡的 `.oasis\backup\` |

在完整的 Oasis 裡，**System › Recovery** 會列出它們，並且可以還原。單一 app 沒有 Recovery app：請用檔案總管從這兩個資料夾把檔案複製回去。

## 14. 按鍵

| 按鍵 | 位置 | 作用 |
|---|---|---|
| Backspace | 清單 | 回上一層 |
| F1 | 任何地方 | 這份手冊 |
| F5 | 任何地方 | 重新載入（也會套用新設定） |
| Ctrl+S | 編輯器 | 存檔 |
| Tab | 編輯器 | 插入兩個空格 |
| Esc | 選單 | 關閉右鍵選單 |
| Alt+Home | 完整 Oasis | 回到啟動器 |

## 15. 權限與安全

Handy Folder 向 Oasis 要求這些權限：

| 權限 | 用途 |
|---|---|
| `fs` | 讀寫檔案 |
| `shell` | ► Run、資料夾工具、Open、Reveal、`run:` 連結。**Oasis 會先問你一次。** |
| `ext` | Excel 預覽 |
| `system` | 把額外的資料夾加進 Oasis 設定 |

- 點檔案永遠不會執行它。只有 **► Run**、資料夾工具和 `run:` 連結會執行。
- 指令以你的 Windows 權限執行，跟你自己啟動的任何程式一樣。
- HTML 檔放在沙盒框架裡顯示：它的程式碰不到 Oasis，也碰不到你的檔案。
- **翻譯**會把頁面文字送到 Google（§8）。除此之外，沒有任何資料會離開你的電腦。

## 16. 限制

- 只能在 Windows 上用（Oasis 使用 WebView2，Windows 10 和 11 都已內建）。
- 文字檔以 UTF-8 讀取。
- 輸出面板一次只能跑一個指令。需要鍵盤輸入的指令收不到輸入；互動式程式請用 `start …` 開頭的工具。
- 預覽上限：文字 2 MB、圖片 25 MB、PDF 40 MB、CSV 5000 列、Excel 2000 列、拖放的檔案每個 100 MB。
- 沒有 OCR，也不能搜尋檔案內容。

## 17. 疑難排解

| 問題 | 解決方法 |
|---|---|
| 出現「outside the sandbox」 | 用 **＋ Add a folder…** 並允許它（§12） |
| ► Run 沒反應，或顯示「not recognized」 | 那個程式（python、node…）沒安裝或不在 PATH 裡；在 `run` 裡改用完整路徑（§10） |
| Excel 預覽出現錯誤 | 缺少 `excel` 擴充（Python + openpyxl）；請改用 **Open** |
| 改了設定卻沒變化 | 用 Ctrl+S 存檔後按 F5；確認 JSON 格式正確 |
| 列印預覽超出螢幕 | 在預覽裡按 Ctrl+Shift+P，改用 Windows 列印對話框 |
| 存過或刪掉的檔案不見了 | 到 Oasis 資料夾的 `.oasis\backup\` 或 `.oasis\trash\` 找（§13） |
| Oasis 詢問 `shell` 權限 | 這是執行指令要的一次性權限；要用 ► Run 和工具就選 Allow |
