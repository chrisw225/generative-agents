# Generative Agents 項目用戶指南

歡迎使用生成式代理（Generative Agents）項目用戶指南！此指南包含完整的使用流程、地圖編輯方法和技術說明。

## 📚 指南目錄

### 🚀 核心使用流程
- [[generative-agents-workflow]] - 生成式代理項目完整用戶流程
  - 環境設置和服務器啟動
  - 模擬運行和Web界面使用
  - 回放觀看和控制操作
  - 時間管理和活躍時段建議

### 🗺️ 地圖編輯指南  
- [[map-editing-workflow]] - 地圖編輯完整流程
  - 地圖文件結構說明
  - 視覺和邏輯組件編輯
  - 測試和部署流程

### 🛠️ 工具使用指南
- [[tiled-map-editor-guide]] - Tiled Map Editor 使用說明
  - Tiled編輯器安裝和配置
  - TMX文件編輯和PNG導出
  - 碰撞檢測層設置

- [[manage-py-complete-guide]] - Django manage.py 完整技術指南與回放教程
  - Django 服務器啟動和管理
  - 模擬文件命名規範詳解
  - 回放系統完整操作指南
  - JSON 數據結構和壓縮算法
  - 故障排除和最佳實踐

### 🔧 技術深度解析
- [[color-id-system-guide]] - 顏色ID系統完整指南
  - 顏色編碼系統技術原理
  - 五層地圖系統架構解析
  - maze.py處理流程詳解
  - 實際案例分析和調試指南

- [[maze-py-technical-guide]] - maze.py 核心技術詳解
  - 地圖管理系統架構和數據結構
  - 四層空間組織系統：世界→區域→場所→對象
  - 坐標轉換、事件管理和視野計算
  - 性能優化和調試指南

- [[step-by-step-data-flow-guide]] - Step數據流程完整指南
  - 完整數據流程架構和時序分析
  - 三大核心腳本詳解：reverie.py, persona.py, maze.py
  - 認知模塊深度解析：感知→檢索→規劃→反思→執行
  - JSON數據格式規範和調試監控指南

## 🕐 最後更新

- **創建日期**: 2025年1月8日
- **最後更新**: 2025年1月8日
- **版本**: v1.0

## 💡 使用建議

1. **新用戶**: 建議先閱讀 [[generative-agents-workflow]] 了解基本操作
2. **地圖編輯**: 需要修改地圖佈局時參考 [[map-editing-workflow]]  
3. **技術細節**: 深入了解工具使用可查看 [[tiled-map-editor-guide]]

## 🔗 相關資源

- [項目GitHub庫](https://github.com/joonspk-research/generative_agents)
- [Tiled Map Editor官網](https://www.mapeditor.org/)
- [Memory Bank文檔](../memory-bank/)

---
*注意：本指南為用戶參考文檔，與AI記憶庫（memory-bank）功能獨立，專門為人類用戶提供學習和操作指導。*
