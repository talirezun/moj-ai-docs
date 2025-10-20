# Moj AI - Complete Capabilities Overview

**What Users and Admins Can Do**  
**Last Updated:** 2025-10-20  
**Version:** 1.0.0

---

## 🎯 **EXECUTIVE SUMMARY**

Moj AI is a comprehensive AI-powered assistant for navigating Slovenian building regulations, construction law, and real estate development. This document provides a complete overview of all capabilities for both **users** and **admins**.

---

## 👥 **USER CAPABILITIES**

### **What Users Can Do**

Users can leverage Moj AI for any building-related question or project in Slovenia, from simple permit inquiries to complex multi-factor development scenarios.

---

### **1. Ask Questions About Building Regulations**

**Capabilities:**
- ✅ Building permit requirements and processes
- ✅ Zoning and spatial planning regulations
- ✅ Environmental compliance requirements
- ✅ Safety and accessibility standards
- ✅ Municipal ordinances (Ljubljana, Maribor, etc.)
- ✅ Construction timelines and costs
- ✅ Legal compliance verification

**Example Questions:**
```
"How do I get a building permit in Ljubljana for a two-family house?"
"What are the setback requirements for residential buildings?"
"Can I convert my garage into a living space?"
"What are current construction costs in Slovenia?"
```

**Modes Available:**
- **Lightning (⚡):** Quick answers, 15-30s, 0.5 questions
- **Frontier (🚀):** Comprehensive analysis, 60-240s, 1.0 questions

---

### **2. Upload and Analyze Documents**

**Capabilities:**
- ✅ Upload PDFs, DOC/DOCX, TXT files (max 10MB)
- ✅ OCR for scanned documents
- ✅ Table extraction and analysis
- ✅ Multi-document comparison
- ✅ Conversation-specific document storage

**Supported Document Types:**
- Architectural plans and blueprints
- Property deeds and ownership documents
- Existing permits and approvals
- Contracts and agreements
- Technical specifications
- Survey reports
- Environmental assessments

**Example Use Cases:**
```
"Analyze my architectural plans and check compliance with regulations"
"Review this contract and highlight any issues"
"Compare my property deed with zoning requirements"
"What permits do I need based on these blueprints?"
```

---

### **3. Get Comprehensive Market Research**

**Capabilities (Frontier Mode Only):**
- ✅ Current construction costs (€/m²)
- ✅ Real estate market trends
- ✅ Property valuation estimates
- ✅ Investment analysis
- ✅ Economic feasibility studies
- ✅ ROI calculations

**Example Questions:**
```
"What are current construction costs for a 150m² house in Ljubljana?"
"What are real estate market trends in Slovenia for 2024-2025?"
"Is it economically feasible to build a 4-unit residential building in Maribor?"
"What is the expected ROI for converting residential to commercial?"
```

**Data Sources:**
- 🌐 Real-time internet research via Perplexity
- 📊 Market data and statistics
- 📈 Trend analysis
- 💰 Cost databases

---

### **4. Manage Conversations**

**Capabilities:**
- ✅ Create unlimited conversations
- ✅ Rename conversations
- ✅ Delete conversations
- ✅ Search conversations
- ✅ Organize by project

**Best Practices:**
```
Conversation Names:
- "Ljubljana Apartment Project"
- "Maribor Extension Permit"
- "Commercial Conversion Research"
- "Building Code Questions"
```

---

### **5. Track Question Usage**

**Capabilities:**
- ✅ View question balance (Premium, Bonus, Free)
- ✅ See next question cost (0.5 vs 1.0)
- ✅ Monitor usage history
- ✅ Get low balance warnings
- ✅ Purchase more questions via Stripe

**Question Types:**
- 💎 **Premium:** Purchased questions (never expire)
- 🎁 **Bonus:** Admin-granted questions (may expire)
- 🆓 **Free:** Welcome bonus (5 questions for new users)

**Pricing:**
- 15 questions: €20.00 (€1.33/question)
- 50 questions: €50.00 (€1.00/question)
- 100 questions: €75.00 (€0.75/question)

---

### **6. View AI Orchestration Details**

**Capabilities:**
- ✅ See which tools were used (RAG, Perplexity, Legal Reasoning)
- ✅ View processing time and metrics
- ✅ Check confidence scores
- ✅ Review sources and citations
- ✅ Understand response quality

**Orchestration Report Includes:**
- 🤖 Model used (GPT-4o or higher or Claude Sonnet 4 or higher)
- 🔧 Tools engaged (RAG, Perplexity, Legal, Quality)
- ⏱️ Processing time
- 📄 Response length
- 💰 Question cost
- 🎯 Complexity level
- 📊 Confidence score
- 📚 Sources count

---

### **7. Access on Any Device**

**Capabilities:**
- ✅ Desktop web browser
- ✅ Mobile web browser (responsive)
- ✅ Tablet optimized
- ✅ Touch-friendly interface
- ✅ Voice input (beta)

---

## 🔧 **ADMIN CAPABILITIES**

### **What Admins Can Do**

Admins have complete control over AI orchestration, knowledge base, users, and system configuration.

---

### **1. Configure AI Orchestration**

**Capabilities:**

**Master System Messages:**
- ✅ Configure main system message for all models
- ✅ Set Lightning mode behavior
- ✅ Set Frontier mode behavior
- ✅ Control response format and tone
- ✅ Define AI personality

**Model Selection:**
- ✅ Choose Lightning mode model (GPT-4o-mini, GPT-4o, Claude Haiku)
- ✅ Choose Frontier mode model (Claude Sonnet 4, GPT-5)
- ✅ Configure temperature and max tokens
- ✅ A/B test different models
- ✅ Monitor model performance

**API Key Management:**
- ✅ Add/update OpenAI API keys
- ✅ Add/update Anthropic API keys
- ✅ Add/update Perplexity API keys
- ✅ Automatic encryption (Fernet)
- ✅ Secure storage in PostgreSQL
- ✅ Usage monitoring and cost tracking

**Orchestration Strategy:**
- ✅ Enable/disable RAG search
- ✅ Enable/disable Perplexity (per mode)
- ✅ Configure legal reasoning
- ✅ Configure quality validation
- ✅ Set timeouts and limits
- ✅ Fine-tune tool coordination

---

### **2. Manage Knowledge Base (RAG)**

**Capabilities:**

**Upload Legal Documents:**
- ✅ Upload core legal PDFs
- ✅ OCR for scanned documents
- ✅ Table extraction
- ✅ Semantic chunking
- ✅ Automatic embedding generation
- ✅ Version control

**The multi-core legal Document excamples:**
1. Pravilnik - Regulations on building permits
2. TSG - Technical guidelines
3. Uredba - Government ordinances
4. Zakon - Law on construction
5. OPN_MOM - Municipal plan (Maribor)
6. GZ - Building Act
7. Odlok_OPN_MOL_ID - Municipal ordinance (Ljubljana)

**View & Verify:**
- ✅ View extracted text
- ✅ Inspect document chunks
- ✅ Check embedding quality
- ✅ Verify semantic coherence
- ✅ Quality metrics dashboard

**Test Retrieval:**
- ✅ Test RAG with sample queries
- ✅ View relevance scores
- ✅ Analyze result quality
- ✅ Identify gaps in knowledge base
- ✅ Optimize chunking strategy

**Delete & Replace:**
- ✅ Remove outdated documents
- ✅ Upload new versions
- ✅ Track document versions
- ✅ Rollback if needed

---

### **3. Manage Users**

**Capabilities:**

**View All Users:**
- ✅ Complete user list with activity
- ✅ Filter by role, status, usage
- ✅ Search by email or name
- ✅ Sort by various criteria
- ✅ Export user data

**Grant Bonus Questions:**
- ✅ Give users bonus questions
- ✅ Set expiration dates
- ✅ Add reason/note
- ✅ Send notification email
- ✅ Track bonus question usage

**Common Bonus Reasons:**
- Welcome bonus (5 questions)
- Referral reward (10 questions)
- Contest prize (20 - 50 questions)
- Beta testing (20 questions)

**Manage Roles:**
- ✅ Promote users to admin
- ✅ Demote admins to users
- ✅ Track role changes
- ✅ Audit admin access

**Monitor Activity:**
- ✅ View user usage statistics
- ✅ Track recent activity
- ✅ Identify unusual patterns
- ✅ Flag potential issues
- ✅ Monitor satisfaction

**Suspend/Ban Accounts:**
- ✅ Temporarily suspend users
- ✅ Permanently ban users
- ✅ Document reasons
- ✅ Preserve data for investigation

---

### **4. Analytics & Monitoring**

**Capabilities:**

**Usage Statistics:**
- ✅ Total users and active users
- ✅ Questions asked (total and by mode)
- ✅ Daily/weekly/monthly trends
- ✅ Mode distribution (Lightning vs Frontier)
- ✅ Peak usage hours
- ✅ User engagement metrics

**Cost Analysis:**
- ✅ Monthly API costs by provider
- ✅ Cost per query by mode
- ✅ Revenue from purchases
- ✅ Profit margin calculation
- ✅ Cost optimization opportunities
- ✅ Budget forecasting

**Model Performance:**
- ✅ Response time by model
- ✅ Response length by model
- ✅ User satisfaction by model
- ✅ Error rate by model
- ✅ Cost per query by model
- ✅ A/B test results

**Error Monitoring:**
- ✅ Error types and frequencies
- ✅ Recent error log
- ✅ Error rate alerts
- ✅ Root cause analysis
- ✅ Automated notifications
- ✅ Error resolution tracking

---

### **5. System Configuration**

**Capabilities:**

**Tenant Settings:**
- ✅ Configure tenant name and branding
- ✅ Set default mode
- ✅ Configure free questions for new users
- ✅ Set usage limits
- ✅ Configure file upload limits

**Email Notifications:**
- ✅ Configure user notifications
- ✅ Configure admin notifications
- ✅ Customize email templates
- ✅ Set notification triggers
- ✅ Test email delivery

**Feature Flags:**
- ✅ Enable/disable Lightning mode
- ✅ Enable/disable Frontier mode
- ✅ Enable/disable document upload
- ✅ Enable/disable Perplexity
- ✅ Enable/disable beta features
- ✅ Gradual feature rollout

**Pricing Configuration:**
- ✅ Configure question packages
- ✅ Set pricing tiers
- ✅ Configure Stripe integration
- ✅ Set up webhooks
- ✅ Test payment flow

---

## 📊 **CAPABILITY COMPARISON**

### **User vs Admin**

| Capability | User | Admin |
|------------|------|-------|
| Ask questions | ✅ | ✅ |
| Upload documents | ✅ | ✅ |
| View orchestration details | ✅ | ✅ |
| Purchase questions | ✅ | ❌ (unlimited) |
| Configure AI models | ❌ | ✅ |
| Manage knowledge base | ❌ | ✅ |
| Grant bonus questions | ❌ | ✅ |
| View all users | ❌ | ✅ |
| Analytics dashboard | ❌ | ✅ |
| System configuration | ❌ | ✅ |

---

## 🎯 **USE CASE CATEGORIES**

### **For Users**

**1. Building Permits (8 scenarios)**
- New residential buildings
- Multi-unit buildings
- Extensions and additions
- Legalizing unpermitted structures
- Commercial buildings
- Agricultural buildings
- Renovation permits
- Timeline and cost estimation

**2. Legal Compliance (10 scenarios)**
- Understanding building codes
- Zoning regulations
- Environmental compliance
- Safety requirements
- Accessibility standards
- Municipal ordinances
- Court cases and precedents
- Regulatory changes

**3. Renovation & Extension (12 scenarios)**
- Home extensions
- Garage conversions
- Attic conversions
- Basement conversions
- Residential to commercial
- Historic building renovations
- Energy efficiency upgrades
- Structural modifications

**4. New Construction (8 scenarios)**
- Single-family homes
- Multi-family buildings
- Commercial buildings
- Mixed-use developments
- Industrial buildings
- Agricultural structures
- Special use buildings
- Sustainable construction

**5. Document Analysis (6 scenarios)**
- Architectural plan review
- Contract analysis
- Technical specification review
- Permit document verification
- Multi-document comparison
- Compliance checking

**6. Market Research (6 scenarios)**
- Construction cost estimation
- Real estate market trends
- Property valuation
- Investment analysis
- Economic feasibility
- ROI calculation

**7. Complex Scenarios (5 scenarios)**
- Multi-factor projects
- Cross-jurisdictional issues
- Special use cases
- Problem-solving
- Troubleshooting

**8. Professional Use (8 scenarios)**
- For architects
- For engineers
- For developers
- For legal professionals
- For government officials
- For contractors
- For real estate agents
- For investors

---

### **For Admins**

**1. AI Orchestration**
- Configure system messages
- Select and test models
- Manage API keys
- Fine-tune orchestration
- Monitor performance

**2. Knowledge Base**
- Upload legal documents
- Verify extraction quality
- Test retrieval accuracy
- Update documents
- Version control

**3. User Management**
- View and search users
- Grant bonus questions
- Manage roles
- Monitor activity
- Handle issues

**4. Analytics**
- Track usage trends
- Analyze costs
- Compare models
- Monitor errors
- Generate reports

**5. Configuration**
- Tenant settings
- Email notifications
- Feature flags
- Pricing setup
- System maintenance

---

## 📚 **DOCUMENTATION REFERENCES**

**For Users:**
- [User Scenarios Index](user-scenarios/00_USER_SCENARIOS_INDEX.md)
- [Building Permits](user-scenarios/01_BUILDING_PERMITS.md)
- [UI/UX Features](ui-ux-features/)

**For Admins:**
- [Admin Capabilities Complete](admin/ADMIN_CAPABILITIES_COMPLETE.md)
- [Admin Guide](admin/admin-guide.md)
- [System Overview](architecture/system-overview.md)

**Technical:**
- [AI Orchestration](ui-ux-features/01_AI_ORCHESTRATION_OVERVIEW.md)
- [RAG System](ui-ux-features/03_RAG_SYSTEM.md)
- [Database Schema](architecture/database-schema.md)

---

## 🎓 **GETTING STARTED**

### **For New Users**
1. Sign up with Google OAuth
2. Receive 5 free questions
3. Try Lightning mode with simple question
4. Try Frontier mode with complex question
5. Upload a document for analysis
6. Purchase questions if needed

### **For New Admins**
1. Get admin access from existing admin
2. Review admin capabilities documentation
3. Configure AI orchestration settings
4. Upload/verify knowledge base documents
5. Test RAG retrieval quality
6. Monitor usage and costs

---

**Moj AI provides comprehensive capabilities for both users navigating Slovenian building regulations and admins managing the AI-powered system.**

