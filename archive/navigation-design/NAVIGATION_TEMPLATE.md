# 书籍式导航系统标准模板

> 本文档为所有章节提供统一的导航系统模板，确保用户体验的一致性

## 📋 页面头部标准模板

```markdown
# [章节标题]

<!-- 📍 导航面包屑 -->
<nav style="background: #f8f9fa; padding: 8px 16px; border-radius: 6px; margin-bottom: 20px;">
<span style="color: #6c757d;">📚 提示工程精通指南</span>
<span style="color: #6c757d;"> > </span>
<span style="color: #6c757d;">[部分名称]</span>
<span style="color: #6c757d;"> > </span>
<strong>[当前章节]</strong>
</nav>

<!-- 📊 学习进度指示器 -->
<div style="background: #e9ecef; height: 4px; border-radius: 2px; margin-bottom: 20px;">
<div style="background: #007bff; height: 4px; border-radius: 2px; width: [进度百分比]%;"></div>
</div>
<p style="text-align: center; color: #6c757d; font-size: 12px;">
学习进度：第 [当前章节序号] 章 / 共 18 章 ([进度百分比]% 完成)
</p>

<!-- ⏱️ 预计阅读时间和学习概览 -->
<div style="background: #fff3cd; border: 1px solid #ffeaa7; border-radius: 6px; padding: 12px; margin-bottom: 20px;">
📖 <strong>本章概览</strong><br>
📚 预计阅读时间：[X] 分钟<br>
🎯 学习目标：[核心目标1-3个]<br>
📋 前置知识：[可选，如需要]
</div>
```

## 🎯 用户类型导航面板

### 初学者导航面板
```markdown
<!-- 初学者专用导航面板 -->
<div style="background: #d4edda; border: 1px solid #c3e6cb; border-radius: 6px; padding: 15px; margin-bottom: 20px;">
🌱 <strong>初学者指引</strong><br><br>
📍 <strong>您的位置</strong>：第 [X] 章，建议学习路径的第 [Y] 步<br>
⏰ <strong>建议时间</strong>：每章15-20分钟，不要急躁<br>
💡 <strong>学习提示</strong>：重点理解概念，先跳过复杂细节<br><br>
<strong>下一步建议</strong>：
1. 完成本章阅读
2. 尝试章末练习  
3. 进入 → [下一章](链接)
</div>
```

### 进阶者导航面板
```markdown
<!-- 进阶者专用导航面板 -->
<div style="background: #d1ecf1; border: 1px solid #bee5eb; border-radius: 6px; padding: 15px; margin-bottom: 20px;">
🎯 <strong>进阶者快速通道</strong><br><br>
🚀 <strong>核心要点</strong>：[本章3个关键概念]<br>
🔗 <strong>关联技术</strong>：与[相关章节]密切相关<br>
💼 <strong>实战价值</strong>：可直接应用于[具体场景]<br><br>
<strong>快速行动</strong>：
- 📖 [深度阅读建议](链接)
- 💡 [进阶技巧](链接)  
- 🔄 [相关技术对比](链接)
</div>
```

### 企业用户导航面板
```markdown
<!-- 企业用户专用导航面板 -->
<div style="background: #fff3cd; border: 1px solid #ffeaa7; border-radius: 6px; padding: 15px; margin-bottom: 20px;">
🏢 <strong>企业应用指引</strong><br><br>
💰 <strong>商业价值</strong>：[本章对业务的具体价值]<br>
⏱️ <strong>实施时间</strong>：预计 [X] 天可见效果<br>
📊 <strong>ROI评估</strong>：预期效率提升 [X]%<br><br>
<strong>快速部署</strong>：
- 📋 [实施检查清单](链接)
- 🎯 [企业案例参考](链接)
- 📞 [技术支持](链接)
</div>
```

## 📱 响应式章节导航

### 桌面版完整导航
```markdown
<!-- 详细章节导航 - 适用于桌面版 -->
<details open>
<summary><strong>📚 完整章节导航</strong></summary>

### 🌱 第一部分：理论基础与核心原理
- ✅ [快速入门指南](快速入門指南.md) `30分钟`
- ✅ [00-核心基礎：AI互動通用原理](00-核心基礎：AI互動通用原理.md) `45分钟`
- [ ] [01A-提示工程核心概念](01A-提示工程核心概念.md) `40分钟`
- [ ] [01B-提示工程實踐應用](01B-提示工程實踐應用.md) `50分钟`
- [ ] [02-思維鏈技術詳解](02-思維鏈技術詳解.md) `35分钟`

### 🎯 第二部分：主流平台深度應用
- [ ] [03-OpenAI提示工程指南](03-OpenAI提示工程指南.md) `45分钟`
- [ ] [04A-Claude核心特性指南](04A-Claude核心特性指南.md) `40分钟`
- [ ] [04B-Claude進階應用技巧](04B-Claude進階應用技巧.md) `50分钟`
- [ ] [05-Google AI提示工程指南](05-Google AI提示工程指南.md) `45分钟`
- [ ] [06-其他LLM提供商指南](06-其他LLM提供商指南.md) `35分钟`

### ⚡ 第三部分：進階技術與方法創新
- [ ] [07-LLM特性與教學指南](07-LLM特性與教學指南.md) `40分钟`
- [ ] [09A-核心進階技術](09A-核心進階技術.md) `60分钟`
- [ ] [09B-技術組合應用](09B-技術組合應用.md) `55分钟`
- [ ] [10A-前沿研究技術](10A-前沿研究技術.md) `50分钟`
- [ ] [10B-特殊領域應用](10B-特殊領域應用.md) `45分钟`

### 🏢 第四部分：企業應用與最佳實踐
- [ ] [08A-企業級實戰案例](08A-企業級實戰案例.md) `55分钟`
- [ ] [08B-最佳實踐框架](08B-最佳實踐框架.md) `45分钟`

### 🚀 第五部分：前沿探索與未來發展
- [ ] [12-提示模板庫與檢核清單](12-提示模板庫與檢核清單.md) `30分钟`

</details>
```

### 移动版简化导航
```markdown
<!-- 移动设备简化导航 -->
<details>
<summary>📱 <strong>章节导航</strong> (点击展开)</summary>

### 快速跳转
- 🏠 [返回首页](README.md)
- ⬅️ [上一章](上一章链接) 
- ➡️ [下一章](下一章链接)

### 学习路径
- 🌱 基础入门：快速入门 → 核心概念 → 实践应用
- 🎯 平台专精：OpenAI → Claude → Google AI → 其他平台
- ⚡ 技术深度：LLM特性 → 核心进阶 → 技术组合 → 前沿研究
- 🏢 企业应用：实战案例 → 最佳实践 → 模板库

### 工具区域
- 📊 [学习进度追踪](#进度追踪)
- 🔍 [概念快速查找](#概念索引)
- 📝 [个人学习笔记](#学习记录)

</details>
```

## 📊 进度追踪系统

### 全书进度概览
```markdown
## 📚 全书学习地图

<div style="overflow-x: auto;">
<table>
<tr><th>部分</th><th>章节</th><th>状态</th><th>预计时间</th><th>完成度</th></tr>
<tr><td rowspan="5">第一部分<br>理论基础</td><td>快速入门</td><td>✅ 已完成</td><td>30分钟</td><td>100%</td></tr>
<tr><td>核心基础</td><td>📖 进行中</td><td>45分钟</td><td>60%</td></tr>
<tr><td>核心概念</td><td>⏳ 待学习</td><td>40分钟</td><td>0%</td></tr>
<tr><td>实践应用</td><td>⏳ 待学习</td><td>50分钟</td><td>0%</td></tr>
<tr><td>思维链技术</td><td>⏳ 待学习</td><td>35分钟</td><td>0%</td></tr>
<tr><td colspan="5" style="text-align: center; background: #f8f9fa;">
📊 <strong>第一部分进度：1/5 章节完成 (20%)</strong>
</td></tr>
</table>
</div>

**总体进度**：
<div style="background: #e9ecef; height: 8px; border-radius: 4px; margin: 10px 0;">
<div style="background: #28a745; height: 8px; border-radius: 4px; width: 20%;"></div>
</div>
<p style="text-align: center;">5.6% 完成 (1/18 章节) | 预计剩余学习时间：12.5小时</p>
```

### 个人学习记录区域
```markdown
## 📝 我的学习记录

> **使用说明**：此区域供您记录学习心得、重要概念和疑问
> 可以直接在此编辑，作为个人学习档案

### 📅 学习时间记录
- **开始学习时间**：[填写时间]
- **预计完成时间**：[填写时间]  
- **实际学习进度**：[更新进度]

### 🎯 重点概念掌握情况
- [ ] **概念1**：[理解程度/疑问记录]
- [ ] **概念2**：[理解程度/疑问记录]
- [ ] **概念3**：[理解程度/疑问记录]

### 💡 实践应用计划
- [ ] **应用场景1**：[具体计划]
- [ ] **应用场景2**：[具体计划]

### 🔖 需要回顾的章节
- 📚 [章节名称](链接)：[回顾原因]
- 🔗 [章节名称](链接)：[关联学习]

### ❓ 疑问与解答
- **疑问1**：[描述疑问] 
  - 解答：[记录解答或寻求帮助的地方]
- **疑问2**：[描述疑问]
  - 解答：[记录解答或寻求帮助的地方]
```

## ⚡ 智能推荐系统

### 章节结尾推荐模板
```markdown
## 🚀 继续学习建议

<!-- 基于当前章节的智能推荐 -->
### 📈 按学习目标选择

<div style="display: flex; gap: 15px; margin: 20px 0;">

<div style="flex: 1; background: #e3f2fd; padding: 15px; border-radius: 8px;">
<strong>🌱 巩固基础</strong><br>
如果您是初学者或想加强理解：<br>
→ [推荐章节](链接)<br>
→ [相关练习](链接)
</div>

<div style="flex: 1; background: #f3e5f5; padding: 15px; border-radius: 8px;">
<strong>🎯 深入专精</strong><br>
如果您想深入某个平台：<br>
→ [专业指南](链接)<br>
→ [进阶技巧](链接)
</div>

<div style="flex: 1; background: #e8f5e8; padding: 15px; border-radius: 8px;">
<strong>⚡ 立即实践</strong><br>
如果您想马上应用：<br>
→ [实战案例](链接)<br>
→ [模板库](链接)
</div>

</div>

### 🎓 个性化学习路径

**根据您的兴趣领域**：
- 🤖 对**OpenAI/ChatGPT**感兴趣 → [OpenAI专精指南](03-OpenAI提示工程指南.md)
- 🎭 对**Claude**感兴趣 → [Claude核心特性](04A-Claude核心特性指南.md)
- 🎯 对**Google AI**感兴趣 → [Google AI指南](05-Google AI提示工程指南.md)
- 🔬 对**进阶技术**感兴趣 → [核心进阶技术](09A-核心進階技術.md)
- 🏢 对**企业应用**感兴趣 → [企业实战案例](08A-企業級實戰案例.md)

### ⏰ 时间规划建议

**如果您有...**
- **15分钟**：快速浏览 [下一章概要](#) 
- **30分钟**：完整学习 [推荐章节](链接)
- **1小时**：深度学习并实践 [实战练习](链接)
- **半天时间**：系统学习整个 [主题模块](链接)
```

## 🏷️ 快速查找与索引系统

### 概念索引页面模板
```markdown
# 📇 全书概念快速索引

> 快速查找任何概念、技巧或平台特性

## 🔍 字母索引

<div style="text-align: center; margin: 20px 0;">
<a href="#A">A</a> | <a href="#B">B</a> | <a href="#C">C</a> | <a href="#D">D</a> | <a href="#E">E</a> | <a href="#F">F</a> | <a href="#G">G</a> | <a href="#H">H</a> | <a href="#I">I</a> | <a href="#J">J</a> | <a href="#K">K</a> | <a href="#L">L</a> | <a href="#M">M</a> | <a href="#N">N</a> | <a href="#O">O</a> | <a href="#P">P</a> | <a href="#Q">Q</a> | <a href="#R">R</a> | <a href="#S">S</a> | <a href="#T">T</a> | <a href="#U">U</a> | <a href="#V">V</a> | <a href="#W">W</a> | <a href="#X">X</a> | <a href="#Y">Y</a> | <a href="#Z">Z</a>
</div>

### A-C {#A}
- **AI工作原理** → [核心基础：现代语言模型](链接) | [技术应用](链接)
- **Attention机制** → [核心基础：注意力机制原理](链接)
- **Constitutional AI** → [Claude核心特性](链接)
- **Chain of Thought** → [思维链技术详解](链接) | [实践应用](链接)

### D-F {#D}
- **Few-shot Learning** → [实践应用：范例学习](链接) | [OpenAI应用](链接)

## 🏷️ 主题分类索引

### 📚 核心技术
<table>
<tr><th>技术</th><th>基础概念</th><th>实践应用</th><th>进阶技巧</th></tr>
<tr><td><strong>思维链技术</strong></td><td><a href="链接">基础原理</a></td><td><a href="链接">实战应用</a></td><td><a href="链接">高级技巧</a></td></tr>
<tr><td><strong>角色设定</strong></td><td><a href="链接">理论基础</a></td><td><a href="链接">实践技巧</a></td><td><a href="链接">专业应用</a></td></tr>
<tr><td><strong>Few-shot Learning</strong></td><td><a href="链接">概念理解</a></td><td><a href="链接">示例设计</a></td><td><a href="链接">优化策略</a></td></tr>
</table>

### 🤖 平台特性
<table>
<tr><th>平台</th><th>核心特性</th><th>应用指南</th><th>最佳实践</th></tr>
<tr><td><strong>OpenAI</strong></td><td><a href="链接">GPT特性</a></td><td><a href="链接">实用指南</a></td><td><a href="链接">企业应用</a></td></tr>
<tr><td><strong>Claude</strong></td><td><a href="链接">核心特性</a></td><td><a href="链接">基础应用</a></td><td><a href="链接">进阶技巧</a></td></tr>
<tr><td><strong>Google AI</strong></td><td><a href="链接">Gemini特色</a></td><td><a href="链接">应用指南</a></td><td><a href="链接">企业部署</a></td></tr>
</table>

## ⭐ 热门搜索

### 🔥 最常查找的问题
1. **如何让AI一步步思考？** → [快速入门：神奇公式](链接) | [思维链详解](链接)
2. **如何设计完美提示词？** → [实践应用：设计原则](链接) | [模板库](链接)
3. **各个AI平台有什么区别？** → [平台对比表](链接) | [选择指南](链接)
4. **提示工程的商业价值？** → [企业案例](链接) | [ROI分析](链接)
5. **如何入门提示工程？** → [30分钟快速入门](链接) | [学习路径](链接)

### 💡 实用技巧速查
- **提高回答准确性** → [思维链技术](链接) | [结构化提示](链接)
- **获得专业回答** → [角色设定技巧](链接) | [专业提示模板](链接) 
- **处理复杂任务** → [任务分解方法](链接) | [进阶技术组合](链接)
- **企业级应用** → [实战案例集](链接) | [部署指南](链接)
```

## 📋 页面底部导航模板

```markdown
---

## 📋 章节导航

<div style="display: flex; justify-content: space-between; align-items: center; margin: 30px 0; padding: 20px; background: #f8f9fa; border-radius: 10px;">

<div style="text-align: left;">
<h4>⬅️ 上一章</h4>
<a href="[上一章链接]" style="text-decoration: none;">
<strong>[上一章标题]</strong><br>
<span style="color: #6c757d; font-size: 14px;">[简短描述]</span>
</a>
</div>

<div style="text-align: center;">
<a href="README.md" style="text-decoration: none;">
<div style="background: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">
<strong>📚 返回目录</strong><br>
<span style="font-size: 12px;">查看完整学习地图</span>
</div>
</a>
</div>

<div style="text-align: right;">
<h4>下一章 ➡️</h4>
<a href="[下一章链接]" style="text-decoration: none;">
<strong>[下一章标题]</strong><br>
<span style="color: #6c757d; font-size: 14px;">[简短描述]</span>
</a>
</div>

</div>

### 🎯 快速行动

<div style="display: flex; gap: 10px; margin: 20px 0; flex-wrap: wrap;">
<a href="[相关练习链接]" style="background: #28a745; color: white; padding: 8px 16px; border-radius: 5px; text-decoration: none; font-size: 14px;">💡 立即练习</a>
<a href="[模板下载链接]" style="background: #17a2b8; color: white; padding: 8px 16px; border-radius: 5px; text-decoration: none; font-size: 14px;">📋 下载模板</a>
<a href="[深度学习链接]" style="background: #ffc107; color: black; padding: 8px 16px; border-radius: 5px; text-decoration: none; font-size: 14px;">🔍 深度学习</a>
<a href="[社群讨论链接]" style="background: #6f42c1; color: white; padding: 8px 16px; border-radius: 5px; text-decoration: none; font-size: 14px;">💬 讨论交流</a>
</div>

---

<div style="text-align: center; margin: 30px 0; color: #6c757d;">
<p>📚 <strong>提示工程精通指南</strong> | 让AI成为你最得力的工作伙伴</p>
<p>💡 学习过程中有任何问题，欢迎查阅 <a href="FAQ.md">FAQ</a> 或参与 <a href="DISCUSSION.md">社群讨论</a></p>
</div>
```

## 🎨 样式与布局标准

### CSS样式定义
```css
/* 导航系统统一样式 */
.navigation-breadcrumb {
    background: #f8f9fa;
    padding: 8px 16px;
    border-radius: 6px;
    margin-bottom: 20px;
}

.progress-bar {
    background: #e9ecef;
    height: 4px;
    border-radius: 2px;
    margin-bottom: 20px;
}

.progress-fill {
    background: #007bff;
    height: 4px;
    border-radius: 2px;
}

.chapter-overview {
    background: #fff3cd;
    border: 1px solid #ffeaa7;
    border-radius: 6px;
    padding: 12px;
    margin-bottom: 20px;
}

.user-guide-panel {
    border-radius: 6px;
    padding: 15px;
    margin-bottom: 20px;
}

.beginner-panel {
    background: #d4edda;
    border: 1px solid #c3e6cb;
}

.advanced-panel {
    background: #d1ecf1;
    border: 1px solid #bee5eb;
}

.enterprise-panel {
    background: #fff3cd;
    border: 1px solid #ffeaa7;
}
```

---

**此模板确保全书导航系统的一致性和专业性，可根据具体章节内容进行个性化调整。**