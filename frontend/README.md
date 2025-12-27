# Frontend - NotebookLM Chatbot

Next.js 前端應用，與 Python FastAPI 後端通信。

## 快速開始

### 1. 安裝依賴

```bash
npm install
```

### 2. 啟動開發伺服器

```bash
npm run dev
```

前端將在 `http://localhost:3000` 啟動。

> ⚠️ **注意**：前端需要後端服務才能正常運作。請確保後端已在 `http://localhost:8000` 啟動。

## 專案結構

```
frontend/
├── src/
│   ├── app/                    # Next.js App Router
│   │   ├── layout.tsx          # 根佈局
│   │   ├── page.tsx            # 首頁
│   │   └── globals.css         # 全域樣式
│   ├── components/             # React 元件
│   │   ├── chat-panel/         # 聊天介面元件
│   │   ├── document-panel/     # 文件管理元件（未來功能）
│   │   ├── explain-panel/      # 解釋面板元件（未來功能）
│   │   ├── layout/             # 佈局元件
│   │   ├── providers/          # Context Providers
│   │   └── ui/                 # shadcn/ui 元件
│   ├── contexts/               # React Context
│   │   ├── SimpleChatContext.tsx    # 對話狀態管理
│   │   ├── DocumentContext.tsx      # 文件狀態（未來）
│   │   └── ExplainContext.tsx       # 解釋面板狀態（未來）
│   ├── lib/
│   │   ├── api.ts              # API 客戶端（呼叫 FastAPI 後端）
│   │   └── utils.ts            # 工具函數
│   └── types/
│       └── index.ts            # TypeScript 型別定義
├── public/                     # 靜態資源
├── package.json                # 專案配置
├── next.config.ts              # Next.js 配置
├── tailwind.config.ts          # Tailwind CSS 配置
└── tsconfig.json               # TypeScript 配置
```

## 主要功能

### 已實作
- ✅ 聊天介面
- ✅ 訊息歷史顯示
- ✅ PTKB（個人知識庫）整合
- ✅ 深淺主題切換
- ✅ 響應式設計
- ✅ 載入狀態顯示
- ✅ 錯誤處理

### 未來功能（預留 UI）
- 📄 文件上傳與管理
- 🔍 IR 檢索結果顯示
- 📊 引文標註
- 🎯 可解釋性面板

## API 整合

前端透過 `src/lib/api.ts` 與後端通信：

```typescript
// 發送對話訊息
export const sendChatMessage = async (request: SimpleChatRequest): Promise<SimpleChatResponse> => {
  const response = await fetch('http://localhost:8000/api/chat', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(request),
  });
  return response.json();
};
```

### API 請求格式

```typescript
{
  query: string;                  // 使用者查詢
  conversation_id?: string;       // 對話 ID
  history?: Array<{               // 對話歷史
    role: "user" | "assistant";
    content: string;
  }>;
  ptkb_list?: string[];           // 個人知識庫列表
}
```

### API 回應格式

```typescript
{
  answer: string;                 // 助手回應
  conversation_id: string;        // 對話 ID
  ptkb_used: string[];            // 使用的 PTKB 事實
  new_ptkb?: string;              // 新提取的 PTKB 事實
}
```

## 開發指南

### 啟動開發環境

```bash
# 終端機 1 - 啟動後端
cd ../backend
source venv/bin/activate
uvicorn main:app --reload --port 8000

# 終端機 2 - 啟動前端
npm run dev
```

### 建置生產版本

```bash
npm run build
npm start
```

### Linting

```bash
npm run lint
```

## 環境變數

前端不需要環境變數（所有敏感資訊在後端處理）。

## 技術棧

- **Framework**: Next.js 14 (App Router)
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **UI Components**: shadcn/ui
- **State Management**: React Context
- **HTTP Client**: Fetch API
- **Theme**: next-themes

## 除錯

### 前端無法連接後端

**問題**：`Failed to fetch` 或 CORS 錯誤

**解決方法**：
1. 確認後端正在執行：`curl http://localhost:8000/health`
2. 檢查後端 CORS 設定（應允許 `http://localhost:3000`）
3. 確認 API endpoint 正確（`src/lib/api.ts`）

### 對話沒有回應

**問題**：訊息發送後無回應

**解決方法**：
1. 開啟瀏覽器 DevTools (F12)
2. 查看 Console 錯誤訊息
3. 查看 Network 標籤的請求/回應
4. 確認後端 Console 的日誌

### 樣式問題

**問題**：元件樣式不正確

**解決方法**：
1. 確認 Tailwind CSS 正確編譯：`npm run dev`
2. 清除 Next.js 快取：`rm -rf .next`
3. 重新安裝依賴：`rm -rf node_modules && npm install`

## 相關文件

- [專案根目錄 README](../README.md)
- [後端 README](../backend/README.md)
- [後端設定指南](../backend/SETUP_GUIDE.md)
- [快速開始指南](../QUICK_START_BACKEND.md)

## 授權

與專案主目錄相同


