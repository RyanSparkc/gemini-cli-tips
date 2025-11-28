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

