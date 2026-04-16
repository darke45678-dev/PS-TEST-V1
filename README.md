# DV Editor (v8.0) - 專業影像編輯器

這是一個基於全原生 Web 技術開發的單檔案影像編輯應用程式，旨在瀏覽器中提供接近 Photoshop 的圖層化編輯體驗與高效能影像處理能力。

## 🚀 部署連結 (Deployed URL)
[https://darke45678-dev.github.io/PS-TEST-V1/](https://darke45678-dev.github.io/PS-TEST-V1/)

---

## 🛠 技術棧與架構解析 (Technical Stack & Architecture)

本專案採用的架構設計核心在於「高效能渲染」與「非破壞性編輯」，主要技術細節如下：

### 1. 核心渲染引擎 (Rendering Engine)
- **Canvas API (2D Context)**: 使用多層離屏畫布 (Offscreen Canvas) 進行複合渲染。每一層圖層都有獨立的參數（座標、縮放、旋轉、不透明度、混合模式）。
- **圖層系統 (Layering System)**: 模擬 PS 的圖層架構，支援多種圖層類型：
  - `img`: 點陣圖像。
  - `text`: 動態向量文字（Canvas FillText）。
  - `draw`: 自由塗鴉（Path-based）。
  - `pen`: 鋼筆曲線。
  - `group`: 資料結構層級群組。

### 2. 影像處理演算法 (Image Processing)
- **非破壞性濾鏡 (Non-destructive Adjustments)**: 
  - **CSS Filter 模擬**: 利用 `canvasContext.filter` 實現即時的曝光、對比、飽和度與色溫調整，確保在 60FPS 下進行無延遲預覽。
  - **自定義疊加層**: 透過 `globalCompositeOperation` 實現複雜的高光/暗部補償與暈影 (Vignette) 效果。
- **Web Workers (多執行緒)**: 針對耗時的像素級運算（如：色階 Levels、高斯模糊 Blur），將運算邏輯封裝在動態建立的 Web Worker 中，避免影像處理時造成 UI 執行緒凍結。

### 3. 進階修圖工具機制
- **魔術棒 (Magic Wand)**: 基於 Flood Fill 演算法的透明遮罩生成技術，可快速實現去背。
- **智慧修復 (Spot Healing)**: 基於熱傳導擴散方程式 (Diffusion Equation) 的圖像修復技術，在本地端實現無縫接邊填充。
- **仿製圖章 (Clone Stamp)**: 使用 `drawImage` 的局部採樣與筆刷裁切路徑實現像素轉錄。

### 4. 數據與持久化 (Data Management)
- **IndexedDB**: 考慮到 4K 等級影像的體積修正，拋棄傳統 5MB 限制的 LocalStorage，使用本地索引數據庫進行完整專案檔存檔。
- **狀態快照 (Undo/Redo)**: 基於資料結構深度序列化的歷史棧紀錄，支援多達 20 步的操作還原。

### 5. UI/UX 與佈局
- **Photoshop 式介面**: 
  - 具備圖層鎖定（透明度、像素、位置、全部同步鎖定）。
  - 獨立的混合模式選單與 Fill (填滿) 參數控制。
  - 動態畫布縮放與捲軸同步系統 (`applyZoom`)，確保在大畫布作業時的流暢導覽。

---

## 📂 檔案結構紀錄
- `index.html`: 核心應用程式（包含所有 CSS 與 JS）。
- `WORK_LOG.md`: 詳細的開發紀錄與功能演進日誌。
- `.github/workflows/static.yml`: GitHub Actions 自動集部署指令碼。

---

## 🎨 聲明
本專案為開發驗證版本，所有影像處理邏輯均在用戶端完成，不涉及服務端運算，確保用戶隱私。
