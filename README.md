# RAGify: NotebookLM-style Chatbot

一個結合 Personal Knowledge Base (PTKB) 管理的對話系統，採用前後端分離架構。

## 🏗️ 專案架構

```
irtm2025-group1-webapp/
├── frontend/           # Next.js 前端應用
│   ├── src/
│   ├── public/
│   └── package.json
├── backend/            # FastAPI Python 後端
│   ├── api/
│   ├── services/
│   ├── models/
│   └── main.py
└── reference/          # 參考實作（Python）
```

## 🚀 快速開始

### 前置需求

- Node.js 18+
- Python 3.8+
- Google Gemini API Key（免費）
- Java 21 以上

### 1. 設定後端

```bash
cd backend
python3 -m venv venv
source venv/bin/activate  # Windows: .\venv\Scripts\Activate.ps1
pip install --upgrade pip
pip install -r requirements.txt
pip install pyserini google-generativeai flask-cors pypdf python-dotenv

# 設定 API Key
cp .env.example .env

# 如果是 Windows 環境
# New-Item .env
# notepad .env
# 輸入 GEMINI_API_KEY=你的api key

python main.py
```




### 2. 設定前端

```bash
cd frontend
npm install
```

### 3. 啟動服務

**終端機 1 - 後端：**
```bash
cd backend
source venv/bin/activate  # Windows: .\venv\Scripts\Activate.ps1
uvicorn main:app --reload --port 8000
```



**終端機 2 - 前端：**
```bash
cd frontend
npm run dev
```

### 4. 開始使用

開啟瀏覽器訪問 `http://localhost:3000`

試試看：
1. 「你好，我叫小明」
2. 「我喜歡吃披薩」
3. 「推薦適合我的餐廳」

Bot 會記住你的個人資訊並提供個人化回應！

## 📚 詳細文件

- **[快速開始](QUICK_START_BACKEND.md)** - 3 分鐘設定指南
- **[前端 README](frontend/README.md)** - 前端專案說明
- **[後端 README](backend/README.md)** - 後端專案說明
- **[後端設定指南](backend/SETUP_GUIDE.md)** - 詳細設定步驟
- **[實作總結](BACKEND_IMPLEMENTATION_COMPLETE.md)** - 技術細節

## 🎯 主要功能

### 已實作
- ✅ 前後端分離架構（Next.js + FastAPI）
- ✅ PTKB（個人知識庫）自動提取與應用
- ✅ 多輪對話上下文管理
- ✅ 個人化回應生成
- ✅ Google Gemini API 整合
- ✅ 深淺主題切換
- ✅ 響應式設計
- ✅ 錯誤處理與重試機制

### 未來功能（UI 已預留）
- 📄 文件上傳與索引
- 🔍 IR（Information Retrieval）檢索
- 📊 引文標註與來源追蹤
- 🎯 可解釋性面板（顯示 RAG 流程）
- 💾 資料庫持久化儲存

## 🛠️ 技術棧

### 前端
- Next.js 14 (App Router)
- TypeScript
- Tailwind CSS + shadcn/ui
- React Context

### 後端
- FastAPI
- Python 3.8+
- Google Gemini API
- Pydantic

### 通信
- REST API (HTTP)
- JSON 資料格式
- CORS 設定

## 📊 架構圖

```
┌─────────────────┐      HTTP REST API      ┌──────────────────┐
│   Frontend      │◄─────────────────────────►│    Backend       │
│   (Next.js)     │   http://localhost:8000  │   (FastAPI)      │
│   Port: 3000    │                          │   Port: 8000     │
└─────────────────┘                          └──────────────────┘
        │                                              │
        │                                              │
        ▼                                              ▼
   Browser UI                               ┌──────────────────┐
   - Chat Panel                             │  Gemini API      │
   - Theme Toggle                           │  (gemini-2.5-    │
   - Responsive                             │   flash)         │
                                            └──────────────────┘
```

## 🔧 API 端點

### POST /api/chat
對話端點，支援 PTKB 功能。

**請求：**
```json
{
  "query": "使用者查詢",
  "conversation_id": "對話 ID（可選）",
  "history": [{"role": "user", "content": "..."}],
  "ptkb_list": ["個人事實1", "個人事實2"]
}
```

**回應：**
```json
{
  "answer": "助手回應",
  "conversation_id": "對話 ID",
  "ptkb_used": ["使用的個人事實"],
  "new_ptkb": "新提取的個人事實"
}
```

## 🧪 測試

### 測試後端
```bash
# 健康檢查
curl http://localhost:8000/health

# 測試對話
curl -X POST http://localhost:8000/api/chat \
  -H "Content-Type: application/json" \
  -d '{"query": "你好", "history": [], "ptkb_list": []}'

# API 文件
open http://localhost:8000/docs
```

### 測試前端
1. 開啟 `http://localhost:3000`
2. 輸入訊息並觀察回應
3. 測試 PTKB 提取（說出個人資訊）
4. 測試 PTKB 應用（要求個人化建議）

## 📖 專案歷史

這個專案原本是一個 Next.js 全端應用（前端 + Next.js API Routes），現已重構為前後端分離架構：

- **前端**：純 Next.js UI，透過 HTTP API 與後端通信
- **後端**：獨立的 Python FastAPI 服務，處理所有 AI 邏輯

這樣的架構帶來：
- ✅ 更好的關注點分離
- ✅ 易於獨立擴展
- ✅ 可使用 Python 生態系統（Pyserini 等）
- ✅ 更容易加入 IR 功能

## 🤝 貢獻

這是一個課程專案。如有問題請聯繫專案成員。

## 📝 授權

此專案為學術用途。

## 🙏 致謝

- Google Gemini API
- FastAPI Framework
- Next.js Framework
- shadcn/ui Components

---

**需要幫助？** 查看 [快速開始指南](QUICK_START_BACKEND.md) 或 [設定指南](backend/SETUP_GUIDE.md)
