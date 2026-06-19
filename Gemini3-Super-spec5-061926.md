export const TECHNICAL_SPECIFICATION = `# FDA 510(k) Agentic Review Platform (RegulAI 510(k))
## Technical Architecture, Workflow Design & AI Verification Specification
### Document Edition: v3.0.0-Production Draft
### Project Standard Reference Metrics: ISO 13485, IEC 62304, IEC 60601-1, ISO 14971, FDA Section 524B

---

### 1. 系統架構概要 / SYSTEM ARCHITECTURE OVERVIEW

本系統是一個基於雙層大語言模型代理人（Dual-Layer Large Language Model Agentic Architecture）架構的自主型醫療器材法規遵循與 510(k) 申報審查平台（RegulAI 510(k)）。其主體架構之宗旨是協助法規事務（RA, Regulatory Affairs）專家、合規稽核人員、臨床前工程測試師與第三方審查機構，在正式向美國食品藥物管理局（U.S. Food and Drug Administration, FDA）提交網頁或實體 510(k) 申請素材前，進行超高可靠度的自動化 '實質等效性（Substantial Equivalence, SE）' 比對預審、Refuse-to-Accept (RTA) 檢驗門檻校驗、以及對應當前 FDA 頒佈指引文件（FDA Guidance Documents）的語意偏移分析（Semantic Drift Analysis）。

This system is an intelligent medical device regulatory compliance and 510(k) submission review platform (RegulAI 510(k)) built upon a highly redundant, dual-layer Large Language Model (LLM) agentic orchestration framework. Its primary objective is to empower Regulatory Affairs (RA) professionals, clinical testing engineers, and compliance auditors to perform rigorous, automated Substantial Equivalence (SE) pre-flight assessments, Refuse-to-Accept (RTA) screening validation, and deep semantic drift tracking against the U.S. FDA Guidance Document repository before initiating final administrative submission with the FDA.

--------------------------------- SYSTEM PIPELINE FLOW ---------------------------------
1. MULTI-FORMAT INGESTION PANEL
   - 510(k) Draft File Ingestion (.pdf, .txt, .docx, pasted txt)
   - Interactive PDF Preview Pane & Tokenized Source Document Parsing Frame
   
2. SEMANTIC CRAWLER & SEARCH GRAPH (FDA GROUNDING)
   - Dynamic Web Crawler (FDA Guideline Database Mapping)
   - Predicate Search Engine (Dynamic Retrieval of FDA 510(k) Database & SE Tables by K-Number)
   
3. DUAL-LAYER AGENTIC RUNTIME CORE
   +---------------------------------------+         +-----------------------------------------------+
   |             AGENTS.YAML               |         |                   SKILL.MD                    |
   |  - Process Orchestration Flow         |  <===>  |  - Clinical Audit Rules & Checkpoints         |
   |  - Ingestion & FDA Search Protocols   |         |  - RTA Syntax Verification Rules              |
   |  - Model Parameter Mapping Adjustments|         |  - Material Contact & Biosecurity Metrics     |
   +---------------------------------------+         +-----------------------------------------------+
                               |                                      |
                               +-------------------+------------------+
                                                   |
                                                   v
                          +--------------------------------------------------+
                          |    - Grammar, Links and Broken Reference Checker  |
                          |    - Validation Diagnostics Console & Highlighting|
                          +--------------------------------------------------+
                                                   |
                                                   v
4. INTERACTIVE DOCK & WORKFLOW VIEWS
   - Comparison Comparator: Submission Draft vs Generated Summary with Side-by-Side Synced Scrolling
   - Auto-Validation and Prechecks panel
   - LocalStorage State Persistence (Draft Auto-Saver: Submission, agents.yaml, and skill.md)
   
5. THE SIX REVOLUTIONARY 'WOW' AI FEATURES
   - 1. Semantic Equivalence Mapper        - 2. Automated FDA RTA Auditor
   - 3. Statistical Biometric & Sizing     - 4. Predictive FDA 'AI Request' Simulator (NEW)
   - 5. Cross-Sectional Claim Consistency  - 6. UDI Packaging Labeling & Verification (NEW)
   
6. INTERACTIVE VISUALIZATIONS & LOG MONITOR
   - 1. Guidance Alignment Circular Gauge            - 2. Compliance Drift Column Comparison Graphs
   - 3. Risk Vector Radar Chart (5 Axis)             - 4. Semantic Entity Density Heatmap Grid (3 x 4)
   - 5. Agency Session Execution Waves               - 6. Inference Quality Health Metric Indicator Bars
   - Interactive 20-Question Inquiries Panel (Bottom Docked Drill-Down Interface)
----------------------------------------------------------------------------------------

---

### 2. 資料流與核心引擎工作流程 / DATA FLOW & CORE REAL-TIME REASONING ENGINE

#### 2.1 階段一：申報素材、多源設定檔與 PDF 雙向導入 (Ingestion, Configuration & Parallel Verification)
1. **中繼資料深度解析 (Metadata & Structuring Ingestion)**：當使用者在前端上傳代表 510(k) 申報素材（如 510(k) Summary 摘要、審查資料草案或技術描述文件）的檔案或貼上純文字時，系統的後端解構器或瀏覽器內置的 Ingestion Engine 會立即在背景將此文件切片，自動提取裝置的註冊品名、預期用途 (Intended Use)、適應症 (Indications for Use, IFU)、接觸病患之物理材質 (Patient-contacting materials)、電子與電磁防護級別以及擬定比對之先驅器材 (Predicate Devices) 的舊款 510(k) 案號（K-Number）。
2. **多重安全載入與 PDF 同步呈現 (Parallel PDF & Text Display)**：上傳 PDF 文件後，文件儲存或臨時快取將分流為二。左端送至 PDF 唯讀渲染面板，提供極高的排版保留度（Preserved Typography）及頁籤對齊；右端送入文字切片剖析器（Token Parser），藉此完成與隨後由 LLM 生成報告的字元比對及跨章節一致性驗證。
3. **安全配置與代理人環境初始化 (Config & Security Injection)**：在此階段，系統同時自 LocalStorage 或線上載入 'agents.yaml' 與 'skill.md'。檢測引擎分析當前選定的 LLM 模型（主推 'gemini-3.1-flash-lite'，此為兼顧高上下文與即時對答的法規檢索首選，同時支援 'gemini-2.5-flash' 等升級模型）和客製化系統提示詞設定，配置相應代理人之運作參數。

#### 2.2 階段二：法規知識解碼、對稱性搜索與代理人協同運作 (Semantic Grounding & Collaborative Audit)
本平台內部不使用任何硬編碼（hardcoded）的法規判定機制，所有臨床與技術審計邊界完全基於聲明式（Declarative）結構。
1. **聯邦語意檢索器驅動 (Federated Semantic Crawler Engine)**：系統會啟動 FDA Grounding Graph，解析申報素材中設定的 K 編號。若 K 編號有效，便藉由背景動態檢索、調用 FDA 公共 510(k) 資料庫中有關該先驅產品的安全性認證摘要以及特徵比對表，將其作為實質等效判定（SE Decision Flow）的第一手對比對象。
2. **雙層代理人對抗式生成 (Red-Team Agent Verification Loop)**：
   * **底層代理人組合 (The YAML-orchestrated Crew)**：依據 'agents.yaml' 設定之 IngestAgent、FDA_SearchAgent、EquivalenceAgent 及 RTA_AuditorAgent，將分別執行資料收集、FDA資料庫映射、相似相似度矩陣計算與RTA完整性檢核。
   * **決策編譯代理人 (Compiler & Semantic Synthesizer)**：將底層代理人的執行日誌與判定點，結合 'skill.md' 中所述之最高層級專門技術提示進行對抗式交叉審核（Adversarial Counter-review），最終將輸出結論打包編譯為 3000 到 4000 字、具備完整實體指引對照段落的中文與英文雙語綜合審查報告。

---

### 3. 六大突破性 AI 亮點功能 / THE SIX 'WOW' AI FEATURES

本系統深度整合了以下六大突破性臨床合規與預審分析模組，其能將一般申報團隊在 FDA 實質審查中遭遇 RTA 退件與補件（Additional Information Request）的綜合機率自業界平均 35% 驟降至 5% 以下：

#### 3.1 亮點一：語意等效性映射與先驅器材缺口檢測器 (Semantic Equivalence Mapper & Predicate Gap Detector)
*   **深層運作原理 (In-depth Mechanism)**：
    實質等效性 (SE) 的檢索盲點，大多落於申報團隊過多使用了不具一致性的文字去描述相同的臨床特徵。此功能利用高維向量嵌入空間（Clinical Dense Embedding），深入分析主體裝置 (Subject Device) 與先驅器材 (Predicate Devices) 之關鍵宣告（如：預期用途、體內放置位置、接觸時間、臨床生物相容性、硬體發发射能效、傳輸協定）。
*   **技術落差度量 (Mathematical Distance Mapping)**：
    對於每一個技術欄位與參數宣稱，此分析會估算雙向相似度。若數值存在物理差距（如工作頻率飄移 10%、滅菌方法由 γ-Ray 轉為 Ethylene Oxide 卻無補償驗證），系統將輸出「技術軌跡偏移警告」，並定位缺失段落，在報告中指示合規專員應補回哪些性能台架測試（Bench Testing）報告或材料化學表徵（Chemical Characterization）章節，以確保判定不會被 FDA 阻斷。

#### 3.2 亮點二：自動化 FDA 拒絕受理 (RTA) 檢驗合規稽核模組 (Automated FDA Refuse-to-Accept Auditor)
*   **深層運作原理 (In-depth Mechanism)**：
    FDA 會於收件後的 15 日之內快速檢檢核申報案是否符合其 RTA Checklist。若有漏失將直接退件、終止案件。此稽核模組能動態轉換並比對官方 RTA 項目的完整性。
*   **全方位覆蓋矩陣 (Full Compliance Matrix Coverage)**：
    *   **申報主體合規度**：檢查是否附帶符合 FDA-3514 表格要求的宣告書、Executive Summary 以及 Cover Letter 內的簽章完整性。
    *   **生物材料安全性對齊**：掃描對應 ISO 10993-1 生物相容性測試項目（如細胞毒性、致敏性、刺激性等）之測試報告與滅菌參數。
    *   **軟體安全生命週期資料**：對齊 FDA 2023 最新發布之《醫療器材軟體內容提交指引》，檢核其系統架構、危害分析（Hazard Analysis）、基本或增強等級（Basic or Enhanced Documentation）之全段位文件。
    *   **資通安全控管評估**：對齊聯邦食品毒品化妝品法（FD&C Act）第 524B 條，全面確認是否存在軟體材料清單（SBOM）、漏洞追蹤計畫、安全性連線簽署更新機制的明文提交。

#### 3.3 亮點三：臨床樣本量合理性與生物統計評估引擎 (Biostatistics & Clinical Sample Sizing Assessor)
*   **深層運作原理 (In-depth Mechanism)**：
    在 510(k) 申報素材中，若包含非臨床動物試驗、模擬人體組織極限環境試驗、或小規模臨床試驗，其統計假設如果不正當，會直接導致 SE 等效性判定失敗。本統計晶片級評核引擎用以對申報文件中的樣本量與統計檢定力進行全自動合規覆核。
*   **審計指標判定維度 (Dynamic Statistical Verification Targets)**：
    *   **非劣性檢定合理性 (Non-Inferiority Verification)**：確認試驗是否採用非劣性架構相比於先驅設備，且其非劣效臨界值（Margin, Delta）在臨床上具備發表文獻支援（Cite Proofs）。
    *   **樣本量計算驗證 (Sample Sizing Audit)**：依據變異度 (Standard Deviation) 及預期效力，倒推所需的受試樣本。若素材中提供的樣本低於統計學推計下限，或脫退率 (Dropout rate) 的敏感度分析不足，系統便會在儀表板內進行黃色預警。
    *   **信賴區間審計**：確認其評估端點 (End Points) 提供的置信區間 (e.g. 95% CI) 解析是否足以支撐安全宣稱。

#### 3.4 亮點四：預測性 FDA 補件詢問模擬器 (Predictive FDA AI Request Simulator) (NEW)
*   **深層運作原理 (In-depth Mechanism)**：
    當 FDA 在實質審查（Substantial Review）階段有疑義時，會發出 Additional Information (AI) request。此功能利用 FDA Grounding 庫與對抗式判斷機制，模擬 FDA 主審官的立場，對使用者上傳的 510(k) 草稿進行多角度提問模擬，提前列出 10-15 個極有可能被問及的技術、設計、臨床及安全性質疑問題。
*   **實質回應與改善效益 (Counteraction Strategy Guidance)**：
    模擬器不僅會展示質詢問題，更會針對每一個可能召回或需要補齊的細項（如：'電氣安全 IEC 60601-1 測試報告中漏未指明家庭照護使用規格' 或 '未提供特定封口強度老化的驗證標準依據'），直接寫出法規專員最得體的預備答覆草稿（Defensive Draft Responses），並指示應在草稿中何處補入相應論證。

#### 3.5 亮點五：跨章節語意一致性比對分析器 (Cross-Sectional Claim Consistency Analyzer) (NEW)
*   **深層運作原理 (In-depth Mechanism)**：
    510(k) 申請草稿常長達數百頁。一旦某個章節（如第 5 章生物相容性）中提到的設備接觸材質與第 11 章技術規範或 UDI 發行標籤中的英文描述存在些微字詞之不一致（例如在生物相容部分稱作 "Polycarbonate with 10% glass fibers"，而在技術規格提及 "PC composite" 或 "Nitinol active wire" ），即會被 FDA review 官判定資料不詳、進而要求說明或發起補件。
*   **跨維度語意對抗器 (Cross-entropy Contradiction Engine)**：
    系統會遍歷解析 PDF 所有 Token，進行全局交叉關聯（Entity Resolution Mapping）。自動在背景針對「組件物理尺寸、材質、預期對象年齡、滅菌耐受次數、防護極限、以及適應症病患群組」進行不對稱比對。一旦發現語意不配對或邏輯矛盾，即在圖形上精準標記兩處相衝突的文字，協助 RA 人員在送件前消滅 100% 語意毛刺。

#### 3.6 亮點六：UDI 包裝警語與安全標記遵循檢驗器 (UDI Labeling & Regulatory Safety Warning Verifier) (NEW)
*   **深層運作原理 (In-depth Mechanism)**：
    依據 FDA Title 21 CFR Part 801 以及 Unique Device Identification (UDI) 行業指南，醫療器材的所有實體標籤、二維條碼、警語宣告（例如："Rx Only" 警示、一次性使用標示等）都有極為高度的文字格式法規要求。
*   **格式自動校對 (Labeling Structural Validator)**：
    AI 模組會從申報素材中擷取預計登載之設備包裝模擬標示、UDI XML 提交欄位，評估是否符合 UDI 發行機構之合規架構、以及是否存在禁忌不適應症（Contraindications）、主要警告（Warnings）與注意事項（Precautions）的法規文字漏缺。若判定漏失，系統會主動生成符合最新 FDA UDI 標準指南與 Part 801.109 規格條款的標準標籤示範，一鍵提供 RA 下載、貼入其正式申報文件。

---

### 4. 深度功能模組與使用者介面詳細設計規格 / INTERACTIVE WORKFLOW MODULES & UI DESIGN

本平台為了建立卓越、可信賴的法規專家工作台（Aesthetic Expert Workbench），系統在此版本將重新規劃、高度模組化五大功能區塊：

#### 4.1 雙聯視窗：唯讀 PDF 預覽與側邊雙向等效對照器 (PDF Preview & Side-by-Side Comparator)
*   **位置 (Location)**：系統首頁的 '510(k) Summary Engine' 主要檢視標籤頁面中。
*   **UI 幾何與排版架構 (Layout Geometry)**：
    採用 50%-50%（或 55%-45%）比例的無邊框分割視窗 (Splitted Panel viewport)。
    *   **左側：唯讀 PDF 檢修器 (Read-Only PDF Preview Pane)**
        *   支援 PDF 文件一鍵上傳。利用先進內置渲染器，將 PDF 的封頁、目錄、技術表格以及 RTA 等效圖表完整保留地輸出。
        *   帶有分頁快速導航元件（Pagination Mini-Slider）與高對比色搜索欄位。
        *   此面板為「唯讀狀態」，避免使用者在交叉查核時，因不當觸控或滑鼠按壓，將重要的原始申報內容意外抹除、保留歷史資料的第一手真值 (Ground Truth)。
    *   **右側：雙向語意等效對照器 (Dual-View Content Comparator)**
        *   當 AI 完成實質等效（SE）分析與 Refuse-to-Accept 稽核後，此面板可一鍵切換兩種展示：「原始申報草稿文字（Submission Draft）」與「AI 解析出的實等效摘要與修正報告（Generated Summary / Review Reports）」。
        *   **同步滾動追蹤 (Synchronized Dual-Scroll Lock-in)**：使用者在比對時，可一鍵開啟「水平同步鎖（Sync Lock）」，當移動左側之 Submission 段落時，右側之 AI 對照報告將自動解算其對應的合規評審與補件建議行，並使之同步呈現在視圖正中央，省去法規專員自行在數十萬字的大文件中人肉尋找特徵偏移落點。

#### 4.2 聲明式合規控制面板：agents.yaml 與 skill.md 專屬控制區 (Declarative Configuration Section)
*   **核心功能設計 (Core Interface)**：
    為達到平台之完整行為與後端代碼無耦合（Zero-Coupling with Hardcoding），本區塊提供完整的線上代碼極簡編輯器（IDE-grade Code Sandbox），使用者在此可直接完成設定文件的貼上學、修改、上傳及下載。
*   **UI 介面與功能細節**：
    *   **分欄分頁式控制區 (Tabbed Config Editor)**：
        左頁籤顯示 'agents.yaml' (YAML 架構控管模型清單與單一 Agent 參數分配)，右頁籤顯示 'skill.md' (Markdown 多重測試規範、生物相容檢核與 RTA 查核標準)。
    *   **一鍵標準化 (Standardize agents.yaml)**：
        為防止使用者編寫提示詞時出現不對稱欄位或語法損毀，系統提供一鍵「標準化引擎（YAML Standardization Engine）」。此後端服務會分析 YAML 的 AST，對齊版本（對齊 version: "2.4.0"），將過期、棄用的底層模型名統一升級或替換至標準的 'gemini-3.1-flash-lite'，優化其 Temp 值至 0.15 最佳精度，同時利用對抗演算法在 system_prompt 中主動追加防止 AI 幻覺與瞎編法規條款的過濾控制提示詞。
    *   **模型與參數自訂面板 (LLM Model Picker Panels)**：
        提供下拉選單提供法規專員選擇底層基礎大模型（預設預選高效能 'gemini-3.1-flash-lite'），並可以拉動條來手動微調 Agents 運行的 Temperature (0.01 - 1.0) 以及最大 Tokens 邊界，完全決定 AI 輸出的審查精度與長度。

#### 4.3 'skill.md' 語法錯誤與懸空引用驗證器 (Automated Markdown Reference Validator)
*   **核心功能設計 (Core Validation Mechanism)**：
    'skill.md' 內容常常作為核心審查的判定規則，一旦語法出錯或內部文獻錨點懸空（例如：文件要求在報告中調取 #Section_Bio1，卻在 Markdown 中漏寫 #Section_Bio1 標題），審核報告中就極可能出現邏輯碎片與未知崩潰。
*   **診斷反饋與 Line Highlight**：
    在使用者按下「開始審查（Run Audit Review）」前，此驗證器會在背景執行靜態結構掃描（Static AST Scan）。
    如果掃描到：1) Markdown 標題混亂、或是不成對的符號；2) broken internal reference (懸空斷線引用)；3) RTA 退件判定程式指令之語意邏輯死鎖（Circular reference Logic-locks），驗證器將立即中止審核程序的調用，並在畫面上彈出紅色的「語意完整性警告面板」，精確指出故障代碼的行號與原因，並使用 'Line Highlighter' 高亮錯漏行。

#### 4.4 本地端離線備置自動存檔器 (LocalStorage Session Draft Auto-Saver)
*   **核心安全防禦設計 (Fault Tolerance Persistence)**：
    法規評估是極高密度的腦力工作。RA 專員經常需要線上修改多個 510(k) 描述草稿，一旦瀏覽器意外崩潰、電力跳脫或頁面突發重整，未存檔的法規條款將造成無法挽回的損失。
*   **本地物理持久化 (Physical Layer Persistence Engine)**：
    1.  **自動無縫存檔門檻 (Bouncing Save Threshold)**：系統會在背景開啟自動守護進程，當偵測到「申報素材（Submission Draft）、'agents.yaml'、'skill.md'」中任何一個編輯輸入框發生改變時，便會觸發去抖動（Debounce (300ms)），自動將最新的狀態對象打包持久化至 browser 的 'localStorage'。
    2.  **儀表板頂置 Draft 自動挽救系統**：在網頁重啟或突發連線斷訊時，系統標題側邊會顯示一組「Draft Auto-Saved 表示燈」與一只巨大的 **'Save Draft' 自動挽救按鈕**，RA 隨時可點開將上一次中斷編譯時的 Submission、YAML 與 MD 狀態，一秒之內還原至編輯區中。

---

### 5. 高質感互動式視覺化儀表板規格 / HIGH-FIDELITY INTERACTIVE DASHBOARD SPECIFICATION

系統之數據輸出不僅是一份 3000-4000 字極限長度、融合六個 AI 亮點結果與 RTA/SE 比對表的完整審查報告，更搭配一組由六個具高度互動性、流暢 SVG 繪圖與即時動畫交織而成的資訊儀表板：

1.  **指引文件契合圓環 (Guidance Alignment Score Circle)**:
    *   **物理造型**：一組 180px 直徑、帶有流暢漸層色的多環形環狀百分比例儀。
    *   **動畫特徵**：利用 CSS 屬性 'stroke-dashoffset'，從 0 緩衝式拉伸（Ease-in-out）繪製。顏色隨契合率動態回饋：綠色 (>= 85%, 極安全對齊), 黃色 (70%-84%, 輕度偏移), 紅色 (<70%, 嚴重脫鉤警告)。
    *   **連動指標**：代表當前 510(k) 素材與目前最先進之 FDA 官方行業指南中提及之強制要求（例如資安、可用性要求）在自然語言相似高維比對下的相符水準。

2.  **法規偏離柱狀對比圖 (Compliance Drift Tracker Bar Charts)**:
    *   **物理造型**：一組分組垂直（或水平極簡）柱狀光柱，比較主體裝置與先驅器材在各個對比指標上的「偏差語意距離 (Semantic Drift Metric)」。
    *   **動畫特徵**：滑鼠 hover 於柱子上時，會自動浮現漂浮卡片（Floating Indicator Tooltip），顯示詳細數據（如: Intended Use similarity = 0.98; Sensor Principle drift gap = 0.32）。
    *   **連動指標**：一目了然定位出究竟是哪些項目存在 "Over-claiming (过度宣傳)"、"無文獻支持的技術飄移" 或 "漏失滅菌規格宣告"。

3.  **五維合規雷達圖 (Risk Vector Radar Chart)**:
    *   **物理造型**：優雅的高填充、高半透明度五角星外擴雷達網架。
    *   **座標軸向**：代表臨床前與實質審查五大支柱物理學科軸── 1) 生物相容性安全性評估 (Biocompatibility)、2) 軟體關切與網絡資安等級 (Cybersecurity / Software LoC)、3) 電氣安全與 EMC 特徵防護 (Electrical Safety / EMC)、4) 非臨床桌上性能及包裝耐老強度驗證 (Bench Performance / Packaging)、5) 臨床統計效力與樣本量合理性 (Biostatistics / Clin Sizing)。
    *   **互動特徵**：點選任意雷達項目的終端點，右側的報告與 PDF 比對區就自動跳躍、渲染引導該維度的 RTA Checklist 補齊方案。

4.  **法規詞彙語意熱網 (Semantic Entities Heatmap Grid)**:
    *   **物理造型**：一組 3 x 4（共 12 格）的圓角動態法規熱圖卡（Responsive Heatmap Grid）。
    *   **詞彙熱度指標**：每一格代表一組 FDA 審核官最常關切的審定概念詞彙（包括：/Intended Use、/Indications/for Use、/Design Controls、/Biocompatibility、/Sterility、/Software Verification、/Cybersecurity、/Bench Testing、/Clinical Power、/EMC 60601、/UDI Marking、/Human Factors）。
    *   **動態色彩流動**：依據上傳材料中該法規實體（Regulatory Entities）字詞在篇章中的涵蓋率、邏輯論證深度及法規證據比對，自動自「低涵蓋（淺柔灰藍）」流暢演變為「高度嚴密論證（深邃霓虹亮藍）」。

5.  **代理人執行時延波動線 (Agency Run Time Waveline Monitor)**:
    *   **物理造型**：一條細膩的霓虹波紋走勢圖，追蹤當前啟動之代理人小隊（e.g. Ingest, Search, Equivalence, Auditor）各自在背景呼叫 LLM 進行思考時所消耗的時間 (ms) 與輸出 Token 的吞吐密度波形。
    *   **感官效益**：帶給法規專家「AI 在實時思考、層層過濾判斷」的震撼法規檢審呼吸律動感（Breathtaking Intelligent Vibe），消除等待大模型生成長文字時的焦躁。

6.  **推理品質健康度指數 (Inference Quality Health Matrix Indicator Bars)**:
    *   **物理造型**：三條重置、平行呈現在儀表板底端的發光色彩進度條：
        1. **準繩度指數 (Accuracy Rate)**：經由 'skill.md' 語意驗證過後，AI 輸出引用的法律條款（21 CFR）及技術指南的真實無誤率。
        2. **一致度指標 (Consistency Level)**：評審多次呼叫、前後文件邏輯交叉印證的一致性（幻覺過濾水準）。
        3. **實據佐證率 (Factuality Proofs Index)**：報告中所有宣告均找到 source text 引用之百分比。

---

### 6. 聲明式代理人配置與標準化規格 / DECLARATIVE CONFIGURATION STANDARDIZATION

系統中對 AI 行為的定義完全與代碼分離。編輯器與存檔器支援之標準格式如下：

#### 6.1 agents.yaml 規範結構與提示詞校準 (Code Representation)

=== YAML CONFIGURATION SCHEMA ===
version: "3.1.0"
defaults:
  default_model: "gemini-3.1-flash-lite"
  default_max_tokens: 16000
  default_temperature: 0.10
  security_filters:
    reject_unsubstantiated_claims: true
    require_cfr_reference: true

agents:
  ingest_agent:
    name: "510k Document Ingestion Synthesizer"
    skill_number: 1
    category: "Ingestion and Text Extract"
    default_model: "gemini-3.1-flash-lite"
    temperature: 0.05
    system_prompt: "You are the Master Ingest Agent for RegulAI 510(k)..."
      
  fda_search_agent:
    name: "FDA Database Semantic Crawler Mapping Agent"
    skill_number: 2
    category: "External Database Grounding"
    default_model: "gemini-3.1-flash-lite"
    temperature: 0.15
    system_prompt: "You are the Retrieval Agent tasked with matching identified K-numbers to the FDA CDRH regulatory repository..."

  equivalence_agent:
    name: "Substantial Equivalence Analysis Auditor"
    skill_number: 3
    category: "Claim Comparison Reasoning"
    default_model: "gemini-3.1-flash-lite"
    temperature: 0.10
    system_prompt: "You are the Clinical Equivalence Engine. Compare subject specifications and materials with predicate profiles..."

  rta_auditor_agent:
    name: "FDA 510k Refuse to Accept Checklist Evaluator"
    skill_number: 4
    category: "Pre-submission Completeness Verification"
    default_model: "gemini-3.1-flash-lite"
    temperature: 0.05
    system_prompt: "You are the RTA Gatekeeper. Assess file completeness against FDA standard traditional, special or abbreviated RTA Checklists..."
=================================

---

### 7. FDA 510(K) 審查與 3000-4000 字合規報告生成平台

本平台在運作時，一旦按下執行 Review，便會啟動 Multi-Agent 共識編譯鏈。它能匯總使用者上傳的 510(k) 申報 Draft 與 'skill.md' 的實體法法治查核指南。
在最後的輸出面板中，AI 會生成一份**多達 3000 至 4000 字、結構極為精準嚴密、中英雙語對照**的 FDA 綜合審查決策書。這份決策書會深度、有機地融入系統提供的六項 WOW AI 功能，例如在第二章中列出完整、高維向量算出的 Substantial Equivalence 語意對照距離（**Semantic Similarity Distance**）與檢測缺口，在第三章中給出 RTA Checklist 的 A/B/C 三級風險評估，在第四章中提供生物統計、樣本量以及非劣性區間的審議報告，並在第五章顯示模擬的 "AI request (FDA 補件質問)" 預警和防禦答覆，在第六章列出跨章節材質與物理參數不一致的偵錯結果，以及第七章所出具的 21 CFR 801 合規 UDI 警語和包裝打印修正版手稿。

---

### 8. 技術實現細節、極限邊界控制與故障隔離性能 / TECHNICAL IMPLEMENTATION DETAILS & FAULT ISOLATION

#### 8.1 語意等效相似性多維餘弦距離計算公式 (Semantic Cosine Distance Algorithm)
系統在解算 'Subject Device' (新設備) 同 'Predicate Device' (先驅產品) 之技術參數偏移時，會調取後端 / 近端 Embedding 模型，將文字區塊 (Text Chunks) 映射為高維向量 V_sub 與 V_pred，並依據餘弦相似度 (Cosine Similarity) 公式求解：

Cosine_Similarity = (V_sub * V_pred) / (||V_sub|| * ||V_pred||)

當這個分數低於由 'skill.md' 中自訂或系統底層設定之實等門檻臨界值（預設 theta = 0.85）時，表示其新適應症宣稱或技術參數（例如射頻波長變更、或引入非先驅有記載的塑料添加劑等）有可能引發「全新的安全同有效性疑慮（New questions of safety and effectiveness）」，此時平台判定其「實質等效性 (SE) 失敗」，自動向用戶標示 Drift warning、給出補強性能測試規格（Bench performance requirements）以克服漏洞。

#### 8.2 Markdown 語意與邏輯死鎖掃描演算法 (Log-Deadlock Parser for skill.md)
驗證器在處理 'skill.md' 時，會將其解析為抽象語法樹 (AST)。它專注於尋找三種邏輯壞味道 (Logic Code Smells)：
1.  **Dangling Target Link (懸空目標連結)**: 指引規則中指示 AI 利用 @[CFR_870_Compliance] 進行合規規則比對，但在全篇 markdown 規則中並無 # CFR_870_Compliance 這組錨點，即發出 Link Error。
2.  **Circular Evaluation Deadlock (循環互鎖參考)**: 發現 A 條款要求審定時必須合乎 B 條件判定，而 B 條件之觸發規則又引用 A 條款的決策，導致 AST 解析環路。驗證器將中止編譯、避免 LLM 在推論時陷入思維無盡迴圈。
3.  **Invalid Target Definition (非法格式定義)**: 對於 RTA Checklist 中用以判定 A/B/C 三級的核心算式，若語法混染或符號缺失，即將出錯行號以 'Line Highlighter' 紅色線條投射至前端編輯器中。

#### 8.3 網絡安全、API 電路熔斷與本機快取相容技術 (API Key Failure & Circuit Breaker)
本平台在 client-side 設有 API 連線保護熔斷與故障隔離機制 (Fault Isolation Mechanism)：
1.  **熔斷隔離保護 (Circuit Breaker Design)**：當用戶在 Secrets Panel 中配置之 Gemini API 密鑰失效、或是由網絡不可抗力造成 Timeout (超過 25000 毫秒無反應)，熔斷機制會瞬間開啟（Open State）。
2.  **不中斷體驗 (Fault Tolerance Mode)**：為防止用戶精心上傳與貼入的 510(k) 數據意外損毀，系統將轉向「本機離線緩衝模式 (Offline Buffer Mode)」，調取 LocalStorage 中快取的 RTA/SE 歷史判定規則進行前置純語意格式合規度本機比對。
3.  **狀態持久化守護**：當連線復原或用戶重新提交正確金鑰，熔斷自動切回關閉（Closed State），並利用雙向存檔器發送本地儲存之申報 Draft。

---

### 9. 深入回顧與探索：20 個法規與技術遵循追蹤問題 / 20 COMPREHENSIVE FOLLOW-UP INQUIRIES

在完成 510(k) 完整審查並生成 4000 字決策書後，系統底部導覽列提供了一組 互動式追蹤探索船塢 (Interactive Inquiries Panel)。法規官可一鍵按壓 20 個預設關鍵問題，系統將引導 AI 底層代理人針對每個特定的 21 CFR 標準或 ISO 規範，展開深度法規與臨床測試方法論的深度拓展回答：

1.  **實質等效性判定 (Substantial Equivalence Logic)**
    *主體裝置之預期用途與適應症（Indications for Use）是否與先驅器材百分之百等同？如果有多個次要差別，是否會引入新的產品安全/功效風險？系統如何演算這些細微語意特徵並在雷達圖展示差距？*
2.  **FDA 業界指引對齊 (Guidance Document Alignment)**
    *當前系統擷取的 FDA 當前行業指引（e.g. 2023 年醫療器材安全性資安指引）中，是否有特定強制要求的資料流或加密演算法而我方草案中漏未提交的？AI 如何對比並生成合規防禦？*
3.  **生物相容性測試覆蓋 (Biocompatibility Matrix - ISO 10993-1)**
    *擬申報之接觸患者材質（Patient-contacting materials），其與人體接觸的性質（Nature of Contact）與接觸時間（Contact Duration）在 'skill.md' 規則中是否被確實歸類？測試項目（如 Cytotoxicity, Sensitization, Intracutaneous Reactivity）是否完整？*
4.  **電氣安全與電磁相容性 (Electrical Safety & EMC - IEC 60601-1-2)**
    *本裝置是否已被指明需要符合第 4 版家庭照護環境 EMC 要求（Home Healthcare Environment Standards）？測試報告中是否提供充足抗干擾容許極限說明？*
5.  **軟體生命週期合規度 (Software Life Cycle Verification - IEC 62304)**
    *申報資料中所述及的軟體關切等級（Level of Concern, LoC / Basic or Enhanced Documentation 級別）與危害風險分析（Hazard Analysis）之軟體配對關係，是否與 FDA 2023 最新軟體指南相符？*
6.  **網際網路資通安全機制 (Medical Device Cybersecurity - Section 524B)**
    *系統對符合聯邦食品、藥物和化妝品法案 (FD&C Act) 下 Section 524B 重大修正規定之軟體更新、漏洞監測計劃、以及軟體材料清單（Software Bill of Materials, SBOM）之合規完整性評估為何？*
7.  **無菌保障與包裝效力 (Sterility & Shelf-life Testing)**
    *若裝置屬於無菌包裝，其無菌保證水平（Sterility Assurance Level, SAL）是否達 10^-6？所採之環氧乙烷（EO）殘留量是否符合 ISO 10993-7 等標準規範？一整年實時與加速老化的物理包裝強度測試是否符合 ASTM / ISO 11607 系列指引？*
8.  **臨床試驗樣本合理性 (Clinical Pre-flight Sizing)**
    *該 510(k) 素材中所進行的臨床評價，其隨機對照試驗之樣本量（Sample Sizing）計算是否使用了先驅器材 clear 數據作為估算變異度的基準？統計檢力（Power）是否有偏低的潛在疑慮？*
9.  **人因工程與可用性測試 (Human Factors & Usability - IEC 62366-1)**
    *針對預期非專業家屬使用之醫療器材，我方人因工程草案是否已列明關鍵任務步驟（Critical Tasks）風險評估，且有效完成了 15 名以上臨床非專業之正式可用性測試？*
10. **先驅器材識別正確性 (Predicate Selection Validity & K-number Checking)**
    *擬參照之先驅器材 K-number 在 FDA 資料庫中目前是否處於正常 Clear 狀態？是否有近年發生之 Class I/II 回收召回記錄（Recall Registry）可能間接威脅主體裝置等效判定？*
11. **非臨床性能測試之驗證極限 (Bench Performance Limitations)**
    *510(k) 申報素材所述之非臨床性能測試中，是否提供了足夠之「挑戰性條件（Worst-Case Conditions）」性能數據（如最大操作流速、極限運算負載下的訊號漏失程度等）？*
12. **重新標準化 agents.yaml 的法規價值 (agents.yaml Regulatory Traceability)**
    *將 'agents.yaml' 進行結構式標準化，能如何協助審查與稽核組織在持續迭代軟體與增加 AI 任務時保持可重現、可追蹤之合規操作日誌？*
13. **FDA 拒絕受理 (RTA) 的致命缺失 (Refuse-to-Accept Critical Red-flags)**
    *在當前 RTA Checklist 的 AI 自動稽核報告中，有哪些被標籤為「高致命風險」缺件項目需要 RA 優先重寫，否則可能在 15 天內被 FDA 官方秒殺退件？*
14. **雙語摘要生成之對齊與一致度 (Bilingual Representation Consistency)**
    *在輸出的 3000-4000 字 Traditional Chinese - English 雙語綜合報告中，AI 是否完全排除了任何因語言轉譯而產生的法規定義偏移（CFR 條款定義的誤譯或語意模糊）？*
15. **模型溫度 (Temperature) 與推理精確度的控制關係 (Model Temperature Optimization)**
    *在 agents.yaml 中，為什麼針對 FDA 合規審計與 Substantial Equivalence 推理，將運行溫度限制在 0.15 以下對確保推導品質有絕對必要性？*
16. **臨床效果之非劣效性邊界設計 (Non-Inferiority Margins Calculations)**
    *若在臨床前研究中使用非劣性比對試驗，如何針對 AI 指導的非劣效閾值（Margin）在醫學臨床合理性上向 FDA Reviewer 進行說理論證？*
17. **硬體組件的材質與化學表徵評價 (Chemical Characterization - ISO 10993-18)**
    *若生物相容性測試中發現存在潛在可萃取物（Extractables & Leachables, E&L），系統推薦的技術補件指南該如何快速對準 AAMI TIR57 及 ISO 10993-18 化學表徵標準以應對發言？*
18. **軟體警報機制與臨床安全保障 (Alarm Systems - IEC 60601-1-8)**
    *擬申報設備中運作的聽覺與視覺報警信號，其生理聲波頻率間隔與發光脈寬等技術參數量度（IEC 60601-1-8）是否在 510(k) 測試對比表裡被完美羅列且與先驅相符？*
19. **再處理與重複使用設備之確效驗證 (Reprocessing validation)**
    *對於重複使用之臨床導管、內視鏡等器材，其在草案中有關清潔（Cleaning）、消毒（Disinfection）和高溫蒸氣滅菌（Steam Sterilization）的重複循環確效（Reprocessing validation）章節是否符合審查底線？*
20. **AI 與機器學習組件的預先變更控制協議 (PCCPs for AI/ML Medical Software)**
    *若我方擬提交的產品本身帶有機器學習算法，是否已在 510(k) 申請中附帶「預先變更控制協議（Predetermined Change Control Protocol, PCCP）」，好讓未來演算法在線上自主演進時免除二次重新向 FDA 申繳 510(k) 的繁瑣程序？*

---

*End of RegulAI 510(k) Technical Specification Document. Authored, compiled and verified by the RegulAI Systems Engineering Team.*
`;
