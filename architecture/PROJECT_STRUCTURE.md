# 📁 Moj AI Project Structure

**Last Updated**: October 6, 2025  
**Status**: ✅ Clean and Organized

---

## 🎯 **Overview**

This document provides a complete overview of the Moj AI project structure after the October 6, 2025 cleanup. The project is now organized with clear separation between active code, documentation, and archived materials.

---

## 📂 **Root Directory**

```
moj-ai-v11/
├── README.md                   # Main project README (V7 status)
├── docker-compose.yml          # Service orchestration (PostgreSQL, Redis, Weaviate)
├── backend/                    # FastAPI backend application
├── frontend/                   # Next.js frontend application
├── docs/                       # Complete documentation
├── training-files/             # RAG documents (Slovenian legislation PDFs)
├── fixtures/                   # Test fixtures
├── docker/                     # Docker configurations
├── tmp/                        # Temporary files (cleaned)
└── venv/                       # Root virtual environment
```

**Key Points**:
- ✅ Only 1 README file at root (consolidated)
- ✅ No operational guides at root (moved to docs/)
- ✅ No test files at root (archived)
- ✅ Clean, minimal structure

---

## 🔧 **Backend Directory**

```
backend/
├── minimal_backend.py          # Main production server (FastAPI)
├── requirements.txt            # Python dependencies
├── alembic.ini                 # Database migration config
├── alembic/                    # Database migrations
│   ├── env.py
│   ├── versions/               # Migration scripts
│   └── ...
├── app/                        # Main application code
│   ├── __init__.py
│   ├── main.py                 # FastAPI app initialization
│   ├── api/                    # API endpoints
│   │   └── v1/                 # API v1 routes
│   ├── models/                 # SQLAlchemy database models
│   ├── schemas/                # Pydantic schemas
│   ├── services/               # Business logic
│   │   ├── orchestration/      # AI orchestration (V6 - to be replaced with V7)
│   │   ├── rag/                # RAG system
│   │   ├── stripe/             # Stripe integration
│   │   └── ...
│   ├── core/                   # Core utilities
│   ├── config/                 # Configuration
│   ├── database/               # Database connection
│   ├── middleware/             # FastAPI middleware
│   └── utils/                  # Utility functions
├── uploads/                    # User-uploaded files
│   ├── admin/                  # Admin uploads
│   ├── rag/                    # RAG documents
│   └── [user-id]/              # User-specific uploads
└── venv/                       # Virtual environment (Python 3.11+)
```

**Key Points**:
- ✅ No utility scripts at root (archived)
- ✅ No test files (archived)
- ✅ Only one venv directory
- ✅ Clean, production-ready structure

**Archived**:
- `add_master_system_message.py` → `docs/archive/backend-scripts/`
- `fix_api_keys_encryption.py` → `docs/archive/backend-scripts/`
- `update_master_system_message.py` → `docs/archive/backend-scripts/`
- `scripts/` → `docs/archive/backend-scripts/scripts/`
- `tests/` → `docs/archive/backend-tests/`

---

## 🎨 **Frontend Directory**

```
frontend/
├── package.json                # Node.js dependencies
├── next.config.js              # Next.js configuration
├── tsconfig.json               # TypeScript configuration
├── tailwind.config.js          # Tailwind CSS configuration
├── playwright.config.ts        # Playwright test configuration
├── app/                        # Next.js App Router pages
│   ├── page.tsx                # Home page
│   ├── chat/                   # Chat interface
│   ├── admin/                  # Admin panel
│   ├── pricing/                # Pricing pages
│   └── ...
├── components/                 # React components
│   ├── ui/                     # Shadcn/ui components
│   ├── chat/                   # Chat components
│   ├── admin/                  # Admin components
│   └── ...
├── contexts/                   # React contexts
│   ├── AuthContext.tsx         # Authentication context
│   └── ...
├── hooks/                      # Custom React hooks
│   ├── useChat.ts              # Chat logic with SSE
│   └── ...
├── lib/                        # Utilities and API client
│   ├── api.ts                  # API client
│   └── ...
├── public/                     # Static assets
│   ├── images/
│   └── ...
├── styles/                     # Global styles
│   └── globals.css
└── node_modules/               # Node.js dependencies
```

**Key Points**:
- ✅ No test directories (archived)
- ✅ No test results (removed)
- ✅ Clean, production-ready structure
- ✅ Playwright config kept for future testing

**Archived**:
- `tests/` → `docs/archive/frontend-tests/`
- `test-results/` → removed (artifacts)

---

## 📚 **Documentation Directory**

```
docs/
├── CLEANUP_SUMMARY.md                  # This cleanup summary
├── PROJECT_STRUCTURE.md                # This file
├── operations-startup-guide.md         # How to start the application
├── operations-stop-guide.md            # How to stop the application
├── master-system-message.md            # System message configuration
├── admin/
│   └── admin-guide.md                  # Admin panel usage guide
├── architecture/
│   ├── system-overview.md              # High-level system architecture
│   ├── database-schema.md              # Complete database schema
│   ├── multimodal-rag-architecture.md  # RAG system architecture
│   └── v7/                             # V7 Orchestration Specifications
│       ├── README.md                   # V7 overview
│       ├── V7_ARCHITECTURE_OVERVIEW.md # Complete V7 architecture
│       ├── V7_QUERY_ANALYZER_SPEC.md   # Query analyzer specification
│       ├── V7_RAG_INTEGRATION.md       # RAG integration details
│       ├── V7_TOOL_SPECIFICATIONS.md   # All 6 tools specifications
│       ├── V7_RESPONSE_SYNTHESIS.md    # Response synthesis algorithm
│       ├── V7_IMPLEMENTATION_PLAN.md   # Step-by-step implementation guide
│       ├── V7_TESTING_STRATEGY.md      # Comprehensive testing approach
│       └── V7_HANDOFF_TO_NEXT_AGENT.md # Complete handoff document
└── archive/                            # Historical documentation
    ├── backend-scripts/                # Old backend utility scripts
    ├── backend-tests/                  # Old backend tests
    ├── frontend-tests/                 # Old frontend tests
    ├── playwright-tests/               # Old E2E tests
    ├── root-archive/                   # Old root-level docs
    ├── v6/                             # V6 orchestration docs
    ├── handoffs/                       # Historical handoffs
    └── phases/                         # Historical phase summaries
```

**Key Points**:
- ✅ No README in docs/ (consolidated into main README)
- ✅ No README at docs/architecture/ (removed redundancy)
- ✅ Operational guides moved from root to docs/
- ✅ Clear separation: active docs vs archived docs
- ✅ V7 specs organized under architecture/v7/

**Active Documentation** (15 files):
1. `docs/CLEANUP_SUMMARY.md`
2. `docs/PROJECT_STRUCTURE.md`
3. `docs/operations-startup-guide.md`
4. `docs/operations-stop-guide.md`
5. `docs/master-system-message.md`
6. `docs/admin/admin-guide.md`
7. `docs/architecture/system-overview.md`
8. `docs/architecture/database-schema.md`
9. `docs/architecture/multimodal-rag-architecture.md`
10. `docs/architecture/v7/README.md`
11. `docs/architecture/v7/V7_ARCHITECTURE_OVERVIEW.md`
12. `docs/architecture/v7/V7_QUERY_ANALYZER_SPEC.md`
13. `docs/architecture/v7/V7_RAG_INTEGRATION.md`
14. `docs/architecture/v7/V7_TOOL_SPECIFICATIONS.md`
15. `docs/architecture/v7/V7_RESPONSE_SYNTHESIS.md`
16. `docs/architecture/v7/V7_IMPLEMENTATION_PLAN.md`
17. `docs/architecture/v7/V7_TESTING_STRATEGY.md`
18. `docs/architecture/v7/V7_HANDOFF_TO_NEXT_AGENT.md`

**Archived Documentation** (50+ files):
- All V6 orchestration documentation
- Historical handoff documents
- Old test files and scripts
- Phase summaries
- Root-level historical docs

---

## 📦 **Training Files Directory**

```
training-files/
├── 220528_OPN_MOM.pdf                                          # Ljubljana building regulations
├── Odlok_OPN_MOL_ID.pdf                                        # Ljubljana spatial plan
├── Pravilnik o projektni dokumentaciji - *.txt                 # Building documentation regulations
├── Pravilnik o projektni in drugi dokumentaciji *.pdf          # Building documentation rules
├── Uredba o razvrščanju objektov (PISRS).pdf                   # Building classification regulation
└── Zakon o urejanju prostora (ZUreP-3) (PISRS).pdf            # Spatial planning law
```

**Key Points**:
- ✅ Slovenian building legislation documents
- ✅ Used by RAG system for legal queries
- ✅ PDFs and text files
- ✅ Uploaded by admins via admin UI

---

## 🐳 **Docker Directory**

```
docker/
└── postgres/
    └── init.sql                # PostgreSQL initialization script
```

**Key Points**:
- ✅ Docker configurations for services
- ✅ PostgreSQL initialization

---

## 🧪 **Fixtures Directory**

```
fixtures/
└── rag/
    └── [test documents]        # Test documents for RAG system
```

**Key Points**:
- ✅ Test fixtures for development
- ✅ RAG test documents

---

## 📊 **File Statistics**

### **Active Files**
- **Root**: 1 file (README.md)
- **Backend**: 1 main file (minimal_backend.py) + app/ directory
- **Frontend**: Essential files only (no tests)
- **Docs**: 18 active documentation files
- **Training Files**: 11 Slovenian legislation documents

### **Archived Files**
- **Backend Scripts**: 4 files
- **Backend Tests**: 5 files
- **Frontend Tests**: 4 files
- **Playwright Tests**: 2 files
- **Root Archive**: 30+ files
- **V6 Docs**: 6 files
- **Handoffs**: 7 files
- **Phases**: 3 files

**Total Archived**: 60+ files (all preserved, none deleted)

---

## 🎯 **Navigation Guide**

### **For New Developers**
1. Start with `README.md` (project overview with complete documentation links)
2. Review `docs/architecture/system-overview.md` (system architecture)
3. Study `docs/architecture/v7/` (V7 orchestration specs)
4. Check `docs/PROJECT_STRUCTURE.md` (this file) for project organization

### **For Implementation Agent**
1. **START HERE**: `docs/architecture/v7/V7_HANDOFF_TO_NEXT_AGENT.md`
2. Follow `docs/architecture/v7/V7_IMPLEMENTATION_PLAN.md`
3. Reference other V7 specs as needed

### **For Operations**
1. Use `docs/operations-startup-guide.md` to start the app
2. Use `docs/operations-stop-guide.md` to stop the app
3. Reference `docs/admin/admin-guide.md` for admin tasks

### **For Admins**
1. Read `docs/admin/admin-guide.md`
2. Configure system via admin UI
3. Upload RAG documents via admin UI

---

## ✅ **Verification Checklist**

- [x] Only 1 README at root (no docs/README.md)
- [x] No operational guides at root
- [x] No test files at root
- [x] Backend has only essential files
- [x] Frontend has only essential files
- [x] Docs are organized with clear hierarchy
- [x] V7 specs are under architecture/v7/
- [x] All old files are archived (not deleted)
- [x] Archive is organized by category
- [x] Documentation structure in main README
- [x] All documentation links updated

---

## 🔄 **Maintenance**

### **Adding New Documentation**
- Active docs go in `docs/` or appropriate subdirectory
- Historical docs go in `docs/archive/` with appropriate category

### **Adding New Code**
- Backend code goes in `backend/app/`
- Frontend code goes in `frontend/`
- Tests go in appropriate test directories (will be created when needed)

### **Archiving Old Files**
- Move to `docs/archive/` with appropriate category
- Update this document
- Update `docs/CLEANUP_SUMMARY.md`

---

**Last Updated**: October 6, 2025  
**Status**: ✅ Clean and Organized  
**Next Steps**: V7 Implementation

