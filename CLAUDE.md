# GPAL MK-VII — Claude Code 指引

## 專案概述
個人生產力追蹤系統，Stark/JARVIS 風格 HTML 單頁應用，託管於 GitHub Pages。

## 檔案結構
```
gpal_optimized.html   # 完整版（~5450 行）
gpal_family.html      # 家庭版（~2580 行）
Code.gs               # Google Apps Script 後端（手動部署至 GAS，不走 Pages）
GPAL_Project_Brief.md # 完整專案摘要（新對話時上傳用）
CLAUDE.md             # 本文件
```

## 後端
- Google Apps Script Web App（URL 寫死在兩個 HTML 的 CONFIG.WEB_APP_URL）
- 後端改動需手動到 GAS 編輯器重新部署，與 GitHub 無關

## 每次修改後必做
1. **同步更新兩版版本號（4 處都要改，缺一不可）：**
   - `<title>` 裡的 vX.XX
   - `brand-bold` span 裡的 vX.XX
   - `<meta name="version" content="時間戳">` 的時間戳（格式 YYYYMMDDHHMI）
   - JS 內 `const CURRENT_VERSION = '時間戳'`（與 meta 相同值）
2. **git commit 並 push 到 main**
3. GitHub Pages 自動重建（1–2 分鐘），使用者 app 的 checkVersion 偵測到新版後亮 UPDATE 按鈕

## 重要開發規則
- Family 版移除了 Finance HUD、Financial Intel、Health Bar、Portfolio HUD；HealthModule 是空殼 stub
- 所有 `getElementById` 在 Family 版必須做 null 檢查
- 日期計算用 `T12:00:00` 防跨時區
- Apps Script 支出存負數，前端顯示用 `Math.abs()`
- 手機按鈕用 `onclick` attribute 而非 `addEventListener`
- 雙版本同步更新；惟功能差異處依各版實際需要，不強行一致
- `GoalModule.create()` 和 `PlanModule.create()` 接受可選 `parent` 參數（用於 DocumentFragment 批次插入）
- `fetchData` 用 `?type=bundle&date=X` 端點（一次取日記 + health）
- UPDATE 按鈕用 `location.replace(location.pathname + '?v=' + Date.now())` 強制繞快取

## Git 操作
每次改完直接：
```bash
git add gpal_optimized.html gpal_family.html
git commit -m "vX.XX: 簡短描述改動"
git push
```

## 完整專案細節
詳見 `GPAL_Project_Brief.md`（後端端點、Sheet 結構、模組架構等）
