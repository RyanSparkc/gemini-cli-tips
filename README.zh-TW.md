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

