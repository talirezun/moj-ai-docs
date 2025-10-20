# Moj AI - UI/UX Features Documentation Index

**Last Updated:** 2025-10-20  
**Version:** 1.0.0

---

## 📚 **DOCUMENTATION STRUCTURE**

This folder contains comprehensive documentation for all UI/UX features in Moj AI, covering both user-facing and admin-facing functionality, with technical implementation details.

---

## 📖 **CORE DOCUMENTATION**

### **1. AI Orchestration System**
- **[01_AI_ORCHESTRATION_OVERVIEW.md](01_AI_ORCHESTRATION_OVERVIEW.md)**
  - Dual-mode AI orchestration (Lightning vs Frontier)
  - Query analysis and tool coordination
  - Response synthesis and quality validation
  - Technical architecture and implementation

### **2. Mode Selection & Comparison**
- **[02_MODE_SELECTOR.md](02_MODE_SELECTOR.md)**
  - Lightning Mode (⚡ 0.5 questions, <30s, gpt-4o-mini)
  - Frontier Mode (🚀 1.0 questions, 60-240s, Claude Sonnet 4.5)
  - Mode comparison dialog
  - User decision-making support

### **3. RAG System (Retrieval Augmented Generation)**
- **[03_RAG_SYSTEM.md](03_RAG_SYSTEM.md)**
  - Dual RAG architecture (Admin RAG + User RAG)
  - 7 core legal documents (AdminKnowledgeBase)
  - Document processing and chunking
  - Semantic search and retrieval
  - Source attribution and citations

### **4. Question Allowance System**
- **[04_QUESTION_ALLOWANCE.md](04_QUESTION_ALLOWANCE.md)**
  - Fractional question deduction (0.5 vs 1.0)
  - Question types (Premium, Bonus, Free)
  - Visual question bank display
  - Usage tracking and limits
  - Stripe integration for purchases

### **5. Response Quality Dashboard**
- **[05_QUALITY_DASHBOARD.md](05_QUALITY_DASHBOARD.md)**
  - Confidence scoring
  - Tools engaged visualization
  - Processing metrics
  - Quality metrics (comprehensiveness, accuracy, relevance)
  - Model transparency

### **6. Real-Time Progress Tracking**
- **[06_PROGRESS_TRACKING.md](06_PROGRESS_TRACKING.md)**
  - Live orchestration timeline
  - Step-by-step progress indicators
  - SSE (Server-Sent Events) implementation
  - Zustand state management
  - User experience considerations

### **7. Source Attribution & Citations**
- **[07_SOURCE_ATTRIBUTION.md](07_SOURCE_ATTRIBUTION.md)**
  - RAG sources (legal documents)
  - Internet sources (Perplexity)
  - General knowledge sources
  - Citation format and display
  - Clickable source links

### **8. Anti-Hallucination System**
- **[08_ANTI_HALLUCINATION.md](08_ANTI_HALLUCINATION.md)**
  - Quality validation tool
  - Confidence scoring
  - Source verification
  - Response validation
  - Error detection and handling

### **9. Admin Features**
- **[09_ADMIN_FEATURES.md](09_ADMIN_FEATURES.md)**
  - Master system messages
  - Model configuration
  - API key management
  - RAG document management
  - User management
  - Bonus question allocation

### **10. Mobile & Responsive Design**
- **[10_MOBILE_RESPONSIVE.md](10_MOBILE_RESPONSIVE.md)**
  - Mobile-first approach
  - Responsive breakpoints
  - Touch-friendly interactions
  - Compact component modes
  - Performance optimization

---

## 🎯 **FEATURE CATEGORIES**

### **User-Facing Features**
1. Mode Selector (Lightning vs Frontier)
2. Question Allowance Display
3. Real-Time Progress Tracking
4. Response Quality Dashboard
5. Source Attribution
6. Conversation Management
7. File Upload (User RAG)
8. Voice Input
9. Mobile Interface

### **Admin-Facing Features**
1. Master System Messages
2. Model Configuration
3. API Key Management
4. Admin RAG Management (7 core PDFs)
5. User Management
6. Bonus Question Allocation
7. Usage Analytics
8. Settings Management

### **Technical Features**
1. AI Orchestration V7.2
2. Dual RAG Architecture
3. Fractional Question Deduction
4. SSE Progress Streaming
5. Zustand State Management
6. Multi-Tenant Architecture
7. API Key Encryption
8. Stripe Integration

---

## 🔑 **KEY CONCEPTS**

### **Lightning Mode**
- **Cost:** 0.5 questions (half unit)
- **Time:** 15-30 seconds
- **Length:** 2,000-5,000 characters
- **Model:** GPT-4o or higher
- **Tools:** RAG + Legal Reasoning (NO Perplexity)
- **Best For:** Quick answers, known regulations, simple queries

### **Frontier Mode**
- **Cost:** 1.0 questions (full unit)
- **Time:** 60-240 seconds
- **Length:** 10,000-30,000 characters
- **Model:** Claude Sonnet 4 or higher
- **Tools:** RAG + Perplexity + Legal Reasoning + Anti-Halucination
- **Best For:** Comprehensive analysis, market research, complex scenarios

### **Admin RAG**
- **Core PDFs:** Pravilnik, TSG, Uredba, Zakon, OPN_MOM, GZ, Odlok_OPN_MOL_ID and more
- **Collection:** AdminKnowledgeBase
- **Scope:** Permanent knowledge base for all users
- **Management:** Admin-only upload and management

### **User RAG**
- **Scope:** Conversation-specific temporary documents
- **Upload:** User can upload PDFs, DOC/DOCX, text files
- **Lifecycle:** Tied to specific conversation
- **Processing:** OCR, table extraction, chunking

---

## 🎨 **DESIGN SYSTEM**

### **Color Palette**
- **Lightning Mode:** Yellow (#FBBF24) - Fast, efficient
- **Frontier Mode:** Purple (#A855F7) - Comprehensive, premium
- **Premium Questions:** Purple (#A855F7)
- **Bonus Questions:** Green (#10B981)
- **Free Questions:** Blue (#3B82F6)
- **RAG Sources:** Blue (#3B82F6)
- **Internet Sources:** Green (#10B981)
- **General Knowledge:** Purple (#A855F7)

### **Icons**
- ⚡ Lightning Mode
- 🚀 Frontier Mode
- 💎 Premium Questions
- 🎁 Bonus Questions
- 🆓 Free Questions
- 📚 RAG Search
- 🌐 Internet Research (Perplexity)
- ⚖️ Legal Reasoning
- ✓ Quality Validation
- 🔍 Query Analysis
- ✍️ Response Synthesis

---

## 🛠️ **TECHNICAL STACK**

### **Frontend**
- **Framework:** Next.js 14
- **Language:** TypeScript
- **Styling:** Tailwind CSS
- **State Management:** Zustand
- **UI Components:** Radix UI + shadcn/ui
- **Real-time:** Server-Sent Events (SSE)

### **Backend**
- **Framework:** FastAPI (Python)
- **Database:** PostgreSQL
- **Vector DB:** Weaviate
- **Cache:** Redis
- **AI Models:** OpenAI (GPT-4o-mini), Anthropic (Claude Sonnet 4.5), Perplexity
- **Orchestration:** Custom V7.2 engine

---

## 📊 **METRICS & ANALYTICS**

### **Performance Metrics**
- Lightning Mode: <30s processing time
- Frontier Mode: 60-240s processing time
- RAG retrieval: <2s
- Perplexity research: 30-60s
- Legal reasoning: 10-120s

### **Quality Metrics**
- Confidence score: 0-100%
- Comprehensiveness: Based on response length
- Accuracy: Based on confidence score
- Relevance: Based on tools used and sources

### **Usage Metrics**
- Questions per user
- Mode distribution (Lightning vs Frontier)
- Average response length
- Tool engagement rates
- Source citation rates

---

## 🔗 **RELATED DOCUMENTATION**

- **Architecture:** `docs/architecture/system-overview.md`
- **Database:** `docs/architecture/database-schema.md`
- **AI Orchestration:** `docs/architecture/ai-orchestration.md`
- **Admin Guide:** `docs/admin/admin-guide.md`
- **Testing:** `docs/testing/`

---

## 📝 **CHANGELOG**

### **Version 1.0.0 (2025-10-13)**
- Initial comprehensive UI/UX documentation
- Documented all features from user and admin perspectives
- Added technical implementation details
- Created 10 detailed feature documents
- Established design system and color palette
- Defined key concepts and metrics

---

**For questions or updates, refer to the specific feature documentation files listed above.**

