# Claude 提示工程指南

> 基於Anthropic官方文檔的Claude專屬優化策略

## 📋 概述

Claude是Anthropic開發的AI助理，具有獨特的架構和能力。本指南基於Anthropic官方文檔和研究，提供針對Claude模型的專業提示工程策略。

## 🎯 Claude 獨特特性

### 1. 擴展思考模式（Extended Thinking）
Claude支援特殊的思考模式，能夠進行更深入的推理：

```
思考預算層級：
- "think" → 基礎思考模式
- "think hard" → 深度思考模式  
- "think harder" → 更深度思考
- "ultrathink" → 最深度思考模式
```

### 2. XML標籤優勢
Claude在訓練時使用了XML標籤，因此對結構化提示特別敏感：
```xml
<instructions>
你的具體指令
</instructions>

<example>
範例內容
</example>

<document>
參考文檔
</document>
```

### 3. 思考標籤支援
Claude支援內部思考過程的可視化：
```xml
<thinking>
讓我分析這個問題...
首先，我需要考慮...
然後，我應該...
</thinking>

基於以上思考，我的答案是...
```

## 🚀 核心最佳實踐

### 1. 「請一步一步思考」的Claude優化版本

#### 基礎版本
```
請一步一步思考這個問題，並在 <thinking></thinking> 標籤中顯示你的推理過程。
```

#### 進階版本
```
請深入思考這個問題。使用 <thinking> 標籤記錄你的思考過程，然後提供最終答案。

<thinking>
[Claude會在這裡顯示詳細的思考過程]
</thinking>

[最終答案]
```

### 2. 思考預算管理

#### 不同層級的使用場景
```
- "think" → 簡單分析任務
  例：「請think一下這個數學問題的解法」

- "think hard" → 複雜推理任務
  例：「請think hard分析這個商業策略的可行性」

- "think harder" → 多步驟邏輯推理
  例：「請think harder解決這個邏輯謎題」

- "ultrathink" → 最複雜的創新性問題
  例：「請ultrathink設計一個創新的解決方案」
```

## 🏗️ XML結構化提示設計

### 1. 基本結構模板

#### 分析任務模板
```xml
<task>
分析以下商業案例並提供策略建議
</task>

<context>
公司背景：中型科技公司，年收入5000萬
市場情況：競爭激烈，增長放緩
挑戰：客戶獲取成本上升，用戶留存率下降
</context>

<requirements>
1. 分析根本原因
2. 提出3個具體解決方案
3. 評估每個方案的風險和收益
4. 推薦最佳方案並說明理由
</requirements>

<format>
請按照以下格式回答：
1. 問題分析
2. 解決方案
3. 風險評估
4. 最終建議
</format>
```

### 2. 教學場景模板

#### 蘇格拉底式教學
```xml
<role>
你是一位經驗豐富的教師，採用蘇格拉底式教學方法
</role>

<goal>
幫助學生理解[概念名稱]，但不要直接給出答案
</goal>

<method>
1. 通過提問引導學生思考
2. 基於學生回答調整問題
3. 逐步引導到核心概念
4. 鼓勵學生自己得出結論
</method>

<student_level>
[描述學生的背景知識水平]
</student_level>

<instructions>
開始與學生的對話，用第一個引導性問題開始...
</instructions>
```

### 3. 代碼生成模板

#### 結構化代碼請求
```xml
<task>
創建一個Python類來管理用戶帳戶
</task>

<specifications>
<functionality>
- 用戶註冊和登錄
- 密碼加密存儲
- 用戶資料更新
- 帳戶狀態管理
</functionality>

<requirements>
- Python 3.8+
- 使用類型提示
- 遵循PEP 8風格
- 包含完整的錯誤處理
- 添加詳細的文檔字符串
</requirements>

<constraints>
- 不使用外部數據庫
- 密碼必須加密
- 包含輸入驗證
</constraints>
</specifications>

<output_format>
請提供：
1. 完整的類定義
2. 使用範例
3. 測試代碼
4. 設計說明
</output_format>
```

## 💡 Claude專屬進階技巧

### 1. 角色一致性維護

#### 持續角色設定
```xml
<role_definition>
你是一位專業的數據科學家，有以下特徵：
- 10年機器學習經驗
- 擅長商業應用
- 溝通清晰直接
- 重視數據驗證
- 謹慎對待不確定性
</role_definition>

<behavior_guidelines>
在整個對話中，請始終：
1. 用數據支持你的觀點
2. 承認分析的局限性
3. 提供可執行的建議
4. 使用專業但易懂的語言
</behavior_guidelines>

<context_memory>
記住我們正在討論客戶流失預測項目，
已確定使用隨機森林模型，
現在需要討論特徵工程策略。
</context_memory>
```

### 2. 多步驟任務鏈

#### 提示鏈設計
```xml
<!-- 步驟1：數據分析 -->
<step1>
<task>分析以下銷售數據，識別趨勢和異常</task>
<data>[數據內容]</data>
<output>將結果保存為analysis_result</output>
</step1>

<!-- 步驟2：基於分析結果的建議 -->
<step2>
<task>基於analysis_result，提供銷售策略建議</task>
<context>使用前一步驟的分析結果</context>
<focus>可執行的具體行動</focus>
</step2>
```

### 3. 自我修正機制

#### 內建驗證流程
```xml
<primary_task>
計算複合增長率並分析投資回報
</primary_task>

<verification_process>
完成計算後，請執行以下檢查：
1. 數學計算是否正確？
2. 假設是否合理？
3. 結論是否符合邏輯？
4. 是否考慮了重要風險因素？
</verification_process>

<correction_instruction>
如果發現錯誤，請在<correction>標籤中說明並提供修正版本
</correction_instruction>
```

## 🔬 Claude特色功能應用

### 1. 憲章訓練影響

Claude經過憲章訓練，特別注重：
- **有益性**：提供真正有幫助的回答
- **無害性**：避免有害或偏見內容
- **誠實性**：承認不確定性和知識局限

#### 利用這些特性的提示設計
```xml
<task>
評估這個商業決策的潛在風險
</task>

<ethical_considerations>
請特別關注：
1. 對員工的影響
2. 客戶利益保護
3. 環境責任
4. 長期可持續性
</ethical_considerations>

<honesty_requirement>
如果某些風險難以評估或數據不足，
請明確說明這些限制，不要推測
</honesty_requirement>
```

### 2. 多語言能力

#### 跨語言任務設計
```xml
<task>
翻譯並分析以下商業文檔的關鍵要點
</task>

<source_language>英文</source_language>
<target_language>正體中文</target_language>

<requirements>
1. 準確翻譯專業術語
2. 保持原文的語調和風格
3. 標註文化差異需要注意的地方
4. 提供關鍵概念的雙語對照
</requirements>

<cultural_sensitivity>
注意商業慣例和法律制度的差異
</cultural_sensitivity>
```

## 📊 性能優化策略

### 1. 思考預算優化

#### 根據任務複雜度調整
```
簡單任務（事實查詢）：
→ 不使用特殊思考指令

中等任務（分析比較）：
→ 「請think並分析...」

複雜任務（創新設計）：
→ 「請think harder，創造性地解決...」

極複雜任務（多維度戰略規劃）：
→ 「請ultrathink，綜合考慮...」
```

### 2. 上下文管理

#### 長對話優化
```xml
<conversation_summary>
總結到目前為止討論的要點：
1. 已確定的決策
2. 待解決的問題
3. 下一步行動
</conversation_summary>

<current_focus>
現在我們專注於：[具體議題]
</current_focus>

<relevant_context>
與當前討論最相關的背景：[關鍵信息]
</relevant_context>
```

## 🎓 教學應用特色

### 1. 個性化學習指導

#### 適應性教學提示
```xml
<learner_profile>
<background>[學習者背景]</background>
<level>[當前水平]</level>
<goals>[學習目標]</goals>
<preferences>[學習偏好]</preferences>
</learner_profile>

<adaptive_instruction>
根據學習者特徵調整：
1. 解釋的深度和複雜度
2. 使用的例子和類比
3. 練習的難度和類型
4. 反饋的詳細程度
</adaptive_instruction>

<progress_tracking>
在每次互動後評估學習進展並調整策略
</progress_tracking>
```

### 2. 批判性思維培養

#### 引導深度思考
```xml
<thinking_framework>
請使用以下思考框架分析問題：
1. 問題的本質是什麼？
2. 有哪些不同的觀點？
3. 證據的品質如何？
4. 邏輯推理是否有效？
5. 結論的局限性在哪裡？
</thinking_framework>

<socratic_questions>
通過以下類型問題引導思考：
- 「你為什麼這樣認為？」
- 「有沒有其他可能的解釋？」
- 「這個假設的前提是什麼？」
- 「如果情況相反會怎樣？」
</socratic_questions>
```

## 🛠️ 工具整合與API使用

### 1. 系統消息最佳實踐

#### Claude API系統消息設計
```python
system_message = """
你是一位專業的商業分析師。請遵循以下指導原則：

核心特質：
- 數據驅動的分析方法
- 實用且可執行的建議
- 承認分析的局限性
- 清晰的溝通風格

工作方法：
1. 總是要求澄清模糊的要求
2. 提供結構化的分析
3. 包含風險評估
4. 建議具體的下一步行動

輸出格式：
- 使用清晰的標題和要點
- 優先顯示關鍵發現
- 包含支持數據
- 明確標註假設和限制
"""
```

### 2. 多輪對話管理

#### 對話狀態維護
```xml
<conversation_state>
<current_topic>[當前主題]</current_topic>
<decisions_made>[已做決定]</decisions_made>
<pending_questions>[待解決問題]</pending_questions>
<next_actions>[下一步行動]</next_actions>
</conversation_state>

<context_preservation>
在每次回應中，考慮完整的對話歷史，
確保一致性和連續性
</context_preservation>
```

## 🔍 常見問題與解決方案

### 1. 過度思考問題

#### 問題
使用「ultrathink」導致回應過於冗長

#### 解決方案
```xml
<thinking_constraint>
請ultrathink分析這個問題，但將最終回應控制在300字以內。
在thinking標籤中詳細思考，在最終回應中提供精煉的要點。
</thinking_constraint>
```

### 2. XML格式錯誤

#### 問題
XML標籤格式不正確導致解析問題

#### 解決方案
```xml
<!-- 正確格式示範 -->
<instructions>
清晰的指令內容
</instructions>

<examples>
<example1>範例內容1</example1>
<example2>範例內容2</example2>
</examples>

<!-- 避免嵌套複雜結構 -->
```

### 3. 角色漂移問題

#### 問題
長對話中Claude偏離設定的角色

#### 解決方案
```xml
<role_reminder>
記住你是[角色描述]，請保持這個角色的特徵：
- [特徵1]
- [特徵2]
- [特徵3]
</role_reminder>

<behavior_check>
在回應前檢查：這個回答符合我的角色設定嗎？
</behavior_check>
```

## 📈 高級應用案例

### 1. 研究助手配置

#### 學術研究支援
```xml
<research_role>
你是一位研究助手，專精於[研究領域]
</research_role>

<research_methodology>
1. 批判性評估來源可靠性
2. 識別研究空白和矛盾
3. 建議進一步調查方向
4. 提供平衡的觀點分析
</research_methodology>

<citation_requirement>
所有主張都必須有適當的支持證據，
如果缺乏證據請明確說明
</citation_requirement>
```

### 2. 創意協作夥伴

#### 創意專案支援
```xml
<creative_collaboration>
我們正在共同開發一個創意項目。
你的角色是創意夥伴和批評者。
</creative_collaboration>

<creative_process>
1. 鼓勵創新思維
2. 提供建設性反饋
3. 建議改進方向
4. 幫助克服創意障礙
</creative_process>

<balance_guidance>
在創意自由和實用性之間找到平衡
</balance_guidance>
```

## 📚 學習資源與參考

### Anthropic官方資源
- [Claude提示工程概述](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [擴展思考技巧](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/extended-thinking-tips)
- [Claude 4最佳實踐](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)

### 互動式學習
- [Anthropic互動式提示工程教程](https://github.com/anthropics/prompt-eng-interactive-tutorial)

## 🔑 關鍵要點總結

1. **利用XML結構**：Claude對XML標籤特別敏感，善用結構化提示
2. **思考預算管理**：根據任務複雜度選擇適當的思考層級
3. **思考標籤可視化**：使用`<thinking>`標籤了解推理過程
4. **角色一致性**：明確設定並維護角色特徵
5. **自我修正機制**：內建驗證和修正流程
6. **憲章訓練優勢**：利用Claude的有益、無害、誠實特性
7. **提示鏈設計**：將複雜任務分解為連續步驟

---

*充分發揮Claude的獨特優勢，創造更智能的AI協作體驗！*