# 🚀 Moj AI - AI-Powered Slovenian Building Legislation Assistant

**Status**: ✅ **PRODUCTION READY** | **Version**: v7.2.1 | **Last Updated**: October 16, 2025

> **Advanced AI orchestration platform for Slovenian building permits and legislation**
> Combining Claude Sonnet 4.5, GPT-4o-mini, and Perplexity with multimodal RAG for research-quality legal analysis

**Deployment**: DigitalOcean App Platform | **Domain**: [app.mojai.xyz](https://app.mojai.xyz)

---

## 🎯 **What is Moj AI?**

Moj AI is a production-grade, multi-tenant SaaS platform that helps users navigate complex Slovenian building legislation using advanced AI orchestration. The system intelligently combines multiple AI models, legal document databases, and real-time internet research to deliver comprehensive, source-backed answers.

### **Core Value Proposition**

- **🎯 Specialized Knowledge**: 7 core Slovenian building legislation PDFs (Pravilnik, TSG, Uredba, Zakon, OPN_MOM, GZ, Odlok_OPN_MOL_ID) permanently loaded
- **🤖 Dual-Mode AI**: Choose between comprehensive research (Frontier) or fast answers (Lightning)
- **📚 Multimodal RAG**: Upload your own PDFs, DOCX, or scanned documents for conversation-specific analysis
- **🌐 Real-Time Research**: Internet research via Perplexity for current prices, regulations, and market data
- **✅ Source Attribution**: Every claim backed by clickable sources (RAG documents, internet links, or general knowledge)
- **⚡ Transparent Processing**: Real-time progress showing actual AI orchestration steps

---

## ⚡ **Dual-Mode AI Orchestration**

Moj AI offers two AI modes optimized for different use cases:

### **🚀 Frontier Mode** (Research-Quality Responses)

**Perfect for**: Complex legal analysis, building permit applications, comprehensive research

| Feature | Details |
|---------|---------|
| **AI Model** | Claude Sonnet 4.5 (Anthropic) |
| **Response Length** | 10,000-30,000 characters |
| **Tools Used** | RAG + Perplexity + Legal Reasoning |
| **Processing Time** | 60-240 seconds |
| **Question Cost** | 1.0 questions |
| **Real Example** | 28,942 characters, 236.9s, 34 sources |

**What you get**:
- Comprehensive 5-10 page reports
- Detailed legal analysis with court cases
- Real-time market data and prices
- Step-by-step procedures
- Complete source citations

### **⚡ Lightning Mode** (Fast Answers)

**Perfect for**: Quick questions, simple queries, cost-effective responses

| Feature | Details |
|---------|---------|
| **AI Model** | GPT-4o-mini (OpenAI) |
| **Response Length** | 2,000-5,000 characters |
| **Tools Used** | RAG + Legal Reasoning (Perplexity skipped) |
| **Processing Time** | <30 seconds |
| **Question Cost** | 0.5 questions (half price!) |
| **Real Example** | 2,365 characters, 23.5s, 4 sources |

**What you get**:
- Concise, focused answers
- Essential legal information
- Quick turnaround
- Cost-effective (50% cheaper)

---

## 🌟 **Key Features**

### **🤖 AI Orchestration (V7.2)**
- ✅ **Dual-Mode Selection** - User chooses Frontier or Lightning mode
- ✅ **Intelligent Query Analysis** - Automatically detects when RAG, internet research, or legal reasoning is needed
- ✅ **Multi-Source Intelligence** - Combines RAG documents, Perplexity research, and LLM knowledge
- ✅ **Real-Time Progress** - Live updates showing actual orchestration steps ("Searching legal documents...", "Researching current prices...")
- ✅ **Orchestration Reports** - Detailed breakdown of tools used, processing time, confidence scores
- ✅ **Fractional Question Deduction** - 0.5 questions for Lightning, 1.0 for Frontier
- ✅ **Admin-Configurable** - Master system messages, model selection, API keys managed via admin UI

### **📚 Dual RAG System**
- ✅ **Admin RAG** - 7 core Slovenian building legislation PDFs permanently loaded for all users
- ✅ **User RAG** - Upload conversation-specific documents (PDF, DOCX, DOC, TXT, scanned PDFs with OCR)
- ✅ **Quality Validation** - Document quality scoring (0-100) prevents bad uploads
- ✅ **Multimodal Processing** - Table extraction, OCR for scanned documents, semantic chunking
- ✅ **Source Attribution** - Every RAG-sourced claim includes document name and page reference

### **👤 User Experience**
- ✅ **Google OAuth** - Secure authentication with role-based access (user/admin)
- ✅ **Conversation Management** - Create, rename, delete, persist conversations across sessions
- ✅ **Question Allowance** - Stacking system (Premium + Bonus + Free questions)
- ✅ **Real-Time Streaming** - Server-Sent Events for live progress updates
- ✅ **Dual Language** - Slovenian/English UI with error notifications in both languages
- ✅ **Mobile Compatible** - Responsive design for all devices
- ✅ **File Upload** - Drag-and-drop document upload with quality validation

### **⚙️ Admin Panel**
- ✅ **AI Configuration** - Configure orchestration strategies, select models (Claude/GPT-4), manage API keys
- ✅ **RAG Management** - Upload/manage 7 core legal PDFs, view extracted text, test retrieval accuracy
- ✅ **Subscription Management** - Sync Stripe products, manage subscription plans
- ✅ **Bonus Questions** - Grant additional questions to users with expiration dates
- ✅ **System Monitoring** - Real-time health checks, API status, database connections
- ✅ **Master System Messages** - Fine-tune AI behavior for Frontier and Lightning modes separately

### **💳 Billing & Subscriptions (Stripe)**
- ✅ **Subscription Tiers** - Free (5 questions), Basic (15/month), Advanced (50/month), Max (150/month)
- ✅ **Question Stacking** - Purchased questions add to remaining balance (3 remaining + 15 purchased = 18 total)
- ✅ **Fractional Deduction** - Lightning mode costs 0.5 questions, Frontier costs 1.0 questions
- ✅ **Bonus Questions** - Admin can grant bonus questions that stack on top of purchased questions
- ✅ **Stripe Integration** - Checkout sessions, webhook handling, subscription management

---

---

## 🛠️ **Technology Stack**

### **Frontend**
| Technology | Purpose |
|------------|---------|
| **Next.js 14** | React framework with App Router |
| **TypeScript** | Type-safe development |
| **Tailwind CSS** | Utility-first styling |
| **Shadcn/ui** | Modern UI components (Radix UI) |
| **Zustand** | State management |
| **Server-Sent Events** | Real-time progress streaming |

### **Backend**
| Technology | Purpose |
|------------|---------|
| **FastAPI** | High-performance async Python web framework |
| **Python 3.11** | Core programming language |
| **SQLAlchemy** | ORM for PostgreSQL |
| **Pydantic** | Data validation and settings |
| **Alembic** | Database migrations |
| **aiofiles** | Async file operations |

### **Databases**
| Database | Purpose |
|----------|---------|
| **PostgreSQL** | Core data (users, conversations, messages, admin settings, subscriptions) |
| **Weaviate** | Vector database for RAG (semantic search, embeddings) |
| **Redis** | Caching and session management |

### **AI/LLM Providers**
| Provider | Models | Purpose |
|----------|--------|---------|
| **Anthropic** | Claude Sonnet 4.5 | Frontier mode orchestrator, legal reasoning |
| **OpenAI** | GPT-4o-mini, text-embedding-3-small | Lightning mode orchestrator, embeddings |
| **Perplexity** | sonar-pro | Real-time internet research (Frontier mode only) |

### **Infrastructure & Services**
| Service | Purpose |
|---------|---------|
| **DigitalOcean App Platform** | Application hosting (backend + frontend) |
| **DigitalOcean Managed PostgreSQL** | Production database |
| **DigitalOcean Managed Redis** | Production cache |
| **DigitalOcean Spaces** | S3-compatible object storage for file uploads |
| **Docker Compose** | Local development environment |
| **Google OAuth** | Authentication |
| **Stripe** | Payment processing and subscriptions |

---

---

## 🏗️ **System Architecture**

### **High-Level Overview**

```
┌─────────────────────────────────────────────────────────────────┐
│                         User Interface                          │
│                    (Next.js 14 + TypeScript)                    │
│  - Chat Interface  - Admin Panel  - Subscription Management     │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│                      FastAPI Backend                            │
│  - REST API  - Server-Sent Events  - Google OAuth  - Stripe    │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│                   V7 AI Orchestration Engine                    │
│                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐        │
│  │Query Analyzer│→ │Tool Executor │→ │Response Synth│        │
│  └──────────────┘  └──────────────┘  └──────────────┘        │
│                                                                 │
│  Tools: RAG Search | Perplexity | Legal Reasoning              │
└────────────────────────────┬────────────────────────────────────┘
                             │
                ┌────────────┼────────────┐
                ↓            ↓            ↓
         ┌──────────┐  ┌──────────┐  ┌──────────┐
         │PostgreSQL│  │ Weaviate │  │  Redis   │
         │(Core Data)│  │(Vectors) │  │ (Cache)  │
         └──────────┘  └──────────┘  └──────────┘
                             │
                ┌────────────┼────────────┐
                ↓            ↓            ↓
         ┌──────────┐  ┌──────────┐  ┌──────────┐
         │ Claude   │  │ OpenAI   │  │Perplexity│
         │Sonnet 4.5│  │GPT-4o-mini│  │sonar-pro │
         └──────────┘  └──────────┘  └──────────┘
```

### **V7 Orchestration Flow**

1. **Query Analysis** - Detect if query needs RAG, internet research, or legal reasoning
2. **Tool Selection** - Choose required tools based on analysis and selected mode (Frontier/Lightning)
3. **Tool Execution** - Execute tools in parallel/sequence with context passing
4. **Response Synthesis** - Combine results with inline citations and source attribution
5. **Quality Validation** - Verify response quality and completeness
6. **Usage Tracking** - Deduct appropriate questions (0.5 or 1.0) based on mode

**See**: [docs/architecture/ai-orchestration.md](docs/architecture/ai-orchestration.md) for complete details

---

---

## 📚 **Documentation**

Comprehensive documentation is available in the `docs/` directory.

### **Architecture Documentation** (`docs/architecture/`)

| Document | Description |
|----------|-------------|
| **[system-overview.md](docs/architecture/system-overview.md)** | High-level system architecture, component interactions, data flow |
| **[ai-orchestration.md](docs/architecture/ai-orchestration.md)** | V7.2 dual-mode orchestration, tool coordination, response synthesis |
| **[database-schema.md](docs/architecture/database-schema.md)** | Complete database schema, relationships, indexes |
| **[multimodal-rag-architecture.md](docs/architecture/multimodal-rag-architecture.md)** | Dual RAG system, document processing, quality validation |
| **[authentication-system.md](docs/architecture/authentication-system.md)** | Google OAuth implementation, JWT tokens, role-based access |
| **[master-system-message.md](docs/architecture/master-system-message.md)** | AI system prompts for Frontier and Lightning modes |
| **[PROJECT_STRUCTURE.md](docs/architecture/PROJECT_STRUCTURE.md)** | Codebase organization, file structure, key components |

### **UI/UX Features** (`docs/ui-ux-features/`)

| Document | Description |
|----------|-------------|
| **[README.md](docs/ui-ux-features/README.md)** | Complete UI/UX documentation index |
| **[01_AI_ORCHESTRATION_OVERVIEW.md](docs/ui-ux-features/01_AI_ORCHESTRATION_OVERVIEW.md)** | Core orchestration system from user perspective |
| **[02_MODE_SELECTOR.md](docs/ui-ux-features/02_MODE_SELECTOR.md)** | Lightning vs Frontier mode selection UI |
| **[03_RAG_SYSTEM.md](docs/ui-ux-features/03_RAG_SYSTEM.md)** | Dual RAG architecture and document upload |
| **[04_QUESTION_ALLOWANCE.md](docs/ui-ux-features/04_QUESTION_ALLOWANCE.md)** | Fractional question deduction system |

### **User Scenarios** (`docs/user-scenarios/`)

| Document | Description |
|----------|-------------|
| **[00_USER_SCENARIOS_INDEX.md](docs/user-scenarios/00_USER_SCENARIOS_INDEX.md)** | Complete index of user scenarios |
| **[01_BUILDING_PERMITS.md](docs/user-scenarios/01_BUILDING_PERMITS.md)** | Building permit application scenarios |

### **Admin Documentation** (`docs/admin/`)

| Document | Description |
|----------|-------------|
| **[admin-guide.md](docs/admin/admin-guide.md)** | Complete admin panel usage guide |

### **Operations** (`docs/agent_manuals/`)

| Document | Description |
|----------|-------------|
| **[operations-startup-guide.md](docs/agent_manuals/operations-startup-guide.md)** | How to start the application |
| **[operations-stop-guide.md](docs/agent_manuals/operations-stop-guide.md)** | How to stop the application |

### **Production Deployment**

| Document | Description |
|----------|-------------|
| **[PRODUCTION_DEPLOYMENT_GUIDE.md](docs/PRODUCTION_DEPLOYMENT_GUIDE.md)** | Complete DigitalOcean deployment guide |
| **[SECURITY_AUDIT_REPORT.md](docs/audits/SECURITY_AUDIT_REPORT.md)** | Security audit findings and recommendations |
| **[GITHUB_SETUP_GUIDE.md](docs/GITHUB_SETUP_GUIDE.md)** | GitHub repository setup and sync workflow |

---

---

## 🚀 **Quick Start**

### **Prerequisites**

- **Docker & Docker Compose** - For local database services
- **Node.js 18+** - For frontend development
- **Python 3.11+** - For backend development
- **API Keys** - OpenAI, Anthropic, Perplexity, Stripe, Google OAuth

### **Local Development Setup**

**1. Clone the repository**
```bash
git clone https://github.com/talirezun/moj-ai-v11.git
cd moj-ai-v11
```

**2. Start database services**
```bash
docker-compose up -d postgres redis weaviate
```

**3. Configure environment variables**
```bash
# Backend
cp backend/.env.example backend/.env
# Edit backend/.env with your API keys

# Frontend
cp frontend/.env.example frontend/.env.local
# Edit frontend/.env.local with your configuration
```

**4. Start backend**
```bash
cd backend
source venv/bin/activate
export PYTHONPATH=/path/to/moj-ai-v11/backend
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

**5. Start frontend**
```bash
cd frontend
npm install
npm run dev
```

**6. Access the application**
- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:8000
- **API Docs**: http://localhost:8000/docs

### **Test Accounts**

| Email | Role | Purpose |
|-------|------|---------|
| `produkcija97@gmail.com` | Admin | Full admin access, configuration management |
| `blocklabstech@gmail.com` | User | Standard user testing |

**See**: [docs/agent_manuals/operations-startup-guide.md](docs/agent_manuals/operations-startup-guide.md) for detailed instructions

---

---

## 💡 **Use Cases & Examples**

### **Example 1: Building Permit Application (Frontier Mode)**

**User Query**: "I want to build a 4-unit residential building in Ljubljana's Bežigrad district. What permits do I need, what are the costs, and how long will it take?"

**AI Response** (10,000+ characters):
- Complete list of required permits (building permit, location permit, etc.)
- Step-by-step application process
- Current fees and costs (from Perplexity real-time research)
- Timeline estimates
- Required documentation
- Relevant legal articles from RAG (Zakon, Pravilnik, OPN_MOL)
- Court cases and precedents
- 20+ clickable sources

**Processing**: 90 seconds | **Cost**: 1.0 questions

### **Example 2: Quick Legalization Question (Lightning Mode)**

**User Query**: "Can I legalize a 15m² extension to my house built in 2018?"

**AI Response** (2,500 characters):
- Yes/No answer with conditions
- Key legal requirements
- Simplified procedure steps
- Estimated costs
- 4-5 relevant sources from RAG

**Processing**: 25 seconds | **Cost**: 0.5 questions

### **Example 3: Document-Specific Analysis (User RAG)**

**User Action**: Uploads building inspection decision PDF

**User Query**: "What violations are mentioned in this document and how do I fix them?"

**AI Response**:
- Extracts specific violations from uploaded document
- Cross-references with legal requirements from Admin RAG
- Provides remediation steps
- Cites both uploaded document and legal framework

---

## 🔧 **Development**

### **Project Structure**

```
moj-ai-v11/
├── backend/                    # FastAPI backend
│   ├── app/
│   │   ├── api/v1/            # API endpoints
│   │   ├── services/          # Business logic
│   │   │   ├── orchestration_v7/  # V7 orchestration engine
│   │   │   ├── rag_service_multimodal.py  # RAG system
│   │   │   ├── storage_service.py  # DigitalOcean Spaces
│   │   │   └── ...
│   │   ├── models/            # Database models
│   │   ├── schemas/           # Pydantic schemas
│   │   └── core/              # Config, auth, database
│   ├── alembic/               # Database migrations
│   └── .env.example           # Environment template
│
├── frontend/                   # Next.js frontend
│   ├── app/                   # Pages (App Router)
│   │   ├── admin/             # Admin panel pages
│   │   ├── dashboard/         # User dashboard
│   │   └── ...
│   ├── components/            # React components
│   │   ├── chat/              # Chat interface components
│   │   ├── admin/             # Admin components
│   │   └── ...
│   ├── contexts/              # React contexts (auth, usage, language)
│   ├── hooks/                 # Custom hooks (useChat, useAuth, etc.)
│   └── lib/                   # Utilities and API client
│
├── docs/                      # Documentation
│   ├── architecture/          # Architecture docs
│   ├── ui-ux-features/        # UI/UX documentation
│   ├── user-scenarios/        # User scenarios
│   ├── admin/                 # Admin guides
│   └── agent_manuals/         # Operations guides
│
├── training-files/            # 7 core legal PDFs
│   ├── Pravilnik_o_projektni_in_drugi_dokumentaciji_PISRS.pdf
│   ├── TSG-V-006_2022_razvrscanje_objektov.pdf
│   ├── Uredba_o_razvrscanju_objektov_PISRS.pdf
│   ├── Zakon_o_urejanju_prostora_ZUreP-3_PISRS.pdf
│   ├── 220528_OPN_MOM.pdf
│   ├── GZ 1.pdf
│   └── Odlok_OPN_MOL_ID.pdf
│
├── docker-compose.yml         # Local development services
├── LICENSE                    # Proprietary license
└── README.md                  # This file
```

### **Key Files**

| File | Purpose |
|------|---------|
| `backend/app/main.py` | FastAPI application entry point |
| `backend/app/services/orchestration_v7/orchestration_engine_v7.py` | V7 orchestration engine |
| `backend/app/services/rag_service_multimodal.py` | Dual RAG system |
| `backend/app/services/storage_service.py` | DigitalOcean Spaces integration |
| `frontend/hooks/useChat.ts` | Chat logic with SSE streaming |
| `frontend/components/chat/ModeSelector.tsx` | Frontier/Lightning mode selector |
| `frontend/components/chat/QuestionAllowanceDisplay.tsx` | Question balance display |

---

## 🔧 **Development**

### **Project Structure**
```
moj-ai-v11/
├── backend/
│   ├── minimal_backend.py          # Main production server
│   ├── app/                        # FastAPI application
│   │   ├── services/               # Business logic (orchestration, RAG, etc.)
│   │   ├── models/                 # Database models
│   │   └── api/v1/                 # API endpoints
│   └── requirements.txt            # Python dependencies
├── frontend/
│   ├── app/                        # Next.js pages (App Router)
│   ├── components/                 # React components
│   ├── contexts/                   # State management
│   ├── hooks/                      # Custom hooks
│   └── lib/                        # Utilities and API client
├── docs/                           # Complete documentation
│   ├── architecture/               # Architecture docs
│   │   └── v7/                     # V7 orchestration specs
│   ├── admin/                      # Admin guides
│   └── archive/                    # Historical docs
├── training-files/                 # RAG documents (Slovenian legislation)
└── docker-compose.yml              # Service orchestration
```

### **Key Files**
- **Backend**: `backend/minimal_backend.py` - Main server
- **Frontend**: `frontend/hooks/useChat.ts` - Chat logic with SSE
- **V7 Specs**: `docs/architecture/v7/` - Complete V7 documentation
- **Database Models**: `backend/app/models/` - All data models

---

---

## 📊 **Current Status**

### **✅ Production Ready**

| Component | Status | Notes |
|-----------|--------|-------|
| **V7.2 Dual-Mode Orchestration** | ✅ Operational | Frontier + Lightning modes fully tested |
| **Dual RAG System** | ✅ Operational | Admin RAG (7 PDFs) + User RAG working |
| **Google OAuth** | ✅ Operational | Secure authentication with role-based access |
| **Stripe Integration** | ✅ Operational | Subscriptions, question purchases, webhooks |
| **PostgreSQL Database** | ✅ Operational | All tables, indexes, relationships |
| **Weaviate Vector DB** | ✅ Operational | Semantic search, embeddings |
| **Redis Cache** | ✅ Operational | Session management, caching |
| **Admin Panel** | ✅ Operational | Full configuration management |
| **Real-Time Progress** | ✅ Operational | Server-Sent Events streaming |
| **Question Allowance** | ✅ Operational | Fractional deduction (0.5/1.0) |
| **File Upload** | ✅ Operational | Quality validation, multimodal processing |
| **Mobile Responsive** | ✅ Operational | Works on all devices |
| **Security Audit** | ✅ Complete | No hardcoded secrets, SQL injection safe |
| **DigitalOcean Spaces** | ✅ Ready | Storage service implemented |

### **🚀 Ready for Deployment**

- ✅ Security audit passed
- ✅ Environment configuration templates created
- ✅ DigitalOcean Spaces integration complete
- ✅ GitHub sync workflow configured
- ✅ Production deployment guide written
- ⏳ Awaiting API key rotation and final testing

---

## 📄 **License**

This project is proprietary software owned by Skupina Svetilnik d.o.o.

Copyright (c) 2025 Skupina Svetilnik d.o.o. All rights reserved.

See [LICENSE](LICENSE) file for full license terms.

---

## 🚀 **Production Deployment**

### **Infrastructure**
- **Platform**: DigitalOcean App Platform
- **Domain**: app.mojai.xyz (configured via Cloudflare)
- **Database**: DigitalOcean Managed PostgreSQL
- **Cache**: DigitalOcean Managed Redis
- **Vector DB**: Weaviate on DigitalOcean Droplet
- **Object Storage**: DigitalOcean Spaces (for file uploads)

### **Environment Configuration**
- See `backend/.env.example` for required environment variables
- See `frontend/.env.example` for frontend configuration
- All secrets managed via DigitalOcean App Platform environment variables

### **Security**
- ✅ No hardcoded secrets in codebase
- ✅ SQL injection protection via SQLAlchemy ORM
- ✅ XSS protection via input validation middleware
- ✅ CORS configured for production domain
- ✅ Rate limiting implemented (60 req/min, 1000 req/hour)
- ✅ JWT-based authentication with Google OAuth
- ⚠️ Rotate all API keys before production deployment

See [docs/SECURITY_AUDIT_REPORT.md](docs/SECURITY_AUDIT_REPORT.md) for complete security audit.

### **Deployment Checklist**
- [ ] Rotate all API keys (OpenAI, Anthropic, Perplexity, Stripe, Google OAuth)
- [ ] Generate strong JWT_SECRET and SECRET_KEY
- [ ] Change PostgreSQL password from default
- [ ] Configure DigitalOcean Managed Databases
- [ ] Set up DigitalOcean Spaces for file uploads
- [ ] Update CORS_ORIGINS to production domain only
- [ ] Set ENVIRONMENT=production and DEBUG=false
- [ ] Configure SSL for all database connections
- [ ] Set up monitoring with Sentry
- [ ] Test all features in staging environment

---

---

## 🏢 **Company**

**Skupina Svetilnik d.o.o.**
Cesta 24. Junija 25
1000 Ljubljana, Slovenia

**Production URL**: [app.mojai.xyz](https://app.mojai.xyz)

---

## 📞 **Contact & Support**

For inquiries about:
- **Licensing**: See [LICENSE](LICENSE) file
- **Partnerships**: Contact via GitHub
- **Technical Support**: Review documentation first
- **Private Repository Access**: Contact repository owner

---

## 🙏 **Acknowledgments**

Built with:
- **Anthropic Claude** - Frontier mode orchestration
- **OpenAI GPT-4** - Lightning mode orchestration and embeddings
- **Perplexity** - Real-time internet research
- **Weaviate** - Vector database
- **DigitalOcean** - Infrastructure
- **Stripe** - Payment processing
- **Next.js & FastAPI** - Application frameworks

---

**Last Updated**: October 16, 2025 | **Version**: v7.2.1 | **Status**: Production Ready
