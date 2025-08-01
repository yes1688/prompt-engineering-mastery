# 其他LLM提供商指南

> **探索多元AI生態系統的專業指南** - 從開源方案到企業級部署的全方位策略

## 📖 概述

現代AI生態系統呈現多元化發展，除了OpenAI、Anthropic和Google等主流提供商外，Meta Llama、Microsoft Azure AI、Cohere、Hugging Face等眾多優秀平台各具特色。本指南深入分析主要LLM提供商的技術特點、最佳實踐和應用策略，幫助您構建多元化、高效能的AI解決方案。

## 🎯 學習目標

完成本章學習後，您將能夠：

- ✅ **掌握多平台特性**：深入理解不同LLM提供商的技術優勢和適用場景
- ✅ **實現智能選擇**：根據業務需求科學選擇最適合的AI平台組合
- ✅ **建構整合系統**：設計跨平台的LLM編排和管理架構
- ✅ **優化成本效益**：掌握多平台成本分析和優化策略
- ✅ **確保企業合規**：實現符合企業級安全和合規要求的部署

## 📚 先決條件

在開始學習本章之前，建議您：

- ✅ 完成前序章節的主流平台學習（OpenAI、Claude、Gemini）
- ✅ 具備基本的雲端服務和API整合經驗
- ✅ 了解企業級軟體架構設計原則
- ✅ 熟悉Docker、Kubernetes等容器化技術基礎

---

## 🦙 Meta Llama生態系統深度解析

### 🔓 開源優勢與技術特性

> 💡 **白話解釋**  
> **為什麼開源AI很重要？** 想像您買了一輛車，封閉式的AI就像只能去原廠保養，所有零件都是黑盒子，您不知道引擎怎麼運作。開源AI就像開放式的引擎，您不只能看到所有零件怎麼運作，還能自己改裝、維修，甚至建立自己的維修廠！Meta Llama就是這樣的「開放引擎」，讓企業可以完全掌控AI系統，不用擔心被某個廠商「綁架」，還能根據自己的需求客製化。

<div style="background-color: #E8F5E8; padding: 20px; border-left: 4px solid #4CAF50; margin: 20px 0;">

**Meta Llama核心優勢**

1. **完全開源**：MIT許可證，支援商業使用和修改
2. **社群驅動**：活躍的開發社群，持續更新和優化
3. **多語言優化**：對繁體中文等亞洲語言有良好支援
4. **硬體適應性**：可在不同硬體配置上靈活部署
5. **成本可控**：無API使用費，僅需基礎設施成本

</div>

#### 🧠 Llama 2/3架構深度分析

<table>
<tr>
<th width="50%">🎯 技術特點</th>
<th width="50%">📈 企業應用優勢</th>
</tr>
<tr>
<td valign="top">

**Transformer優化架構**
- RMSNorm標準化技術
- SwiGLU激活函數
- 旋轉位置編碼(RoPE)

**多尺度模型系列**
- 7B：輕量級部署
- 13B：效能平衡點
- 70B：企業級性能

**對話專業化**
- Chat版本針對對話優化
- Constitutional AI安全對齊
- 人類回饋強化學習

</td>
<td valign="top">

**部署靈活性**
- 本地化部署避免資料外洩
- 客製化微調滿足特定需求
- 成本可預測性強

**擴展性優勢**
- 支援大規模並發處理
- 可與企業現有系統整合
- 支援離線運行

**合規性保障**
- 資料處理全程可控
- 符合各國資料保護法規
- 審計追蹤完整透明

</td>
</tr>
</table>

#### 💻 企業級Llama部署架構

**高可用性部署範例**
```python
import torch
from transformers import AutoTokenizer, AutoModelForCausalLM
from torch.nn.parallel import DataParallel
import logging
from typing import Dict, List, Optional
import asyncio
import redis
from datetime import datetime

class EnterpriseLlamaService:
    def __init__(self, model_path: str, gpu_count: int = 4):
        self.model_path = model_path
        self.gpu_count = gpu_count
        self.setup_logging()
        self.load_model()
        self.setup_cache()
        
    def setup_logging(self):
        logging.basicConfig(
            level=logging.INFO,
            format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
            handlers=[
                logging.FileHandler('llama_service.log'),
                logging.StreamHandler()
            ]
        )
        self.logger = logging.getLogger(__name__)
    
    def load_model(self):
        """載入和配置Llama模型"""
        try:
            self.tokenizer = AutoTokenizer.from_pretrained(
                self.model_path,
                trust_remote_code=True
            )
            
            # 配置模型以支援多GPU
            self.model = AutoModelForCausalLM.from_pretrained(
                self.model_path,
                torch_dtype=torch.float16,
                device_map="auto",
                trust_remote_code=True,
                load_in_8bit=True  # 記憶體優化
            )
            
            if self.gpu_count > 1:
                self.model = DataParallel(self.model)
            
            self.logger.info(f"Llama模型載入成功，使用{self.gpu_count}個GPU")
            
        except Exception as e:
            self.logger.error(f"模型載入失敗: {str(e)}")
            raise
    
    def setup_cache(self):
        """設置Redis快取系統"""
        try:
            self.cache = redis.Redis(
                host='localhost', 
                port=6379, 
                decode_responses=True,
                socket_timeout=5
            )
            self.cache.ping()
            self.logger.info("Redis快取連接成功")
        except Exception as e:
            self.logger.warning(f"Redis連接失敗，將使用記憶體快取: {str(e)}")
            self.cache = {}
    
    async def generate_response(
        self, 
        prompt: str, 
        max_length: int = 512,
        temperature: float = 0.7,
        top_p: float = 0.9,
        system_message: str = None
    ) -> Dict:
        """生成回應的主要方法"""
        
        start_time = datetime.now()
        
        try:
            # 檢查快取
            cache_key = self._generate_cache_key(prompt, max_length, temperature)
            if isinstance(self.cache, redis.Redis):
                cached_response = self.cache.get(cache_key)
                if cached_response:
                    self.logger.info("從快取返回回應")
                    return {
                        "response": cached_response,
                        "cached": True,
                        "processing_time": 0.01
                    }
            
            # 構建完整提示
            full_prompt = self._build_prompt(prompt, system_message)
            
            # Token化
            inputs = self.tokenizer.encode(full_prompt, return_tensors="pt")
            
            if torch.cuda.is_available():
                inputs = inputs.cuda()
            
            # 生成回應
            with torch.no_grad():
                outputs = self.model.generate(
                    inputs,
                    max_length=inputs.shape[1] + max_length,
                    temperature=temperature,
                    top_p=top_p,
                    do_sample=True,
                    pad_token_id=self.tokenizer.eos_token_id,
                    attention_mask=torch.ones_like(inputs)
                )
            
            # 解碼回應
            response_text = self.tokenizer.decode(
                outputs[0][inputs.shape[1]:],
                skip_special_tokens=True,
                clean_up_tokenization_spaces=True
            )
            
            processing_time = (datetime.now() - start_time).total_seconds()
            
            # 儲存到快取
            if isinstance(self.cache, redis.Redis):
                self.cache.setex(cache_key, 3600, response_text)  # 1小時快取
            
            result = {
                "response": response_text.strip(),
                "cached": False,
                "processing_time": processing_time,
                "tokens_generated": len(outputs[0]) - len(inputs[0]),
                "model_path": self.model_path
            }
            
            self.logger.info(f"成功生成回應，處理時間: {processing_time:.2f}秒")
            return result
            
        except Exception as e:
            self.logger.error(f"回應生成失敗: {str(e)}")
            return {
                "error": str(e),
                "processing_time": (datetime.now() - start_time).total_seconds()
            }
    
    def _build_prompt(self, user_prompt: str, system_message: str = None) -> str:
        """建構符合Llama格式的提示"""
        
        default_system = """You are a helpful, respectful and honest assistant. 
Always answer as helpfully as possible, while being safe. 
Your answers should not include harmful, unethical, racist, sexist, toxic, dangerous, or illegal content. 
Please ensure that your responses are socially unbiased and positive in nature."""
        
        system = system_message or default_system
        
        # Llama Chat格式
        formatted_prompt = f"""<s>[INST] <<SYS>>
{system}
<</SYS>>

{user_prompt} [/INST]"""
        
        return formatted_prompt
    
    def _generate_cache_key(self, prompt: str, max_length: int, temperature: float) -> str:
        """生成快取鍵值"""
        import hashlib
        content = f"{prompt}_{max_length}_{temperature}"
        return f"llama_cache:{hashlib.md5(content.encode()).hexdigest()}"

# 使用範例
async def main():
    # 初始化服務
    llama_service = EnterpriseLlamaService(
        model_path="meta-llama/Llama-2-13b-chat-hf",
        gpu_count=2
    )
    
    # 商業分析範例
    business_prompt = """
    我們是一家中小型電商公司，最近3個月銷售額下降了15%。
    主要原因包括：
    1. 競爭對手推出更優惠的價格
    2. 我們的網站使用體驗較差
    3. 客戶服務回應速度慢
    
    請提供一個全面的改善策略，包含短期和長期的具體行動方案。
    """
    
    result = await llama_service.generate_response(
        prompt=business_prompt,
        max_length=800,
        temperature=0.3,  # 較低溫度確保邏輯性
        system_message="你是一位資深的商業顧問，專精於電商營運改善。"
    )
    
    print(f"分析結果: {result['response']}")
    print(f"處理時間: {result['processing_time']:.2f}秒")

if __name__ == "__main__":
    asyncio.run(main())
```

### 🎯 Llama專屬提示工程技巧

#### 1. 系統訊息最佳化設計

> 💡 **白話解釋**  
> **什麼是系統訊息？** 就像您第一天上班時，主管會跟您說「我們公司的文化是什麼、工作標準是什麼、應該注意什麼事項」一樣。系統訊息就是告訴AI「您的角色是什麼、應該怎麼表現、要遵守什麼原則」。對Llama來說，一個好的系統訊息就像一個清楚的工作手冊，能讓AI更準確地理解您的期望和要求！

<div style="background-color: #FFF3E0; padding: 20px; border-left: 4px solid #FF9800; margin: 20px 0;">

**企業級系統訊息模板庫**
```python
class LlamaSystemPrompts:
    """Llama系統提示模板庫"""
    
    @staticmethod
    def business_analyst():
        return """您是一位資深商業分析師，具備以下專業特質：

角色定位：
- 擁有10年以上商業分析經驗
- 精通數據分析和市場研究
- 善於將複雜分析轉化為可執行建議

工作原則：
- 基於數據和事實進行分析
- 提供結構化和邏輯清晰的建議
- 考慮實際執行的可行性
- 平衡短期效益和長期發展

溝通風格：
- 專業但易懂的表達方式
- 使用具體數據支撐論點
- 提供明確的行動步驟
- 適時提醒風險和限制"""
    
    @staticmethod
    def technical_consultant():
        return """您是一位技術顧問專家，專長如下：

技術背景：
- 15年軟體開發和系統架構經驗
- 熟悉雲端技術和微服務架構
- 精通DevOps和自動化部署
- 了解最新技術趨勢

服務準則：
- 提供技術可行且成本合理的方案
- 考慮系統的可擴展性和維護性
- 重視安全性和效能最佳化
- 提供具體的實施路徑

回應格式：
- 技術方案的詳細說明
- 實施複雜度和時程評估
- 所需資源和技能要求
- 潛在風險和緩解措施"""
    
    @staticmethod
    def customer_service():
        return """您是專業的客戶服務代表，服務特色：

服務理念：
- 客戶滿意是最高優先級
- 耐心、同理心和專業並重
- 主動解決問題而非被動回應
- 將每次互動視為建立信任的機會

處理原則：
- 仔細聆聽並確認客戶需求
- 提供準確和有用的資訊
- 如無法立即解決會明確說明後續步驟
- 確保客戶感受到被重視和理解

溝通技巧：
- 使用親切且專業的語調
- 避免技術術語，以客戶能理解的方式說明
- 提供多種解決方案選擇
- 主動確認客戶是否滿意處理結果"""
```

</div>

#### 2. 思維鏈推理優化

**複雜商業決策分析範例**
```python
def create_business_analysis_prompt(scenario: Dict) -> str:
    return f"""
<s>[INST] <<SYS>>
{LlamaSystemPrompts.business_analyst()}
<</SYS>>

商業情境分析任務：

【背景資訊】
公司：{scenario['company_info']}
行業：{scenario['industry']}
當前挑戰：{scenario['challenge']}
可用資源：{scenario['resources']}

【分析要求】
請採用結構化的思維鏈分析方法：

步驟1：現況診斷
- 識別核心問題和症狀
- 分析問題的根本原因
- 評估問題的影響範圍和嚴重程度

步驟2：環境分析
- SWOT分析（優勢、劣勢、機會、威脅）
- 競爭對手分析
- 市場趨勢和外部因素影響

步驟3：解決方案設計
- 提出3-5個可行的解決方案
- 評估每個方案的優缺點
- 分析實施難度和資源需求

步驟4：實施建議
- 推薦最佳方案組合
- 制定詳細的實施計劃
- 設定關鍵績效指標(KPI)
- 識別風險和制定應對策略

步驟5：效果預測
- 預估各項改善的量化效果
- 設定realistic的時程目標
- 規劃效果評估和調整機制

請確保每個步驟的分析都有具體的數據支撐和邏輯推理過程。 [/INST]
"""

# 使用範例
scenario = {
    "company_info": "中型製造業公司，員工300人，年營收5億元",
    "industry": "汽車零組件製造",
    "challenge": "原料成本上漲30%，利潤率下降至5%，部分客戶流失",
    "resources": "現金流充足，有研發團隊，生產設備需要更新"
}

analysis_prompt = create_business_analysis_prompt(scenario)
```

---

## 🔷 Microsoft Azure AI企業級整合

### 🏢 Azure AI生態系統架構

> 💡 **白話解釋**  
> **Azure AI為什麼適合企業？** 想像您要建立一個大型購物中心，您不會只考慮店面，還需要考慮停車場、安全系統、消防設備、清潔維護等等。Azure AI就像提供「一站式購物中心解決方案」的廠商，不只給您AI大腦，還提供資料保護、系統監控、自動擴展、災難備援等企業必需的「基礎設施」。這樣企業就能專注在業務創新，而不用擔心底層的技術複雜性！

<div style="background-color: #E3F2FD; padding: 20px; border-left: 4px solid #2196F3; margin: 20px 0;">

**Azure AI服務矩陣**

| 服務類別 | 核心產品 | 企業優勢 | 適用場景 |
|---------|----------|----------|----------|
| **語言理解** | Azure OpenAI Service | 企業級SLA保證 | 客戶服務、內容生成 |
| **認知服務** | Cognitive Services | 多模態整合 | 文檔處理、語音分析 |
| **機器學習** | Azure ML Studio | 端到端MLOps | 模型訓練、部署管理 |
| **知識挖掘** | Cognitive Search | 企業搜尋整合 | 知識管理、文檔檢索 |
| **對話AI** | Bot Framework | 企業級安全 | 智能客服、內部助理 |

</div>

#### 🔐 企業級安全與合規架構

**完整的Azure AI安全實施**
```python
import asyncio
import logging
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient
from azure.monitor.opentelemetry import configure_azure_monitor
from azure.core.exceptions import AzureError
from typing import Dict, List, Optional
import json
from datetime import datetime
import hashlib

class SecureAzureAIService:
    """企業級安全的Azure AI服務"""
    
    def __init__(self, key_vault_url: str, config: Dict):
        self.key_vault_url = key_vault_url
        self.config = config
        self.setup_security()
        self.setup_monitoring()
        self.setup_compliance()
        
    def setup_security(self):
        """設置安全性組件"""
        try:
            # Azure身份驗證
            self.credential = DefaultAzureCredential()
            
            # Key Vault客戶端
            self.secret_client = SecretClient(
                vault_url=self.key_vault_url,
                credential=self.credential
            )
            
            # 取得API密鑰
            self.openai_key = self.secret_client.get_secret("openai-api-key").value
            self.storage_key = self.secret_client.get_secret("storage-key").value
            
            logging.info("安全性組件初始化完成")
            
        except Exception as e:
            logging.error(f"安全性設置失敗: {str(e)}")
            raise
    
    def setup_monitoring(self):
        """設置監控和遙測"""
        # Azure Monitor整合
        configure_azure_monitor(
            connection_string=self.config.get("monitor_connection_string")
        )
        
        self.logger = logging.getLogger(__name__)
        self.logger.setLevel(logging.INFO)
        
        # 自訂遙測
        self.metrics = {
            "request_count": 0,
            "error_count": 0,
            "security_violations": 0,
            "compliance_checks": 0
        }
    
    def setup_compliance(self):
        """設置合規性檢查"""
        self.compliance_rules = {
            "pii_detection": True,
            "content_filtering": True,
            "data_retention": 90,  # 天數
            "audit_logging": True,
            "gdpr_compliance": True
        }
        
        self.prohibited_patterns = [
            r'\b\d{4}-\d{4}-\d{4}-\d{4}\b',  # 信用卡號
            r'\b\d{3}-\d{2}-\d{4}\b',        # 社會安全號碼
            r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b'  # 郵件地址
        ]
    
    async def secure_ai_request(
        self, 
        prompt: str, 
        user_id: str,
        request_context: Dict
    ) -> Dict:
        """安全的AI請求處理"""
        
        request_id = self._generate_request_id()
        start_time = datetime.now()
        
        try:
            # 1. 合規性檢查
            compliance_result = await self._compliance_check(
                prompt, user_id, request_context
            )
            
            if not compliance_result["passed"]:
                return {
                    "error": "合規性檢查失敗",
                    "details": compliance_result["violations"],
                    "request_id": request_id
                }
            
            # 2. 內容過濾
            filtered_prompt = await self._content_filter(prompt)
            
            # 3. 執行AI請求
            ai_response = await self._execute_ai_request(
                filtered_prompt, request_context
            )
            
            # 4. 回應後處理
            processed_response = await self._post_process_response(
                ai_response, request_context
            )
            
            # 5. 審計日誌
            await self._audit_log(
                request_id, user_id, prompt, processed_response, 
                start_time, "SUCCESS"
            )
            
            return {
                "response": processed_response,
                "request_id": request_id,
                "compliance_passed": True,
                "processing_time": (datetime.now() - start_time).total_seconds()
            }
            
        except Exception as e:
            await self._audit_log(
                request_id, user_id, prompt, None, 
                start_time, "ERROR", str(e)
            )
            
            return {
                "error": "請求處理失敗",
                "request_id": request_id,
                "details": str(e) if self.config.get("debug") else "內部錯誤"
            }
    
    async def _compliance_check(
        self, 
        prompt: str, 
        user_id: str, 
        context: Dict
    ) -> Dict:
        """執行合規性檢查"""
        
        violations = []
        
        # PII檢測
        if self.compliance_rules["pii_detection"]:
            pii_detected = self._detect_pii(prompt)
            if pii_detected:
                violations.append({
                    "type": "PII_DETECTED",
                    "details": "偵測到個人識別資訊",
                    "items": pii_detected
                })
        
        # 用戶權限檢查
        if not self._check_user_permissions(user_id, context):
            violations.append({
                "type": "INSUFFICIENT_PERMISSIONS",
                "details": "用戶權限不足"
            })
        
        # 內容政策檢查
        policy_violations = self._check_content_policy(prompt)
        if policy_violations:
            violations.extend(policy_violations)
        
        self.metrics["compliance_checks"] += 1
        if violations:
            self.metrics["security_violations"] += len(violations)
        
        return {
            "passed": len(violations) == 0,
            "violations": violations
        }
    
    def _detect_pii(self, text: str) -> List[str]:
        """檢測個人識別資訊"""
        import re
        
        detected_pii = []
        
        for pattern in self.prohibited_patterns:
            matches = re.findall(pattern, text)
            if matches:
                detected_pii.extend(matches)
        
        return detected_pii
    
    async def _content_filter(self, prompt: str) -> str:
        """內容過濾和清理"""
        # 移除敏感資訊
        filtered_prompt = prompt
        
        for pattern in self.prohibited_patterns:
            import re
            filtered_prompt = re.sub(pattern, "[REDACTED]", filtered_prompt)
        
        return filtered_prompt
    
    async def _execute_ai_request(self, prompt: str, context: Dict) -> str:
        """執行AI請求"""
        # 這裡整合Azure OpenAI Service
        # 實際實作會呼叫Azure OpenAI API
        
        enhanced_prompt = f"""
        <系統身份驗證>
        請注意：這是來自已驗證企業用戶的請求
        合規等級：{context.get('compliance_level', 'STANDARD')}
        業務領域：{context.get('business_domain', 'GENERAL')}
        </系統身份驗證>
        
        {prompt}
        
        <回應要求>
        請確保回應符合企業標準：
        - 專業且準確的內容
        - 避免敏感或爭議性話題
        - 提供可執行的建議
        - 包含適當的免責聲明
        </回應要求>
        """
        
        # 模擬AI回應（實際應用中會呼叫真實API）
        return "基於您的需求，我建議採用以下策略..."
    
    async def _post_process_response(self, response: str, context: Dict) -> str:
        """後處理AI回應"""
        # 再次檢查回應內容
        if self._detect_pii(response):
            response = "[此回應包含敏感資訊，已被過濾]"
        
        # 添加免責聲明
        if context.get("add_disclaimer", True):
            disclaimer = "\n\n免責聲明：此建議僅供參考，具體決策請諮詢相關專業人士。"
            response += disclaimer
        
        return response
    
    async def _audit_log(
        self,
        request_id: str,
        user_id: str,
        prompt: str,
        response: Optional[str],
        start_time: datetime,
        status: str,
        error: Optional[str] = None
    ):
        """記錄審計日誌"""
        
        audit_entry = {
            "request_id": request_id,
            "user_id": user_id,
            "timestamp": datetime.now().isoformat(),
            "prompt_hash": hashlib.sha256(prompt.encode()).hexdigest(),
            "response_hash": hashlib.sha256(response.encode()).hexdigest() if response else None,
            "processing_time": (datetime.now() - start_time).total_seconds(),
            "status": status,
            "error": error,
            "compliance_checks_passed": self.metrics["compliance_checks"]
        }
        
        # 寫入Azure Log Analytics或其他審計系統
        self.logger.info(f"AI請求審計: {json.dumps(audit_entry)}")
    
    def _generate_request_id(self) -> str:
        """生成請求ID"""
        import uuid
        return str(uuid.uuid4())
    
    def _check_user_permissions(self, user_id: str, context: Dict) -> bool:
        """檢查用戶權限"""
        # 實際應用中會檢查Azure AD權限
        return True
    
    def _check_content_policy(self, text: str) -> List[Dict]:
        """檢查內容政策"""
        violations = []
        
        # 檢查禁用詞彙
        prohibited_words = ["密碼", "機密", "internal"]
        for word in prohibited_words:
            if word.lower() in text.lower():
                violations.append({
                    "type": "PROHIBITED_CONTENT",
                    "details": f"包含禁用詞彙: {word}"
                })
        
        return violations

# 使用範例
async def main():
    config = {
        "monitor_connection_string": "InstrumentationKey=your-key",
        "debug": False
    }
    
    service = SecureAzureAIService(
        key_vault_url="https://your-vault.vault.azure.net/",
        config=config
    )
    
    result = await service.secure_ai_request(
        prompt="請幫我分析Q3季度的銷售報告，找出改進機會",
        user_id="user@company.com",
        request_context={
            "compliance_level": "HIGH",
            "business_domain": "SALES_ANALYSIS",
            "add_disclaimer": True
        }
    )
    
    print(f"AI回應: {result}")

if __name__ == "__main__":
    asyncio.run(main())
```

---

## 🤖 新興LLM提供商生態

### 🌟 Cohere：企業級RAG專家

> 💡 **白話解釋**  
> **什麼是RAG（檢索增強生成）？** 想像您是一個很聰明但記憶有限的顧問，客戶問您一個專業問題時，您會先去查閱最新的資料和文件，然後結合您的專業知識給出回答。RAG就是這個概念！它讓AI不只依賴訓練時的知識，還能即時查詢最新、最相關的資料，然後整合這些資訊給出更準確、更符合實際情況的回答。Cohere在這方面特別擅長，就像專業的研究助理一樣！

#### 🔍 Cohere RAG企業實施

<div style="background-color: #F1F8E9; padding: 20px; border-left: 4px solid #8BC34A; margin: 20px 0;">

**企業知識管理RAG系統**
```python
import cohere
import asyncio
from typing import List, Dict, Optional
import chromadb
from sentence_transformers import SentenceTransformer
import logging
from datetime import datetime
import json

class CohereEnterpriseRAG:
    """Cohere企業級RAG系統"""
    
    def __init__(self, cohere_api_key: str, knowledge_base_path: str):
        self.cohere_client = cohere.Client(cohere_api_key)
        self.embedding_model = SentenceTransformer('all-MiniLM-L6-v2')
        self.vector_db = chromadb.Client()
        self.collection = self.vector_db.create_collection("enterprise_knowledge")
        self.knowledge_base_path = knowledge_base_path
        self.setup_logging()
        
    def setup_logging(self):
        logging.basicConfig(level=logging.INFO)
        self.logger = logging.getLogger(__name__)
    
    async def ingest_documents(self, documents: List[Dict]):
        """攝取企業文檔到向量資料庫"""
        
        self.logger.info(f"開始攝取 {len(documents)} 個文檔")
        
        for doc in documents:
            try:
                # 文檔分割
                chunks = self._chunk_document(doc['content'])
                
                # 生成向量嵌入
                embeddings = self.embedding_model.encode(chunks)
                
                # 準備元數據
                metadatas = [
                    {
                        "doc_id": doc['id'],
                        "title": doc.get('title', ''),
                        "department": doc.get('department', ''),
                        "last_updated": doc.get('last_updated', ''),
                        "chunk_index": i,
                        "security_level": doc.get('security_level', 'public')
                    }
                    for i in range(len(chunks))
                ]
                
                # 儲存到向量資料庫
                self.collection.add(
                    embeddings=embeddings.tolist(),
                    documents=chunks,
                    metadatas=metadatas,
                    ids=[f"{doc['id']}_chunk_{i}" for i in range(len(chunks))]
                )
                
                self.logger.info(f"文檔 {doc['id']} 攝取完成，分割為 {len(chunks)} 個片段")
                
            except Exception as e:
                self.logger.error(f"文檔 {doc['id']} 攝取失敗: {str(e)}")
    
    def _chunk_document(self, content: str, chunk_size: int = 500) -> List[str]:
        """將文檔分割為較小的片段"""
        sentences = content.split('。')
        chunks = []
        current_chunk = ""
        
        for sentence in sentences:
            if len(current_chunk + sentence) < chunk_size:
                current_chunk += sentence + "。"
            else:
                if current_chunk:
                    chunks.append(current_chunk.strip())
                current_chunk = sentence + "。"
        
        if current_chunk:
            chunks.append(current_chunk.strip())
        
        return chunks
    
    async def rag_query(
        self, 
        query: str, 
        user_context: Dict,
        max_retrieved_docs: int = 5
    ) -> Dict:
        """執行RAG查詢"""
        
        start_time = datetime.now()
        
        try:
            # 1. 檢索相關文檔
            retrieved_docs = await self._retrieve_relevant_documents(
                query, user_context, max_retrieved_docs
            )
            
            if not retrieved_docs:
                return {
                    "response": "抱歉，我找不到相關的資訊來回答您的問題。",
                    "confidence": 0.0,
                    "sources": [],
                    "processing_time": (datetime.now() - start_time).total_seconds()
                }
            
            # 2. 建構增強提示
            enhanced_prompt = self._build_rag_prompt(query, retrieved_docs, user_context)
            
            # 3. 使用Cohere生成回應
            response = self.cohere_client.generate(
                model='command-xlarge',
                prompt=enhanced_prompt,
                max_tokens=500,
                temperature=0.3,
                k=0,
                stop_sequences=["--END--"],
                return_likelihoods='GENERATION'
            )
            
            # 4. 後處理和信心度評估
            confidence_score = self._calculate_confidence(
                response, retrieved_docs, query
            )
            
            # 5. 準備回應
            result = {
                "response": response.generations[0].text.strip(),
                "confidence": confidence_score,
                "sources": [
                    {
                        "doc_id": doc["metadata"]["doc_id"],
                        "title": doc["metadata"]["title"],
                        "department": doc["metadata"]["department"],
                        "relevance_score": doc["distance"],
                        "excerpt": doc["document"][:200] + "..."
                    }
                    for doc in retrieved_docs
                ],
                "processing_time": (datetime.now() - start_time).total_seconds(),
                "retrieved_count": len(retrieved_docs)
            }
            
            self.logger.info(f"RAG查詢完成，信心度: {confidence_score:.2f}")
            return result
            
        except Exception as e:
            self.logger.error(f"RAG查詢失敗: {str(e)}")
            return {
                "error": str(e),
                "processing_time": (datetime.now() - start_time).total_seconds()
            }
    
    async def _retrieve_relevant_documents(
        self, 
        query: str, 
        user_context: Dict,
        max_docs: int
    ) -> List[Dict]:
        """檢索相關文檔"""
        
        # 生成查詢向量
        query_embedding = self.embedding_model.encode([query])
        
        # 建構查詢過濾器
        where_filter = {}
        if user_context.get("department"):
            where_filter["department"] = user_context["department"]
        if user_context.get("security_level"):
            where_filter["security_level"] = {"$in": ["public", user_context["security_level"]]}
        
        # 執行向量相似性搜尋
        results = self.collection.query(
            query_embeddings=query_embedding.tolist(),
            n_results=max_docs,
            where=where_filter if where_filter else None
        )
        
        # 格式化結果
        retrieved_docs = []
        for i in range(len(results['documents'][0])):
            retrieved_docs.append({
                "document": results['documents'][0][i],
                "metadata": results['metadatas'][0][i],
                "distance": results['distances'][0][i]
            })
        
        return retrieved_docs
    
    def _build_rag_prompt(
        self, 
        query: str, 
        retrieved_docs: List[Dict],
        user_context: Dict
    ) -> str:
        """建構RAG提示"""
        
        context_docs = "\n\n".join([
            f"文檔來源: {doc['metadata']['title']} (部門: {doc['metadata']['department']})\n"
            f"內容: {doc['document']}"
            for doc in retrieved_docs
        ])
        
        prompt = f"""基於以下企業內部文檔回答問題：

相關文檔內容：
{context_docs}

用戶問題：{query}

用戶背景：
- 部門：{user_context.get('department', '未指定')}
- 權限等級：{user_context.get('security_level', 'standard')}

回答要求：
1. 請基於上述文檔內容回答問題
2. 如果文檔中沒有足夠資訊，請明確說明
3. 引用具體的文檔來源支撐您的回答
4. 提供實用的建議或下一步行動
5. 保持回答的準確性和客觀性

回答："""
        
        return prompt
    
    def _calculate_confidence(
        self, 
        response, 
        retrieved_docs: List[Dict], 
        query: str
    ) -> float:
        """計算回應信心度"""
        
        # 基於多個因素計算信心度
        factors = []
        
        # 1. 檢索文檔的相關性
        if retrieved_docs:
            avg_distance = sum(doc["distance"] for doc in retrieved_docs) / len(retrieved_docs)
            relevance_score = max(0, 1 - avg_distance)
            factors.append(relevance_score)
        
        # 2. 生成回應的似然度
        if hasattr(response, 'generations') and response.generations:
            # Cohere提供的生成似然度
            likelihood_score = 0.8  # 簡化計算
            factors.append(likelihood_score)
        
        # 3. 回應長度和完整性
        response_text = response.generations[0].text if response.generations else ""
        completeness_score = min(1.0, len(response_text) / 200)
        factors.append(completeness_score)
        
        # 加權平均
        if factors:
            return sum(factors) / len(factors)
        else:
            return 0.0

# 使用範例
async def main():
    # 初始化RAG系統
    rag_system = CohereEnterpriseRAG(
        cohere_api_key="your-cohere-api-key",
        knowledge_base_path="./enterprise_docs"
    )
    
    # 攝取企業文檔
    sample_documents = [
        {
            "id": "hr_001",
            "title": "員工手冊 - 請假政策",
            "content": "員工請假需要提前3天申請，病假需要醫生證明...",
            "department": "human_resources",
            "security_level": "internal",
            "last_updated": "2024-01-15"
        },
        {
            "id": "it_001", 
            "title": "資安政策指南",
            "content": "公司資安政策要求所有員工使用雙因子認證...",
            "department": "information_technology",
            "security_level": "confidential",
            "last_updated": "2024-02-01"
        }
    ]
    
    await rag_system.ingest_documents(sample_documents)
    
    # 執行RAG查詢
    result = await rag_system.rag_query(
        query="請假政策是什麼？需要什麼文件？",
        user_context={
            "department": "sales",
            "security_level": "internal"
        }
    )
    
    print(f"回答: {result['response']}")
    print(f"信心度: {result['confidence']:.2f}")
    print(f"參考來源: {len(result['sources'])} 個文檔")

if __name__ == "__main__":
    asyncio.run(main())
```

</div>

### 🤗 Hugging Face：開源創新引擎

#### 🛠️ 企業級Hugging Face Hub整合

**模型管理與部署平台**
```python
from transformers import (
    AutoTokenizer, AutoModelForCausalLM, 
    AutoModelForSequenceClassification, pipeline
)
from huggingface_hub import HfApi, Repository
import torch
import asyncio
from typing import Dict, List, Optional
import logging
from datetime import datetime
import json
import os

class HuggingFaceEnterpriseHub:
    """企業級Hugging Face模型管理平台"""
    
    def __init__(self, hf_token: str, organization: str):
        self.hf_token = hf_token
        self.organization = organization
        self.api = HfApi(token=hf_token)
        self.loaded_models = {}
        self.setup_logging()
        
    def setup_logging(self):
        logging.basicConfig(level=logging.INFO)
        self.logger = logging.getLogger(__name__)
    
    async def deploy_custom_model(
        self, 
        model_name: str,
        base_model: str,
        training_data: List[Dict],
        task_type: str = "text-generation"
    ) -> Dict:
        """部署客製化模型"""
        
        start_time = datetime.now()
        
        try:
            # 1. 載入基礎模型
            self.logger.info(f"載入基礎模型: {base_model}")
            
            tokenizer = AutoTokenizer.from_pretrained(base_model)
            if task_type == "text-generation":
                model = AutoModelForCausalLM.from_pretrained(
                    base_model,
                    torch_dtype=torch.float16,
                    device_map="auto"
                )
            elif task_type == "classification":
                model = AutoModelForSequenceClassification.from_pretrained(
                    base_model,
                    num_labels=len(set(item['label'] for item in training_data))
                )
            
            # 2. 準備訓練資料
            processed_data = self._prepare_training_data(
                training_data, tokenizer, task_type
            )
            
            # 3. 微調模型
            fine_tuned_model = await self._fine_tune_model(
                model, tokenizer, processed_data, task_type
            )
            
            # 4. 評估模型效能
            evaluation_results = await self._evaluate_model(
                fine_tuned_model, tokenizer, processed_data
            )
            
            # 5. 上傳到Hugging Face Hub
            model_id = f"{self.organization}/{model_name}"
            self._upload_to_hub(fine_tuned_model, tokenizer, model_id)
            
            # 6. 註冊到企業模型庫
            registration_info = {
                "model_id": model_id,
                "base_model": base_model,
                "task_type": task_type,
                "training_samples": len(training_data),
                "evaluation_results": evaluation_results,
                "created_at": datetime.now().isoformat(),
                "created_by": self.organization
            }
            
            self._register_enterprise_model(registration_info)
            
            deployment_time = (datetime.now() - start_time).total_seconds()
            
            return {
                "success": True,
                "model_id": model_id,
                "evaluation_results": evaluation_results,
                "deployment_time": deployment_time,
                "model_size_mb": self._calculate_model_size(fine_tuned_model),
                "next_steps": [
                    "模型已部署到企業Hub",
                    "可通過API進行推理",
                    "建議進行A/B測試驗證效果"
                ]
            }
            
        except Exception as e:
            self.logger.error(f"模型部署失敗: {str(e)}")
            return {
                "success": False,
                "error": str(e),
                "deployment_time": (datetime.now() - start_time).total_seconds()
            }
    
    def _prepare_training_data(
        self, 
        data: List[Dict], 
        tokenizer, 
        task_type: str
    ) -> Dict:
        """準備訓練資料"""
        
        if task_type == "text-generation":
            # 準備生成任務資料
            texts = []
            for item in data:
                formatted_text = f"問題: {item['input']}\n回答: {item['output']}<|endoftext|>"
                texts.append(formatted_text)
            
            encodings = tokenizer(
                texts,
                truncation=True,
                padding=True,
                max_length=512,
                return_tensors="pt"
            )
            
            return {
                "input_ids": encodings["input_ids"],
                "attention_mask": encodings["attention_mask"],
                "labels": encodings["input_ids"].clone()
            }
            
        elif task_type == "classification":
            # 準備分類任務資料
            texts = [item['text'] for item in data]
            labels = [item['label'] for item in data]
            
            # 建立標籤對映
            unique_labels = list(set(labels))
            label_to_id = {label: idx for idx, label in enumerate(unique_labels)}
            label_ids = [label_to_id[label] for label in labels]
            
            encodings = tokenizer(
                texts,
                truncation=True,
                padding=True,
                max_length=512,
                return_tensors="pt"
            )
            
            return {
                "input_ids": encodings["input_ids"],
                "attention_mask": encodings["attention_mask"],
                "labels": torch.tensor(label_ids),
                "label_mapping": label_to_id
            }
    
    async def _fine_tune_model(
        self, 
        model, 
        tokenizer, 
        data: Dict, 
        task_type: str
    ):
        """微調模型"""
        
        from transformers import Trainer, TrainingArguments
        from torch.utils.data import Dataset
        
        class CustomDataset(Dataset):
            def __init__(self, encodings):
                self.encodings = encodings
            
            def __getitem__(self, idx):
                return {key: val[idx] for key, val in self.encodings.items()}
            
            def __len__(self):
                return len(self.encodings['input_ids'])
        
        # 建立資料集
        dataset = CustomDataset(data)
        
        # 設定訓練參數
        training_args = TrainingArguments(
            output_dir='./tmp_trainer',
            num_train_epochs=3,
            per_device_train_batch_size=4,
            per_device_eval_batch_size=4,
            warmup_steps=100,
            weight_decay=0.01,
            logging_dir='./logs',
            logging_steps=50,
            save_steps=500,
            evaluation_strategy="steps",
            eval_steps=500,
            load_best_model_at_end=True,
        )
        
        # 建立訓練器
        trainer = Trainer(
            model=model,
            args=training_args,
            train_dataset=dataset,
            eval_dataset=dataset,  # 實際應用中應該分離驗證集
            tokenizer=tokenizer,
        )
        
        # 開始微調
        self.logger.info("開始模型微調...")
        trainer.train()
        
        return model
    
    async def _evaluate_model(self, model, tokenizer, data: Dict) -> Dict:
        """評估模型效能"""
        
        evaluation_results = {
            "accuracy": 0.0,
            "loss": 0.0,
            "inference_time_ms": 0.0,
            "sample_predictions": []
        }
        
        model.eval()
        with torch.no_grad():
            # 簡化的評估過程
            sample_inputs = data["input_ids"][:5]  # 取5個樣本
            
            start_time = datetime.now()
            outputs = model(sample_inputs)
            inference_time = (datetime.now() - start_time).total_seconds() * 1000
            
            evaluation_results["inference_time_ms"] = inference_time / len(sample_inputs)
        
        return evaluation_results
    
    def _upload_to_hub(self, model, tokenizer, model_id: str):
        """上傳模型到Hugging Face Hub"""
        
        try:
            # 上傳模型
            model.push_to_hub(model_id, token=self.hf_token)
            tokenizer.push_to_hub(model_id, token=self.hf_token)
            
            # 建立模型卡片
            model_card = f"""
---
language: 
- zh
- en
library_name: transformers
pipeline_tag: text-generation
tags:
- enterprise
- custom-trained
- {self.organization}
---

# {model_id}

這是 {self.organization} 組織訓練的企業級模型。

## 使用方式

```python
from transformers import AutoTokenizer, AutoModelForCausalLM

tokenizer = AutoTokenizer.from_pretrained("{model_id}")
model = AutoModelForCausalLM.from_pretrained("{model_id}")

# 生成文本
inputs = tokenizer("您的提示", return_tensors="pt")
outputs = model.generate(**inputs, max_length=100)
response = tokenizer.decode(outputs[0], skip_special_tokens=True)
```

## 注意事項

- 此模型專為企業內部使用而設計
- 請遵循公司的AI使用政策
- 如有問題請聯繫IT部門
            """
            
            # 上傳模型卡片
            self.api.upload_file(
                path_or_fileobj=model_card.encode(),
                path_in_repo="README.md",
                repo_id=model_id,
                token=self.hf_token
            )
            
            self.logger.info(f"模型成功上傳到Hub: {model_id}")
            
        except Exception as e:
            self.logger.error(f"模型上傳失敗: {str(e)}")
            raise
    
    def _register_enterprise_model(self, registration_info: Dict):
        """註冊到企業模型庫"""
        
        # 寫入企業模型註冊表
        registry_file = "enterprise_model_registry.json"
        
        if os.path.exists(registry_file):
            with open(registry_file, 'r', encoding='utf-8') as f:
                registry = json.load(f)
        else:
            registry = {"models": []}
        
        registry["models"].append(registration_info)
        
        with open(registry_file, 'w', encoding='utf-8') as f:
            json.dump(registry, f, ensure_ascii=False, indent=2)
        
        self.logger.info(f"模型已註冊到企業庫: {registration_info['model_id']}")
    
    def _calculate_model_size(self, model) -> float:
        """計算模型大小（MB）"""
        
        param_count = sum(p.numel() for p in model.parameters())
        # 假設每個參數4 bytes (float32)
        size_mb = param_count * 4 / (1024 * 1024)
        return round(size_mb, 2)
    
    async def create_inference_pipeline(
        self, 
        model_id: str, 
        task_type: str = "text-generation"
    ):
        """建立推理管道"""
        
        if model_id not in self.loaded_models:
            self.logger.info(f"載入模型: {model_id}")
            
            pipe = pipeline(
                task_type,
                model=model_id,
                tokenizer=model_id,
                torch_dtype=torch.float16,
                device_map="auto"
            )
            
            self.loaded_models[model_id] = pipe
        
        return self.loaded_models[model_id]
    
    async def batch_inference(
        self, 
        model_id: str, 
        inputs: List[str],
        **generation_kwargs
    ) -> List[Dict]:
        """批量推理"""
        
        pipe = await self.create_inference_pipeline(model_id)
        
        results = []
        for input_text in inputs:
            try:
                output = pipe(input_text, **generation_kwargs)
                results.append({
                    "input": input_text,
                    "output": output,
                    "success": True
                })
            except Exception as e:
                results.append({
                    "input": input_text,
                    "error": str(e),
                    "success": False
                })
        
        return results

# 使用範例
async def main():
    # 初始化企業Hub
    enterprise_hub = HuggingFaceEnterpriseHub(
        hf_token="your-huggingface-token",
        organization="your-company"
    )
    
    # 準備訓練資料
    training_data = [
        {
            "input": "如何申請年假？",
            "output": "請登入人事系統填寫年假申請表，並提前7天提交給直屬主管審核。"
        },
        {
            "input": "公司的加班政策是什麼？",
            "output": "加班需要事前申請，超過晚上9點的加班需要部門主管特別批准。"
        }
        # 更多訓練資料...
    ]
    
    # 部署客製化模型
    deployment_result = await enterprise_hub.deploy_custom_model(
        model_name="company-hr-assistant",
        base_model="microsoft/DialoGPT-medium",
        training_data=training_data,
        task_type="text-generation"
    )
    
    if deployment_result["success"]:
        print(f"模型部署成功: {deployment_result['model_id']}")
        
        # 測試批量推理
        test_inputs = [
            "請假流程是什麼？",
            "如何申請出差？"
        ]
        
        inference_results = await enterprise_hub.batch_inference(
            model_id=deployment_result['model_id'],
            inputs=test_inputs,
            max_length=100,
            temperature=0.7
        )
        
        for result in inference_results:
            print(f"問題: {result['input']}")
            print(f"回答: {result['output']}")
            print("---")

if __name__ == "__main__":
    asyncio.run(main())
```

---

## 🔗 跨平台整合架構

### 🎯 智能編排系統

> 💡 **白話解釋**  
> **什麼是AI編排系統？** 想像您是一個大型醫院的總指揮，有心臟科、腦科、骨科等不同專科醫生。當病人來看病時，您需要根據病情決定派哪個醫生最合適，有時候還需要多個專科醫生會診。AI編排系統就是這樣的「總指揮」，它會根據用戶的問題類型、複雜度、成本考量等因素，自動決定要用哪個AI平台，或是讓多個AI平台一起工作，確保每個問題都能得到最適合、最高品質的回答！

<div style="background-color: #E1F5FE; padding: 20px; border-left: 4px solid #03A9F4; margin: 20px 0;">

**企業級AI編排系統架構**
```python
import asyncio
import logging
from typing import Dict, List, Optional, Any
from dataclasses import dataclass
from enum import Enum
import json
from datetime import datetime
import aiohttp
import hashlib

class TaskComplexity(Enum):
    SIMPLE = "simple"
    MEDIUM = "medium"
    COMPLEX = "complex"
    CRITICAL = "critical"

class ProviderCapability(Enum):
    TEXT_GENERATION = "text_generation"
    CODE_GENERATION = "code_generation"
    ANALYSIS = "analysis"
    CREATIVE_WRITING = "creative_writing"
    MULTILINGUAL = "multilingual"
    MULTIMODAL = "multimodal"
    RAG = "rag"

@dataclass
class TaskRequest:
    task_id: str
    user_id: str
    prompt: str
    task_type: str
    complexity: TaskComplexity
    requirements: Dict[str, Any]
    constraints: Dict[str, Any]
    context: Dict[str, Any]

class AIProviderOrchestrator:
    """AI提供商智能編排系統"""
    
    def __init__(self, config: Dict):
        self.config = config
        self.providers = self._initialize_providers()
        self.routing_rules = self._load_routing_rules()
        self.performance_metrics = {}
        self.setup_logging()
        
    def setup_logging(self):
        logging.basicConfig(level=logging.INFO)
        self.logger = logging.getLogger(__name__)
    
    def _initialize_providers(self) -> Dict:
        """初始化所有AI提供商"""
        
        providers = {
            "openai": {
                "capabilities": [
                    ProviderCapability.TEXT_GENERATION,
                    ProviderCapability.CODE_GENERATION,
                    ProviderCapability.CREATIVE_WRITING
                ],
                "cost_per_1k_tokens": 0.002,
                "avg_response_time": 1.2,
                "reliability_score": 0.98,
                "max_context_length": 16000,
                "supported_languages": ["en", "zh", "ja", "ko"],
                "api_endpoint": self.config.get("openai_endpoint"),
                "api_key": self.config.get("openai_api_key")
            },
            
            "anthropic": {
                "capabilities": [
                    ProviderCapability.TEXT_GENERATION,
                    ProviderCapability.ANALYSIS,
                    ProviderCapability.CODE_GENERATION
                ],
                "cost_per_1k_tokens": 0.008,
                "avg_response_time": 1.8,
                "reliability_score": 0.99,
                "max_context_length": 100000,
                "supported_languages": ["en", "zh"],
                "api_endpoint": self.config.get("anthropic_endpoint"),
                "api_key": self.config.get("anthropic_api_key")
            },
            
            "cohere": {
                "capabilities": [
                    ProviderCapability.TEXT_GENERATION,
                    ProviderCapability.MULTILINGUAL,
                    ProviderCapability.RAG
                ],
                "cost_per_1k_tokens": 0.001,
                "avg_response_time": 0.9,
                "reliability_score": 0.96,
                "max_context_length": 4000,
                "supported_languages": ["en", "zh", "ja", "ko", "es", "fr"],
                "api_endpoint": self.config.get("cohere_endpoint"),
                "api_key": self.config.get("cohere_api_key")
            },
            
            "azure": {
                "capabilities": [
                    ProviderCapability.TEXT_GENERATION,
                    ProviderCapability.MULTIMODAL,
                    ProviderCapability.ANALYSIS
                ],
                "cost_per_1k_tokens": 0.002,
                "avg_response_time": 1.5,
                "reliability_score": 0.99,
                "max_context_length": 32000,
                "supported_languages": ["en", "zh"],
                "api_endpoint": self.config.get("azure_endpoint"),
                "api_key": self.config.get("azure_api_key"),
                "compliance_level": "enterprise"
            },
            
            "llama": {
                "capabilities": [
                    ProviderCapability.TEXT_GENERATION,
                    ProviderCapability.CODE_GENERATION,
                    ProviderCapability.MULTILINGUAL
                ],
                "cost_per_1k_tokens": 0.0,  # 自建無API費用
                "avg_response_time": 2.1,
                "reliability_score": 0.94,
                "max_context_length": 8000,
                "supported_languages": ["en", "zh", "es", "fr"],
                "api_endpoint": self.config.get("llama_endpoint"),
                "deployment_type": "self_hosted"
            }
        }
        
        return providers
    
    def _load_routing_rules(self) -> Dict:
        """載入路由規則"""
        
        return {
            "creative_writing": {
                "preferred_providers": ["openai", "anthropic"],
                "fallback_providers": ["cohere", "llama"],
                "min_quality_score": 0.8
            },
            
            "business_analysis": {
                "preferred_providers": ["anthropic", "azure"],
                "fallback_providers": ["openai", "cohere"],
                "min_quality_score": 0.9,
                "requires_compliance": True
            },
            
            "code_generation": {
                "preferred_providers": ["openai", "llama"],
                "fallback_providers": ["anthropic"],
                "min_quality_score": 0.85
            },
            
            "multilingual_tasks": {
                "preferred_providers": ["cohere", "llama"],
                "fallback_providers": ["openai"],
                "min_quality_score": 0.8
            },
            
            "cost_sensitive": {
                "preferred_providers": ["llama", "cohere"],
                "fallback_providers": ["openai"],
                "max_cost_per_request": 0.01
            },
            
            "high_reliability": {
                "preferred_providers": ["azure", "anthropic"],
                "fallback_providers": ["openai"],
                "min_reliability_score": 0.98
            }
        }
    
    async def route_request(self, request: TaskRequest) -> Dict:
        """智能路由請求到最適合的提供商"""
        
        start_time = datetime.now()
        
        try:
            # 1. 分析請求特性
            request_analysis = self._analyze_request(request)
            
            # 2. 選擇候選提供商
            candidates = self._select_candidates(request, request_analysis)
            
            # 3. 評估和排序候選者
            ranked_providers = self._rank_providers(candidates, request, request_analysis)
            
            # 4. 執行請求（含故障轉移）
            result = await self._execute_with_fallback(request, ranked_providers)
            
            # 5. 記錄效能指標
            self._update_performance_metrics(
                result["provider_used"], 
                request, 
                result["response_time"],
                result["success"]
            )
            
            processing_time = (datetime.now() - start_time).total_seconds()
            
            return {
                **result,
                "routing_time": processing_time,
                "request_analysis": request_analysis,
                "considered_providers": [p["name"] for p in ranked_providers]
            }
            
        except Exception as e:
            self.logger.error(f"請求路由失敗: {str(e)}")
            return {
                "success": False,
                "error": str(e),
                "routing_time": (datetime.now() - start_time).total_seconds()
            }
    
    def _analyze_request(self, request: TaskRequest) -> Dict:
        """分析請求特性"""
        
        analysis = {
            "estimated_tokens": self._estimate_token_count(request.prompt),
            "language": self._detect_language(request.prompt),
            "domain": self._detect_domain(request.prompt),
            "sensitivity_level": self._assess_sensitivity(request.prompt),
            "required_capabilities": self._identify_capabilities(request),
            "performance_requirements": self._extract_performance_requirements(request)
        }
        
        return analysis
    
    def _select_candidates(self, request: TaskRequest, analysis: Dict) -> List[str]:
        """選擇候選提供商"""
        
        candidates = []
        
        # 基於任務類型選擇
        if request.task_type in self.routing_rules:
            rule = self.routing_rules[request.task_type]
            candidates.extend(rule["preferred_providers"])
        
        # 基於能力需求過濾
        for capability in analysis["required_capabilities"]:
            capability_providers = [
                name for name, info in self.providers.items()
                if capability in info["capabilities"]
            ]
            candidates = list(set(candidates) & set(capability_providers))
        
        # 基於約束條件過濾
        filtered_candidates = []
        for provider_name in candidates:
            provider = self.providers[provider_name]
            
            # 檢查語言支援
            if analysis["language"] not in provider["supported_languages"]:
                continue
            
            # 檢查合規要求
            if (request.constraints.get("requires_compliance") and 
                provider.get("compliance_level") != "enterprise"):
                continue
            
            # 檢查成本限制
            max_cost = request.constraints.get("max_cost_per_request", float('inf'))
            estimated_cost = self._estimate_cost(provider, analysis["estimated_tokens"])
            if estimated_cost > max_cost:
                continue
            
            filtered_candidates.append(provider_name)
        
        return filtered_candidates
    
    def _rank_providers(
        self, 
        candidates: List[str], 
        request: TaskRequest, 
        analysis: Dict
    ) -> List[Dict]:
        """評估和排序提供商"""
        
        ranked = []
        
        for provider_name in candidates:
            provider = self.providers[provider_name]
            
            score = self._calculate_provider_score(
                provider, request, analysis
            )
            
            ranked.append({
                "name": provider_name,
                "info": provider,
                "score": score,
                "estimated_cost": self._estimate_cost(provider, analysis["estimated_tokens"]),
                "estimated_time": provider["avg_response_time"]
            })
        
        # 按分數排序
        ranked.sort(key=lambda x: x["score"], reverse=True)
        
        return ranked
    
    def _calculate_provider_score(
        self, 
        provider: Dict, 
        request: TaskRequest, 
        analysis: Dict
    ) -> float:
        """計算提供商評分"""
        
        score = 0.0
        
        # 可靠性權重 (30%)
        score += provider["reliability_score"] * 0.3
        
        # 效能權重 (25%)
        performance_score = 1.0 / (1.0 + provider["avg_response_time"])
        score += performance_score * 0.25
        
        # 成本效益權重 (20%)
        estimated_cost = self._estimate_cost(provider, analysis["estimated_tokens"])
        if estimated_cost == 0:
            cost_score = 1.0
        else:
            cost_score = 1.0 / (1.0 + estimated_cost)
        score += cost_score * 0.20
        
        # 能力匹配權重 (15%)
        capability_match = len(
            set(analysis["required_capabilities"]) & 
            set(provider["capabilities"])
        ) / len(analysis["required_capabilities"])
        score += capability_match * 0.15
        
        # 歷史效能權重 (10%)
        historical_score = self.performance_metrics.get(
            provider.get("name", "unknown"), {}
        ).get("avg_quality_score", 0.8)
        score += historical_score * 0.10
        
        return score
    
    async def _execute_with_fallback(
        self, 
        request: TaskRequest, 
        ranked_providers: List[Dict]
    ) -> Dict:
        """執行請求並支援故障轉移"""
        
        last_error = None
        
        for provider_info in ranked_providers:
            try:
                provider_name = provider_info["name"]
                self.logger.info(f"嘗試使用提供商: {provider_name}")
                
                result = await self._execute_request(request, provider_info)
                
                if result["success"]:
                    result["provider_used"] = provider_name
                    result["fallback_count"] = ranked_providers.index(provider_info)
                    return result
                else:
                    last_error = result.get("error", "未知錯誤")
                    self.logger.warning(f"提供商 {provider_name} 請求失敗: {last_error}")
                    
            except Exception as e:
                last_error = str(e)
                self.logger.error(f"提供商 {provider_name} 執行異常: {last_error}")
        
        # 所有提供商都失敗
        return {
            "success": False,
            "error": f"所有提供商都失敗，最後錯誤: {last_error}",
            "provider_used": None,
            "fallback_count": len(ranked_providers)
        }
    
    async def _execute_request(self, request: TaskRequest, provider_info: Dict) -> Dict:
        """執行具體的AI請求"""
        
        provider_name = provider_info["name"]
        provider = provider_info["info"]
        
        start_time = datetime.now()
        
        # 建構提供商特定的請求
        api_request = self._build_provider_request(request, provider_name)
        
        # 執行HTTP請求
        async with aiohttp.ClientSession() as session:
            headers = {
                "Authorization": f"Bearer {provider['api_key']}",
                "Content-Type": "application/json"
            }
            
            async with session.post(
                provider["api_endpoint"],
                headers=headers,
                json=api_request,
                timeout=aiohttp.ClientTimeout(total=30)
            ) as response:
                
                response_time = (datetime.now() - start_time).total_seconds()
                
                if response.status == 200:
                    result = await response.json()
                    processed_result = self._process_provider_response(
                        result, provider_name
                    )
                    
                    return {
                        "success": True,
                        "response": processed_result,
                        "response_time": response_time,
                        "raw_response": result
                    }
                else:
                    error_text = await response.text()
                    return {
                        "success": False,
                        "error": f"HTTP {response.status}: {error_text}",
                        "response_time": response_time
                    }
    
    def _build_provider_request(self, request: TaskRequest, provider_name: str) -> Dict:
        """建構提供商特定的請求格式"""
        
        if provider_name == "openai":
            return {
                "model": "gpt-4",
                "messages": [
                    {"role": "system", "content": "You are a helpful assistant."},
                    {"role": "user", "content": request.prompt}
                ],
                "temperature": request.requirements.get("temperature", 0.7),
                "max_tokens": request.requirements.get("max_tokens", 500)
            }
        
        elif provider_name == "anthropic":
            return {
                "model": "claude-3-sonnet-20240229",
                "max_tokens": request.requirements.get("max_tokens", 500),
                "temperature": request.requirements.get("temperature", 0.7),
                "messages": [
                    {"role": "user", "content": request.prompt}
                ]
            }
        
        elif provider_name == "cohere":
            return {
                "model": "command-xlarge",
                "prompt": request.prompt,
                "max_tokens": request.requirements.get("max_tokens", 500),
                "temperature": request.requirements.get("temperature", 0.7)
            }
        
        # 其他提供商的格式...
        return {"prompt": request.prompt}
    
    def _process_provider_response(self, response: Dict, provider_name: str) -> str:
        """處理提供商回應格式"""
        
        if provider_name == "openai":
            return response["choices"][0]["message"]["content"]
        
        elif provider_name == "anthropic":
            return response["content"][0]["text"]
        
        elif provider_name == "cohere":
            return response["generations"][0]["text"]
        
        # 預設處理
        return str(response)
    
    def _estimate_token_count(self, text: str) -> int:
        """估計Token數量"""
        # 簡化估計：1個中文字符約等於1.5個token，英文單字約1個token
        chinese_chars = len([c for c in text if '\u4e00' <= c <= '\u9fff'])
        english_words = len([w for w in text.split() if w.isalpha()])
        return int(chinese_chars * 1.5 + english_words)
    
    def _detect_language(self, text: str) -> str:
        """檢測文本語言"""
        chinese_chars = len([c for c in text if '\u4e00' <= c <= '\u9fff'])
        if chinese_chars > len(text) * 0.3:
            return "zh"
        return "en"
    
    def _detect_domain(self, text: str) -> str:
        """檢測領域類型"""
        # 簡化的領域檢測
        keywords = {
            "business": ["營收", "利潤", "市場", "客戶", "銷售"],
            "technology": ["程式", "系統", "API", "資料庫", "演算法"],
            "healthcare": ["醫療", "病患", "治療", "診斷", "藥物"],
            "education": ["學習", "教育", "課程", "學生", "教學"]
        }
        
        for domain, words in keywords.items():
            if any(word in text for word in words):
                return domain
        
        return "general"
    
    def _assess_sensitivity(self, text: str) -> str:
        """評估敏感度等級"""
        sensitive_keywords = ["機密", "內部", "私人", "敏感", "confidential"]
        
        if any(keyword in text.lower() for keyword in sensitive_keywords):
            return "high"
        
        return "normal"
    
    def _identify_capabilities(self, request: TaskRequest) -> List[ProviderCapability]:
        """識別所需能力"""
        capabilities = []
        
        if request.task_type == "code_generation":
            capabilities.append(ProviderCapability.CODE_GENERATION)
        elif request.task_type == "creative_writing":
            capabilities.append(ProviderCapability.CREATIVE_WRITING)
        elif request.task_type == "analysis":
            capabilities.append(ProviderCapability.ANALYSIS)
        else:
            capabilities.append(ProviderCapability.TEXT_GENERATION)
        
        # 檢查多語言需求
        if self._detect_language(request.prompt) != "en":
            capabilities.append(ProviderCapability.MULTILINGUAL)
        
        return capabilities
    
    def _extract_performance_requirements(self, request: TaskRequest) -> Dict:
        """提取效能需求"""
        return {
            "max_response_time": request.constraints.get("max_response_time", 10.0),
            "min_quality_score": request.constraints.get("min_quality_score", 0.8),
            "max_cost": request.constraints.get("max_cost_per_request", 0.1)
        }
    
    def _estimate_cost(self, provider: Dict, token_count: int) -> float:
        """估計請求成本"""
        cost_per_1k = provider.get("cost_per_1k_tokens", 0.0)
        return (token_count / 1000) * cost_per_1k
    
    def _update_performance_metrics(
        self, 
        provider_name: str, 
        request: TaskRequest,
        response_time: float,
        success: bool
    ):
        """更新效能指標"""
        
        if provider_name not in self.performance_metrics:
            self.performance_metrics[provider_name] = {
                "total_requests": 0,
                "successful_requests": 0,
                "total_response_time": 0.0,
                "avg_quality_score": 0.8
            }
        
        metrics = self.performance_metrics[provider_name]
        metrics["total_requests"] += 1
        metrics["total_response_time"] += response_time
        
        if success:
            metrics["successful_requests"] += 1
        
        # 計算平均回應時間
        metrics["avg_response_time"] = (
            metrics["total_response_time"] / metrics["total_requests"]
        )
        
        # 計算成功率
        metrics["success_rate"] = (
            metrics["successful_requests"] / metrics["total_requests"]
        )

# 使用範例
async def main():
    config = {
        "openai_endpoint": "https://api.openai.com/v1/chat/completions",
        "openai_api_key": "your-openai-key",
        "anthropic_endpoint": "https://api.anthropic.com/v1/messages",
        "anthropic_api_key": "your-anthropic-key",
        "cohere_endpoint": "https://api.cohere.ai/v1/generate",
        "cohere_api_key": "your-cohere-key"
    }
    
    orchestrator = AIProviderOrchestrator(config)
    
    # 建立測試請求
    request = TaskRequest(
        task_id="test_001",
        user_id="user@company.com",
        prompt="請分析我們公司Q3季度的銷售數據，並提供改善建議",
        task_type="business_analysis",
        complexity=TaskComplexity.MEDIUM,
        requirements={
            "temperature": 0.3,
            "max_tokens": 800
        },
        constraints={
            "requires_compliance": True,
            "max_cost_per_request": 0.05,
            "max_response_time": 5.0
        },
        context={
            "user_department": "sales",
            "user_role": "manager"
        }
    )
    
    # 執行智能路由
    result = await orchestrator.route_request(request)
    
    if result["success"]:
        print(f"使用提供商: {result['provider_used']}")
        print(f"回應時間: {result['response_time']:.2f}秒")
        print(f"回應內容: {result['response']}")
        print(f"考慮的提供商: {result['considered_providers']}")
    else:
        print(f"請求失敗: {result['error']}")

if __name__ == "__main__":
    asyncio.run(main())
```

</div>

---

## 📊 成本效益分析與優化

### 💰 多平台成本模型

> 💡 **白話解釋**  
> **為什麼要做成本分析？** 就像您要開一家餐廳，不能只考慮菜好不好吃，還要考慮食材成本、人工成本、租金等等，才能訂出合理的價格並確保獲利。使用AI也是一樣，不同的AI平台就像不同等級的廚師，能力好的比較貴，但不是每道菜都需要米其林主廚來做。成本分析幫助您找到「性價比最高」的AI組合，讓您在預算內獲得最好的效果！

#### 🔍 動態成本優化系統

<div style="background-color: #FFF8E1; padding: 20px; border-left: 4px solid #FFC107; margin: 20px 0;">

**企業級成本分析與優化平台**
```python
import asyncio
import json
from typing import Dict, List, Optional, Tuple
from dataclasses import dataclass, asdict
from datetime import datetime, timedelta
import matplotlib.pyplot as plt
import pandas as pd
from statistics import mean, median
import logging

@dataclass
class CostModel:
    provider: str
    model: str
    input_cost_per_1k: float
    output_cost_per_1k: float
    setup_cost: float = 0.0
    monthly_minimum: float = 0.0
    free_tier_tokens: int = 0
    additional_fees: Dict[str, float] = None

@dataclass
class UsagePattern:
    pattern_name: str
    daily_requests: int
    avg_input_tokens: int
    avg_output_tokens: int
    peak_hours: List[int]
    seasonal_multiplier: Dict[str, float]
    quality_requirements: Dict[str, float]

class EnterpriseCostOptimizer:
    """企業級AI成本優化系統"""
    
    def __init__(self):
        self.cost_models = self._initialize_cost_models()
        self.usage_history = []
        self.optimization_rules = self._load_optimization_rules()
        self.setup_logging()
    
    def setup_logging(self):
        logging.basicConfig(level=logging.INFO)
        self.logger = logging.getLogger(__name__)
    
    def _initialize_cost_models(self) -> Dict[str, List[CostModel]]:
        """初始化成本模型"""
        
        return {
            "openai": [
                CostModel(
                    provider="openai",
                    model="gpt-4",
                    input_cost_per_1k=0.03,
                    output_cost_per_1k=0.06,
                    free_tier_tokens=0
                ),
                CostModel(
                    provider="openai",
                    model="gpt-3.5-turbo",
                    input_cost_per_1k=0.001,
                    output_cost_per_1k=0.002,
                    free_tier_tokens=0
                )
            ],
            
            "anthropic": [
                CostModel(
                    provider="anthropic",
                    model="claude-3-opus",
                    input_cost_per_1k=0.015,
                    output_cost_per_1k=0.075,
                    free_tier_tokens=0
                ),
                CostModel(
                    provider="anthropic",
                    model="claude-3-sonnet",
                    input_cost_per_1k=0.003,
                    output_cost_per_1k=0.015,
                    free_tier_tokens=0
                )
            ],
            
            "cohere": [
                CostModel(
                    provider="cohere",
                    model="command",
                    input_cost_per_1k=0.001,
                    output_cost_per_1k=0.002,
                    free_tier_tokens=5000000  # 每月5M free tokens
                )
            ],
            
            "azure": [
                CostModel(
                    provider="azure",
                    model="gpt-4",
                    input_cost_per_1k=0.03,
                    output_cost_per_1k=0.06,
                    setup_cost=100.0,  # 部署成本
                    monthly_minimum=50.0,  # 最低月費
                    additional_fees={"enterprise_support": 500.0}
                )
            ],
            
            "llama_self_hosted": [
                CostModel(
                    provider="llama",
                    model="llama-2-70b",
                    input_cost_per_1k=0.0,  # 無API費用
                    output_cost_per_1k=0.0,
                    setup_cost=2000.0,  # 硬體和設置成本
                    monthly_minimum=800.0,  # GPU租用費用
                    additional_fees={"maintenance": 200.0, "electricity": 150.0}
                )
            ]
        }
    
    def _load_optimization_rules(self) -> Dict:
        """載入優化規則"""
        
        return {
            "cost_thresholds": {
                "low_usage": 100.0,      # 月費用 < $100
                "medium_usage": 1000.0,  # $100 <= 月費用 < $1000
                "high_usage": 5000.0     # $1000 <= 月費用 < $5000
            },
            
            "optimization_strategies": {
                "low_usage": {
                    "preferred_providers": ["cohere", "openai:gpt-3.5-turbo"],
                    "avoid_setup_costs": True,
                    "maximize_free_tiers": True
                },
                
                "medium_usage": {
                    "preferred_providers": ["openai", "anthropic:claude-3-sonnet"],
                    "consider_volume_discounts": True,
                    "balance_cost_quality": True
                },
                
                "high_usage": {
                    "preferred_providers": ["azure", "llama_self_hosted"],
                    "setup_costs_acceptable": True,
                    "focus_on_unit_cost": True,
                    "consider_dedicated_instances": True
                }
            },
            
            "task_routing": {
                "simple_tasks": ["cohere", "openai:gpt-3.5-turbo"],
                "complex_analysis": ["anthropic:claude-3-opus", "openai:gpt-4"],
                "high_volume_generation": ["llama_self_hosted", "azure"],
                "enterprise_critical": ["azure", "anthropic"]
            }
        }
    
    async def analyze_usage_patterns(
        self, 
        historical_data: List[Dict]
    ) -> Dict[str, UsagePattern]:
        """分析使用模式"""
        
        patterns = {}
        
        # 按任務類型分組分析
        task_groups = {}
        for record in historical_data:
            task_type = record.get("task_type", "general")
            if task_type not in task_groups:
                task_groups[task_type] = []
            task_groups[task_type].append(record)
        
        for task_type, records in task_groups.items():
            # 計算基本統計
            daily_requests = len(records) / 30  # 假設30天數據
            input_tokens = [r.get("input_tokens", 0) for r in records]
            output_tokens = [r.get("output_tokens", 0) for r in records]
            
            # 分析時間模式
            hours = [datetime.fromisoformat(r["timestamp"]).hour for r in records]
            peak_hours = self._identify_peak_hours(hours)
            
            # 季節性分析（簡化）
            seasonal_multiplier = {
                "Q1": 1.0, "Q2": 1.2, "Q3": 0.8, "Q4": 1.5
            }
            
            # 品質要求分析
            quality_scores = [r.get("quality_score", 0.8) for r in records if r.get("quality_score")]
            
            patterns[task_type] = UsagePattern(
                pattern_name=task_type,
                daily_requests=int(daily_requests),
                avg_input_tokens=int(mean(input_tokens)) if input_tokens else 0,
                avg_output_tokens=int(mean(output_tokens)) if output_tokens else 0,
                peak_hours=peak_hours,
                seasonal_multiplier=seasonal_multiplier,
                quality_requirements={
                    "min_quality": min(quality_scores) if quality_scores else 0.7,
                    "avg_quality": mean(quality_scores) if quality_scores else 0.8,
                    "target_quality": 0.9
                }
            )
        
        return patterns
    
    def _identify_peak_hours(self, hours: List[int]) -> List[int]:
        """識別尖峰時段"""
        
        hour_counts = {}
        for hour in hours:
            hour_counts[hour] = hour_counts.get(hour, 0) + 1
        
        # 找出使用量最高的時段
        avg_count = mean(hour_counts.values())
        peak_hours = [hour for hour, count in hour_counts.items() 
                     if count > avg_count * 1.5]
        
        return sorted(peak_hours)
    
    async def calculate_monthly_costs(
        self, 
        usage_patterns: Dict[str, UsagePattern]
    ) -> Dict:
        """計算月度成本"""
        
        cost_analysis = {
            "by_provider": {},
            "by_task_type": {},
            "total_comparison": {},
            "optimization_opportunities": []
        }
        
        # 計算每個提供商的成本
        for provider, models in self.cost_models.items():
            cost_analysis["by_provider"][provider] = {}
            
            for model in models:
                monthly_cost = await self._calculate_provider_monthly_cost(
                    model, usage_patterns
                )
                cost_analysis["by_provider"][provider][model.model] = monthly_cost
        
        # 計算每個任務類型的最佳方案
        for task_type, pattern in usage_patterns.items():
            best_options = await self._find_best_cost_options(
                pattern, top_n=3
            )
            cost_analysis["by_task_type"][task_type] = best_options
        
        # 總體成本比較
        cost_analysis["total_comparison"] = await self._compare_total_costs(
            usage_patterns
        )
        
        # 優化建議
        cost_analysis["optimization_opportunities"] = await self._identify_optimization_opportunities(
            usage_patterns, cost_analysis
        )
        
        return cost_analysis
    
    async def _calculate_provider_monthly_cost(
        self, 
        model: CostModel, 
        usage_patterns: Dict[str, UsagePattern]
    ) -> Dict:
        """計算特定提供商模型的月度成本"""
        
        total_input_tokens = 0
        total_output_tokens = 0
        total_requests = 0
        
        # 彙總所有任務類型的使用量
        for pattern in usage_patterns.values():
            monthly_requests = pattern.daily_requests * 30
            total_requests += monthly_requests
            total_input_tokens += monthly_requests * pattern.avg_input_tokens
            total_output_tokens += monthly_requests * pattern.avg_output_tokens
        
        # 計算token成本
        input_cost = 0
        output_cost = 0
        
        # 考慮免費額度
        if model.free_tier_tokens > 0:
            free_input_tokens = min(total_input_tokens, model.free_tier_tokens // 2)
            free_output_tokens = min(total_output_tokens, model.free_tier_tokens // 2)
            
            paid_input_tokens = max(0, total_input_tokens - free_input_tokens)
            paid_output_tokens = max(0, total_output_tokens - free_output_tokens)
        else:
            paid_input_tokens = total_input_tokens
            paid_output_tokens = total_output_tokens
        
        input_cost = (paid_input_tokens / 1000) * model.input_cost_per_1k
        output_cost = (paid_output_tokens / 1000) * model.output_cost_per_1k
        
        # 額外費用
        additional_cost = sum(model.additional_fees.values()) if model.additional_fees else 0
        
        # 總成本
        total_monthly_cost = max(
            input_cost + output_cost + additional_cost,
            model.monthly_minimum
        ) + (model.setup_cost / 12)  # 攤銷設置成本
        
        return {
            "total_cost": round(total_monthly_cost, 2),
            "input_cost": round(input_cost, 2),
            "output_cost": round(output_cost, 2),
            "additional_cost": round(additional_cost, 2),
            "setup_cost_monthly": round(model.setup_cost / 12, 2),
            "total_tokens": total_input_tokens + total_output_tokens,
            "cost_per_1k_tokens": round(total_monthly_cost / ((total_input_tokens + total_output_tokens) / 1000), 4) if total_input_tokens + total_output_tokens > 0 else 0,
            "cost_per_request": round(total_monthly_cost / total_requests, 4) if total_requests > 0 else 0
        }
    
    async def _find_best_cost_options(
        self, 
        pattern: UsagePattern,
        top_n: int = 3
    ) -> List[Dict]:
        """找出最佳成本選項"""
        
        options = []
        
        for provider, models in self.cost_models.items():
            for model in models:
                # 簡化的單一任務成本計算
                monthly_requests = pattern.daily_requests * 30
                input_tokens = monthly_requests * pattern.avg_input_tokens
                output_tokens = monthly_requests * pattern.avg_output_tokens
                
                # 成本計算
                input_cost = (input_tokens / 1000) * model.input_cost_per_1k
                output_cost = (output_tokens / 1000) * model.output_cost_per_1k
                additional_cost = sum(model.additional_fees.values()) if model.additional_fees else 0
                
                total_cost = max(
                    input_cost + output_cost + additional_cost,
                    model.monthly_minimum
                ) + (model.setup_cost / 12)
                
                # 品質評估（簡化）
                quality_score = self._estimate_quality_score(model, pattern)
                
                # 性價比計算
                value_score = quality_score / total_cost if total_cost > 0 else 0
                
                options.append({
                    "provider": model.provider,
                    "model": model.model,
                    "monthly_cost": round(total_cost, 2),
                    "cost_per_request": round(total_cost / monthly_requests, 4) if monthly_requests > 0 else 0,
                    "estimated_quality": quality_score,
                    "value_score": round(value_score, 4),
                    "setup_required": model.setup_cost > 0,
                    "free_tier_benefit": model.free_tier_tokens > 0
                })
        
        # 按性價比排序
        options.sort(key=lambda x: x["value_score"], reverse=True)
        
        return options[:top_n]
    
    def _estimate_quality_score(self, model: CostModel, pattern: UsagePattern) -> float:
        """估計品質分數（簡化模型）"""
        
        # 基於模型複雜度的品質估計
        quality_mapping = {
            "gpt-4": 0.95,
            "claude-3-opus": 0.94,
            "claude-3-sonnet": 0.88,
            "gpt-3.5-turbo": 0.82,
            "command": 0.80,
            "llama-2-70b": 0.78
        }
        
        base_quality = quality_mapping.get(model.model, 0.75)
        
        # 根據任務需求調整
        required_quality = pattern.quality_requirements.get("target_quality", 0.8)
        if base_quality < required_quality:
            return base_quality * 0.8  # 降低不符合需求的分數
        
        return base_quality
    
    async def _compare_total_costs(
        self, 
        usage_patterns: Dict[str, UsagePattern]
    ) -> Dict:
        """比較總體成本"""
        
        strategies = {
            "single_provider_premium": {
                "description": "使用單一高品質提供商",
                "assignments": {"*": "openai:gpt-4"}
            },
            
            "single_provider_balanced": {
                "description": "使用單一平衡型提供商", 
                "assignments": {"*": "anthropic:claude-3-sonnet"}
            },
            
            "cost_optimized_mix": {
                "description": "成本優化混合策略",
                "assignments": {
                    "simple_tasks": "cohere:command",
                    "complex_analysis": "anthropic:claude-3-opus",
                    "code_generation": "openai:gpt-4",
                    "high_volume": "llama_self_hosted:llama-2-70b"
                }
            },
            
            "quality_optimized_mix": {
                "description": "品質優化混合策略",
                "assignments": {
                    "simple_tasks": "openai:gpt-3.5-turbo",
                    "complex_analysis": "anthropic:claude-3-opus", 
                    "code_generation": "openai:gpt-4",
                    "high_volume": "azure:gpt-4"
                }
            }
        }
        
        comparison_results = {}
        
        for strategy_name, strategy in strategies.items():
            total_cost = 0
            weighted_quality = 0
            total_requests = 0
            
            for task_type, pattern in usage_patterns.items():
                # 確定使用的模型
                if task_type in strategy["assignments"]:
                    provider_model = strategy["assignments"][task_type]
                else:
                    provider_model = strategy["assignments"]["*"]
                
                provider, model_name = provider_model.split(":")
                model = next(
                    (m for m in self.cost_models[provider] if m.model == model_name),
                    None
                )
                
                if model:
                    # 計算成本
                    cost_info = await self._calculate_provider_monthly_cost(
                        model, {task_type: pattern}
                    )
                    
                    task_cost = cost_info["total_cost"]
                    task_requests = pattern.daily_requests * 30
                    task_quality = self._estimate_quality_score(model, pattern)
                    
                    total_cost += task_cost
                    weighted_quality += task_quality * task_requests
                    total_requests += task_requests
            
            avg_quality = weighted_quality / total_requests if total_requests > 0 else 0
            
            comparison_results[strategy_name] = {
                "description": strategy["description"],
                "total_monthly_cost": round(total_cost, 2),
                "average_quality": round(avg_quality, 3),
                "cost_per_request": round(total_cost / total_requests, 4) if total_requests > 0 else 0,
                "value_score": round(avg_quality / total_cost * 1000, 2) if total_cost > 0 else 0
            }
        
        return comparison_results
    
    async def _identify_optimization_opportunities(
        self, 
        usage_patterns: Dict[str, UsagePattern],
        cost_analysis: Dict
    ) -> List[Dict]:
        """識別優化機會"""
        
        opportunities = []
        
        # 分析當前總成本
        total_current_cost = sum(
            min(options[0]["monthly_cost"] for options in task_costs.values())
            for task_costs in cost_analysis["by_task_type"].values()
        )
        
        # 機會1：免費額度利用
        for provider, models in self.cost_models.items():
            for model in models:
                if model.free_tier_tokens > 0:
                    opportunities.append({
                        "type": "free_tier_utilization",
                        "description": f"利用{provider}的免費額度({model.free_tier_tokens:,} tokens)",
                        "potential_savings": "最高可節省50%初期成本",
                        "requirements": ["註冊帳戶", "監控用量限制"],
                        "risk_level": "低"
                    })
        
        # 機會2：任務分層策略
        if len(usage_patterns) > 1:
            opportunities.append({
                "type": "task_stratification",
                "description": "根據任務複雜度選擇不同等級的模型",
                "potential_savings": f"預估節省20-40%（約${total_current_cost * 0.3:.0f}/月）",
                "requirements": ["實施智能路由", "任務分類系統"],
                "risk_level": "中"
            })
        
        # 機會3：自建部署
        high_volume_tasks = [
            name for name, pattern in usage_patterns.items()
            if pattern.daily_requests > 1000
        ]
        
        if high_volume_tasks:
            opportunities.append({
                "type": "self_hosted_deployment",
                "description": "高用量任務考慮自建Llama部署",
                "potential_savings": f"長期可節省60%以上（月用量>10萬次請求）",
                "requirements": ["初期投資$2000", "技術維護能力", "GPU資源"],
                "risk_level": "高",
                "break_even_point": "6-12個月"
            })
        
        # 機會4：批量折扣
        if total_current_cost > 1000:
            opportunities.append({
                "type": "volume_discount",
                "description": "與提供商談判大用量折扣",
                "potential_savings": "5-15%的價格優惠",
                "requirements": ["企業合約談判", "承諾最低用量"],
                "risk_level": "低"
            })
        
        return opportunities
    
    def generate_cost_report(
        self, 
        cost_analysis: Dict,
        usage_patterns: Dict[str, UsagePattern]
    ) -> str:
        """生成成本分析報告"""
        
        report = f"""
# AI成本分析報告
生成時間：{datetime.now().strftime('%Y-%m-%d %H:%M:%S')}

## 執行摘要
- 分析了 {len(usage_patterns)} 種使用模式
- 比較了 {len(self.cost_models)} 個AI提供商
- 識別了 {len(cost_analysis['optimization_opportunities'])} 個優化機會

## 使用模式概覽
"""
        
        for task_type, pattern in usage_patterns.items():
            monthly_requests = pattern.daily_requests * 30
            monthly_tokens = monthly_requests * (pattern.avg_input_tokens + pattern.avg_output_tokens)
            
            report += f"""
### {task_type}
- 月度請求量：{monthly_requests:,} 次
- 月度Token用量：{monthly_tokens:,} tokens
- 尖峰時段：{', '.join(map(str, pattern.peak_hours))}:00
- 品質要求：{pattern.quality_requirements['target_quality']:.1%}
"""
        
        report += "\n## 成本比較\n"
        
        for strategy, results in cost_analysis["total_comparison"].items():
            report += f"""
### {results['description']}
- 月度總成本：${results['total_monthly_cost']:,.2f}
- 平均品質分數：{results['average_quality']:.3f}
- 每次請求成本：${results['cost_per_request']:.4f}
- 性價比評分：{results['value_score']:.2f}
"""
        
        report += "\n## 優化建議\n"
        
        for i, opportunity in enumerate(cost_analysis["optimization_opportunities"], 1):
            report += f"""
### {i}. {opportunity['description']}
- 類型：{opportunity['type']}
- 潛在節省：{opportunity['potential_savings']}
- 實施要求：{', '.join(opportunity['requirements'])}
- 風險等級：{opportunity['risk_level']}
"""
            
            if 'break_even_point' in opportunity:
                report += f"- 回本週期：{opportunity['break_even_point']}\n"
        
        return report

# 使用範例
async def main():
    optimizer = EnterpriseCostOptimizer()
    
    # 模擬歷史使用數據
    historical_data = [
        {
            "timestamp": "2024-01-15T10:30:00",
            "task_type": "customer_service",
            "input_tokens": 150,
            "output_tokens": 200,
            "quality_score": 0.85
        },
        {
            "timestamp": "2024-01-15T14:20:00", 
            "task_type": "business_analysis",
            "input_tokens": 800,
            "output_tokens": 600,
            "quality_score": 0.92
        }
        # 更多數據...
    ] * 100  # 模擬更多數據
    
    # 分析使用模式
    usage_patterns = await optimizer.analyze_usage_patterns(historical_data)
    
    # 計算成本
    cost_analysis = await optimizer.calculate_monthly_costs(usage_patterns)
    
    # 生成報告
    report = optimizer.generate_cost_report(cost_analysis, usage_patterns)
    print(report)
    
    # 保存分析結果
    with open("cost_analysis_report.json", "w", encoding="utf-8") as f:
        json.dump({
            "usage_patterns": {k: asdict(v) for k, v in usage_patterns.items()},
            "cost_analysis": cost_analysis,
            "generated_at": datetime.now().isoformat()
        }, f, ensure_ascii=False, indent=2)

if __name__ == "__main__":
    asyncio.run(main())
```

</div>

---

## 💡 關鍵要點總結

<div style="background-color: #F0F4C3; padding: 20px; border-left: 4px solid #CDDC39; margin: 20px 0;">

### 🎯 平台選擇策略
1. **開源需求**：Meta Llama提供完全可控的開源解決方案
2. **企業合規**：Azure AI具備最完整的企業級安全和合規功能
3. **成本效益**：Cohere和Hugging Face在特定場景下性價比最高
4. **技術創新**：各平台都有獨特的技術優勢，需要根據具體需求選擇

### 🛠️ 整合最佳實踐
1. **智能編排**：建立跨平台的智能路由和故障轉移機制
2. **成本優化**：實施動態成本分析和多層次優化策略
3. **品質保證**：建立統一的品質評估和監控標準
4. **安全合規**：確保所有平台都符合企業級安全要求

### 📈 未來發展趨勢
1. **標準化進程**：API接口和功能逐步標準化
2. **專業分工**：不同提供商在特定領域的專業化程度提升
3. **成本競爭**：激烈的價格競爭帶來更好的成本效益
4. **生態整合**：多模態和跨平台整合成為主流趨勢

### 🔮 企業決策指南
1. **評估現狀**：分析當前AI使用情況和業務需求
2. **制定策略**：根據成本、品質、合規要求制定多平台策略
3. **分階段實施**：從低風險場景開始，逐步擴展到核心業務
4. **持續優化**：建立監控和優化機制，持續改進效果和成本效益

</div>

---

<p align="center">
<strong>🚀 恭喜您完成其他LLM提供商指南學習！</strong><br>
<em>接下來，讓我們深入探討LLM特性與教學應用</em>
</p>

<p align="center">
<a href="./07-LLM特性與教學指南.md">
<img src="https://img.shields.io/badge/下一章-LLM特性與教學-blue?style=for-the-badge" alt="下一章">
</a>
<a href="./05-Gemini提示工程指南.md">
<img src="https://img.shields.io/badge/回顧-Gemini指南-green?style=for-the-badge" alt="Gemini指南">
</a>
<a href="./README.md">
<img src="https://img.shields.io/badge/返回-主頁-orange?style=for-the-badge" alt="返回主頁">
</a>
</p>