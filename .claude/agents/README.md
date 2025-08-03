# 通用文檔改進 Agents 使用指南

## 📋 總覽

本專案包含五個專業的文檔改進專家 Agents，經過通用化設計，可以處理各種類型的文檔改進專案。這些 Agents 可以單獨使用，也可以組合使用以處理複雜的文檔改進任務，特別是將文檔專案轉化為專業指南書。

## 🔧 Agents 介紹

### 1. doc-improver（通用文檔改進專家）
**用途**：根據品質評估報告優化任何類型的文檔

**專長**：
- 現代文檔風格優化
- 結構重組和邏輯改善
- 內容可讀性提升
- 視覺設計簡化

**適用場景**：
- 技術文檔改進（API 文檔、開發指南、系統文檔）
- 用戶文檔優化（產品手冊、教學材料、操作指南）
- 企業文檔改進（流程文檔、政策文件、培訓材料）
- 教育文檔優化（課程材料、學習指南、研究報告）

### 2. quality-validator（通用品質驗證專家）
**用途**：驗證文檔改進成果並生成詳細的分析報告

**專長**：
- 多維度品質評估
- 改進效果驗證
- 數據對比分析
- 建設性改進建議

**適用場景**：
- 改進成果驗證
- 品質控制檢查
- 專案成效評估
- 持續改進規劃

### 3. style-unifier（通用風格統一專家）
**用途**：確保文檔集合在風格、術語、格式等方面保持高度一致

**專長**：
- 多類型文檔風格標準制定
- 跨領域術語統一
- 通用格式規範化
- 精密一致性檢查

**適用場景**：
- 大型文檔專案的風格統一
- 組織文檔標準建立
- 品牌形象一致性維護
- 多作者協作專案的規範化

### 4. outcome-analyzer（通用成果分析專家）
**用途**：總結改進結果，提供 ROI 分析和戰略建議

**專長**：
- 跨領域數據分析
- 多維效果評估
- 通用趨勢分析
- 適應性戰略建議

**適用場景**：
- 專案成果總結
- 投資回報分析
- 戰略決策支援
- 未來規劃制定

### 5. guidebook-strategist（專業指南書策略專家）
**用途**：將文檔專案轉化為專業指南書的戰略規劃和實施指導

**專長**：
- 出版策略規劃與市場定位
- 內容架構設計與學習路徑優化
- 品質標準制定與流程管理
- 多Agent協調與工作流統籌

**適用場景**：
- 文檔專案指南書轉化
- 出版品質標準制定
- 學習體驗設計
- 市場策略規劃
- Agent工作流協調

## 🎯 使用模式

### 單獨使用模式

每個 Agent 都可以獨立使用來處理特定的文檔改進任務：

#### 快速改進單個文檔
```bash
# 使用 doc-improver 改進單個文檔
使用 doc-improver，設定參數：
- 文檔類型：technical
- 目標受眾：expert
- 改進重點：accuracy, usability
```

#### 驗證改進效果
```bash
# 使用 quality-validator 驗證改進結果
使用 quality-validator 對改進前後的文檔進行品質評估
```

#### 統一多文檔風格
```bash
# 使用 style-unifier 統一文檔集合風格
使用 style-unifier 確保 5-10 個相關文檔的風格一致性
```

#### 分析專案成果
```bash
# 使用 outcome-analyzer 總結專案成果
使用 outcome-analyzer 分析整個改進專案的效果和價值
```

#### 制定指南書策略
```bash
# 使用 guidebook-strategist 制定指南書轉化戰略
使用 guidebook-strategist 分析文檔專案並制定指南書製作計劃
```

### 組合使用模式

#### 模式一：順序處理流程
適用於系統性的文檔改進專案：

1. **評估階段**：使用內建評估方法或外部品質報告
2. **改進階段**：使用 `doc-improver` 根據評估結果改進文檔
3. **驗證階段**：使用 `quality-validator` 驗證改進效果
4. **統一階段**：使用 `style-unifier` 確保多文檔一致性
5. **分析階段**：使用 `outcome-analyzer` 總結專案成果

#### 模式二：並行處理流程  
適用於大規模文檔改進專案：

1. **並行改進**：使用多個 `doc-improver` 任務同時處理不同文檔
2. **並行驗證**：使用多個 `quality-validator` 任務驗證改進結果
3. **統一處理**：使用 `style-unifier` 統一所有改進後的文檔
4. **綜合分析**：使用 `outcome-analyzer` 分析整體成果

#### 模式三：迭代改進流程
適用於需要多輪優化的複雜專案：

```
初始評估 → doc-improver → quality-validator → 
（如果需要）→ doc-improver → quality-validator → 
style-unifier → outcome-analyzer
```

#### 模式四：指南書製作流程
適用於將文檔專案轉化為專業指南書：

```
guidebook-strategist（戰略規劃）→ doc-improver（內容改進）→ 
quality-validator（品質驗證）→ style-unifier（風格統一）→ 
outcome-analyzer（成果分析）→ guidebook-strategist（最終指導）
```

## ⚙️ 配置參數說明

### doc-improver 配置

#### 文檔類型（必選）
- `technical` - 技術文檔：重點改善準確性和邏輯性
- `user-facing` - 用戶文檔：重點提升易讀性和友善度  
- `educational` - 教學材料：重點優化學習體驗和漸進性
- `enterprise` - 企業文檔：重點強化專業性和實用性

#### 目標受眾（必選）
- `expert` - 技術專家：保持技術深度，優化效率
- `general` - 一般用戶：降低門檻，提升親和力
- `beginner` - 初學者：強化引導，增加範例

#### 改進重點（可選）
- `accuracy` - 準確性：確保技術內容正確無誤
- `readability` - 可讀性：提升語言流暢度和理解度
- `completeness` - 完整性：補充缺失內容，完善結構
- `usability` - 實用性：增強可操作性和實際應用價值

### guidebook-strategist 配置

#### 專案類型（必選）
- `technical-education` - 技術教育專案：程式設計、系統管理、數據科學指南
- `professional-skills` - 專業技能專案：商業技能、創意技能、認證考試指南
- `knowledge-transfer` - 知識傳承專案：企業內訓、學術研究、個人成長指南

#### 指南書類型（建議）
- `beginner-oriented` - 入門導向型：零基礎友善、循序漸進
- `practical-application` - 實戰應用型：問題導向、實用技巧
- `in-depth-expertise` - 深度專精型：理論深度、技術細節
- `comprehensive` - 綜合全面型：系統完整、多角度覆蓋

### 其他 Agents 配置

其他四個 Agents 主要根據文檔類型和專案需求自動調整配置，一般不需要額外參數設定。當與 guidebook-strategist 配合時，將根據指南書策略進行優化配置。

## 📊 成功案例模板

### 技術文檔專案範例
```markdown
專案類型：API 文檔改進
文檔數量：8 個文檔
使用流程：
1. doc-improver (technical, expert, accuracy+usability)
2. quality-validator（技術文檔驗證標準）
3. style-unifier（技術文檔風格統一）
4. outcome-analyzer（技術專案 ROI 分析）

預期成果：
- 開發者使用效率提升 30%
- 技術支援請求減少 40%
- 文檔維護成本降低 25%
```

### 用戶文檔專案範例
```markdown
專案類型：產品使用手冊優化
文檔數量：12 個文檔
使用流程：
1. doc-improver (user-facing, general, readability+usability)
2. quality-validator（用戶文檔驗證標準）
3. style-unifier（用戶文檔風格統一）
4. outcome-analyzer（用戶專案 ROI 分析）

預期成果：
- 用戶滿意度提升至 4.5/5.0
- 任務完成時間減少 35%
- 客戶支援工作量減少 50%
```

### 企業文檔專案範例
```markdown
專案類型：內部流程文檔標準化
文檔數量：15 個文檔
使用流程：
1. doc-improver (enterprise, general, completeness+usability)
2. quality-validator（企業文檔驗證標準）
3. style-unifier（企業文檔風格統一）
4. outcome-analyzer（企業專案 ROI 分析）

預期成果：
- 流程執行效率提升 40%
- 員工培訓時間減少 30%
- 合規風險降低 60%
```

### 指南書製作專案範例
```markdown
專案類型：提示工程學習指南書
文檔數量：10 個文檔
使用流程：
1. guidebook-strategist（專案評估與戰略規劃）
2. doc-improver (educational, general, readability+usability)
3. quality-validator（指南書品質驗證標準）
4. style-unifier（指南書風格統一）
5. outcome-analyzer（學習效果與市場價值分析）
6. guidebook-strategist（出版策略與維護規劃）

預期成果：
- 學習效果提升 50%
- 市場競爭力建立
- 出版品質達標
- 可持續收益模式
```

## 💡 最佳實踐建議

### 1. 專案規劃建議
- **小型專案**（1-3 個文檔）：建議使用順序處理模式
- **中型專案**（4-10 個文檔）：建議使用並行處理模式
- **大型專案**（10+ 個文檔）：建議使用迭代改進模式

### 2. 品質控制建議
- 每個階段完成後都進行中間檢查
- 使用 quality-validator 的建議進行必要的調整
- 確保 style-unifier 的一致性標準得到執行

### 3. 效率最佳化建議
- 根據文檔類型選擇合適的 Agent 配置
- 利用並行處理節省時間
- 建立標準化的工作流程和檢查清單

### 4. 持續改進建議
- 使用 outcome-analyzer 的戰略建議進行後續規劃
- 建立文檔品質監控機制
- 定期回顧和更新改進標準

### 5. 指南書製作建議
- 使用 guidebook-strategist 進行專案可行性評估
- 制定明確的出版品質標準和時程規劃
- 建立市場導向的內容架構和學習體驗
- 協調所有 Agents 形成完整的指南書製作工作流

## 🔍 故障排除

### 常見問題解決

#### Agent 無法啟動
- 檢查 `.claude/agents/` 目錄是否存在
- 確認 Agent 檔案格式正確（YAML frontmatter + Markdown）
- 檢查檔案權限設定

#### 改進效果不理想
- 確認文檔類型和目標受眾設定正確
- 檢查原始文檔品質評估是否準確
- 考慮使用迭代改進模式進行多輪優化

#### 風格統一效果不佳
- 確保所有文檔都已完成基本改進
- 檢查是否有特殊格式需求未考慮
- 考慮手動調整部分特殊情況

#### 成果分析數據不準確
- 確認所有改進前後的數據都已收集
- 檢查評估標準的一致性
- 驗證數據計算方法的正確性

## 📞 支援與回饋

如果您在使用過程中遇到問題或有改進建議，請：

1. 檢查本指南的故障排除章節
2. 查看各個 Agent 的詳細說明文檔
3. 記錄具體的使用場景和問題描述
4. 提供改進建議和使用體驗回饋

---

**準備開始使用通用文檔改進 Agents！選擇適合的 Agent 和工作模式，讓專業的文檔改進專家幫助您提升文檔品質。**