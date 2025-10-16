# AI Orchestration Architecture (V7)

**Last Updated:** October 12, 2025
**Status:** ✅ PRODUCTION READY - Dual-Mode Orchestration (Frontier + Lightning)
**Version:** V7.2.0

---

## 🎉 MAJOR UPDATE v7.2 - Dual-Mode Orchestration (October 12, 2025)

**NEW CAPABILITIES:**
- ✅ **Dual-Mode Orchestration**: Frontier (research-quality) + Lightning (fast answers)
- ✅ **Frontier Mode**: Claude Sonnet 4.5, 10k-25k chars, RAG + Perplexity + LLM, 1.0 questions, 60-90s
- ✅ **Lightning Mode**: GPT-4o-mini, 2k-5k chars, RAG + LLM only, 0.5 questions, <30s
- ✅ **Fractional Question Deduction**: 0.5 questions for Lightning, 1.0 for Frontier
- ✅ **Performance Optimization**: Lightning mode skips Perplexity for 2.4x speedup
- ✅ **Dual System Messages**: Separate prompts for Frontier (comprehensive) and Lightning (concise)
- ✅ **Dynamic Model Selection**: Admin configures both models, system selects based on mode

**Previous Updates (v7.1):**
- ✅ **RAG Quality Verification**: Complete quality scoring system for all documents (0-100 scale)
- ✅ **Admin Quality Dashboard**: Document management UI with quality badges and detailed metrics
- ✅ **RAG Testing Interface**: `/admin/rag-testing` for query testing with relevance scores
- ✅ **Enhanced User Chat**: Prominent sources display with type icons (RAG 📚, Internet 🌐, General 🧠)
- ✅ **User Document Upload**: Quality validation prevents bad files from entering RAG system
- ✅ **Orchestration Transparency**: Full visibility into tools used, sources, and processing time

**See `/docs/handoff/LIGHTNING_MODE_COMPLETE.md` for complete dual-mode implementation details.**
**See `/docs/architecture/multimodal-rag-architecture.md` for RAG Quality Verification details.**

---

## Executive Summary

The V7 AI Orchestration system is the **core product value** of Moj AI. It coordinates multiple AI models and tools to deliver **two types of responses**:

### **Frontier Mode** (Research-Quality)
- **Model**: Claude Sonnet 4.5 (admin-configurable)
- **Tools**: RAG + Perplexity + General Knowledge
- **Response**: 10,000-25,000 characters
- **Time**: 60-90 seconds
- **Cost**: 1.0 questions
- **Use Case**: Comprehensive research, legal analysis, detailed reports

### **Lightning Mode** (Fast Answers)
- **Model**: GPT-4o-mini (admin-configurable)
- **Tools**: RAG + General Knowledge (Perplexity skipped for speed)
- **Response**: 2,000-5,000 characters
- **Time**: <30 seconds
- **Cost**: 0.5 questions
- **Use Case**: Quick answers, simple queries, cost-effective responses

**Key Principle:** ONE main orchestrator LLM per mode with tools at its disposal:
- **Perplexity** - Internet research tool (Frontier only)
- **RAG** - Document knowledge base (both modes)
- **Orchestrator's general knowledge** - Legal reasoning and analysis (both modes)

**NOT** three separate LLMs in sequence - the orchestrator calls tools as needed.

---

## Architecture Overview

### **Dual-Mode Flow**

```
User Query + Mode Selection (Frontier/Lightning)
    ↓
┌─────────────────────────────────────────────────────────┐
│  Query Analyzer V7                                      │
│  - Detects: RAG, Perplexity, Forms, Legal Reasoning    │
│  - Lightning Mode: Skip Perplexity for speed ⚡         │
│  - Frontier Mode: Use all tools for quality 🚀         │
└─────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────┐
│  Tool Coordinator V7                                    │
│  - Executes tools in parallel/sequence                  │
│  - RAG Search (Weaviate) - BOTH MODES                  │
│  - Internet Research (Perplexity) - FRONTIER ONLY       │
│  - Form Discovery - BOTH MODES                          │
└─────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────┐
│  Legal Reasoning Tool V7                                │
│  - Frontier: Claude Sonnet 4.5 + Frontier System Msg    │
│  - Lightning: GPT-4o-mini + Lightning System Msg        │
│  - Selects config based on lightning_mode parameter     │
└─────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────┐
│  Response Synthesizer V7                                │
│  - Combines tool results                                │
│  - Formats final response                               │
│  - Adds source citations                                │
└─────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────┐
│  Usage Tracker                                          │
│  - Frontier: Deduct 1.0 questions                       │
│  - Lightning: Deduct 0.5 questions                      │
│  - Atomic database-level increments                     │
└─────────────────────────────────────────────────────────┘
    ↓
Final Response + Metadata
    ↓
Response Synthesizer
    ↓
Final Response (5-10 pages, Slovenian legal format)
```

---

## Dual-Mode Configuration

### **Mode Comparison**

| Feature | Frontier Mode 🚀 | Lightning Mode ⚡ |
|---------|------------------|-------------------|
| **Model** | Claude Sonnet 4.5 | GPT-4o-mini |
| **System Message** | 3,773 chars (comprehensive) | 2,474 chars (concise) |
| **Response Length** | 10,000-25,000 chars | 2,000-5,000 chars |
| **Processing Time** | 60-90 seconds | <30 seconds |
| **Tools Used** | RAG + Perplexity + LLM | RAG + LLM only |
| **Question Cost** | 1.0 questions | 0.5 questions |
| **Use Case** | Research, analysis, reports | Quick answers, simple queries |
| **Perplexity** | ✅ Enabled | ❌ Skipped for speed |

### **Admin Configuration**

Admins can configure both modes independently via Admin UI:

**Frontier Mode Settings:**
- Model: `claude-sonnet-4-5-20250929` (or other Claude/GPT models)
- System Message: Stored in `admin_settings.frontier_system_message`
- Max Tokens: 4000
- Temperature: 0.7

**Lightning Mode Settings:**
- Model: `gpt-4o-mini` (or other fast models)
- System Message: Stored in `admin_settings.lightning_system_message`
- Max Tokens: 4000
- Temperature: 0.7

**Database Storage:**
```sql
-- admin_settings table (key-value JSONB)
{
  "lead_orchestrator": {"provider": "anthropic", "model": "claude-sonnet-4-5-20250929"},
  "lightning_orchestrator": {"provider": "openai", "model": "gpt-4o-mini"},
  "frontier_system_message": "You are an expert AI orchestrator...",
  "lightning_system_message": "You are an expert AI assistant optimized for fast...",
  "api_keys": {"anthropic": "...", "openai": "...", "perplexity": "..."}
}
```

---

## Core Components

### 1. Query Analyzer (`query_analyzer_v7.py`)
**Purpose:** Analyze user query and determine which tools are needed

**Capabilities:**
- Detects need for internet research (keywords: "preišči", "internet", "aktualno")
- Detects need for RAG (cities, districts, legal keywords, uploaded documents)
- Detects need for form discovery (keywords: "obrazec", "formular")
- Always includes legal reasoning (lead orchestrator)
- Always includes quality validation
- **NEW:** Skips Perplexity in Lightning mode for speed optimization

**Output:** `QueryRequirements` object with:
- `needs`: List of tool names
- `complexity`: SIMPLE, MODERATE, COMPLEX
- `geographic_scope`: City/district if detected
- `legal_domain`: Type of legal question
- `confidence`: 0.0-1.0

**Lightning Mode Optimization:**
```python
# Skip Perplexity in Lightning mode
lightning_mode = user_context.get("lightning_mode", False)
if internet_needed and not lightning_mode:
    needs.append("internet_research")
elif internet_needed and lightning_mode:
    logger.info("⚡ Internet research SKIPPED (Lightning mode)")
```

### 2. Tool Coordinator (`tool_coordinator_v7.py`)
**Purpose:** Execute tools in optimal order and manage context flow

**Execution Strategy:**
1. **Gathering Phase** (parallel): Internet research, RAG search, form discovery
2. **Reasoning Phase** (sequential): Legal reasoning with context from gathering tools
3. **Validation Phase**: Quality validation of final response

**Tools Available:**
- `rag_search` - Search uploaded documents (Weaviate)
- `internet_research` - Search internet (Perplexity Sonar Pro)
- `form_discovery` - Find relevant forms
- `legal_reasoning` - Main orchestrator (Claude/GPT)
- `quality_validation` - Validate response quality

### 3. Legal Reasoning Tool (`legal_reasoning_tool_v7.py`)
**Purpose:** Main orchestrator that performs legal analysis with dual-mode support

**Key Features:**
- **Dual Config Support**: Frontier (Claude Sonnet 4.5) + Lightning (GPT-4o-mini)
- **Mode Selection**: Chooses config based on `lightning_mode` parameter
- Receives context from RAG and internet research
- Applies mode-specific system message
- Generates mode-appropriate responses (10k-25k for Frontier, 2k-5k for Lightning)
- Includes source citations

**Mode Selection Logic:**
```python
# Select config and system message based on mode
if lightning_mode:
    active_config = self.lightning_config
    base_system_message = self.lightning_system_message
    mode_name = "LIGHTNING"
else:
    active_config = self.frontier_config
    base_system_message = self.frontier_system_message
    mode_name = "FRONTIER"

logger.info(f"🔧 Using {mode_name} mode: {active_config.provider}/{active_config.model}")
```

**System Message Structure:**
```
Mode-Specific System Message (Frontier or Lightning from admin UI)
+
RAG Context (if available)
+
Internet Research Context (if available, Frontier only)
+
Quality Requirements (mode-specific length targets)
```

**Frontier System Message** (3,773 chars):
- Comprehensive instructions for research-quality responses
- Target: 10,000-25,000 characters
- Detailed sourcing requirements
- Slovenian legal format (IZVRŠNI POVZETEK, KLJUČNE UGOTOVITVE, etc.)

**Lightning System Message** (2,474 chars):
- Concise instructions for fast responses
- Target: 2,000-5,000 characters
- Essential sourcing only
- Simplified format for quick answers

### 4. Response Synthesizer (`response_synthesizer_v7.py`)
**Purpose:** Combine tool results into final response

**Synthesis Strategy:**
- Primary: Legal reasoning response (if successful)
- Fallback: Combine internet research + RAG results
- Always: Add sources section with citations
- Format: Slovenian legal structure (IZVRŠNI POVZETEK, KLJUČNE UGOTOVITVE, etc.)

---

## Configuration System

### Admin Settings Service (`admin_settings_service.py`)
**Purpose:** Load and cache admin configuration from database

**Critical Fix (Oct 9, 2025):**
- **DECRYPTS API KEYS** when loading from database (lines 276-287)
- Keys stored encrypted in `admin_settings` table
- Decryption happens ONCE here, not in config builder

**Settings Loaded (Dual-Mode):**
- `lead_orchestrator`: Frontier mode provider, model, temperature, max_tokens
- `lightning_orchestrator`: Lightning mode provider, model, temperature, max_tokens
- `api_keys`: Anthropic, OpenAI, Perplexity (DECRYPTED)
- `tools_config`: RAG enabled, Perplexity enabled, models
- `frontier_system_message`: Comprehensive system message for Frontier mode
- `lightning_system_message`: Concise system message for Lightning mode
- `quality_settings`: Temperature, max_tokens

**Caching:**
- 5-minute TTL
- Invalidated on admin settings update
- Fresh load on every orchestration request

### Config Builder (`config_builder_v7.py`)
**Purpose:** Build V7 config objects from admin settings with dual-mode support

**Critical Fix (Oct 9, 2025):**
- **REMOVED DOUBLE DECRYPTION** (lines 116-123, 137-147)
- Now uses already-decrypted keys from admin settings service
- No longer calls `self._decrypt_key()`

**Dual-Mode Update (Oct 12, 2025):**
- **Builds TWO configs**: Frontier + Lightning
- Extracts `lead_orchestrator` for Frontier mode
- Extracts `lightning_orchestrator` for Lightning mode
- Extracts both `frontier_system_message` and `lightning_system_message`
- Passes both configs to orchestration engine

**Builds:**
- `LeadOrchestratorConfig` (Frontier): Provider, model, API key, temperature, max_tokens
- `LightningOrchestratorConfig` (Lightning): Provider, model, API key, temperature, max_tokens
- `ToolsConfig`: RAG/Perplexity enabled, API keys, models
- `AdminConfig`: Complete configuration object with dual configs

---

## API Key Management

### Encryption Flow
```
Admin UI (plaintext key)
    ↓
Backend API (/api/v1/admin/orchestration/settings)
    ↓
AdminSettingsService.encrypt() [Fernet]
    ↓
Database (encrypted string)
```

### Decryption Flow (FIXED Oct 9, 2025)
```
Database (encrypted string)
    ↓
AdminSettingsService._load_settings_from_db()
    ↓
self._encryption.decrypt() [Fernet] ← HAPPENS HERE (ONCE)
    ↓
Config Builder (uses decrypted keys directly)
    ↓
Tools (receive valid API keys)
```

**Encryption Details:**
- **Algorithm:** Fernet (symmetric encryption)
- **Key:** Stored in `.env` as `API_KEY_ENCRYPTION_KEY`
- **Current Key:** `8VrK3B1kyrkrO65fvYvkl6FnKWwWJlTDvCKyChOuYig=`

---

## Usage Tracking & Billing

### Fractional Question Deduction (Oct 12, 2025)

**Implementation:** `usage_tracker.py` + `conversation_production.py`

**Question Costs:**
- **Frontier Mode**: 1.0 questions
- **Lightning Mode**: 0.5 questions

**Tracking Flow:**
```python
# Determine question cost based on mode
question_cost = 0.5 if request.lightning_mode else 1.0

# Track usage with fractional cost
await usage_tracker.track_usage(
    tenant_id=str(tenant.id),
    user_id=str(user.id),
    action_type="question",
    tokens_used=orchestration_result.metadata.get("total_tokens", 1000),
    cost_estimate=orchestration_result.metadata.get("total_cost_usd", 0.01),
    metadata={
        "lightning_mode": request.lightning_mode,
        "question_cost": question_cost
    },
    db=self.db,
    question_cost=question_cost  # NEW: Fractional support
)
```

**Database Updates:**
```python
# Atomic increment with fractional support
await db.execute(
    update(Subscription)
    .where(Subscription.id == subscription.id)
    .values(questions_used=Subscription.questions_used + question_cost)
)
```

**Benefits:**
- Users get 2x more Lightning queries for same cost
- Encourages use of fast mode for simple questions
- Reduces infrastructure costs (GPT-4o-mini cheaper than Claude Sonnet 4.5)
- Improves user experience with faster responses

---

## Master System Messages (Dual-Mode)

### Frontier System Message

**Location:** Admin UI → Orchestration → Frontier System Message
**Storage:** `admin_settings` table, key: `frontier_system_message`
**Current Version:** V7.2.0 (3,773 characters)

**Purpose:** Comprehensive instructions for research-quality responses

**Key Sections:**
1. Role definition (Slovenian building legislation expert)
2. Response quality requirements (10,000-25,000 chars, research-quality)
3. Tool usage strategy (RAG-first, then internet, then general knowledge)
4. Source citation requirements (inline [1], [2], [3])
5. Response format (Slovenian legal structure: IZVRŠNI POVZETEK, KLJUČNE UGOTOVITVE, etc.)
6. Language requirements (always Slovenian)

**Application:**
- Applied to Frontier mode (Claude Sonnet 4.5)
- Combined with RAG + Perplexity context
- Enforces comprehensive response quality

### Lightning System Message

**Location:** Admin UI → Orchestration → Lightning System Message
**Storage:** `admin_settings` table, key: `lightning_system_message`
**Current Version:** V7.2.0 (2,474 characters)

**Purpose:** Concise instructions for fast, accurate responses

**Key Sections:**
1. Role definition (Slovenian building legislation assistant)
2. Response quality requirements (2,000-5,000 chars, concise)
3. Tool usage strategy (RAG-first, then general knowledge)
4. Source citation requirements (essential sources only)
5. Response format (simplified structure: KRATEK ODGOVOR, KLJUČNE TOČKE, etc.)
6. Language requirements (always Slovenian)

**Application:**
- Applied to Lightning mode (GPT-4o-mini)
- Combined with RAG context only (no Perplexity)
- Optimized for speed and cost-effectiveness

---

## Response Quality Requirements (Dual-Mode)

### Frontier Mode Standards
- **Length:** 10,000-25,000 characters (10-25 pages)
- **Language:** Slovenian (always)
- **Format:** Slovenian legal structure
  - IZVRŠNI POVZETEK (Executive Summary)
  - KLJUČNE UGOTOVITVE (Key Findings)
  - PRAVNA PODLAGA (Legal Basis)
  - TRENUTNE TRŽNE RAZMERE (Current Market Conditions)
  - PRIPOROČILA (Recommendations)
  - VIRI (Sources)
- **Sources:** Minimum 5-10 sources with inline citations
- **Citations:** Embedded in text [1], [2], [3] with full list at end
- **Quality:** Comparable to Claude Sonnet 4.5 research mode with internet access
- **Depth:** Comprehensive analysis with real-world examples
- **Specificity:** Exact forms, procedures, calculations
- **Actionability:** Clear next steps and recommendations

### Lightning Mode Standards
- **Length:** 2,000-5,000 characters (2-5 pages)
- **Language:** Slovenian (always)
- **Format:** Simplified structure
  - KRATEK ODGOVOR (Short Answer)
  - KLJUČNE TOČKE (Key Points)
  - VIRI (Sources)
- **Sources:** Minimum 1-3 essential sources
- **Citations:** Essential citations only
- **Quality:** Accurate, concise, actionable
- **Depth:** Focused on core information
- **Specificity:** Key facts and procedures
- **Actionability:** Clear immediate next steps

---

## Progress Tracking

### Real-Time Updates (SSE)
**Endpoint:** `/api/v1/chat/progress/{conversation_id}`

**Progress States:**
1. `analyzing` - Query analysis in progress
2. `reasoning` - Tool selection complete
3. `researching` - Internet research in progress
4. `searching` - RAG search in progress
5. `generating` - Legal reasoning in progress
6. `validating` - Quality validation in progress
7. `complete` - Orchestration complete

**Known Issue:** Progress bar shows generic "100% complete, validating" instead of real-time tool execution updates. Needs frontend investigation.

---

## Orchestration Report

**Purpose:** Show how answer was constructed and all sources used

**Contents:**
- Version (V7.0.0)
- Processing time
- Confidence score
- LLMs used (list of tools executed)
- Sources (count and list)
- Tool execution details:
  - Tool name
  - Success/failure
  - Response length
  - Sources found
  - Execution time

**Storage:** Included in message metadata
**Display:** Icon below response (frontend implementation needed)

---

## Known Issues & Fixes

### ✅ FIXED: Double Decryption Bug (Oct 9, 2025)
**Problem:** API keys decrypted twice, resulting in empty strings → 401 errors  
**Fix:** Decrypt once in `admin_settings_service.py`, use directly in `config_builder_v7.py`  
**Files:** 
- `backend/app/services/orchestration/admin_settings_service.py` (lines 276-287)
- `backend/app/services/orchestration_v7/config_builder_v7.py` (lines 116-123, 137-147)

### ⏳ PENDING: Progress Bar Not Real-Time
**Problem:** Generic progress instead of actual tool execution updates  
**Impact:** User can't see what orchestration is doing during 5+ minute processing  
**Next Steps:** Investigate SSE implementation in frontend

### ⏳ PENDING: Orchestration Report UI
**Problem:** Report exists in backend but not displayed in UI  
**Impact:** Users can't verify how answer was constructed  
**Next Steps:** Add icon below response to show report details

---

## Testing & Validation

### Test Accounts
- **Admin:** `produkcija97@gmail.com`
- **User:** `blocklabstech@gmail.com`

### Test Query (Slovenian Real Estate)
```
Analiziraj trg Slovenije, preišči internet, nepremičninske oglase, preglej odprte nepremičninske forume in naredi podrobno analizo trga glede na cene gradnje in prodaje stanovanj za zadnje 3 leta (2023, 2024, 2025). Primerjaj 3 leta in naredi analizo?
```

### Validation Checklist
- ✅ Claude API receives valid decrypted key
- ✅ Legal reasoning tool executes successfully
- ⏳ Response is 10,000-25,000 characters
- ⏳ Response is in Slovenian
- ⏳ Response includes 5+ sources with citations
- ⏳ Orchestration report shows correct tool usage
- ⏳ API usage visible in Anthropic/Perplexity dashboards

---

## Related Documentation

- **System Overview:** `docs/architecture/system-overview.md`
- **Database Schema:** `docs/architecture/database-schema.md`
- **Admin Guide:** `docs/admin/admin-guide.md`
- **Handoff (Oct 9):** `docs/handoff/2025-10-09-api-key-decryption-fix.md`
- **Issue Tracking:** `docs/issues/ORCHESTRATION_HANGING_FIX_2025-10-09.md`

---

**End of AI Orchestration Architecture Documentation**

