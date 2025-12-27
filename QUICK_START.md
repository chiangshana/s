# 🚀 快速開始 - 前後端分離架構

## 📦 專案結構

```
irtm2025-group1-webapp/
├── frontend/         # Next.js 前端應用
├── backend/          # Python FastAPI 後端
└── reference/        # 參考實作
```

## ⚡ 3 分鐘開始

### 1️⃣ 設定後端

```bash
# 進入後端目錄
cd backend

# 創建虛擬環境（首次）
python3 -m venv venv

# 啟動虛擬環境
source venv/bin/activate  # Windows: venv\Scripts\activate

# 安裝依賴（首次）
pip install -r requirements.txt

# 設定 API Key
cp .env.example .env
# 編輯 .env 並填入你的 GEMINI_API_KEY
```

**取得 Gemini API Key（免費）：**
1. 前往 https://aistudio.google.com/apikey
2. 登入 Google 帳號
3. 點擊「Create API Key」
4. 複製 API Key 並貼到 `.env` 檔案

### 2️⃣ 設定前端

```bash
# 開啟新的終端機，進入前端目錄
cd frontend

# 安裝依賴（首次）
npm install
```

### 3️⃣ 啟動服務

**終端機 1 - 啟動後端：**
```bash
cd backend
source venv/bin/activate
uvicorn main:app --reload --port 8000
```

看到這個訊息表示成功：
```
INFO:     Uvicorn running on http://127.0.0.1:8000
```

**終端機 2 - 啟動前端：**
```bash
cd frontend
npm run dev
```

看到這個訊息表示成功：
```
▲ Next.js 16.0.7
- Local:        http://localhost:3000
```

### 4️⃣ 開始使用

1. 開啟瀏覽器訪問 `http://localhost:3000`
2. 在聊天框輸入訊息，例如：
   - 「你好，我叫小明」
   - 「我喜歡吃披薩」
   - 「推薦適合我的餐廳」

Bot 會記住你的個人資訊並提供個人化回應！🎉

## 🧪 測試

### 測試後端

```bash
# 健康檢查
curl http://localhost:8000/health

# 應該返回：{"status":"healthy"}

# 測試對話 API
curl -X POST http://localhost:8000/api/chat \
  -H "Content-Type: application/json" \
  -d '{"query": "你好", "history": [], "ptkb_list": []}'

# 查看 API 文件
open http://localhost:8000/docs
```

### 測試前端

1. 開啟 `http://localhost:3000`
2. 確認頁面載入正常
3. 測試對話功能
4. 打開瀏覽器 DevTools (F12) 查看 Console 和 Network

## 📁 重要檔案

| 檔案 | 說明 |
|------|------|
| `backend/.env` | **必須設定** - Gemini API Key |
| `backend/main.py` | FastAPI 主應用 |
| `backend/services/chat_service.py` | 對話邏輯 |
| `frontend/src/lib/api.ts` | 前端 API 客戶端 |
| `frontend/src/contexts/SimpleChatContext.tsx` | 聊天狀態管理 |

## 🔧 常見問題

### Q: 前端顯示「Failed to fetch」

**原因**：後端未啟動或 CORS 設定問題

**解決**：
1. 確認後端正在 `http://localhost:8000` 執行
2. 測試：`curl http://localhost:8000/health`
3. 檢查後端 Console 是否有錯誤訊息

### Q: 對話沒有回應

**原因**：Gemini API Key 未設定或無效

**解決**：
1. 確認 `backend/.env` 檔案存在且有 `GEMINI_API_KEY`
2. 檢查 API Key 是否正確
3. 查看後端 Console 的錯誤訊息

### Q: 前端建置失敗（中文路徑問題）

**原因**：Next.js 16 Turbopack 不支援中文路徑

**解決**：
- **方法 1**：使用開發模式（`npm run dev`）而非建置模式
- **方法 2**：將專案移到不含中文的路徑
- **方法 3**：等待 Next.js 修復此問題

### Q: 虛擬環境無法啟動

**macOS/Linux**：
```bash
source venv/bin/activate
```

**Windows**：
```bash
venv\Scripts\activate
```

如果還是不行：
```bash
python3 -m venv venv --clear
source venv/bin/activate
pip install -r requirements.txt
```

## 🎯 下一步

- ✅ 基本對話功能正常運作
- 📄 **未來功能**：文件上傳與索引
- 🔍 **未來功能**：IR 檢索整合
- 📊 **未來功能**：引文標註與可解釋性面板

## 📚 詳細文件

- [專案總覽](README.md)
- [前端文件](frontend/README.md)
- [後端文件](backend/README.md)
- [後端設定指南](backend/SETUP_GUIDE.md)
- [前端清理總結](FRONTEND_CLEANUP_COMPLETE.md)
- [後端實作總結](BACKEND_IMPLEMENTATION_COMPLETE.md)

## 💡 開發技巧

### 同時查看前後端日誌

使用 `tmux` 或 `screen` 分割終端機：

```bash
# 安裝 tmux（如果還沒有）
brew install tmux  # macOS
apt install tmux   # Linux

# 啟動 tmux 並分割視窗
tmux
# Ctrl+B 然後按 " 分割上下
# Ctrl+B 然後按 O 切換窗格
```

### VS Code 整合終端

1. 開啟 VS Code 整合終端（Ctrl+\`）
2. 點擊「+」按鈕旁的下拉選單
3. 選擇「Split Terminal」
4. 一個終端跑後端，一個跑前端

### 自動重啟

- **後端**：`uvicorn --reload` 會在檔案變更時自動重啟
- **前端**：`npm run dev` 支援 Hot Module Replacement

## 🤝 團隊協作

### Git 工作流程

```bash
# 拉取最新程式碼
git pull origin main

# 創建功能分支
git checkout -b feature/your-feature

# 前端開發
cd frontend
npm run dev

# 後端開發
cd backend
source venv/bin/activate
uvicorn main:app --reload

# 提交變更
git add .
git commit -m "feat: add your feature"
git push origin feature/your-feature
```

### 分工建議

- **前端開發者**：專注於 `frontend/` 目錄，只需啟動後端即可
- **後端開發者**：專注於 `backend/` 目錄，可用 Postman/curl 測試 API
- **全端開發者**：同時開發兩邊，注意保持 API 介面一致

---

**準備好了嗎？開始啟動服務吧！** 🚀

有問題請查看詳細文件或聯繫團隊成員。
