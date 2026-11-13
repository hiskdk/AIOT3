# DietSense：對話式日常熱量攝取分析領域智慧代理人 (Domain AI Agent)

[![OpenSpec 規格驅動開發](https://img.shields.io/badge/OpenSpec-規格驅動開發-blue.svg)](https://openspec.dev)
[![Python 3.11+](https://img.shields.io/badge/Python-3.11+-brightgreen.svg)](https://www.python.org/)
[![授權條款：MIT](https://img.shields.io/badge/授權-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

> **DietSense** 是一套結合資料錨定（Data-Grounded）與安全守門（Safety-Aware）的領域智慧代理人（Domain AI Agent），旨在將傳統繁瑣被動的手動熱量記錄，轉化為直覺、具台灣在地飲食理解與情境感知的對話式健康管理體驗。

---

## 🌟 四大核心創新亮點

DietSense 整合**自然語言輸入、主動式智慧互動、情境感知與大數據驅動智慧**四大核心能力，提供個人化、智慧化且可執行的體重管理決策建議：

1. **自然語言輸入（Natural Language Input）**  
   使用者可直接用日常口語記錄飲食（例如：*「我剛剛吃了一碗加蛋的台式牛肉麵，湯喝了一半」*），免除傳統 App 逐項搜尋資料庫與手動換算重量的繁重認知負擔。
2. **主動式智慧互動（Proactive Agentic Interaction）**  
   當食物品項或份量描述不夠明確時，主動觸發澄清迴圈（Clarification Loop）進行針對性追問（例如主動詢問碗的大小、是否有把湯喝完），而非盲目猜測不準確的單一數值。
3. **情境感知（Context Awareness）**  
   結合個人生理參數（年齡、性別、身高、體重、活動量），透過 Mifflin-St Jeor 公式動態計算基礎代謝率（BMR）與每日總消耗熱量（TDEE），並依據用餐時段與作息給予即時回饋。
4. **大數據驅動智慧（Big Data–Driven Intelligence）**  
   結合台灣衛福部食品成分資料庫、美國農業部 USDA 資料庫與台式料理向量檢索庫，以確定性演算法計算熱量信賴區間，並內建臨床安全防護網（攔截每日低於 1,200 大卡的危險過度節食）。

---

## 🛠️ OpenSpec 規格驅動開發（Spec-Driven Development）

本專案全面採用 **OpenSpec 規格驅動開發流程**：
- 所有架構設計與系統行為合約均先於 `openspec/changes/dietsense-proposal-refactor/specs/` 完成規格定義，再行實施編程。
- 完整的系統工程決策、八大模組架構、流程圖與演算法虛擬碼請參閱根目錄的 [`DESIGN.md`](DESIGN.md)。
- 驗證目前規格狀態：
  ```powershell
  openspec validate dietsense-proposal-refactor
  ```

---

## 📂 專案目錄結構

```text
├── .agents/skills/            # Antigravity 專用的 OpenSpec 工具技能庫
├── openspec/                  # OpenSpec 規格設定與變更管理目錄
│   ├── config.yaml            # 規格驅動開發設定檔
│   └── changes/               # 正在進行的規格提案、能力規格與任務清單
├── src/                       # 核心原始程式碼（代理人調度、檢索管線、分析運算）
├── data/                      # 台灣食品資料庫與台式料理向量資料集
├── DESIGN.md                  # 系統架構設計文件（符合期末專題交接規範）
└── README.md                  # 專案介紹與執行說明文件
```

---

## 🚀 快速上手

### 系統環境要求
- Node.js >= 22.12.0
- Python >= 3.11
- OpenSpec 命令列工具 (`npm install -g @fission-ai/openspec@latest`)

### 檢查與啟動
```powershell
# 檢查 OpenSpec CLI 是否正確安裝
openspec --version

# 檢視目前的規格提案儀表板
openspec view
```
