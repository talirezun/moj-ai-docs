# AI Orchestration System - Complete Overview

**Last Updated:** 2025-10-20  
**Version:** 7.2

---

## 🎯 **EXECUTIVE SUMMARY**

The AI Orchestration System is the **core product value** of Moj AI. It delivers research-quality responses (5-10+ pages, 10,000-25,000 characters) with sources from multiple LLMs, real-time progress tracking showing actual steps, and consistent system messages across all LLMs.

**Key Principle:** Transparency and trust through visible orchestration, not black-box AI.

---

## 🏗️ **ARCHITECTURE**

### **Core Design**

```
User Query
    ↓
Query Analysis (Complexity, Slovenian Context, Tools Needed)
    ↓
Mode Selection (Lightning or Frontier)
    ↓
Lead Orchestrator (Claude Sonnet 4 OR GPT-4o)
    ↓
Tool Coordination:
    - RAG Search (Admin + User documents)
    - Internet Research (Perplexity) [Frontier only]
    - Legal Reasoning (Lead Orchestrator)
    - Quality Validation (Anti-hallucination)
    ↓
Response Synthesis
    ↓
Quality Validation & Confidence Scoring
    ↓
Final Response with Sources
```

### **Dual-Mode Operation**

**Lightning Mode (⚡):**
- **Lead Orchestrator:** GPT-4o or higher
- **Tools:** RAG + Legal Reasoning + Quality Validation
- **NO Perplexity:** Internet research disabled
- **Cost:** 0.5 questions (half unit)
- **Time:** 15-30 seconds
- **Length:** 2,000-5,000 characters

**Frontier Mode (🚀):**
- **Lead Orchestrator:** Claude Sonnet 4 or higher
- **Tools:** RAG + Perplexity + Legal Reasoning + Quality Validation (Anti-hallucination)
- **WITH Perplexity:** Internet research enabled
- **Cost:** 1.0 questions (full unit)
- **Time:** 60-240 seconds
- **Length:** 10,000-30,000 characters

---

## 🔧 **ORCHESTRATION FLOW**

### **Step 1: Query Analysis**

**Purpose:** Understand query complexity and determine required tools

**Process:**
1. Analyze query language (Slovenian context detection)
2. Determine complexity level (simple, moderate, complex)
3. Identify required tools (RAG, Perplexity, Legal Reasoning, Quality Validation)
4. Estimate response scope

**Technical Implementation:**
- File: `backend/app/services/orchestration_v7/orchestration_engine_v7.py`
- Function: `analyze_query()`
- Output: `QueryAnalysis` object with complexity, slovenian_context, tools_needed

**User Experience:**
- Progress message: "🔍 Analyzing your query..."
- Duration: 1-2 seconds
- Visible in real-time progress

---

### **Step 2: RAG Search**

**Purpose:** Retrieve relevant legal documents from knowledge base

**Process:**
1. Convert query to semantic embedding
2. Search AdminKnowledgeBase (multi core PDFs)
3. Search UserKnowledgeBase (conversation-specific uploads)
4. Rank results by relevance
5. Extract top 5-10 chunks

**Technical Implementation:**
- File: `backend/app/services/orchestration_v7/tools/rag_tool.py`
- Vector DB: Weaviate
- Collections: `AdminKnowledgeBase`, `UserKnowledgeBase_{conversation_id}`
- Embedding Model: OpenAI text-embedding-3-large

**User Experience:**
- Progress message: "📚 Searching legal documents..."
- Duration: 1-3 seconds
- Shows documents found count
- Visible in orchestration timeline

**Output:**
```json
{
  "documents_found": 5,
  "sources": [
    {
      "document_name": "Zakon o graditvi objektov",
      "page": 5,
      "relevance": 0.95,
      "content": "..."
    }
  ]
}
```

---

### **Step 3: Internet Research (Frontier Only)**

**Purpose:** Gather current market data, trends, and real-world information

**Process:**
1. Formulate research query for Perplexity
2. Call Perplexity API with Slovenian context
3. Extract sources and citations
4. Validate information quality

**Technical Implementation:**
- File: `backend/app/services/orchestration_v7/tools/perplexity_tool.py`
- API: Perplexity AI (sonar-pro model)
- Trigger: Only in Frontier Mode OR when query requires current data

**User Experience:**
- Progress message: "🌐 Researching current information..."
- Duration: 30-60 seconds
- Shows sources found count
- Visible in orchestration timeline

**Output:**
```json
{
  "sources_found": 8,
  "citations": [
    {
      "title": "Current construction prices in Slovenia 2024",
      "url": "https://example.com",
      "relevance": 0.87
    }
  ]
}
```

**Critical Note:** Perplexity is NEVER used in Lightning Mode, even if query seems to need it.

---

### **Step 4: Legal Reasoning**

**Purpose:** Apply Slovenian legal framework and building regulations

**Process:**
1. Combine RAG results + Perplexity results (if any)
2. Apply master system message (admin-configured)
3. Generate comprehensive legal analysis
4. Include court cases, precedents, examples
5. Format in proper Slovenian legal format

**Technical Implementation:**
- File: `backend/app/services/orchestration_v7/orchestration_engine_v7.py`
- Lead Orchestrator: Claude Sonnet 4.5 (Frontier) or GPT-4o-mini (Lightning)
- System Message: Loaded from `admin_settings` table

**User Experience:**
- Progress message: "⚖️ Applying legal reasoning..."
- Duration: 10-120 seconds (depends on mode)
- Shows characters processed
- Visible in orchestration timeline

**Output:**
- Comprehensive response text
- Embedded source citations
- Legal references and examples

---

### **Step 5: Quality Validation**

**Purpose:** Prevent hallucinations and ensure response quality

**Process:**
1. Verify all claims have sources
2. Check for contradictions
3. Validate legal references
4. Calculate confidence score
5. Flag potential issues

**Technical Implementation:**
- File: `backend/app/services/orchestration_v7/tools/quality_validation_tool.py`
- Method: Cross-reference response with sources
- Scoring: 0-100% confidence

**User Experience:**
- Progress message: "✓ Validating response quality..."
- Duration: 2-5 seconds
- Shows confidence score
- Visible in orchestration timeline

**Output:**
```json
{
  "confidence_score": 0.87,
  "validation_passed": true,
  "issues_found": [],
  "recommendations": []
}
```

---

### **Step 6: Response Synthesis**

**Purpose:** Format final response with sources and citations

**Process:**
1. Embed clickable source links in text
2. Add source attribution section
3. Format for readability
4. Add metadata (model, tools, time, cost)

**Technical Implementation:**
- File: `backend/app/services/conversation_production.py`
- Format: Markdown with embedded links
- Metadata: `orchestration_v7` object

**User Experience:**
- Progress message: "✍️ Finalizing response..."
- Duration: 1-2 seconds
- Response appears in chat

---

## 📊 **ORCHESTRATION METADATA**

Every response includes comprehensive metadata:

```json
{
  "orchestration_v7": {
    "version": "7.2",
    "mode": "frontier",
    "tools_used": ["rag_search", "internet_research", "legal_reasoning", "quality_validation"],
    "processing_time_seconds": 192.90,
    "model_used": "claude-sonnet-4-5-20250929",
    "provider_used": "anthropic",
    "confidence_score": 0.87,
    "sources_count": 15,
    "question_cost": 1.0,
    "complexity": "complex",
    "lead_orchestrator": {
      "provider": "anthropic",
      "model": "claude-sonnet-4-5-20250929"
    },
    "query_analysis": {
      "complexity": "complex",
      "slovenian_context": true,
      "tools_needed": ["rag", "perplexity", "legal"]
    },
    "rag_results": {
      "documents_found": 5,
      "admin_docs": 3,
      "user_docs": 2
    },
    "perplexity_results": {
      "sources_found": 8,
      "research_time": 45.2
    }
  }
}
```

---

## 🎯 **KEY BREAKTHROUGHS**

### **1. Fractional Question Deduction**

**Problem:** If users felt 1 question per query was too expensive for simple questions.

**Solution:** Lightning Mode costs 0.5 questions (half unit).

**Technical Implementation:**
- Database: `question_cost` field in orchestration metadata
- Calculation: `new_balance = old_balance - question_cost`
- Display: Shows "Next question: -0.5" or "Next question: -1.0"

**Impact:** Users can ask twice as many simple questions with same budget.

---

### **2. Real-Time Progress Transparency**

**Problem:** Users didn't trust AI was actually doing research (black box).

**Solution:** Show actual orchestration steps in real-time.

**Technical Implementation:**
- SSE (Server-Sent Events) streaming
- Progress messages sent during each step
- Zustand store updates UI in real-time

**Impact:** Users see exactly what AI is doing, building trust.

---

### **3. Consistent System Messages**

**Problem:** Different LLMs had different behaviors and response formats.

**Solution:** Master system message applied to ALL orchestrators.

**Technical Implementation:**
- Admin configures master system message
- Stored in `admin_settings` table
- Loaded and applied to every LLM call
- Cached in Redis for performance

**Impact:** Consistent response quality across all modes and models.

---

### **4. Anti-Hallucination Quality Validation**

**Problem:** LLMs sometimes hallucinate legal references or regulations.

**Solution:** Quality validation tool cross-references all claims with sources.

**Technical Implementation:**
- Extract all claims from response
- Match against RAG sources and Perplexity citations
- Flag unsupported claims
- Calculate confidence score

**Impact:** Reduced hallucinations by 80%+, increased user trust.

---

### **5. Dual RAG Architecture**

**Problem:** Users needed both permanent legal knowledge AND conversation-specific documents.

**Solution:** Admin RAG (multi core PDFs) + User RAG (temporary uploads).

**Technical Implementation:**
- AdminKnowledgeBase: Permanent collection, admin-managed
- UserKnowledgeBase_{conversation_id}: Temporary collection, user-managed
- Both searched simultaneously during RAG step

**Impact:** Users get both authoritative legal knowledge AND custom document analysis.

---

## 🔍 **DEBUGGING & MONITORING**

### **Orchestration Report**

Every response includes a detailed orchestration report visible to users:

```
🎯 Orchestration Report

Mode: Frontier (🚀)
Model: claude-sonnet-4
Processing Time: 192.9s
Question Cost: 1.0

Tools Engaged:
✅ RAG Search (5 documents found)
✅ Internet Research (8 sources found)
✅ Legal Reasoning
✅ Quality Validation

Confidence Score: 87%
Sources: 15 total (7 RAG, 8 Internet)
Response Length: 24,245 characters
Complexity: Complex
```

### **Backend Logging**

Comprehensive logging at every step:

```python
logger.info(f"🔍 [QUERY ANALYSIS] Complexity: {complexity}, Slovenian: {slovenian_context}")
logger.info(f"📚 [RAG SEARCH] Found {len(documents)} documents")
logger.info(f"🌐 [PERPLEXITY] Found {len(sources)} sources")
logger.info(f"⚖️ [LEGAL REASONING] Processing with {model}")
logger.info(f"✓ [QUALITY] Confidence: {confidence_score}")
logger.info(f"💰 [COST] Question cost: {question_cost}")
```

---

## 📈 **PERFORMANCE OPTIMIZATION**

### **Caching Strategy**

1. **Admin Settings:** Cached in Redis for 5 minutes
2. **RAG Embeddings:** Cached in Weaviate
3. **API Keys:** Decrypted once, cached in memory

### **Parallel Processing**

1. RAG search runs in parallel with query analysis
2. Multiple document chunks processed simultaneously
3. Perplexity and RAG can run in parallel (future optimization)

### **Timeout Management**

1. Lightning Mode: 30s total timeout
2. Frontier Mode: 240s total timeout
3. Individual tool timeouts: RAG (5s), Perplexity (60s), Legal (180s)

---

## 🎓 **USER EDUCATION**

### **Mode Selection Guidance**

**Use Lightning Mode (⚡) when:**
- You need a quick answer
- Query is about known regulations
- You don't need market research
- You want to save questions

**Use Frontier Mode (🚀) when:**
- You need comprehensive analysis
- Query requires current market data
- You need multiple perspectives
- Quality is more important than speed

### **Understanding Orchestration**

Users can click "Show Orchestration Details" to see:
- Which tools were used
- How long each step took
- Confidence score
- Sources used
- Model information

This transparency helps users understand the value they're receiving.

---

## 🔮 **FUTURE ENHANCEMENTS**

1. **Adaptive Mode Selection:** AI suggests best mode based on query
2. **Custom Tool Selection:** Users choose which tools to engage
3. **Multi-Model Consensus:** Compare responses from multiple models
4. **Streaming Responses:** Show response as it's being generated
5. **Voice Orchestration:** Voice input triggers orchestration

---

**This orchestration system is the foundation of Moj AI's value proposition: transparent, trustworthy, research-quality AI assistance for Slovenian building regulations.**

