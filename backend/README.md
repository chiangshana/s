# FastAPI Backend for NotebookLM Chatbot

Python FastAPI 後端，支援 PTKB (Personal Knowledge Base) 管理的對話系統。

## 快速開始

### 1. 安裝依賴

```bash
cd backend
python -m venv venv
source venv/bin/activate  # Windows: .\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

### 2. 設定環境變數

複製 `.env.example` 為 `.env` 並填入你的 Gemini API Key：

```bash
cp .env.example .env
```
# 如果使用window請輸入
# New-Item .env
# notepad .env
# 輸入 GEMINI_API_KEY=你的api key

編輯 `.env`：
```
GEMINI_API_KEY=你的_gemini_api_key
```

### 3. 啟動後端服務

```bash
uvicorn main:app --reload --port 8000
```

後端將在 `http://localhost:8000` 啟動。

### 4. 測試 API

訪問 FastAPI 自動生成的文件：
- Swagger UI: http://localhost:8000/docs
- ReDoc: http://localhost:8000/redoc

或使用 curl 測試：

```bash
curl -X POST http://localhost:8000/api/chat \
  -H "Content-Type: application/json" \
  -d '{
    "query": "你好，我叫小明",
    "conversation_id": "test-123",
    "history": [],
    "ptkb_list": []
  }'
```

## API 端點

### POST /api/chat

對話端點，支援 PTKB 功能。

**請求格式：**
```json
{
  "query": "使用者查詢",
  "conversation_id": "對話 ID（可選）",
  "history": [
    {"role": "user", "content": "先前的使用者訊息"},
    {"role": "assistant", "content": "先前的助手回應"}
  ],
  "ptkb_list": ["個人事實1", "個人事實2"]
}
```

**回應格式：**
```json
{
  "answer": "助手回應",
  "conversation_id": "對話 ID",
  "ptkb_used": ["使用的個人事實"],
  "new_ptkb": "新提取的個人事實（如果有）"
}
```

### GET /

健康檢查端點。

### GET /health

詳細健康狀態。

## 專案結構

```
backend/
├── main.py                    # FastAPI 應用主檔案
├── requirements.txt           # Python 依賴套件
├── .env                       # 環境變數（需自行建立）
├── .env.example               # 環境變數範例
├── api/
│   └── routes/
│       └── chat.py            # Chat API endpoint
├── services/
│   ├── gemini_client.py       # Gemini API 封裝
│   ├── ptkb_service.py        # PTKB 處理邏輯
│   └── chat_service.py        # 對話流程主控
├── models/
│   └── schemas.py             # Pydantic models
└── config/
    └── prompts.py             # Prompt 模板
```

## 開發指南

### 重新載入

使用 `--reload` 標誌啟動服務，檔案修改會自動重新載入：

```bash
uvicorn main:app --reload --port 8000
```

### 除錯

查看 Console 輸出以獲取詳細日誌：
- `[INFO]` - 正常資訊
- `[WARNING]` - 警告
- `[ERROR]` - 錯誤

### 測試

測試單個請求：

```bash
# 測試基本對話
curl -X POST http://localhost:8000/api/chat \
  -H "Content-Type: application/json" \
  -d '{"query": "你好", "history": [], "ptkb_list": []}'

# 測試 PTKB 提取
curl -X POST http://localhost:8000/api/chat \
  -H "Content-Type: application/json" \
  -d '{"query": "我叫小明，喜歡吃披薩", "history": [], "ptkb_list": []}'

# 測試 PTKB 應用
curl -X POST http://localhost:8000/api/chat \
  -H "Content-Type: application/json" \
  -d '{"query": "推薦餐廳", "history": [], "ptkb_list": ["I like pizza"]}'
```

## 注意事項

- 確保 `GEMINI_API_KEY` 已正確設定
- CORS 已設定允許 `http://localhost:3000`（前端）
- 預設使用 `gemini-2.5-flash` 模型
- PTKB 目前儲存在記憶體中（每次重啟會清空）
# Backend Service - RAG Pipeline

本後端模組負責處理 PDF 文件解析、Lucene 索引建立以及基於 Gemini API 的 RAG (Retrieval-Augmented Generation) 對話系統。

## 核心功能說明

### 1. 文件處理與自動索引 (Document Service)
* **自動解析**：使用 `pypdf` 提取上傳檔案的文字內容。
* **即時索引**：整合 **Pyserini (Lucene)**，在檔案上傳後立即將文字內容轉換為可檢索的索引檔（儲存於 `backend/lucene_index`）。
* **格式轉換**：自動將 PDF 轉為 JSONL 格式以符合 Pyserini 輸入規範。

### 2. 精確檢索系統 (Chat Service)
* **勾選過濾**：支援根據前端傳入的 `selected_doc_ids` 進行局部檢索，確保 AI 只參考使用者選定的文件。
* **Query 改寫**：內建 Query Rewriter，會將使用者的口語提問優化為適合檢索的關鍵字。
* **上下文整合**：自動從 Lucene 檢索最相關的文字片段 (Chunks)，並注入 Prompt 中。

### 3. AI 生成模組 (Gemini Client)
* **模型版本**：採用 `gemini-2.5-flash` 以獲取最佳的回應速度。
* **安全性機制**：預設開啟 `BLOCK_NONE` 設定，解決 Gemini API 因安全過濾機制導致的對話中斷問題。
* **重試機制**：針對 API Quota 限制或網路波動設有三次自動重試邏輯。

## 技術棧 (Tech Stack)
- **Framework**: Flask (Python)
- **Search Engine**: Pyserini / Apache Lucene
- **LLM API**: Google Gemini API
- **Utilities**: pypdf, python-dotenv

