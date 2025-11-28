# Gemini CLI 技巧與訣竅

**本指南涵蓋了約 30 個專業技巧，幫助你有效使用 Gemini CLI 進行智能代理式程式開發**

**[Gemini CLI](https://github.com/google-gemini/gemini-cli)** 是一個開源 AI 助手，將 Google Gemini 模型的強大功能直接帶進你的[終端機](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=The%20Gemini%20CLI%20is%20an,via%20a%20Gemini%20API%20key)。它是一個具有對話能力的「代理式」命令列工具 - 意思是它能理解你的需求、選擇工具（像是執行 shell 指令或編輯檔案），並執行多步驟計畫來協助你的開發[工作流程](https://cloud.google.com/blog/topics/developers-practitioners/agent-factory-recap-deep-dive-into-gemini-cli-with-taylor-mullen#:~:text=The%20Gemini%20CLI%20%20is,understanding%20of%20the%20developer%20workflow)。

簡單來說，Gemini CLI 就像是一個超強版的結對程式設計夥伴和命令列助手。它擅長處理程式開發任務、除錯、內容生成，甚至系統自動化，全都透過自然語言提示詞就能完成。在深入專業技巧之前，讓我們快速回顧一下如何設定並運行 Gemini CLI。

## 目錄

- [快速入門](#快速入門)
- [技巧 1：使用 `GEMINI.md` 建立持久化上下文](#技巧-1使用-geminimd-建立持久化上下文)
- [技巧 2：建立自訂斜線指令](#技巧-2建立自訂斜線指令)
- [技巧 3：使用自己的 `MCP` 伺服器擴充 Gemini](#技巧-3使用自己的-mcp-伺服器擴充-gemini)
- [技巧 4：善用記憶新增與回想功能](#技巧-4善用記憶新增與回想功能)
- [技巧 5：使用檢查點與 `/restore` 作為復原按鈕](#技巧-5使用檢查點與-restore-作為復原按鈕)
- [技巧 6：讀取 Google Docs、Sheets 等文件](#技巧-6讀取-google-docssheets-等文件配置-workspace-mcp-伺服器後你可以貼上-docssheets-連結讓-mcp-在權限許可下取得內容)
- [技巧 7：使用 `@` 引用檔案與圖片提供明確上下文](#技巧-7使用--引用檔案與圖片提供明確上下文)
- [技巧 8：即時建立工具（讓 Gemini 建立輔助程式）](#技巧-8即時建立工具讓-gemini-建立輔助程式)
- [技巧 9：使用 Gemini CLI 進行系統疑難排解與設定](#技巧-9使用-gemini-cli-進行系統疑難排解與設定)
- [技巧 10：YOLO 模式 - 自動確認工具動作（謹慎使用）](#技巧-10yolo-模式---自動確認工具動作謹慎使用)
- [技巧 11：無頭模式與腳本模式（在背景執行 Gemini CLI）](#技巧-11無頭模式與腳本模式在背景執行-gemini-cli)
- [技巧 12：儲存與恢復對話階段](#技巧-12儲存與恢復對話階段)
- [技巧 13：多目錄工作區 - 一個 Gemini，多個資料夾](#技巧-13多目錄工作區---一個-gemini多個資料夾)
- [技巧 14：用 AI 協助整理與清理檔案](#技巧-14用-ai-協助整理與清理檔案)
- [技巧 15：壓縮長對話以保持在上下文範圍內](#技巧-15壓縮長對話以保持在上下文範圍內)
- [技巧 16：使用 `!` 直通 Shell 指令（與終端機對話）](#技巧-16使用--直通-shell-指令與終端機對話)
- [技巧 17：把每個 CLI 工具都當作潛在的 Gemini 工具](#技巧-17把每個-cli-工具都當作潛在的-gemini-工具)
- [技巧 18：善用多模態 AI - 讓 Gemini 看見圖片等內容](#技巧-18善用多模態-ai---讓-gemini-看見圖片等內容)
- [技巧 19：自訂 `$PATH`（與工具可用性）以提升穩定性](#技巧-19自訂-path與工具可用性以提升穩定性)
- [技巧 20：透過 token 快取與統計追蹤並減少 token 消耗](#技巧-20透過-token-快取與統計追蹤並減少-token-消耗)
- [技巧 21：使用 `/copy` 快速複製到剪貼簿](#技巧-21使用-copy-快速複製到剪貼簿)
- [技巧 22：掌握 `Ctrl+C` 來處理 Shell 模式與離開](#技巧-22掌握-ctrlc-來處理-shell-模式與離開)
- [技巧 23：使用 `settings.json` 自訂 Gemini CLI](#技巧-23使用-settingsjson-自訂-gemini-cli)
- [技巧 24：善用 IDE 整合（VS Code）提供上下文與差異比對](#技巧-24善用-ide-整合vs-code提供上下文與差異比對)
- [技巧 25：使用 `Gemini CLI GitHub Action` 自動化 Repo 任務](#技巧-25使用-gemini-cli-github-action-自動化-repo-任務)
- [技巧 26：啟用遙測以獲得洞察與可觀察性](#技巧-26啟用遙測以獲得洞察與可觀察性)
- [技巧 27：關注開發路線圖（背景代理等功能）](#技巧-27關注開發路線圖背景代理等功能)
- [技巧 28：使用 `擴充功能` 擴展 Gemini CLI](#技巧-28使用-擴充功能-擴展-gemini-cli)
- [技巧 29：柯基模式彩蛋 🐕](#額外趣味柯基模式彩蛋-)

## 快速入門

**安裝：** 你可以透過 npm 安裝 Gemini CLI。若要全域安裝，請使用：

```bash
npm install -g @google/gemini-cli
```

或者使用 `npx` 直接執行而不安裝：

```bash
npx @google/gemini-cli
```

Gemini CLI 支援所有主要平台（它是用 Node.js/TypeScript 建構的）。安裝完成後，只需在終端機執行 `gemini` 指令即可啟動互動式 [CLI](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Interactive%20Mode%20,conversational%20session)。

**驗證：** 初次使用時，你需要向 Gemini 服務進行驗證。你有兩個選項：(1) **Google 帳號登入（免費方案）** - 這讓你可以免費使用 Gemini 2.5 Pro，並享有慷慨的使用額度（約每分鐘 60 次請求，每[日](https://blog.google/technology/developers/introducing-gemini-cli-open-source-ai-agent/#:~:text=Unmatched%20usage%20limits%20for%20individual,developers) 1,000 次請求）。啟動時，Gemini CLI 會提示你使用 Google 帳號登入（不需要[付費](https://genmind.ch/posts/Howto-Supercharge-Your-Terminal-with-Gemini-CLI/#:~:text=%2A%20Google,Google%20AI%20Studio%2C%20then%20run)）。(2) **API 金鑰（付費或更高等級存取）** - 你可以從 Google AI [Studio](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=1,key%20from%20Google%20AI%20Studio) 取得 API 金鑰，並設定環境變數 `GEMINI_API_KEY` 來[使用](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Method%201%3A%20Shell%20Environment%20Variable,zshrc)。

使用 API 金鑰可以提供更高的配額和企業級資料使用保護；在付費/計費使用模式下，提示詞不會被用於訓練，但日誌可能會因[安全](https://genmind.ch/posts/Howto-Supercharge-Your-Terminal-with-Gemini-CLI/#:~:text=responses%20may%20be%20logged%20for,Google%20AI%20Studio%2C%20then%20run)因素而保留。

例如，將以下內容加入你的 shell 設定檔：

```bash
export GEMINI_API_KEY="YOUR_KEY_HERE"
```

**基本使用：** 要開始互動式對話階段，只需執行不帶任何參數的 `gemini`。你會看到 `gemini>` 提示符號，可以輸入請求或指令。例如：

```bash
$ gemini
gemini> Create a React recipe management app using SQLite
```

接著你可以看著 Gemini CLI 建立檔案、安裝相依套件、執行測試等，來實現你的需求。如果你偏好單次執行（非互動式），可以使用 `-p` 旗標配合提示詞，例如：

```bash
gemini -p "Summarize the main points of the attached file. @./report.txt"
```

這會輸出單一回應然後[結束](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=gemini)。你也可以透過管線輸入提示詞給 Gemini CLI：例如，`echo "Count to 10" | gemini` 會透過 [stdin](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=gemini%20,txt) 提供提示詞。

**CLI 介面：** Gemini CLI 提供豐富的類 REPL 介面。它支援**斜線指令**（以 `/` 開頭的特殊指令，用於控制對話階段、工具與設定）以及**驚嘆號指令**（以 `!` 開頭，直接執行 shell 指令）。我們會在下方的專業技巧中介紹許多這些功能。預設情況下，Gemini CLI 在安全模式下運作，任何會修改系統的動作（寫入檔案、執行 shell 指令等）都會要求確認。當工具動作被提出時，你會看到差異比對或指令，並被提示 (`Y/n`) 來核准或拒絕。這確保 AI 不會在未經你同意的情況下做出不想要的變更。

有了基本概念後，讓我們來探索一系列專業技巧和隱藏功能，幫助你充分發揮 Gemini CLI 的功能。每個技巧都會先提供簡單範例，然後是深入細節與細微差異。這些技巧整合了工具創作者（例如 Taylor Mullen）、Google Developer Relations 團隊以及更廣泛社群的建議與洞察，作為 Gemini CLI **進階使用者的權威指南**。

## 技巧 1：使用 `GEMINI.md` 建立持久化上下文

**快速應用場景：** 別再重複相同的提示詞了。透過建立 `GEMINI.md` 檔案來提供專案特定的上下文或指示，讓 AI 始終掌握重要的背景知識，而不需要每次都[重新說明](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Context%20Files%20%28)。

在專案開發過程中，你常會有一些整體性的細節 - 例如程式碼風格指南、專案架構或重要事實 - 希望 AI 能記住。Gemini CLI 讓你可以將這些內容編碼在一個或多個 `GEMINI.md` 檔案中。只需在專案中建立 `.gemini` 資料夾（如果還沒有的話），並新增一個名為 `GEMINI.md` 的 Markdown 檔案，寫入任何你希望 AI 持續記住的筆記或指示。例如：

```markdown
# Project Phoenix - AI 助手

- 所有 Python 程式碼必須遵循 PEP 8 風格。
- 使用 4 個空格縮排。
- 使用者正在建構資料管線；優先使用函數式程式設計範式。
```

將這個檔案放在專案根目錄（或子目錄中以獲得更精細的上下文控制）。現在，每當你在該專案中執行 `gemini` 時，它會自動載入這些指示到[上下文](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Context%20Files%20%28)中。這意味著模型將*永遠*預先載入這些內容，避免每次提示詞都要加上相同的指導方針。

**運作原理：** Gemini CLI 使用階層式上下文載入[系統](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Hierarchical%20Loading%3A%20The%20CLI%20combines,The%20loading%20order%20is)。它會結合**全域上下文**（來自 `~/.gemini/GEMINI.md`，可用於跨專案的預設值）與你的**專案特定 `GEMINI.md`**，甚至子資料夾中的上下文檔案。更具體的檔案會覆蓋更通用的檔案。你可以隨時使用以下指令檢查載入了哪些上下文：

```bash
/memory show
```

這會顯示 AI [看到的](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=,current%20conversation%20with%20a%20tag)完整合併上下文。如果你修改了 `GEMINI.md`，使用 `/memory refresh` 即可重新載入上下文，不需要重啟[對話階段](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=,current%20conversation%20with%20a%20tag)。

**進階技巧：** 使用 `/init` 斜線指令來快速生成初始 `GEMINI.md`。在新專案中執行 `/init` 會建立一個範本上下文檔案，包含偵測到的技術堆疊、專案摘要[等資訊](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=,directory%20workspace%20%28e.g.%2C%20%60add)。你可以接著編輯並擴充該檔案。對於大型專案，可以考慮將上下文拆分成多個檔案，並在 `GEMINI.md` 中使用 `@include` 語法**匯入**它們。例如，你的主 `GEMINI.md` 可以包含像 `@./docs/prompt-guidelines.md` 這樣的行來引入額外的上下文[檔案](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Modularizing%20Context%20with%20Imports%3A%20You,files)。這能讓你的指示保持良好組織。

有了精心設計的 `GEMINI.md`，你基本上給了 Gemini CLI 一個專案需求和慣例的「記憶」。這種**持久化上下文**能帶來更相關的回應，減少來回調整提示詞的需求。

## 技巧 2：建立自訂斜線指令

**快速應用場景：** 透過定義自己的斜線指令來加速重複性任務。例如，你可以建立 `/test:gen` 指令從描述生成單元測試，或 `/db:reset` 指令來刪除並重建測試資料庫。這能以符合你工作流程的單行指令擴充 Gemini CLI 的功能。

Gemini CLI 支援**自訂斜線指令**，你可以在簡單的設定檔中定義它們。本質上，這些是預先定義的提示詞範本。要建立自訂指令，在 `~/.gemini/` 下建立 `commands/` 目錄（全域指令）或在專案的 `.gemini/` 資料夾中建立（專案特定[指令](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Custom%20Commands)）。在 `commands/` 內，為每個新指令建立一個 TOML 檔案。檔案名稱格式決定指令名稱：例如 `test/gen.toml` 檔案定義了 `/test:gen` 指令。

讓我們看個範例。假設你想要一個指令從需求描述生成單元測試。你可以建立 `~/.gemini/commands/test/gen.toml`，內容如下：

```markdown
# Invoked as: /test:gen "Description of the test"
description = "Generates a unit test based on a requirement."
prompt = """
You are an expert test engineer. Based on the following requirement, please write a comprehensive unit test using the Jest framework.

Requirement: {{args}}
"""
```

現在，重新載入或重啟 Gemini CLI 後，你只需輸入：

```bash
/test:gen "Ensure the login button redirects to the dashboard upon success"
```

Gemini CLI 會識別 `/test:gen` 並將你提供的參數（在此例中是需求）替換到提示詞範本的 `{{args}}` 中。接著 AI 會[相應地](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Example%3A%20%60)生成 Jest 單元測試。`description` 欄位是選填的，但當你執行 `/help` 或 `/tools` 列出可用指令時會用到。

這個機制非常強大 - 實際上，你可以用自然語言來編寫 AI 腳本。社群已經建立了許多有用的自訂指令。例如，Google 的 DevRel 團隊分享了一組 *10 個實用工作流程指令*（透過開源儲存庫），展示了如何編寫常見流程的腳本，像是建立 API 文件、清理資料或設定樣板[程式碼](https://cloud.google.com/blog/topics/developers-practitioners/agent-factory-recap-deep-dive-into-gemini-cli-with-taylor-mullen#:~:text=,to%20generate%20a%20better%20output)。透過定義自訂指令，你將複雜的提示詞（或一系列提示詞）打包成可重複使用的快捷方式。

**進階技巧：** 自訂指令也可以用來強制格式化或為 AI 套用特定「角色」來處理某些任務。例如，你可能有個 `/review:security` 指令，總是在提示詞前加上「你是一位安全稽核員...」來審查程式碼中的漏洞。這種方法確保 AI 對特定類別的任務回應保持一致。

要與團隊分享指令，你可以將 TOML 檔案提交到專案儲存庫中（在 `.gemini/commands` 目錄下）。擁有 Gemini CLI 的團隊成員在該專案工作時會自動取得這些指令。這是**標準化團隊 AI 輔助工作流程**的絕佳方式。

## 技巧 3：使用自己的 `MCP` 伺服器擴充 Gemini

**快速應用場景：** 假設你想讓 Gemini 與外部系統或非內建的自訂工具介接 - 例如查詢專有資料庫，或整合 Figma 設計。你可以執行自訂的 **Model Context Protocol (MCP) 伺服器**並將它接入 Gemini [CLI](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Extend%20the%20CLI%20with%20your,add%7Clist%7Cremove%3E%60%20commands)。MCP 伺服器讓你能為 Gemini 新增工具與能力，有效地**擴充代理**。

Gemini CLI 內建了數個 MCP 伺服器（例如啟用 Google 搜尋、程式碼執行沙盒等），你也可以新增自己的。MCP 伺服器本質上是一個外部程序（可以是本地腳本、微服務，甚至是雲端端點），使用簡單的協定來處理 Gemini 的任務。這種架構使 Gemini CLI 具有高度[擴充性](https://blog.google/technology/developers/introducing-gemini-cli-open-source-ai-agent/#:~:text=,interactively%20within%20your%20scripts)。

**MCP 伺服器範例：** 一些社群和 Google 提供的 MCP 整合包括 **Figma MCP**（從 Figma 取得設計細節）、**剪貼簿 MCP**（從系統剪貼簿讀寫）等。實際上，在內部展示中，Gemini CLI 團隊展示了「Google Docs MCP」伺服器，允許直接將內容儲存到 Google [Docs](https://cloud.google.com/blog/topics/developers-practitioners/agent-factory-recap-deep-dive-into-gemini-cli-with-taylor-mullen#:~:text=%2A%20Utilize%20the%20google,summary%20directly%20to%20Google%20Docs)。概念是當 Gemini 需要執行內建工具無法處理的動作時，它可以委派給你的 MCP 伺服器。

**如何新增：** 你可以透過 `settings.json` 或使用 CLI 設定 MCP 伺服器。快速設定可以試試 CLI 指令：

```bash
gemini mcp add myserver --command "python3 my_mcp_server.py" --port 8080
```

這會註冊一個名為「myserver」的伺服器，Gemini CLI 會透過執行指定指令（這裡是 Python 模組）在 8080 埠啟動它。在 `~/.gemini/settings.json` 中，它會在 `mcpServers` 下新增一個條目。例如：

```json
"mcpServers": {
  "myserver": {
    "command": "python3",
    "args": ["-m", "my_mcp_server", "--port", "8080"],
    "cwd": "./mcp_tools/python",
    "timeout": 15000
  }
}
```

這個設定（基於官方文件）告訴 Gemini 如何啟動 MCP 伺服器以及[位置](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Example%20)。一旦執行，該伺服器提供的工具就能讓 Gemini CLI 使用。你可以用斜線指令列出所有 MCP 伺服器及其工具：

```bash
/mcp
```

這會顯示任何已註冊的伺服器以及它們[公開的](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Command%20Description%20,List%20active%20extensions)工具名稱。

**MCP 的威力：** MCP 伺服器可以提供**豐富的多模態結果**。例如，透過 MCP 提供的工具可以在回應中返回圖片或格式化表格給 Gemini [CLI](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Capabilities%3A)。它們也支援 OAuth 2.0，所以你可以安全地連接到 API（如 Google 的 API、GitHub 等），透過 MCP 工具而不暴露[憑證](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Extend%20the%20CLI%20with%20your,add%7Clist%7Cremove%3E%60%20commands)。基本上，只要你能寫成程式碼，就能包裝成 MCP 工具 - 將 Gemini CLI 變成協調多項服務的中心。

**預設 vs. 自訂：** 預設情況下，Gemini CLI 的內建工具已經涵蓋很多（讀取檔案、網頁搜尋、執行 shell 指令等），但 MCP 讓你能走得更遠。一些進階使用者建立了 MCP 伺服器來與內部系統介接或執行專門的資料處理。例如，你可以有個 `database-mcp` 提供 `/query_db` 工具來對公司資料庫執行 SQL 查詢，或 `jira-mcp` 透過自然語言建立工單。

建立自己的 MCP 時，要注意安全性：預設情況下，自訂 MCP 工具需要確認，除非你標記為信任。你可以用像是為伺服器設定 `trust: true`（會自動核准其工具動作）這樣的設定來控制安全性，或透過白名單列出特定安全工具和黑名單列出危險[工具](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=,takes%20precedence)。

總之，**MCP 伺服器解鎖了無限的整合可能**。這是讓 Gemini CLI 成為 AI 助手與任何你需要它協作的系統之間膠水的進階功能。如果你有興趣建立一個，可以查看官方 [MCP 指南](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Transport%20)和社群範例。

## 技巧 4：善用記憶新增與回想功能

**快速應用場景：** 透過將重要事實新增到 AI 的長期記憶中，讓它隨時掌握。例如，找出資料庫埠或 API token 後，你可以：

```bash
/memory add "我們的 staging RabbitMQ 在 5673 埠"
```

這會儲存該事實，讓你（或 AI）[之後](https://binaryverseai.com/gemini-cli-open-source-ai-tool/#:~:text=Gemini%20CLI%20Ultimate%20Agent%3A%2060,a%20branch%20of%20conversation)不會忘記。你可以隨時用 `/memory show` 回想記憶中的所有內容。

`/memory` 指令提供了簡單但強大的*持久記憶*機制。當你使用 `/memory add <文字>` 時，給定的文字會被附加到專案的全域上下文（技術上，它被儲存到全域 `~/.gemini/GEMINI.md` 檔案或專案的 [`GEMINI.md`](https://genmind.ch/posts/Howto-Supercharge-Your-Terminal-with-Gemini-CLI/#:~:text=,load%20memory%20from%20%60GEMINI.md)）。這有點像做筆記並釘在 AI 的虛擬佈告欄上。一旦新增，AI 在未來的互動中會始終在提示詞上下文中看到該筆記，跨對話階段也一樣。

考慮這個範例：你在除錯問題時發現了一個不明顯的洞察（「設定旗標 `X_ENABLE` 必須設為 `true`，否則服務會啟動失敗」）。如果你將這加入記憶，之後當你或 AI 討論相關問題時，它不會忽略這個關鍵細節 - 因為它在上下文中。

**使用 `/memory`：**

* `/memory add "<文字>"` - 新增事實或筆記到記憶（持久上下文）。這會立即用新條目更新 `GEMINI.md`。

* `/memory show` - 顯示記憶的完整內容（即目前載入的合併上下文檔案）。

* `/memory refresh` - 從磁碟重新載入上下文（如果你在 Gemini CLI 外部手動編輯了 `GEMINI.md` 檔案，或多人協作時很有用）。

因為記憶是以 Markdown 儲存的，你也可以手動編輯 `GEMINI.md` 檔案來整理或組織資訊。`/memory` 指令是為了在對話中方便使用，不需要開啟編輯器。

**進階技巧：** 這個功能很適合「決策日誌」。如果你在對話中決定了某個方法或規則（例如要使用的特定函式庫，或約定的程式碼風格），將它加入記憶。AI 接著會記住該決策，避免之後產生矛盾。這在可能跨越數小時或數天的長對話階段中特別有用 - 透過儲存關鍵點，你可以減輕模型在對話變長時忘記早期上下文的傾向。

另一個用途是個人筆記。因為 `~/.gemini/GEMINI.md`（全域記憶）會為所有對話階段載入，你可以在那裡放置一般偏好或資訊。例如，「使用者名稱是 Alice。請禮貌地說話並避免俚語。」這就像配置 AI 的角色或全域知識。只是要注意全域記憶適用於*所有*專案，所以不要在那裡塞入專案特定的資訊。

總結來說，**記憶新增與回想**幫助 Gemini CLI 維持狀態。把它想成一個隨專案成長的知識庫。使用它來避免重複自己，或提醒 AI 那些它原本需要從頭重新發現的事實。

## 技巧 5：使用檢查點與 `/restore` 作為復原按鈕

**快速應用場景：** 如果 Gemini CLI 對你的檔案做了一系列你不滿意的變更，你可以*立即回滾*到先前的狀態。啟動 Gemini 時啟用檢查點（或在設定中），並使用 `/restore` 指令來復原變更，就像輕量級的 Git [還原](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=,Exit%20the%20Gemini%20CLI)。`/restore` 將你的工作區回滾到儲存的檢查點；對話狀態可能會根據檢查點的擷取方式受到影響。

Gemini CLI 的**檢查點**功能就像安全網。啟用時，CLI 會在每次會修改[檔案](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=When%20,snapshot%20before%20tools%20modify%20files)的工具執行*之前*對專案檔案拍攝快照。如果出了問題，你可以還原到上一個已知良好狀態。這本質上是 AI 動作的版本控制，不需要你每次都手動提交到 Git。

**如何使用：** 你可以用 `--checkpointing` 旗標啟動 CLI 來開啟檢查點：

```bash
gemini --checkpointing
```

或者，你可以在設定中新增 `"checkpointing": { "enabled": true }` 到 [`settings.json`](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=%7B%20,true) 讓它成為預設值。啟用後，你會注意到每次 Gemini 要寫入檔案時，它會說類似「檢查點已儲存」的訊息。

如果你接著發現 AI 做的編輯有問題，你有兩個選項：

* 執行 `/restore list`（或只是不帶參數的 `/restore`）來查看最近檢查點的清單，包含時間戳和描述。

* 執行 `/restore <id>` 回滾到特定檢查點。如果你省略 id 且只有一個待處理檢查點，它會[預設](https://medium.com/@ferreradaniel/gemini-cli-free-ai-tool-upgrade-5-new-features-you-need-right-now-04cfefac5e93#:~:text=Step)還原那個。

例如：

```bash
/restore
```

Gemini CLI 可能會輸出：

```
0: [2025-09-22 10:30:15] Before running 'apply_patch'
1: [2025-09-22 10:45:02] Before running 'write_file'
```

接著你可以執行 `/restore 0` 將所有檔案變更（甚至對話上下文）回滾到該檢查點的狀態。這樣你就能「復原」錯誤的程式碼重構或 Gemini [做的](https://medium.com/@ferreradaniel/gemini-cli-free-ai-tool-upgrade-5-new-features-you-need-right-now-04cfefac5e93#:~:text=1,point%20and%20roll%20back%20instantly)任何其他變更。

**會還原什麼：** 檢查點會擷取你工作目錄的狀態（所有 Gemini CLI 被允許修改的檔案）和工作區檔案（對話狀態也可能根據檢查點的擷取方式被回滾）。當你還原時，它會將檔案覆寫為舊版本，並將對話記憶重設為該快照。這就像讓 AI 代理時光旅行回到它走錯路之前。注意它無法復原外部副作用（例如，如果 AI 執行了資料庫遷移，它無法復原那個），但檔案系統和對話上下文中的任何東西都可以處理。

**最佳實踐：** 對於非瑣碎的任務，保持檢查點開啟是個好主意。開銷很小，而且提供安心感。如果你發現不需要某個檢查點（一切順利），你總是可以清除它或讓下一個覆蓋它。開發團隊建議特別在多步驟程式碼[編輯](https://medium.com/@ferreradaniel/gemini-cli-free-ai-tool-upgrade-5-new-features-you-need-right-now-04cfefac5e93#:~:text=Tips%20to%20avoid%20messy%20rollbacks)前使用檢查點。不過對於關鍵任務專案，你仍應使用適當的版本控制（`git`）作為主要安全[網](https://medium.com/@ferreradaniel/gemini-cli-free-ai-tool-upgrade-5-new-features-you-need-right-now-04cfefac5e93#:~:text=No,VS%20Code%20is%20already%20free) - 將檢查點視為快速復原的便利功能，而非完整的 VCS。

本質上，`/restore` 讓你能有信心地使用 Gemini CLI。你可以讓 AI 嘗試大膽的變更，知道如果需要你有個*「天啊不要」按鈕*可以倒帶。

## 技巧 6：讀取 Google Docs、Sheets 等文件。配置 Workspace MCP 伺服器後，你可以貼上 Docs/Sheets 連結，讓 MCP 在權限許可下取得內容

**快速應用場景：** 想像你有一個包含規格或資料的 Google Doc 或 Sheet，你希望 AI 使用它。不用複製貼上內容，你可以提供連結，配置了 Workspace MCP 伺服器後，Gemini CLI 就能擷取並讀取它。

例如：

```bash
總結這份設計文件的需求：https://docs.google.com/document/d/<id>
```

Gemini 可以拉取該 Doc 的內容並整合到回應中。同樣地，它也能透過連結讀取 Google Sheets 或 Drive 檔案。

**運作原理：** 這些功能通常透過 **MCP 整合**啟用。Google 的 Gemini CLI 團隊已經建立（或正在開發）Google Workspace 的連接器。一種方法是執行一個小型 MCP 伺服器，當給定 URL 或 [ID](https://github.com/google-gemini/gemini-cli/issues/7175) 時，使用 Google 的 API（Docs API、Sheets API 等）來擷取文件內容。配置後，你可能有像 `/read_google_doc` 這樣的斜線指令，或是簡單的自動偵測，看到 Google Docs 連結就會呼叫適當的工具來擷取它。

例如，在 Agent Factory podcast 展示中，團隊使用了 **Google Docs MCP** 直接將摘要儲存到[文件](https://cloud.google.com/blog/topics/developers-practitioners/agent-factory-recap-deep-dive-into-gemini-cli-with-taylor-mullen#:~:text=%2A%20Utilize%20the%20google,summary%20directly%20to%20Google%20Docs) - 這意味著他們也能在第一時間讀取文件的內容。實際上，你可能會這樣做：

```bash
@https://docs.google.com/document/d/XYZ12345
```

使用 `@` 搭配 URL（上下文參考語法）告訴 Gemini CLI 擷取該資源。有了 Google Doc 整合，該文件的內容會被拉取，就像是本地檔案一樣。從那裡開始，AI 可以摘要它、回答相關問題，或在對話中以其他方式使用它。

同樣地，如果你貼上 Google Drive **檔案連結**，適當配置的 Drive 工具可以下載或開啟該檔案（假設權限和 API 存取已設定）。**Google Sheets** 可以透過執行查詢或讀取儲存格範圍的 MCP 提供，讓你可以問像「這個 Sheet [連結] 中預算欄位的總和是多少？」這樣的問題，AI 就能計算它。

**設定方式：** 截至目前，Google Workspace 整合可能需要一些調整（取得 API 憑證，執行像 [Kanshi Tanaike](https://medium.com/google-cloud/managing-google-docs-sheets-and-slides-by-natural-language-with-gemini-cli-and-mcp-62f4dfbef2d5#:~:text=To%20implement%20this%20approach%2C%20I,methods%20for%20each%20respective%20API) 所描述的 MCP 伺服器等）。密切關注官方 Gemini CLI 儲存庫和社群論壇，尋找現成的擴充功能 - 例如，官方的 Google Docs MCP 可能會作為外掛程式/擴充功能提供。如果你急於嘗試，可以按照如何在 MCP [伺服器](https://github.com/google-gemini/gemini-cli/issues/7175#:~:text=)中使用 Google API 的指南來撰寫。這通常涉及處理 OAuth（Gemini CLI 為 MCP 伺服器支援），然後公開像 `read_google_doc` 這樣的工具。

**使用技巧：** 當你有這些工具時，使用它們可以很簡單，只需在提示詞中提供連結（AI 可能會自動呼叫工具來擷取它），或使用像 `/doc open <URL>` 這樣的斜線指令。查看 `/tools` 來看有哪些指令可用 - Gemini CLI 會在[那裡](https://dev.to/therealmrmumba/7-insane-gemini-cli-tips-that-will-make-you-a-superhuman-developer-2d7h#:~:text=Gemini%20CLI%20includes%20dozens%20of,can%20supercharge%20your%20dev%20process)列出所有工具和自訂指令。

總之，**Gemini CLI 可以觸及本地檔案系統以外的地方**。無論是 Google Docs、Sheets、Drive，還是其他外部內容，你都可以透過參考來拉取資料。這個專業技巧讓你不用手動複製貼上，保持上下文流暢自然 - 只需參考你需要的文件或資料集，讓 AI 抓取所需內容。這讓 Gemini CLI 成為真正的**知識助手**，適用於你能存取的所有資訊，而不僅僅是磁碟上的檔案。

*（注意：當然，存取私人文件需要 CLI 具有適當的權限。始終確保任何整合都尊重安全和隱私。在企業環境中，設定這類整合可能涉及額外的驗證步驟。）*

## 技巧 7：使用 `@` 引用檔案與圖片提供明確上下文

**快速應用場景：** 不用口頭描述檔案內容或圖片，直接用 Gemini CLI 指向它。使用 `@` 語法，你可以將檔案、目錄或圖片附加到提示詞中。這確保 AI 看到檔案中的確切內容作為[上下文](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Reference%20files%20or%20directories%20in,PDFs%2C%20audio%2C%20and%20video%20files)。例如：

```bash
向我解釋這段程式碼：@./src/main.js
```

這會將 `src/main.js` 的內容包含在提示詞中（最多到 Gemini 的上下文大小限制），讓 AI 可以讀取並[解釋它](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Include%20a%20single%20file%3A)。

這個 `@` *檔案參考*是 Gemini CLI 對開發者最強大的功能之一。它消除了歧義 - 你不是要求模型依賴記憶或猜測檔案內容，而是直接把檔案交給它讀取。你可以用它處理原始碼、文字文件、日誌等。同樣地，你也可以參考**整個目錄**：

```bash
重構 @./utils/ 中的程式碼以使用 async/await。
```

透過附加以斜線結尾的路徑，Gemini CLI 會遞迴地包含該[目錄](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Include%20a%20whole%20directory%20)中的檔案（在合理範圍內，尊重忽略檔案和大小限制）。這對於多檔案重構或分析很棒，因為 AI 可以一起考慮所有相關模組。

更令人印象深刻的是，你可以在提示詞中參考**像圖片這樣的二進位檔案**。Gemini CLI（使用 Gemini 模型的多模態能力）可以理解圖片。例如：

```bash
描述你在這個截圖中看到什麼：@./design/mockup.png
```

圖片會被輸入到模型中，AI 可能會回應類似「這是一個有藍色登入按鈕和標題圖片的登入頁面」[之類的](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Include%20an%20image%3A)。你可以想像各種用途：審查 UI 模擬圖、整理照片（我們會在稍後的技巧中看到），或從圖片中擷取文字（Gemini 也能做 OCR）。

關於有效使用 `@` 參考的一些注意事項：

* **檔案限制：** Gemini 2.5 Pro 有巨大的上下文視窗（最多 100 萬個 [tokens](https://blog.google/technology/developers/introducing-gemini-cli-open-source-ai-agent/#:~:text=To%20use%20Gemini%20CLI%20free,per%20day%20at%20no%20charge)），所以你可以包含相當大的檔案或許多檔案。然而，極大的檔案可能會被截斷。如果檔案巨大（比如說，數十萬行），考慮摘要它或拆分成部分。Gemini CLI 會在參考太大或因大小而跳過某些內容時警告你。

* **自動忽略：** 預設情況下，Gemini CLI 在拉取目錄[上下文](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Reference%20files%20or%20directories%20in,PDFs%2C%20audio%2C%20and%20video%20files)時會尊重你的 `.gitignore` 和 `.geminiignore` 檔案。所以如果你 `@./` 專案根目錄，它不會將巨大的忽略資料夾（像 `node_modules`）傾倒到提示詞中。你可以用 `.geminiignore` 自訂忽略模式，類似 `.gitignore` 的運作方式。

* **明確 vs 隱式上下文：** Taylor Mullen（Gemini CLI 的創作者）強調使用 `@` 進行*明確的上下文注入*，而不是依賴模型的記憶或自己摘要事物。這更精確，並確保 AI 不會虛構內容。只要可能，用 `@` 參考將 AI 指向真實來源（程式碼、設定檔、文件）。這種做法可以顯著提升準確性。

* **串連參考：** 你可以在一個提示詞中包含多個檔案，像是：

```bash
比較 @./foo.py 和 @./bar.py 並告訴我差異。
```

CLI 會包含兩個檔案。只是要注意 token 限制；多個大檔案可能會消耗大量上下文視窗。

使用 `@` 本質上是**即時將知識輸入 Gemini CLI** 的方式。它將 CLI 變成一個可以處理文字和圖片的多模態閱讀器。作為進階使用者，養成善用這個功能的習慣 - 它通常比問 AI 像「開啟檔案 X 並做 Y」這樣的問題（它可能做也可能不做）更快更可靠。相反地，你明確地給它 X 來處理。

## 技巧 8：即時建立工具（讓 Gemini 建立輔助程式）

**快速應用場景：** 如果手邊的任務會受益於小型腳本或工具，你可以要求 Gemini CLI 為你建立該工具 - 就在對話階段中。例如，你可能會說，「寫一個 Python 腳本來解析這個資料夾中的所有 JSON 檔案並擷取錯誤欄位。」Gemini 可以生成腳本，然後你可以透過 CLI 執行它。本質上，你可以**動態擴充工具集**。

Gemini CLI 不限於預先存在的工具；它可以在需要時使用其編碼能力製作新工具。這經常隱式發生：如果你要求複雜的東西，AI 可能會提議寫一個臨時檔案（包含程式碼）然後執行它。作為使用者，你也可以明確引導這個過程：

* **建立腳本：** 你可以提示 Gemini 用你選擇的語言建立腳本或程式。它可能會使用 `write_file` 工具來建立檔案。例如：

```bash
生成一個 Node.js 腳本，讀取目前目錄中的所有 '.log' 檔案並報告每個檔案的行數。
```

Gemini CLI 會起草程式碼，經你核准後，寫入檔案（例如 `script.js`）。接著你可以透過使用 `!` shell 指令（例如 `!node script.js`）來執行它，或要求 Gemini CLI 執行它（如果 AI 認為這是計劃的一部分，它可能會自動使用 `run_shell_command` 來執行剛寫的腳本）。

* **透過 MCP 的臨時工具：** 在進階情境中，AI 甚至可能建議為某些專門任務啟動 MCP 伺服器。例如，如果你的提示詞涉及一些在 Python 中可能更好完成的繁重文字處理，Gemini 可以生成一個簡單的 Python MCP 伺服器並執行它。雖然這比較罕見，但它展示了 AI 可以即時設定新的「代理」。（Gemini CLI 團隊的投影片之一幽默地提到「MCP 伺服器用於一切，甚至有一個叫 LROwn 的」- 暗示你可以讓 Gemini 執行它自己或另一個模型的實例，雖然那更像是個把戲而非實際用途！）。

這裡的關鍵好處是**自動化**。不用你手動停下來寫輔助腳本，你可以讓 AI 作為流程的一部分來做。這就像有個助手可以按需建立工具。這對於資料轉換任務、批次操作或內建工具沒有直接提供的一次性計算特別有用。

**細微差異與安全：** 當 Gemini CLI 為新工具寫程式碼時，你在執行前仍應審查它。`/diff` 視圖（Gemini 會在你核准寫入前顯示檔案差異）是你檢查[程式碼](https://medium.com/@ferreradaniel/gemini-cli-free-ai-tool-upgrade-5-new-features-you-need-right-now-04cfefac5e93#:~:text=Nobody%20enjoys%20switching%20between%20windows,track%20changes%20line%20by%20line)的機會。確保它做你期望的事情，沒有惡意或破壞性的內容（AI 不應該產生有害的東西，除非你的提示詞明確要求，但就像任何來自 AI 的程式碼一樣，要仔細檢查邏輯，特別是對於刪除或修改大量資料的腳本）。

**範例情境：** 假設你有一個 CSV 檔案，想要以複雜的方式過濾它。你要求 Gemini CLI 做，它可能會說：「我會寫一個 Python 腳本來解析 CSV 並套用過濾器。」然後它建立 `filter_data.py`。在你核准並執行後，你得到結果，可能再也不需要那個腳本了。這種工具的臨時建立是個專業技巧 - 它展示了 AI 有效地自主擴充其能力。

**進階技巧：** 如果你發現腳本在當下之外很有用，你可以將它提升為永久工具或指令。例如，如果 AI 生成了一個很棒的日誌處理腳本，你可能之後將它變成自訂斜線指令（技巧 #2）以便重複使用。Gemini 的生成能力與擴充掛鉤的結合，意味著你的工具箱可以在使用 CLI 時不斷進化。

總之，**不要將 Gemini 限制在它附帶的功能上**。把它當作一個可以快速製作新程式甚至迷你伺服器來幫助解決問題的初階開發者。這種方法體現了 Gemini CLI 的代理哲學 - 它會找出需要什麼工具，即使它必須當場編寫它們。

## 技巧 9：使用 Gemini CLI 進行系統疑難排解與設定

**快速應用場景：** 你可以在程式碼專案之外執行 Gemini CLI 來協助一般系統任務 - 把它當作作業系統的智慧助手。例如，如果你的 shell 行為異常，你可以在家目錄開啟 Gemini 並詢問：「修復我的 `.bashrc` 檔案，它有錯誤。」Gemini 可以為你開啟並編輯設定檔。

這個技巧強調 **Gemini CLI 不只是用於程式碼專案 - 它是你整個開發環境的 AI 助手**。許多使用者使用 Gemini 來客製化他們的開發設定或修復機器上的問題：

* **編輯 dotfiles：** 你可以透過參考它（`@~/.bashrc`）來載入你的 shell 設定（`.bashrc` 或 `.zshrc`），然後要求 Gemini CLI 最佳化或疑難排解它。例如，「我的 `PATH` 沒有取得 Go 二進位檔，你可以編輯我的 `.bashrc` 來修復嗎？」AI 可以插入正確的 `export` 行。它會顯示差異供你確認後再儲存變更。

* **診斷錯誤：** 如果你在終端機或應用程式日誌中遇到神秘錯誤，你可以複製它並提供給 Gemini CLI。它會分析錯誤訊息，通常會建議解決步驟。這類似於使用 StackOverflow 或 Google，但 AI 會直接檢查你的情境。例如：「當我執行 `npm install` 時，我得到 `EACCES` 權限錯誤 - 如何修復？」Gemini 可能會偵測到這是 `node_modules` 的權限問題，並引導你更改目錄擁有權或使用適當的 node 版本管理器。

* **在專案外執行：** 預設情況下，如果你在沒有 `.gemini` 上下文的目錄中執行 `gemini`，這只是意味著沒有載入專案特定的上下文 - 但你仍然可以完整使用 CLI。這對於像系統疑難排解這樣的臨時任務很棒。你可能沒有任何程式碼檔案供它考慮，但你仍然可以透過它執行 shell 指令或讓它擷取網路資訊。本質上，你將 Gemini CLI 當作一個可以為你*做事*的 AI 驅動終端機，而不只是聊天。

* **工作站客製化：** 想要更改設定或安裝新工具？你可以問 Gemini CLI，「在我的系統上安裝 Docker」或「配置我的 Git 用 GPG 簽署提交。」CLI 會嘗試執行這些步驟。它可能會從網路擷取指示（使用搜尋工具），然後執行適當的 shell 指令。當然，總是要注意它在做什麼並核准指令 - 但它可以透過自動化多步驟設定程序來節省時間。一個真實例子：一位使用者要求 Gemini CLI「設定我的 macOS Dock 偏好為自動隱藏並移除延遲」，AI 能夠執行必要的 `defaults write` 指令。

把這種模式想成使用 Gemini CLI 作為**智慧 shell**。實際上，你可以將這與技巧 16（shell 直通模式）結合 - 有時你可能會進入 `!` shell 模式來驗證某些東西，然後回到 AI 模式讓它分析輸出。

**注意事項：** 進行系統級任務時，要謹慎處理有廣泛影響的指令（像 `rm -rf` 或系統設定變更）。Gemini CLI 通常會要求確認，而且它不會在你看不到的情況下執行任何東西。但作為進階使用者，你應該清楚正在進行什麼變更。如果不確定，在執行前先要求 Gemini 解釋一個指令（例如，「解釋 `defaults write com.apple.dock autohide-delay -float 0` 做什麼」- 如果你這樣提示，它會很樂意解釋而不是直接執行）。

**疑難排解附加好處：** 另一個巧妙的用途是使用 Gemini CLI 來解析日誌或設定檔尋找問題。例如，「掃描這個 Apache 設定找錯誤」（配合 `@httpd.conf`），或「查看 syslog 中昨天下午 2 點左右的錯誤」（配合 `@/var/log/syslog`，如果可存取）。這就像有個共同管理員。它甚至可以建議崩潰的可能原因或提出常見錯誤模式的修復建議。

總結來說，**不要猶豫，將 Gemini CLI 當作環境問題的助手來啟動**。它在那裡是為了加速你所有的工作流程 - 不只是寫程式碼，還包括維護你寫程式碼的系統。許多使用者報告說，在 Gemini 的協助下客製化他們的開發環境，感覺就像有個技術夥伴隨時待命來處理繁瑣或複雜的設定步驟。

## 技巧 10：YOLO 模式 - 自動確認工具動作（謹慎使用）

**快速應用場景：** 如果你感到有信心（或冒險），你可以讓 Gemini CLI 執行工具動作而不每次都詢問你的確認。這就是 **YOLO 模式**（You Only Live Once）。它透過 `--yolo` 旗標或在[對話階段](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=,prompt%20in%20an%20external%20editor)中按 `Ctrl+Y` 來啟用。在 YOLO 模式下，一旦 AI 決定使用工具（像執行 shell 指令或寫入檔案），它會立即執行，不會出現「核准？(y/n)」提示。

**為什麼使用 YOLO 模式？** 主要是為了速度和便利性，**當你信任 AI 的動作時**。有經驗的使用者可能會在進行大量重複性安全操作時切換 YOLO。例如，如果你要求 Gemini 逐一生成 10 個不同的檔案，核准每個都會減慢流程；YOLO 模式會讓它們全部自動寫入。另一種情境是在完全自動化的腳本或 CI 管線中使用 Gemini CLI - 你可能用 `--yolo` 無頭執行，這樣它就不會暫停等待確認。

要從一開始就在 YOLO 模式下啟動，用以下方式啟動 CLI：

```bash
gemini --yolo
```

或簡短形式 `gemini -y`。你會在 CLI 中看到一些指示（像是不同的提示符號或通知）表示自動核准已[開啟](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=initial%20prompt.%20%2A%20%60,to%20revert%20changes)。在互動對話階段中，你可以隨時按 **Ctrl+Y** 來切換 - CLI 通常會在頁尾顯示像「YOLO 模式已啟用（所有動作自動核准）」這樣的[訊息](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=,prompt%20in%20an%20external%20editor)。

**重大警告：** YOLO 模式很強大但**有風險**。Gemini 團隊自己將它標記為給「大膽的使用者」- 意思是你應該意識到 AI 可能在不詢問的情況下執行危險指令。在正常模式下，如果 AI 決定執行 `rm -rf /`（最壞情況），你顯然會拒絕。在 YOLO 模式下，該指令會立即執行（並可能毀掉你的一天）。雖然這種極端錯誤不太可能（AI 的系統提示詞包含安全指南），但確認的整個意義是抓住任何不想要的動作。YOLO 移除了那個安全網。

**YOLO 的最佳實踐：** 如果你想要一些便利性但不想承擔全部風險，考慮*允許列表*特定指令。例如，你可以在設定中配置某些工具或指令模式不需要確認（像允許所有 `git` 指令，或唯讀動作）。實際上，Gemini CLI 支援跳過特定指令確認的設定：例如，你可以設定像 `"tools.shell.autoApprove": ["git ", "npm test"]` 這樣的東西來總是執行[那些](https://google-gemini.github.io/gemini-cli/docs/cli/configuration.html#:~:text=match%20at%20L247%20%60%5B,Default%3A%20%60undefined)指令。這樣，你可能不需要全域的 YOLO 模式 - 你選擇性地只對安全指令 YOLO。另一種方法：使用 YOLO 時在沙盒或容器中執行 Gemini，這樣即使它做了瘋狂的事情，你的系統也是隔離的（Gemini 有 `--sandbox` 旗標在 Docker [容器](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=echo%20,gemini)中執行工具）。

許多進階使用者頻繁地開關 YOLO - 在進行一連串次要的檔案編輯或查詢時開啟它，在要做關鍵事情時關閉。你可以使用鍵盤快捷鍵作為快速切換。

總結來說，**YOLO 模式以監督為代價消除摩擦**。這是個要謹慎且明智使用的專業功能。它真正展示了對 AI 的信任（或魯莽！）。如果你是 Gemini CLI 的新手，在你清楚理解它傾向做什麼的模式之前，你可能應該避免 YOLO。如果你確實使用它，請加倍確保有版本控制或備份 - 以防萬一。

*（如果這能讓你感到安慰，你不是一個人 - 社群中許多人開玩笑說「我 YOLO 了，結果 Gemini 做了瘋狂的事。」所以要使用它，但是... 嗯，你只活一次。）*

## 技巧 11：無頭模式與腳本模式（在背景執行 Gemini CLI）

**快速應用場景：** 你可以在腳本或自動化中使用 Gemini CLI，透過**無頭模式**執行。這意味著你透過命令列參數或環境變數提供提示詞（或甚至完整對話），Gemini CLI 產生輸出然後結束。這很適合與其他工具整合或按計劃觸發 AI 任務。

例如，要在不開啟 REPL 的情況下獲得一次性答案，你已經看過可以使用 `gemini -p "...提示詞..."`。這已經是無頭使用：它印出模型的回應並回到 [shell](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Non,and%20get%20a%20single%20response)。但你可以做更多：

* **系統提示詞覆寫：** 如果你想用自訂系統角色或指示集（不同於預設）執行 Gemini CLI，你可以使用環境變數 `GEMINI_SYSTEM_MD`。透過設定這個，你告訴 Gemini CLI 忽略其內建系統提示詞，改用你提供的檔案。例如：

```bash
export GEMINI_SYSTEM_MD="/path/to/custom_system.md"
gemini -p "以高度謹慎執行任務 X"
```

這會在執行[提示詞](https://medium.com/google-cloud/practical-gemini-cli-bring-your-own-system-instruction-19ea7f07faa2#:~:text=The%20,rather%20than%20its%20hardcoded%20defaults)前，載入你的 `custom_system.md` 作為系統提示詞（AI 遵循的「角色」和規則）。或者，如果你設定 `GEMINI_SYSTEM_MD=true`，CLI 會在目前專案的 `.gemini` [目錄](https://medium.com/google-cloud/practical-gemini-cli-bring-your-own-system-instruction-19ea7f07faa2#:~:text=The%20feature%20is%20enabled%20by,specific%20configurations)中尋找名為 `system.md` 的檔案。這個功能非常進階 - 它本質上允許你用自己的指示*替換 CLI 的內建大腦*，有些使用者為了專門的工作流程這樣做（像模擬特定角色或執行超嚴格的政策）。小心使用，因為替換核心提示詞會影響工具使用（核心提示詞包含 AI 如何選擇和使用[工具](https://medium.com/google-cloud/practical-gemini-cli-bring-your-own-system-instruction-19ea7f07faa2#:~:text=If%20you%20read%20my%20previous,proper%20functioning%20of%20Gemini%20CLI)的重要指示）。

* **透過 CLI 直接提示詞：** 除了 `-p`，還有 `-i`（互動提示詞），它用初始提示詞啟動對話階段，然後保持開啟。例如：`gemini -i "哈囉，我們來除錯一些東西"` 會開啟 REPL 並已經向模型打過招呼。這在你希望第一個問題在啟動時立即被詢問時很有用。

* **用 shell 管線編寫腳本：** 你可以透過管線不只傳遞文字，還有檔案或指令輸出給 Gemini。例如：`gemini -p "摘要這個日誌：" < big_log.txt` 會將 `big_log.txt` 的內容輸入到提示詞中（在「摘要這個日誌：」這句話之後）。或者你可能會做 `some_command | gemini -p "根據上面的輸出，哪裡出錯了？"`。這個技巧允許你用 AI 分析來組合 Unix 工具。它是無頭的，因為它是單次操作。

* **在 CI/CD 中執行：** 你可以將 Gemini CLI 整合到建構流程中。例如，CI 管線可能執行測試，然後使用 Gemini CLI 自動分析失敗的測試輸出並發布評論。使用 `-p` 旗標和環境驗證，這可以編寫成腳本。（當然，確保環境有所需的 API 金鑰或驗證。）

還有一個無頭技巧：**`--format=json` 旗標**（或設定）。Gemini CLI 可以用 JSON 格式而不是人類可讀的文字輸出回應，如果你[配置](https://google-gemini.github.io/gemini-cli/docs/cli/configuration.html#:~:text=)它。這對於程式化消費很有用 - 你的腳本可以解析 JSON 來獲得答案或任何工具動作細節。

**為什麼無頭模式重要：** 它將 Gemini CLI 從互動助手轉變為其他程式可以呼叫的**後端服務**或工具。你可以安排一個 cronjob 每晚執行 Gemini CLI 提示詞（想像生成報告或用 AI 邏輯清理某些東西）。你可以在 IDE 中連接一個按鈕，觸發針對特定任務的無頭 Gemini 執行。

**範例：** 假設你想要每日新聞網站摘要。你可以有個腳本：

```bash
gemini -p "網頁擷取「https://news.site/top-stories」並擷取標題，然後寫入 headlines.txt"
```

或許配合 `--yolo`，這樣它就不會要求確認來寫入檔案。這會使用網頁擷取工具來取得頁面，以及檔案寫入工具來儲存標題。全自動，沒有人工介入。一旦你把 Gemini CLI 當作可編寫腳本的元件，可能性是無窮的。

總之，**無頭模式**啟用了自動化。這是 Gemini CLI 與其他系統之間的橋樑。掌握它意味著你可以擴大 AI 使用規模 - 不只是在你打字時，即使在你不在時，你的 AI 代理也可以為你工作。

*（提示：對於真正長時間執行的非互動任務，你可能也會研究 Gemini CLI 的「計劃」模式，或它如何在沒有干預的情況下生成多步驟計劃。然而，這些是超出此範圍的進階主題。在大多數情況下，透過無頭模式精心設計的單一提示詞可以完成很多事情。）*

## 技巧 12：儲存與恢復對話階段

**快速應用場景：** 如果你與 Gemini CLI 除錯問題已經一小時需要停下來，你不必失去對話上下文。使用 `/chat save <名稱>` 來儲存對話階段。之後（即使重啟 CLI 後），你可以使用 `/chat resume <名稱>` 從離開的地方[繼續](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=,help%20information%20and%20available%20commands)。這樣，長時間對話可以無縫地暫停和繼續。

Gemini CLI 本質上有內建的對話階段管理器。要知道的指令有：

* `/chat save <標籤>` - 用你[提供的](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=,help%20information%20and%20available%20commands)標籤/名稱儲存目前對話狀態。標籤就像該對話階段的檔案名或鍵。如果想要的話可以經常儲存，它會覆寫該標籤（如果存在）。（使用描述性名稱很有幫助 - 例如，`chat save fix-docker-issue`。）

* `/chat list` - 列出所有已儲存的對話階段（你[使用過的](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=,help%20information%20and%20available%20commands)標籤）。這幫助你記住之前儲存過什麼。

* `/chat resume <標籤>` - 恢復具有該標籤的對話階段，將整個對話上下文和歷史恢復到[儲存時](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=,help%20information%20and%20available%20commands)的狀態。就像你從未離開。你可以從那個點繼續對話。

* `/chat share` - （儲存到檔案）這很有用，因為你可以與其他人分享整個對話，他們可以繼續對話階段。幾乎像協作一樣。

在底層，這些對話階段可能儲存在 `~/.gemini/chats/` 或類似位置。它們包含對話訊息和任何相關狀態。這個功能對以下情況超級有用：

* **長時間除錯對話階段：** 有時與 AI 除錯可能需要長時間來回。如果你不能一次解決，儲存它並稍後回來（也許有新想法）。AI 仍會「記得」之前的所有東西，因為整個上下文都重新載入了。

* **多日任務：** 如果你使用 Gemini CLI 作為專案助手，你可能有一個針對「重構模組 X」的對話階段，跨越多天。你可以每天恢復那個特定對話，這樣上下文就不會每天重設。同時，你可能有另一個針對「撰寫文件」的對話階段分別儲存。切換上下文只是儲存一個和恢復另一個的事情。

* **團隊交接：** 這更實驗性，但理論上，你可以與同事分享已儲存對話的內容（儲存的檔案可能是可攜的）。如果他們把它放在他們的 `.gemini` 目錄並恢復，他們可以看到相同的上下文。**實際上更簡單的協作方法**是只從日誌複製相關的問答，並使用共享的 `GEMINI.md` 或提示詞，但有趣的是對話階段資料是你可以保留的。

**使用範例：**

```bash
/chat save api-upgrade
```

*（對話階段儲存為「api-upgrade」）*

```bash
/quit
```

*（稍後，重新開啟 CLI）*

```bash
$ gemini
gemini> /chat list
```

*（顯示：api-upgrade）*

```bash
gemini> /chat resume api-upgrade
```

現在模型以最後交流的狀態準備好迎接你。你可以向上捲動確認之前的所有訊息都在。

**進階技巧：** 儲存[對話時](https://medium.com/@ferreradaniel/gemini-cli-free-ai-tool-upgrade-5-new-features-you-need-right-now-04cfefac5e93#:~:text=Naming%20conventions%20to%20keep%20projects,organized)使用有意義的標籤。不要用 `/chat save session1`，給它一個與主題相關的名稱（例如 `/chat save memory-leak-bug`）。這會幫助你之後透過 `/chat list` 找到正確的對話階段。沒有宣布對可以儲存多少對話階段的嚴格限制，但偶爾清理舊的對話階段可能是明智的，只是為了組織。

這個功能將 Gemini CLI 變成持久的顧問。你不會失去對話中獲得的知識；你總是可以暫停和恢復。這是與一些其他在關閉時忘記上下文的 AI 介面的差異點。對於進階使用者來說，這意味著**你可以與 AI 維持並行的工作執行緒**。就像你為不同任務有多個終端分頁，你可以有多個已儲存的對話階段，並在任何時候恢復你需要的那個。

## 技巧 13：多目錄工作區 - 一個 Gemini，多個資料夾

**快速應用場景：** 你的專案分散在多個儲存庫或目錄中嗎？你可以啟動 Gemini CLI 並*同時*存取*所有目錄*，這樣它就能看到統一的工作區。例如，如果你的前端和後端是分開的資料夾，你可以包含兩者，這樣 Gemini 就能編輯或參考兩者中的檔案。

有兩種方式使用**多目錄模式**：

* **啟動旗標：** 啟動 Gemini CLI 時使用 `--include-directories`（或 `-I`）旗標。例如：

```bash
gemini --include-directories "../backend:../frontend"
```

這假設你從，比如說，`scripts` 目錄執行指令，並想包含兩個兄弟資料夾。你提供冒號分隔的路徑列表。Gemini CLI 接著會把所有這些目錄當作一個大工作區。

* **持久設定：** 在你的 `settings.json` 中，你可以定義 `"includeDirectories": ["path1", "path2", [...]](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=,61AFEF%22%2C%20%22AccentPurple)`。這在你總是想要載入某些共同目錄（例如，多個專案使用的共享函式庫資料夾）時很有用。路徑可以是相對或絕對的。路徑中的環境變數（像 `~/common-utils`）是[允許的](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=,61AFEF%22%2C%20%22AccentPurple)。

當多目錄模式啟用時，CLI 的上下文和工具會考慮所有包含位置的檔案。`> /directory show` 指令會列出目前[工作區](https://medium.com/@ferreradaniel/gemini-cli-free-ai-tool-upgrade-5-new-features-you-need-right-now-04cfefac5e93#:~:text=How%20to%20add%20multiple%20directories,step)中的目錄。你也可以在對話階段中動態新增目錄，使用 `/directory add [<path>](https://medium.com/@ferreradaniel/gemini-cli-free-ai-tool-upgrade-5-new-features-you-need-right-now-04cfefac5e93#:~:text=How%20to%20add%20multiple%20directories,step)` - 它會即時載入（可能掃描它以取得上下文，就像啟動時一樣）。

**為什麼使用多目錄模式？** 在微服務架構或模組化程式碼庫中，一段程式碼住在一個儲存庫中，另一段住在不同的儲存庫中是很常見的。如果你只在一個中執行 Gemini，它看不到其他的。透過組合它們，你啟用了跨專案的推理。例如，你可以問，「更新前端中的 API 客戶端以匹配後端的新 API 端點」- Gemini 可以開啟後端資料夾來查看 API 定義，同時開啟前端程式碼來相應地修改它。沒有多目錄，你得一次做一邊，並手動傳遞資訊。

**範例：** 假設你有 `client/` 和 `server/`。你啟動：

```bash
cd client
gemini --include-directories "../server"
```

現在在 `gemini>` 提示符號下，如果你執行 `> !ls`，你會看到它可以列出 `client` 和 `server` 中的檔案（它可能將它們顯示為分開的路徑）。你可以：

```bash
並排開啟 server/routes/api.py 和 client/src/api.js 來比較函數名稱。
```

AI 將能存取兩個檔案。或者你可能會說：

```bash
API 變更了：端點「/users/create」現在是「/users/register」。相應地更新後端和前端。
```

它可以同時在後端路由中建立補丁，並調整前端的 fetch 呼叫。

在底層，Gemini 合併這些目錄的檔案索引。如果每個目錄都很大，可能會有一些效能考量，但通常它能很好地處理多個小到中型專案。速查表注意到這有效地建立了一個具有多個[根的](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=%22includeDirectories%22%3A%20%5B%22..%2Fshared,98C379%22%2C%20%22AccentYellow)工作區。

**技巧中的技巧：** 即使你不總是使用多目錄，要知道你仍然可以透過絕對路徑在提示詞中參考整個檔案系統的檔案（`@/path/to/file`）。然而，沒有多目錄，Gemini 可能沒有權限編輯那些檔案或不知道要主動從它們載入上下文。多目錄正式將它們包含在範圍內，所以它知道整個集合的所有檔案，以便進行像搜尋或跨整個集合的程式碼生成這樣的任務。

**移除目錄：** 如果需要，`/directory remove <path>`（或類似指令）可以從工作區移除一個目錄。這比較不常見，但如果你不小心包含了某些東西，你可以移除它。

總之，**多目錄模式統一了你的上下文**。對於多儲存庫專案或任何程式碼被拆分的情況，這是必備功能。它讓 Gemini CLI 的行為更像一個已經開啟整個解決方案的 IDE。作為進階使用者，這意味著你專案的任何部分都不會超出 AI 的觸及範圍。

## 技巧 14：用 AI 協助整理與清理檔案

**快速應用場景：** 厭倦了雜亂的 `Downloads` 資料夾或組織不良的專案資產嗎？你可以徵召 Gemini CLI 作為智慧整理器。透過提供目錄概覽，它可以分類檔案，甚至將它們移動到子資料夾中（經你核准）。例如，「清理我的 `Downloads`：將圖片移到 `Images` 資料夾，PDF 移到 `Documents`，並刪除臨時檔案。」

因為 Gemini CLI 可以讀取檔案名稱、大小，甚至查看檔案內容，它可以對檔案[整理](https://github.com/google-gemini/gemini-cli/discussions/7890#:~:text=We%20built%20a%20CLI%20tool,trash%20folder%20for%20manual%20deletion)做出明智的決策。一個社群建立的工具稱為 **「Janitor AI」** 展示了這點：它透過 Gemini CLI 執行，將檔案分類為重要 vs 垃圾，並[相應地](https://github.com/google-gemini/gemini-cli/discussions/7890#:~:text=We%20built%20a%20CLI%20tool,trash%20folder%20for%20manual%20deletion)分組。這個過程涉及掃描目錄，使用 Gemini 對檔案名和元資料（如果需要還有內容）的推理，然後將檔案移動到類別中。值得注意的是，它不會自動刪除垃圾 - 而是將它們移到 `Trash` 資料夾供[審查](https://github.com/google-gemini/gemini-cli/discussions/7890#:~:text=organize%20files,trash%20folder%20for%20manual%20deletion)。

以下是你如何用 Gemini CLI 手動複製這樣的工作流程：

1. **調查目錄：** 使用提示詞讓 Gemini 列出和分類。例如：

```bash
列出目前目錄中的所有檔案，並將它們分類為「圖片」、「影片」、「文件」、「壓縮檔」或「其他」。
```

Gemini 可能會使用 `!ls` 或類似工具取得檔案列表，然後分析名稱/副檔名來產生類別。

2. **計劃整理：** 詢問 Gemini 它會如何重新組織。例如：

```bash
為這些檔案提出新的資料夾結構。我想按類型分開（圖片、影片、文件等）。同時識別任何看起來重複或不必要的檔案。
```

AI 可能會回應一個計劃：例如，*「建立資料夾：`Images/`、`Videos/`、`Documents/`、`Archives/`。將 `X.png`、`Y.jpg` 移到 `Images/`；將 `A.mp4` 移到 `Videos/`；等等。檔案 `temp.txt` 看起來不必要（可能是臨時檔案）。」*

3. **透過確認執行移動：** 接著你可以指示它執行計劃。它可能會對每個檔案使用像 `mv` 這樣的 shell 指令。因為這會修改你的檔案系統，你會得到每個的確認提示（除非你 YOLO 它）。仔細核准移動。完成後，你的目錄會按建議整齊組織。

在整個過程中，Gemini 的自然語言理解是關鍵。它可以推理，例如，`IMG_001.png` 是圖片或 `presentation.pdf` 是文件，即使沒有明確說明。它甚至可以開啟圖片（使用其視覺能力）來看裡面是什麼 - 例如，區分截圖 vs 照片 vs 圖示 - 並[相應地](https://dev.to/therealmrmumba/7-insane-gemini-cli-tips-that-will-make-you-a-superhuman-developer-2d7h#:~:text=If%20your%20project%20folder%20is,using%20relevant%20and%20descriptive%20terms)命名或排序它。

**按內容重新命名檔案：** 一個特別神奇的用途是讓 Gemini 根據內容重新命名檔案。Dev Community 文章「7 個瘋狂的 Gemini CLI 技巧」描述了 Gemini 如何**掃描圖片並根據其[內容](https://dev.to/therealmrmumba/7-insane-gemini-cli-tips-that-will-make-you-a-superhuman-developer-2d7h#:~:text=If%20your%20project%20folder%20is,using%20relevant%20and%20descriptive%20terms)自動重新命名**。例如，一個名為 `IMG_1234.jpg` 的檔案，如果 AI 看到它是登入畫面的截圖，可能會被重新命名為 `login_screen.jpg`。要做到這點，你可以提示：

```bash
對於這裡的每個 .png 圖片，查看其內容並將其重新命名為描述性名稱。
```

Gemini 會開啟每個圖片（透過視覺工具），取得描述，然後提出像 `mv IMG_1234.png login_screen.png` 這樣的[動作](https://dev.to/therealmrmumba/7-insane-gemini-cli-tips-that-will-make-you-a-superhuman-developer-2d7h#:~:text=If%20your%20project%20folder%20is,using%20relevant%20and%20descriptive%20terms)。這可以大幅改善資產的組織，特別是在設計或照片資料夾中。

**兩階段方法：** Janitor AI 討論注意到一個兩步驟過程：首先廣泛分類（重要 vs 垃圾 vs 其他），然後精煉[分組](https://github.com/google-gemini/gemini-cli/discussions/7890#:~:text=organize%20files,trash%20folder%20for%20manual%20deletion)。你可以模仿這個：首先分開可能可以刪除的檔案（也許是大的安裝程式 `.dmg` 檔案或重複檔案）和要保留的。然後專注於整理要保留的。總是仔細檢查 AI 標記為垃圾的東西；它的猜測可能不總是正確，所以需要人工監督。

**安全提示：** 當讓 AI 處理檔案移動或刪除時，要有備份或至少準備好復原（用 `/restore` 或你自己的備份）。明智的做法是做一次試運行：要求 Gemini 印出它*會*執行的指令來整理，但不實際執行它們，這樣你可以審查。例如：「列出這個計劃所需的 `mv` 和 `mkdir` 指令，但還不要執行它們。」一旦你審查了列表，你可以複製貼上執行它們，或指示 Gemini 繼續。

這是使用 Gemini CLI 進行「非明顯」任務的主要例子 - 它不只是寫程式碼，而是用 **AI 智慧進行系統整理**。它可以節省時間並為混亂帶來一點秩序。畢竟，作為開發者，我們會累積雜亂（日誌、舊腳本、下載），而 AI 清潔工可能相當方便。

## 技巧 15：壓縮長對話以保持在上下文範圍內

**快速應用場景：** 如果你與 Gemini CLI 對話了很長時間，你可能會達到模型的上下文長度限制，或只是發現對話階段變得笨重。使用 `/compress` 指令來摘要到目前為止的對話，用簡潔的[摘要](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Command%20Description%20,files)取代完整歷史。這為更多討論騰出空間，而不用從頭開始。

大型語言模型有固定的上下文視窗（Gemini 2.5 Pro 的非常大，但不是無限的）。如果你超過它，模型可能會開始忘記早期訊息或失去連貫性。`/compress` 功能本質上是你對話階段的 **AI 生成的 tl;dr**，保留重要點。

**運作原理：** 當你輸入 `/compress` 時，Gemini CLI 會取得整個對話（除了系統上下文）並產生摘要。然後它用該摘要作為單一系統或助手訊息取代對話歷史，保留重要細節但丟棄逐分鐘的對話。它會指示壓縮已發生。例如，在 `/compress` 之後，你可能會看到類似：

```
--- 對話已壓縮 ---
討論摘要：使用者和助手一直在除錯應用程式中的記憶體洩漏。關鍵點：問題可能在 `DataProcessor.js` 中，物件沒有被釋放。助手建議新增日誌記錄並識別了可能的無限迴圈。使用者即將測試修復。
--- 摘要結束 ---
```

從那時起，模型只有該摘要（加上新訊息）作為之前發生事情的上下文。如果摘要捕捉到了重要資訊，這通常就足夠了。

**何時壓縮：** 理想情況下在你*達到*限制之前。如果你注意到對話階段變得冗長（數百回合或大量上下文中的程式碼），主動壓縮。速查表提到自動壓縮設定（例如，當上下文超過[最大值](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=%22includeDirectories%22%3A%20%5B%22..%2Fshared,98C379%22%2C%20%22AccentYellow)的 60% 時壓縮）。如果你啟用它，Gemini 可能會自動壓縮並讓你知道。否則，手動 `/compress` 在你的工具箱中。

**壓縮後：** 你可以正常繼續對話。如果需要，你可以在非常長的對話階段中多次壓縮。每次，你會失去一些細緻度，所以不要沒有原因就頻繁壓縮 - 你可能會得到一個對複雜討論過度簡短的記憶。但通常模型自己的摘要在保留關鍵事實方面相當好（你總是可以自己重申任何關鍵的東西）。

**上下文視窗範例：** 讓我們說明一下。假設你透過參考許多檔案輸入了一個大型程式碼庫，並有 1M token 的上下文（最大值）。如果你接著想要轉移到專案的不同部分，而不是開始新對話階段（失去所有那些理解），你可以壓縮。摘要會濃縮從程式碼中獲得的知識（像「我們載入了模組 A、B、C。A 有這些函數... B 以這些方式與 C 互動...」）。現在你可以繼續詢問新事物，那些知識以抽象方式保留。

**記憶 vs 壓縮：** 注意壓縮不會儲存到長期記憶，它是對話的本地操作。如果你有*永遠*不想失去的事實，考慮技巧 4（新增到 `/memory`）- 因為記憶條目會在壓縮中倖存（它們反正會被重新插入，因為它們在 `GEMINI.md` 上下文中）。壓縮更多關於短暫的對話內容。

**小警告：** 壓縮後，AI 的風格可能會稍微改變，因為它實際上看到的是帶有摘要的「新鮮」對話。它可能會重新介紹自己或改變語氣。你可以像「從這裡繼續...（我們壓縮了）」這樣指示它，使其順暢。實際上，它通常會繼續得很好。

總結來說（雙關語），**當對話階段變長時使用 `/compress`**，以維持效能和相關性。它幫助 Gemini CLI 專注於更大的圖景，而不是對話歷史的每個細節。這樣，你可以進行馬拉松式的除錯對話階段或廣泛的設計討論，而不會用完 AI 正在寫作的「心智紙張」。

## 技巧 16：使用 `!` 直通 Shell 指令（與終端機對話）

**快速應用場景：** 在 Gemini CLI 對話階段的任何時候，你可以透過在前面加上 `!` 來執行實際的 shell 指令。例如，如果你想檢查 git 狀態，只需輸入 `!git status`，它會在你的[終端機](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Run%20a%20single%20command%3A)中執行。這讓你不用切換視窗或上下文 - 你仍在 Gemini CLI 中，但實際上是在告訴它「讓我快速執行這個指令」。

這個技巧是關於 Gemini CLI 中的 **Shell 模式**。有兩種使用方式：

* **單一指令：** 只需在提示詞開頭加上 `!`，後面接任何指令和參數。這會在目前工作目錄執行該指令，並[即時](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Run%20shell%20commands%20directly%20in,the%20CLI)顯示輸出。例如：

```bash
!ls -lh src/
```

會列出 `src` 目錄中的檔案，輸出看起來就像在正常終端機中一樣。輸出後，Gemini 提示符號回來，讓你可以繼續對話或發出更多指令。

* **持久 shell 模式：** 如果你單獨輸入 `!` 並按 Enter，Gemini CLI 會切換到一個子模式，你會得到一個 shell 提示符號（通常看起來像 `shell>` 或[類似](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=)）。現在你可以互動式地輸入多個 shell 指令。這基本上是 CLI 內的迷你 shell。你透過在空行上再次輸入 `!`（或 `exit`）來退出這個模式。例如：

```bash
!
shell> pwd
/home/alice/project
shell> python --version
Python 3.x.x
shell> !
```

最後的 `!` 後，你回到正常的 Gemini 提示符號。

**為什麼有用？** 因為開發是動作和詢問的混合。你可能正在與 AI 討論某事，然後意識到需要編譯程式碼或執行測試來看某些東西。不用離開對話，你可以快速做到並將結果反饋到對話中。實際上，Gemini CLI 通常會為你做這個，作為其工具使用的一部分（例如，當你要求修復測試時，它可能會自動執行 `!pytest`）。但作為使用者，你也有完整的控制權可以手動做。

**範例：**

* 在 Gemini 建議程式碼中的修復後，你可以執行 `!npm run build` 來看它是否編譯，然後複製任何錯誤並要求 Gemini 協助處理那些錯誤。

* 如果你想在 `vim` 或 `nano` 中開啟檔案，你甚至可以透過 `!nano filename` 啟動它（雖然注意因為 Gemini CLI 有自己的介面，在裡面使用互動式編輯器可能有點尷尬 - 最好使用內建的編輯器整合或複製到你的編輯器）。

* 你可以使用 shell 指令為 AI 收集資訊：例如，`!grep TODO -R .` 來找到專案中的所有 TODO，然後你可能會要求 Gemini 協助處理那些 TODO。

* 或者簡單地用於環境任務：如果需要，`!pip install some-package` 等，而不離開 CLI。

**無縫互動：** 一個很酷的方面是對話如何參考輸出。例如，你可以執行 `!curl http://example.com` 來擷取一些資料，看到輸出，然後立即對 Gemini 說，「將上面的輸出格式化為 JSON」- 因為輸出在對話中被印出，AI 在上下文中有它來處理（前提是它不是太大）。

**預設為 shell 的終端機：** 如果你發現自己總是在指令前加 `!`，你實際上可以透過啟動時使用特定工具模式來讓 shell 模式持久化（有一個預設工具的概念）。但更容易的是：如果你計劃執行很多手動指令，在對話階段開始時就進入 shell 模式（什麼都不帶的 `!`），只在想問問題時偶爾才退出。然後你可以在想問問題時退出 shell 模式。這幾乎像是把 Gemini CLI 變成你的正常終端機，只是剛好有個 AI 隨時可用。

**與 AI 計劃整合：** 有時 Gemini CLI 本身會提議執行 shell 指令。如果你核准，它實際上做的事情與 `!command` 相同。理解這點，你知道你總是可以介入。如果 Gemini 卡住了或你想嘗試某些東西，你不用等它建議 - 你可以直接做然後繼續。

總之，`!` **直通**意味著*你不必為了 shell 任務而離開 Gemini CLI*。它崩解了與 AI 對話和在系統上執行指令之間的界限。作為進階使用者，這對效率很棒 - 你的 AI 和你的終端機成為一個連續的環境。

## 技巧 17：把每個 CLI 工具都當作潛在的 Gemini 工具

**快速應用場景：** 意識到 Gemini CLI 可以利用你系統上安裝的**任何**命令列工具作為其問題解決的一部分。AI 有 shell 的存取權，所以如果你有 `cURL`、`ImageMagick`、`git`、`Docker` 或任何其他工具，Gemini 可以在適當時呼叫它。換句話說，*你的整個 `$PATH` 就是 AI 的工具箱*。這大大擴展了它能做的事 - 遠超過其內建工具。

例如，假設你問：「將這個資料夾中的所有 PNG 圖片轉換為 WebP 格式。」如果你安裝了 ImageMagick 的 `convert` 工具，Gemini CLI 可能會規劃類似：對每個[檔案](https://genmind.ch/posts/Howto-Supercharge-Your-Terminal-with-Gemini-CLI/#:~:text=%3E%20%21for%20f%20in%20,png%7D.webp%22%3B%20done)使用 shell 迴圈配合 `convert` 指令。確實，早期部落格的一個範例展示了這個，使用者提示批次轉換圖片，Gemini 用 `convert` [工具](https://genmind.ch/posts/Howto-Supercharge-Your-Terminal-with-Gemini-CLI/#:~:text=)執行了一個 shell 單行指令。

另一種情境：「將我的應用程式部署到 Docker。」如果有 `Docker CLI`，AI 可以根據需要呼叫 `docker build` 和 `docker run` 步驟。或「使用 FFmpeg 從 `video.mp4` 擷取音訊」- 它可以建構 `ffmpeg` 指令。

這個技巧是關於心態：**Gemini 不限於編碼到其中的東西**（這已經很廣泛了）。它可以找出如何使用其他可用的程式來達成[目標](https://medium.com/google-cloud/gemini-cli-tutorial-series-part-4-built-in-tools-c591befa59ba#:~:text=In%20this%20part%2C%20we%20looked,In%20the%20next%20part%2C%20we)。它知道常見的語法，如果需要可以閱讀說明文字（它可以在工具上呼叫 `--help`）。唯一的限制是安全性：預設情況下，它會對任何它想出的 `run_shell_command` 要求確認。但當你變得舒適時，你可能會自動允許某些良性指令（參見 YOLO 或允許的工具設定）。

**注意環境：** 「能力越大，責任越大。」因為每個 shell 工具都是公平遊戲，你應該確保你的 `$PATH` 不包括任何你不希望 AI 無意中執行的東西。這就是技巧 19（自訂 PATH）發揮作用的地方 - 有些使用者為 Gemini 建立受限的 `$PATH`，這樣它就不能，比如說，直接呼叫系統破壞性指令，或許不會遞迴地呼叫 `gemini`（以避免迴圈）。重點是，預設情況下如果 `gcc` 或 `terraform` 或任何東西在 `$PATH` 中，Gemini 可以呼叫它。這不意味著它會隨機這樣做 - 只有在任務需要時 - 但這是可能的。

**思考鏈範例：** 想像你問 Gemini CLI：「設定一個提供目前目錄的基本 HTTP 伺服器。」AI 可能會想：「我可以用 Python 的內建伺服器來做這個。」然後它發出 `!python3 -m http.server 8000`。現在它剛剛使用了系統工具（Python）來啟動伺服器。這是個無害的例子。另一個：「檢查這個 Linux 系統的記憶體使用情況。」AI 可能會使用 `free -h` 指令或從 `/proc/meminfo` 讀取。它實際上在做系統管理員會做的事，透過使用可用指令。

**所有工具都是 AI 的擴充：** 這有點未來派，但考慮到任何命令列程式都可以被視為 AI 可以呼叫來擴充其能力的「函數」。需要解數學問題？它可以呼叫 `bc`（計算機）。需要操作圖片？它可以呼叫圖片處理工具。需要查詢資料庫？如果 CLI 客戶端已安裝且有憑證，它可以使用它。可能性是廣闊的。在其他 AI 代理框架中，這被稱為工具使用，Gemini CLI 被設計為對其代理決定正確[工具](https://cloud.google.com/blog/topics/developers-practitioners/agent-factory-recap-deep-dive-into-gemini-cli-with-taylor-mullen#:~:text=The%20Gemini%20CLI%20%20is,understanding%20of%20the%20developer%20workflow)有很大的信任。

**當它出錯時：** 另一面是如果 AI 誤解了工具或對工具有幻覺。它可能會嘗試呼叫不存在的指令，或使用錯誤的旗標，導致錯誤。這不是大問題 - 你會看到錯誤並可以糾正或澄清。實際上，Gemini CLI 的系統提示詞可能會引導它先做試運行（只是提出指令）而不是盲目執行。所以你通常有機會抓住這些。隨著時間推移，開發者正在改進工具選擇邏輯以減少這些失誤。

主要要點是**把 Gemini CLI 想成有非常大的瑞士刀** - 不只是內建的刀片，還有你 OS 中的每個工具。你不用指示它如何使用它們（如果是標準的東西）；通常它知道或可以找出來。這顯著放大了你能完成的事情。這就像有個知道如何執行你安裝的幾乎任何程式的初階開發者或 devops 工程師。

作為進階使用者，你甚至可以專門安裝額外的 CLI 工具來給 Gemini 更多能力。例如，如果你安裝雲端服務的 CLI（AWS CLI、GCloud CLI 等），理論上 Gemini 可以利用它來管理雲端資源（如果被提示這樣做）。總是確保你理解並信任執行的指令，特別是對於強大的工具（你不會想讓它意外地啟動巨大的雲端實例）。但明智地使用，這個概念 - **一切都是 Gemini 工具** - 是讓它在你將它整合到環境中時*指數級*更有能力的原因。

## 技巧 18：善用多模態 AI - 讓 Gemini 看見圖片等內容

**快速應用場景：** Gemini CLI 不限於文字 - 它是多模態的。這意味著它可以分析圖片、圖表，甚至如果給定的話可以分析 PDF。善用這個。例如，你可以說「這是一個錯誤對話框的截圖，`@./error.png` - 幫我疑難排解。」AI 會「看到」圖片並相應回應。

Google 的 Gemini 模型（及其前身 PaLM2 的 Codey 形式）的一個突出功能是圖片理解。在 Gemini CLI 中，如果你用 `@` 參考圖片，模型會收到圖片資料。它可以輸出描述、分類，或對圖片內容進行推理。我們已經討論了按內容重新命名圖片（技巧 14）和描述截圖（技巧 7）。但讓我們考慮其他創意用途：

* **UI/UX 回饋：** 如果你是與設計師合作的開發者，你可以放入 UI 圖片並要求 Gemini 提供回饋或生成程式碼。「看看這個 UI 模擬圖 `@mockup.png` 並為它生成 React 元件結構。」它可以識別圖片中的元素（標題、按鈕等）並概述程式碼。

* **整理圖片：** 除了重新命名，你可能有混合圖片的資料夾並想按內容排序。「將 `./photos/` 中的圖片按主題（例如日落、山脈、人物）排序到子資料夾中。」AI 可以查看每張照片並分類（這類似於一些照片應用程式用 AI 做的 - 現在你可以透過 Gemini 用自己的腳本做到）。

* **OCR 和資料擷取：** 如果你有錯誤文字的截圖或文件的照片，Gemini 通常可以從中讀取文字。例如，「從 `invoice.png` 擷取文字並將其放入結構化格式。」如 Google Cloud 部落格範例所示，Gemini CLI 可以處理一組發票圖片並輸出它們[資訊](https://medium.com/google-cloud/gemini-cli-tutorial-series-part-4-built-in-tools-c591befa59ba#:~:text=Press%20enter%20or%20click%20to,view%20image%20in%20full%20size)的表格。它基本上做了 OCR + 理解來從圖片中獲得發票號碼、日期、金額。這是個進階用例，但用底層的多模態模型完全可行。

* **理解圖表或圖表：** 如果你有圖表截圖，你可以問「解釋這個圖表的關鍵洞察 `@chart.png`。」它可能會解釋軸和趨勢。準確性可能有所不同，但值得一試。

為了讓這個實用：當你 `@image.png` 時，確保圖片不是太大（雖然模型可以處理相當大的圖片）。CLI 可能會編碼它並發送給模型。回應可能包括描述或進一步動作。你也可以在一個提示詞中混合文字和圖片參考。

**非圖片模態：** CLI 和模型可能也可以透過轉換工具處理 PDF 和音訊。例如，如果你 `@report.pdf`，Gemini CLI 可能會在底層使用 PDF 轉文字工具來擷取文字然後摘要。如果你 `@audio.mp3` 並要求轉錄，它可能會使用音訊轉文字工具（像語音辨識函數）。速查表建議參考 PDF、音訊、視訊檔案是[支援的](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Reference%20files%20or%20directories%20in,PDFs%2C%20audio%2C%20and%20video%20files)，大概是透過呼叫適當的內部工具或 API。所以，「轉錄這個訪談音訊：`@interview.wav`」實際上可能會工作（如果現在沒有，很可能很快會，因為底層的 Google 語音轉文字 API 可以被插入）。

**豐富輸出：** 多模態也意味著如果整合的話，AI 可以在回應中返回圖片（雖然在 CLI 中它通常不會*直接顯示*它們，但它可以儲存圖片檔案或輸出 ASCII 藝術等）。MCP 功能提到工具可以返回[圖片](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Capabilities%3A)。例如，AI 繪圖工具可以生成圖片，Gemini CLI 可以呈現它（也許透過開啟它或給連結）。

**重要：** CLI 本身是基於文字的，所以你不會在終端機中*看到*圖片（除非它能做 ASCII 預覽）。你只會得到分析。所以這主要是關於讀取圖片，而不是顯示它們。如果你在 VS Code 整合中，它可能會在對話視圖中顯示圖片。

總之，**使用 Gemini CLI 時不要忘記 GUI 中的「I」** - 它可以在許多情況下處理視覺的東西，就像處理文字一樣好。這開啟了像視覺除錯、設計協助、從截圖擷取資料等工作流程，全都在同一個工具下。這是一些其他 CLI 工具可能還沒有的差異點。隨著模型改進，這種多模態支援只會變得更強大，所以這是個要利用的未來導向技能。

## 技巧 19：自訂 `$PATH`（與工具可用性）以提升穩定性

**快速應用場景：** 如果你發現 Gemini CLI 變得混亂或呼叫錯誤的程式，考慮用調整過的 `$PATH` 執行它。透過限制或排序可用的執行檔，你可以防止 AI，比如說，呼叫一個你不打算的類似名稱的腳本。本質上，你沙盒化它的工具存取到已知良好的工具。

對大多數使用者來說，這不是問題，但對於擁有大量自訂腳本或多個工具版本的進階使用者，這可能很有幫助。開發者提到的一個原因是避免無限迴圈或奇怪的[行為](https://github.com/google-gemini/gemini-cli/discussions/7890#:~:text=We%20built%20a%20CLI%20tool,trash%20folder%20for%20manual%20deletion)。例如，如果 `gemini` 本身在 `$PATH` 中，一個走錯路的 AI 可能會遞迴地從 Gemini 內部呼叫 `gemini`（一個奇怪的情境，但理論上可能）。或許你有個名為 `test` 的指令與某些東西衝突 - AI 可能會呼叫錯誤的那個。

**如何為 Gemini 設定 PATH：** 最簡單的是在啟動時即時設定：

```bash
PATH=/usr/bin:/usr/local/bin gemini
```

這用只有那些目錄的受限 `$PATH` 執行 Gemini CLI。你可能會排除實驗性或危險腳本所在的目錄。或者，建立一個小的 shell 腳本包裝器來清除或調整 `$PATH`，然後執行 `gemini`。

另一種方法是使用環境或設定來明確禁用某些工具。例如，如果你絕對不希望 AI 使用 `rm` 或某個破壞性工具，你技術上可以在安全的 `$PATH` 中建立別名或虛擬 `rm`，什麼都不做（雖然這可能會干擾正常操作，所以也許不是那個）。更好的方法是設定中的**排除列表**。在擴充或 `settings.json` 中，你可以排除工具[名稱](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=)。例如，

```json
"excludeTools": ["run_shell_command"]
```

這個極端的例子會阻止*所有* shell 指令執行（讓 Gemini 實際上只讀）。更細緻的，之前提到過允許跳過某些確認；類似地，你可能會配置像這樣的東西：

```json
"tools": {
  "exclude": ["apt-get", "shutdown"]
}
```

*（這個語法是說明性的；查閱文件以了解確切用法。）*

原則是，透過控制環境，你減少 AI 用工具做蠢事的風險。這類似於為房子做兒童防護。

**防止無限迴圈：** 一個使用者情境是 Gemini 不斷讀取自己的輸出或重複[讀取](https://support.google.com/gemini/thread/337650803/infinite-loops-with-tool-code-in-answers?hl=en#:~:text=Community%20support,screen%20with%20weird%20scrolling)檔案的迴圈。自訂 `$PATH` 不能直接修復邏輯迴圈，但一個原因可能是如果 AI 呼叫了觸發自己的指令。確保它不能意外地產生另一個 AI 實例（像呼叫 `bard` 或 `gemini` 指令，如果它想這樣做）是好的。從 `$PATH` 移除那些（或在該對話階段重新命名它們）有幫助。

**透過沙盒隔離：** 另一個替代搞亂 `$PATH` 的方法是使用 `--sandbox` 模式（它使用 Docker 或 Podman 在隔離[環境](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=echo%20,gemini)中執行工具）。在那種情況下，AI 的動作是被包含的，只有沙盒映像提供的工具。你可以提供一個有精選工具集的 Docker 映像。這很重手但非常安全。

**特定任務的自訂 PATH：** 你可能對不同專案有不同的 `$PATH` 設定。例如，在一個專案中你想要它使用特定版本的 Node 或本地工具鏈。用指向那些版本的 `$PATH` 啟動 `gemini` 會確保 AI 使用正確的那個。本質上，把 Gemini CLI 當作任何使用者 - 它使用你給它的任何環境。所以如果你需要它選擇 `gcc-10` vs `gcc-12`，相應地調整 `$PATH` 或 `CC` 環境變數。

**總結：** *護欄*。作為進階使用者，你有能力微調 AI 的操作條件。如果你發現與工具使用相關的不良行為模式，調整 `$PATH` 是快速補救。對於日常使用，你可能不需要這個，但如果你將 Gemini CLI 整合到自動化或 CI 中，這是個要記住的專業技巧：給它一個受控的環境。這樣，你確切地知道它能和不能做什麼，這增加了可靠性。

---

## 技巧 20：透過 token 快取與統計追蹤並減少 token 消耗

如果你執行長時間對話或重複附加相同的大檔案，你可以透過開啟 token 快取和監控使用情況來削減成本和延遲。使用 API 金鑰或 Vertex AI 驗證，Gemini CLI 會自動重用先前發送的系統指示和上下文，所以後續請求更便宜。你可以在 CLI 中即時看到節省。

**如何使用**

使用啟用快取的驗證模式。當你用 Gemini API 金鑰或 Vertex AI 驗證時，Token 快取可用。它目前不適用於 OAuth 登入。[Google Gemini](https://google-gemini.github.io/gemini-cli/docs/cli/token-caching.html)

檢查你的使用情況和快取命中。在對話階段中執行 `stats` 指令。當快取啟用時，它會顯示總 token 和 `cached` 欄位。

```bash
/stats
```

指令的描述和快取報告行為記錄在指令參考和 FAQ 中。[Google Gemini+1](https://google-gemini.github.io/gemini-cli/docs/cli/commands.html?utm_source=chatgpt.com)

在腳本中捕獲指標。無頭執行時，輸出 JSON 並解析 `stats` 區塊，其中包含每個模型的 `tokens.cached`：

```bash
gemini -p "Summarize README" --output-format json
```

無頭指南記錄了包含快取 token 計數的 JSON 架構。[Google Gemini](https://google-gemini.github.io/gemini-cli/docs/cli/headless.html)

將對話階段摘要儲存到檔案：對於 CI 或預算追蹤，將 JSON 對話階段摘要寫入磁碟。

```bash
gemini -p "Analyze logs" --session-summary usage.json
```

這個旗標列在更新日誌中。[Google Gemini](https://google-gemini.github.io/gemini-cli/docs/changelogs/)

使用 API 金鑰或 Vertex 驗證，CLI 會自動重用先前發送的上下文，所以後來的回合發送更少的 token。保持 `GEMINI.md` 和大型檔案參考在回合之間穩定會增加快取命中率；你會在統計中看到這反映為快取的 token。

