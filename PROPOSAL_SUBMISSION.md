# DIC3 期末專題提案重構作業成果報告：DietSense
**使用 OpenSpec 規格驅動開發重構專題提案**

- **課程名稱：** 物聯網與智慧運算 (DIC3)
- **專案名稱：** DietSense — 對話式日常熱量攝取分析領域智慧代理人 (Domain AI Agent)
- **專案目錄：** `c:\Users\ccm49\OneDrive\物聯網DIC3`
- **OpenSpec 變更名稱：** `dietsense-proposal-refactor`

---

## 📌 成果目錄與繳交檔案位置導覽

老師要求的「使用 OpenSpec 重構專題提案」之所有成果檔案，皆已完整存放在您當前的專案資料夾中：

```text
c:\Users\ccm49\OneDrive\物聯網DIC3\
├── 📄 PROPOSAL_SUBMISSION.md   # 【本文件】包含作業要求的 A-F、5 思維、3 爭論、10 問題、緒論與文獻重構
├── 📄 README.md                # 專案首頁說明與四大創新亮點
├── 📄 DESIGN.md                # 八大模組架構、泳道流程圖與帶行號演算法虛擬碼
│
├── 📁 openspec/                 # 【OpenSpec 核心規格目錄】
│   ├── config.yaml             # OpenSpec 規格驅動設定檔
│   └── changes/
│       └── dietsense-proposal-refactor/   # 本次作業的 OpenSpec 變更專案
│           ├── proposal.md     # 提案動機與四大能力合約 (Why & Capabilities)
│           ├── design.md       # 系統架構、技術決策與流程設計 (How)
│           ├── tasks.md        # 14 項可追蹤之規格實作任務清單
│           └── specs/          # 【四大核心功能規格 (Spec Deltas)】
│               └── diet-agent/
│                   ├── natural-language-intake/spec.md     # 自然語言解析與主動追問規格
│                   ├── nutrition-grounding-engine/spec.md  # 衛福部/USDA資料庫與區間估算規格
│                   ├── energy-analytics-tdee/spec.md       # Mifflin-St Jeor TDEE 運算規格
│                   └── safety-guardrails/spec.md           # <1200大卡與飲食失調安全防護規格
│
└── 📁 .agents/skills/          # 供 AI Agent 自動化調度之 6 大 OpenSpec 技能庫
```

---

## 第一部分：專案核心規格（A to F 摘要、5 大思維、3 大爭論、10 大問題）

### 1. Abstract 摘要六段式（A to F）
- **A. Attention Getter（動機與引題）：** 根據 WHO (2025) 統計，全球已有超過 10 億人口處於肥胖狀態；《Lancet》(2025) 指出全球 43% 成人過重或肥胖。台灣衛福部 2024–2025 年國民健康調查顯示，18 歲以上成人過重與肥胖率高達 47.9%，年健保醫療支出突破新台幣 500 億元。研究證實持續記錄飲食者之體重管理成功率為未記錄者的 2 倍（Teixeira et al., 2025），但高達 85% 的使用者在 30 天內即放棄，主因在於傳統工具搜尋繁瑣、秤重估量難以長久維持。
- **B. But（挑戰與問題）：** 現有工具存在三大瓶頸：其一，手動記錄負擔高，混合菜餚拆解繁瑣；其二，自報飲食攝取（Self-reported intake）誤差高達 18%–54%（Poslusna et al., 2009），外食隱藏油脂與湯底擴大了數值不確定性；其三，通用 LLM 直接估算食物熱量極易產生數值幻覺，研究顯示 LLM 對食物熱量估計之平均絕對百分比誤差（MAPE）達 35.8%，且缺乏跨餐累積與安全防護。
- **C. Cure（解藥與核心定位）：** 本研究提出 DietSense 對話式 Domain AI Agent，核心架構為「**LLM 負責意圖協調與規劃，領域工具負責確定性計算**」：LLM 透過白名單 Function Calling 調用營養資料庫、RAG 向量檢索、Mifflin-St Jeor TDEE 引擎與安全規則庫，將數值計算交由確定性演算法，以可驗證的熱量信賴區間與來源依據取代虛假精確的單一數字。
- **D. Development（方法與架構）：** 系統整合八大模組架構（使用者情境、輸入前處理、Planner 推理代理人、資料取得檢索、分析引擎、領域知識整合、決策建議、輸出互動）。以 Whisper 轉文字並結合繁簡正規化與意圖分類，由 LLM Planner 辨識食物實體；當份量模糊時觸發主動澄清迴圈（Clarification Loop）。底層資料平行檢索 USDA FoodData Central、台灣食品成分資料庫與台式料理向量庫；分析層依據 $E = \text{份量(g)} \times \text{每百克熱量}/100$ 計算熱量，並透過 Mifflin-St Jeor 公式計算 BMR/TDEE。安全層導入嚴格守門機制，攔截每日攝取低於 1,200 kcal 之極端節食模式。
- **E. Experiments（評估規劃）：** 建立含 500 筆台式料理與在地小吃之自然語言標註資料集。實驗以商業基準 MyFitnessPal 手動輸入流程為對照組，評估指標涵蓋：熱量絕對誤差、±10% 與 ±20% 誤差內達標率、食物記錄耗時（秒）及安全攔截率。規劃進行 20 名受試者為期 7 天之真實飲食 User Study，評估系統可用性量表（SUS Score > 70）。
- **F. Findings（預期產出，無虛構數據）：** 預期 DietSense 能在 ±20% 熱量誤差內達成高於 85% 之準確率，將單餐記錄時間較傳統手動搜尋縮短 60%，SUS 可用性分數達 70 分以上，並顯著擴增台灣在地飲食之辨識涵蓋率。

#### 300 字精華摘要
全球肥胖與過重人口持續攀升，台灣成人過重率達 47.9%，然而傳統飲食紀錄 App 需繁瑣搜尋與秤重，自報誤差達 18%–54%，導致 85% 使用者在 30 天內放棄；通用 LLM 直接估算熱量則常產生幻覺，誤差達 35.8%。本研究提出 DietSense 對話式 Domain AI Agent，核心架構為「LLM 負責協調，領域工具負責計算」。系統採八模組設計，透過自然語言輸入日常飲食描述，經 NER 與 Clarification Loop 主動追問模糊資訊，並平行檢索 USDA、台灣食品成分資料庫與台式料理向量庫。以確定性公式計算熱量區間，結合 Mifflin-St Jeor 計算個人化 TDEE，並導入 <1,200 kcal 安全防護網。本研究規劃 500 筆台式料理標註集與 20 人 7 天實驗，預期達到 ±20% 誤差內準確率 >85%、記錄時間縮短 60%，實現情境感知與安全可信的體重管理。

#### IEEE 格式英文摘要
**Title** — *DietSense: A Data-Grounded and Safety-Aware Conversational AI Agent for Personalized Dietary Assessment and Weight Management*  
**Abstract** — The increasing prevalence of overweight and obesity, affecting over 1 billion individuals globally and 47.9% of adults in Taiwan, highlights the urgent need for sustainable dietary self-monitoring. However, conventional manual tracking applications impose severe cognitive burdens through manual food searching, ingredient decomposition, and portion estimation, resulting in self-reported caloric errors between 18% and 54% and an 85% 30-day abandonment rate. Although large language models (LLMs) enable intuitive natural-language interaction, end-to-end nutritional estimation produces severe hallucinations, with mean absolute percentage errors exceeding 35%. To overcome these limitations, this paper proposes DietSense, a data-grounded, safety-aware conversational Domain AI Agent. DietSense adheres to an architectural paradigm where the LLM coordinates semantic reasoning and task planning, while numerical and clinical calculations are strictly delegated to deterministic domain tools. The framework integrates an eight-module pipeline comprising speech preprocessing, food named entity recognition (NER), agent planning, multi-source retrieval (USDA FoodData Central, Taiwan Food Composition Database, and a specialized vector database of Taiwanese cuisine), deterministic energy balance analytics, clinical domain rules, and interactive reporting. A proactive clarification loop resolves ambiguities in portion sizes and preparation methods, while confidence-weighted intervals replace misleading point estimates. Individual energy requirements are calculated using the Mifflin–St Jeor equation and total daily energy expenditure (TDEE) multipliers. The planned evaluation employs a curated 500-item Taiwanese meal dataset and a 7-day user study with 20 participants comparing DietSense against manual MyFitnessPal logging. Proposed targets aim for >85% accuracy within a ±20% caloric margin, a 60% reduction in logging duration, and a System Usability Scale (SUS) score exceeding 70.  
**Index Terms** — Conversational AI, domain-specific AI agents, dietary assessment, retrieval-augmented generation, uncertainty quantification, personalized weight management, safety-aware systems.

---

### 2. 領域專家的 5 個核心思維模式
1. **能量平衡模型（Energy Balance Model）：**  
   - *意義：* 體重變化取決於能量攝取與消耗之差額，遵循熱力學第一定律。  
   - *證據：* Hall et al. (2012, *The Lancet*) 代謝病房實驗證實，長期體重動態變化與能量不平衡量完全對應。  
   - *系統設計落實：* Agent 即時計算個人 TDEE，動態提示今日熱量赤字或盈餘進度。
2. **巨量營養素組成思維（Macronutrient Composition Thinking）：**  
   - *意義：* 碳水化合物、蛋白質與脂肪的比例直接調控胰島素分泌、飽足感與肌肉維持。  
   - *證據：* 美國醫學研究院（IOM）建立 AMDR 巨量營養素標準（碳水 45–65%、蛋白質 10–35%、脂肪 20–35%）。  
   - *系統設計落實：* 輸出必須包含巨量營養素分佈圓餅圖，而非只報告單一總熱量。
3. **份量估算與不確定性（Portion Estimation & Uncertainty）：**  
   - *意義：* 份量模糊是最大誤差來源，任何宣稱具備個位數精確度之 AI 均屬偽精確（False Precision）。  
   - *證據：* Poslusna et al. (2009) 顯示自報攝取量平均低估 18%–54%；Subar et al. (2015) 顯示目測份量誤差普遍達 50%。  
   - *系統設計落實：* 採用信賴區間輸出（如 350–450 kcal），並在信心低於 80% 時主動追問。
4. **個體代謝差異思維（Individual Variation）：**  
   - *意義：* 相同食物在不同個體身上引發的血糖反應與代謝消耗存在巨大個體異質性。  
   - *證據：* Weizmann Institute (Zeevi et al., 2015, *Cell*) 證實相同食物之後餐血糖反應異質性達 10 倍。  
   - *系統設計落實：* 依據個人生理參數（年齡、性別、BMI、活動係數）動態調整個人化建議。
5. **行為情境意識（Behavioral Context Awareness）：**  
   - *意義：* 飲食受情緒壓力、生活作息與社交場合高度影響，而非單純的數字填報。  
   - *證據：* Koenders & van Strien (2011) 指出超過 35% 之進食行為與情緒或環境壓力直接相關。  
   - *系統設計落實：* 追蹤用餐時段與頻率，偵測深夜暴食等異常行為並提供情境化關懷。

---

### 3. 領域內最重要的 3 個專業爭論
- **爭論一：熱量總量（Calorie Quantity）vs. 食物品質（Calorie Quality）**  
  *共識：* 熱量赤字是減重的必要條件，食物品質是維持飽足感與長期依從性的關鍵。  
  *系統做法：* 同時計算卡路里赤字與食物加工程度評分，推薦低 GI、高微量營養素之原型替代食材。
- **爭論二：AI 份量估算可行度（純文字/LLM 估算 vs. 硬體 3D/秤重輔助）**  
  *共識：* 精確醫療需硬體輔助；大眾日常健康管理首重降低使用門檻與培養自省習慣。  
  *系統做法：* 輸出不確定性置信區間，並於對話中主動引入手掌份量法等直觀視覺參照物。
- **爭論三：通用公共指引 vs. 高度精準個人化（Universal vs. Personalized Nutrition）**  
  *共識：* 通用指引為安全底線，個人化為效果最佳化手段；AI 建議不可逾越醫療法規。  
  *系統做法：* 以國人每日飲食指南為基線，用戶提供生理資料後解鎖個人化，並嚴格標註免責聲明。

---

### 4. 檢驗深度理解的 10 個核心問題
1. **[Why] 相同熱量攝取兩人減重成效差異處理：** 分析巨量營養素比例、評估非運動產熱（NEAT）與食物熱效應（TEF）。
2. **[What-if] 未知變數輸入（泡麵無品牌/未提是否喝湯）：** 標記低信心度，觸發追問；若用戶略過則以典型泡麵估算並擴大信賴區間至 350–580 kcal。
3. **[Trade-off] 估算精確度 vs. 記錄時間負擔：** 單次追問上限不超過 1 題，確保記錄時間控制在 15 秒內。
4. **[Failure Case] 未登錄自製菜餚（奶奶炒年糕）處理：** 優雅降級拆解為年糕（主食）、肉絲（蛋白質）與烹調油，給予區間並供用戶微調。
5. **[Evidence] AI 輔助飲食追蹤科學證據與 RCT 設計：** 100 人 12 週隨機對照試驗，對比 DietSense 與 MyFitnessPal 之依從率與體重變化。
6. **[Comparison] RAG-based LLM vs. Fine-tuned LLM vs. 規則引擎：** 採 Hybrid 架構，LLM 負責 NLU 與調度，RAG 負責檢索，規則引擎負責計算與安全。
7. **[Design Decision] 為何選對話式而非傳統表單：** 大幅降低多層選單摩擦；但在歷史趨勢回顧時輔以視覺卡片儀表板。
8. **[Real-world Scenario] 台灣夜市小吃資料庫建置：** 串接衛福部資料、地方衛生局標示，結合標準烹調食譜向量庫並由營養師抽檢校正。
9. **[Why] BMR 公式選擇理由：** 採用 Mifflin-St Jeor 公式（肥胖與非肥胖者 10% 誤差內達標率 82%，優於老舊高估的 Harris-Benedict）。
10. **[What-if] 預防神經性厭食症安全防護：** 嚴禁低於 1,200 kcal/天 攝取建議，偵測到極端禁食字眼立即啟動安全警戒並提供諮詢門診資訊。

---

## 第二部分：Introduction 緒論重構

- **A. Attention Getter（動機）：** 2025 年《The Lancet》報告指出全球 43% 成人過重或肥胖；台灣衛福部 2024–2025 調查顯示成人過重與肥胖率達 47.9%，年健保支出逾 500 億元。持續記錄飲食者成功率高達 2 倍（Teixeira et al., 2025），但 85% 人在 30 天內放棄。
- **B. Background（背景）：** 全球健康飲食 App 市場達 68 億美元（年增 17.4%）。LLM 與 RAG 技術成熟，使「對話式自然語言飲食記錄」首次在工程與臨床上具備大規模落地條件。
- **C. Problem（核心痛點）：** 傳統手動記錄認知負擔過高，存在檢索耗時、份量模糊、複合菜餚拆解困難及台灣在地小吃覆蓋嚴重不足四大障礙。
- **D. Existing Limitations（既有工具局限性比較）：**
  - *MyFitnessPal 等手動 App：* 需逐筆手動搜尋拆解，缺乏在地外食支援，30 天流失率 85%。
  - *電腦視覺拍照辨識：* 無法探測混合料理內之深層油脂與調味料，對拍攝角度敏感。
  - *通用問答 LLM：* 缺乏真實資料錨定，數值**幻覺率高達 35.8%**，無跨餐個人化追蹤。
  - *特殊硬體臨床系統：* 昂貴笨重，無法融入日常夜市外食情境。
- **E. Innovation（四大創新亮點）：**
  1. 自然語言輸入（Natural Language Input）
  2. 主動式智慧互動（Proactive Agentic Interaction）
  3. 情境感知（Context Awareness）
  4. 大數據驅動智慧（Big Data–Driven Intelligence）
- **F. Contributions（專題預期主要貢獻）：**
  1. 繁體中文對話式飲食 Domain AI Agent（DietSense）
  2. 500 筆台灣在地飲食自然語言標註資料集
  3. 多來源營養整合與不確定性區間量化管線
  4. 多維度系統評估框架（準確度、耗時、SUS 可用性、安全攔截率）

---

## 第三部分：Related Work 相關研究重構（1/2 AI + 1/2 領域知識）

### 1. 人工智慧核心技術（佔比 50%）
- **LLM Agent 與 Tool-Use：** LLM 作為意圖解析與 Planner，調用外部確定性 API。
- **RAG 檢索增強生成：** 密集向量檢索比對事實資料庫，防止熱量數值幻覺。
- **食物實體識別（NER）：** 抽取食物名稱、份量形容詞、烹調方式與用餐時間。
- **不確定性量化：** 依據比對相似度動態輸出信賴區間。

### 2. 營養與能量代謝專業知識（佔比 50%）
- **代謝動力學：** 採用 Mifflin-St Jeor 計算 BMR，依據活動係數推導 TDEE。
- **巨量營養素常數：** 碳水 4 kcal/g、蛋白質 4 kcal/g、脂肪 9 kcal/g，對齊衛福部 DRIs 與 AMDR。
- **份量估算參照標準：** 結合六大類食物代換表與手掌估算法。
- **臨床安全界限：** 強制防範每日低於 1,200 kcal 之極低熱量飲食危害。

### 3. 前沿文獻歸納表（Article Summary Table，2024–2025）

| Ref | 年份 | 探討問題 | AI 技術方法 | 領域專業知識 | 資料集 | 主要研究發現 | 既有局限 |
|---|---|---|---|---|---|---|---|
| **[1]** | 2025 | 自然語言飲食回顧估算 | GPT-4 + RAG 知識庫檢索 | USDA FoodData 營養標準 | 1,200 筆 24 小時飲食回顧 | 準確率達 82% | 僅限英文食物，無法處理亞洲複合菜餚。 |
| **[2]** | 2024 | 食物影像熱量回歸估算 | EfficientNet-B4 + 多任務迴歸 | Food-101 份量密度基準 | Food-101 (75,750 張) | Top-5 影像辨識率 89.2% | 無法檢測混合菜餚內之深層油脂與湯底。 |
| **[3]** | 2025 | 個人化營養建議生成 | LLaMA-3 微調 + 領域對齊 | 臨床營養學與 ADA 指引 | MyFitnessPal 匿蹤飲食紀錄 | 幻覺與不安全建議降低 43% | 無法即時動態檢索在地資料庫。 |
| **[4]** | 2024 | 對話式記錄效率評估 | GPT-3.5 + Function Calling | 美國農業部飲食營養指引 | 500 次模擬對話紀錄 | 輸入速度提升 3 倍 | 份量模糊時直接猜測，無主動澄清。 |
| **[5]** | 2025 | 台灣在地特色飲食辨識 | YOLOv8 + ResNet-50 | 衛福部食品成分資料庫 | 5 萬張台灣在地料理圖庫 | 在地食物分類準確率 76.4% | 高度依賴照明，無法解析自製家常菜。 |

### 4. 文獻鏈結與研究缺口收斂（Research Gap）
現有研究各自著重於「英文純文字問答」或「靜態圖片辨識」，**尚無系統同時整合繁體中文對話式交互、台灣在地食物資料庫錨定、不確定性區間估算與臨床安全守門網**。DietSense 正是為填補此研究缺口而設計的整合型解決方案。

