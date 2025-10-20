# RAG System - Retrieval Augmented Generation

**Last Updated:** 2025-10-20  
**Version:** 2.0 (Dual Architecture)

---

## 🎯 **OVERVIEW**

The RAG (Retrieval Augmented Generation) system is a critical component that grounds AI responses in authoritative legal documents, preventing hallucinations and ensuring accuracy. Moj AI uses a **dual RAG architecture** combining permanent admin-managed legal knowledge with temporary user-uploaded documents.

**Key Principle:** Every claim must be backed by a source - no hallucinations allowed.

---

## 🏗️ **DUAL RAG ARCHITECTURE**

### **Architecture Overview**

```
┌─────────────────────────────────────────────────────────┐
│                    RAG SYSTEM                           │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  ┌──────────────────┐      ┌──────────────────┐        │
│  │   Admin RAG      │      │    User RAG      │        │
│  │  (Permanent)     │      │  (Temporary)     │        │
│  ├──────────────────┤      ├──────────────────┤        │
│  │ Multi Core PDFs: │      │ User Uploads:    │        │
│  │ • Pravilnik      │      │ • PDFs           │        │
│  │ • TSG            │      │ • DOC/DOCX       │        │
│  │ • Uredba         │      │ • Text files     │        │
│  │ • Zakon          │      │ • Per convo      │        │
│  │ • OPN_MOM        │      │                  │        │
│  │ • GZ             │      │ Lifecycle:       │        │
│  │ • Odlok_OPN_MOL  │      │ • Conversation   │        │
│  │                  │      │   specific       │        │
│  │ Collection:      │      │ • Auto-deleted   │        │
│  │ AdminKnowledge   │      │   after 30 days  │        │
│  │ Base             │      │                  │        │
│  │                  │      │ Collection:      │        │
│  │ Scope:           │      │ UserKnowledge    │        │
│  │ • All users      │      │ Base_{conv_id}   │        │
│  │ • All tenants    │      │                  │        │
│  │ • Permanent      │      │                  │        │
│  └──────────────────┘      └──────────────────┘        │
│           │                         │                   │
│           └─────────┬───────────────┘                   │
│                     ↓                                   │
│            ┌─────────────────┐                          │
│            │  Weaviate DB    │                          │
│            │  Vector Search  │                          │
│            └─────────────────┘                          │
│                     ↓                                   │
│            ┌─────────────────┐                          │
│            │  Top 5-10 Chunks│                          │
│            │  Ranked by      │                          │
│            │  Relevance      │                          │
│            └─────────────────┘                          │
└─────────────────────────────────────────────────────────┘
```

---

## 📚 **ADMIN RAG (Permanent Knowledge Base)**

### **Purpose**

Provide authoritative Slovenian building regulations and legal framework to ALL users across ALL conversations.

### **Multi Core Legal Documents**

1. **Pravilnik** - Regulations on building permits
2. **TSG** - Technical guidelines for construction
3. **Uredba** - Government ordinances on construction
4. **Zakon** - Law on construction of buildings
5. **OPN_MOM** - Municipal spatial plan (Municipality of Maribor)
6. **GZ** - Building Act
7. **Odlok_OPN_MOL_ID** - Municipal ordinance (City of Ljubljana)
8. **Other Legal Documents**

**Total Pages:** ~5000+ pages of legal text  
**Total Chunks:** ~2,000+ semantic chunks  
**Languages:** Slovenian (primary)

### **Admin Management**

**Upload Process:**
1. Admin logs in to admin panel
2. Navigates to "RAG Management" section
3. Uploads PDF files (one at a time or bulk)
4. System processes each PDF:
   - Extracts text (including OCR for scanned PDFs)
   - Detects tables and preserves structure
   - Chunks text into semantic segments (500-1000 chars)
   - Generates embeddings using OpenAI text-embedding-3-small
   - Stores in Weaviate AdminKnowledgeBase collection
5. Admin can view, test, and delete documents

**Quality Verification:**
- Admin can view extracted text
- Admin can inspect document chunks
- Admin can test retrieval accuracy
- Admin can see what AI receives from RAG

### **Technical Implementation**

**File:** `backend/app/services/rag/admin_rag_service.py`

**Collection Schema:**
```python
{
  "class": "AdminKnowledgeBase",
  "properties": [
    {"name": "document_name", "dataType": ["text"]},
    {"name": "page_number", "dataType": ["int"]},
    {"name": "chunk_index", "dataType": ["int"]},
    {"name": "content", "dataType": ["text"]},
    {"name": "tenant_id", "dataType": ["text"]},
    {"name": "uploaded_by", "dataType": ["text"]},
    {"name": "uploaded_at", "dataType": ["date"]},
    {"name": "file_type", "dataType": ["text"]},
    {"name": "metadata", "dataType": ["text"]}  # JSON string
  ],
  "vectorizer": "none"  # We provide embeddings
}
```

**Embedding Process:**
```python
def create_embedding(text: str) -> List[float]:
    response = openai.Embedding.create(
        model="text-embedding-3-small",
        input=text
    )
    return response['data'][0]['embedding']
```

**Search Process:**
```python
def search_admin_rag(query: str, tenant_id: str, limit: int = 10):
    # Generate query embedding
    query_embedding = create_embedding(query)
    
    # Search Weaviate
    results = weaviate_client.query.get(
        "AdminKnowledgeBase",
        ["document_name", "page_number", "content", "chunk_index"]
    ).with_near_vector({
        "vector": query_embedding,
        "certainty": 0.7  # Minimum relevance threshold
    }).with_where({
        "path": ["tenant_id"],
        "operator": "Equal",
        "valueText": tenant_id
    }).with_limit(limit).do()
    
    return results
```

---

## 📄 **USER RAG (Temporary Document Upload)**

### **Purpose**

Allow users to upload conversation-specific documents for analysis alongside permanent legal knowledge.

### **Supported File Types**

- **PDF:** Including scanned PDFs with OCR
- **DOC/DOCX:** Microsoft Word documents
- **TXT:** Plain text files
- **Tables:** Extracted and preserved from all formats

### **User Upload Process**

1. User clicks "Upload Document" button in chat
2. Selects file from device
3. System validates file:
   - File size < 10MB
   - Supported format
   - Not corrupted
4. System processes file:
   - Extracts text (OCR if needed)
   - Detects and extracts tables
   - Chunks text semantically
   - Generates embeddings
   - Stores in conversation-specific collection
5. User sees confirmation: "Document uploaded: filename.pdf"
6. Document is now searchable in this conversation only

### **Lifecycle Management**

**Scope:**
- Documents are tied to specific conversation
- Only accessible within that conversation
- Not shared across conversations or users

**Retention:**
- Documents stored for 30 days after conversation creation
- Auto-deleted after 30 days
- User can manually delete anytime

**Collection Naming:**
```
UserKnowledgeBase_{conversation_id}
```

Example: `UserKnowledgeBase_a1b2c3d4-e5f6-7890-abcd-ef1234567890`

### **Technical Implementation**

**File:** `backend/app/services/rag/user_rag_service.py`

**Collection Schema:**
```python
{
  "class": f"UserKnowledgeBase_{conversation_id}",
  "properties": [
    {"name": "document_name", "dataType": ["text"]},
    {"name": "page_number", "dataType": ["int"]},
    {"name": "chunk_index", "dataType": ["int"]},
    {"name": "content", "dataType": ["text"]},
    {"name": "user_id", "dataType": ["text"]},
    {"name": "conversation_id", "dataType": ["text"]},
    {"name": "uploaded_at", "dataType": ["date"]},
    {"name": "file_type", "dataType": ["text"]},
    {"name": "metadata", "dataType": ["text"]}
  ],
  "vectorizer": "none"
}
```

**Upload Handler:**
```python
async def upload_user_document(
    file: UploadFile,
    conversation_id: str,
    user_id: str
):
    # Validate file
    validate_file(file)
    
    # Extract text
    text = extract_text(file)
    
    # Chunk text
    chunks = chunk_text(text)
    
    # Create collection if not exists
    create_user_collection(conversation_id)
    
    # Store chunks
    for i, chunk in enumerate(chunks):
        embedding = create_embedding(chunk)
        store_chunk(
            collection=f"UserKnowledgeBase_{conversation_id}",
            chunk=chunk,
            embedding=embedding,
            metadata={
                "document_name": file.filename,
                "chunk_index": i,
                "user_id": user_id,
                "conversation_id": conversation_id
            }
        )
    
    return {"status": "success", "chunks": len(chunks)}
```

---

## 🔍 **COMBINED RAG SEARCH**

### **Search Strategy**

When user sends a query, the system searches BOTH Admin RAG and User RAG simultaneously:

```python
async def search_rag(query: str, conversation_id: str, tenant_id: str):
    # Search Admin RAG
    admin_results = await search_admin_rag(query, tenant_id, limit=5)
    
    # Search User RAG (if exists)
    user_results = []
    if user_collection_exists(conversation_id):
        user_results = await search_user_rag(query, conversation_id, limit=5)
    
    # Combine and rank results
    all_results = admin_results + user_results
    ranked_results = rank_by_relevance(all_results)
    
    # Return top 10
    return ranked_results[:10]
```

### **Ranking Algorithm**

Results are ranked by:
1. **Relevance Score:** Cosine similarity (0-1)
2. **Source Priority:** Admin docs slightly prioritized (legal authority)
3. **Recency:** User docs prioritized if uploaded recently
4. **Diversity:** Ensure results from multiple documents

---

## 📊 **RAG QUALITY METRICS**

### **Retrieval Metrics**

- **Precision:** % of retrieved chunks that are relevant
- **Recall:** % of relevant chunks that are retrieved
- **MRR (Mean Reciprocal Rank):** Position of first relevant result
- **NDCG (Normalized Discounted Cumulative Gain):** Quality of ranking

### **User Experience Metrics**

- **Documents Found:** Number of relevant chunks retrieved
- **Retrieval Time:** Time to search and rank (target: <2s)
- **Source Diversity:** Number of unique documents in results
- **User Satisfaction:** Thumbs up/down on RAG results

---

## 🎨 **UI/UX FOR RAG**

### **Source Attribution Display**

**In Response:**
```markdown
To obtain a building permit, you must submit an application to the administrative unit [1].

The application must include architectural plans [2] and proof of land ownership [3].

Sources:
[1] Zakon o graditvi objektov, Page 15
[2] Pravilnik, Page 8
[3] TSG, Page 23
```

**Source Panel:**
```
📚 Sources Used (7 total)

Admin Documents (5):
📄 Zakon o graditvi objektov - Page 15 (Relevance: 95%)
📄 Pravilnik - Page 8 (Relevance: 92%)
📄 TSG - Page 23 (Relevance: 88%)
📄 Uredba - Page 12 (Relevance: 85%)
📄 GZ - Page 45 (Relevance: 82%)

User Documents (2):
📄 my_building_plans.pdf - Page 3 (Relevance: 90%)
📄 property_deed.pdf - Page 1 (Relevance: 87%)
```

### **Upload Interface**

**Upload Button:**
```
┌─────────────────────────────┐
│  📎 Upload Document         │
│  Add PDFs, DOC, or TXT      │
└─────────────────────────────┘
```

**Upload Progress:**
```
Uploading: my_document.pdf
[████████████████░░░░] 80%
Processing... Extracting text...
```

**Upload Success:**
```
✅ Document uploaded successfully!
📄 my_document.pdf (15 pages, 234 chunks)
Now searchable in this conversation.
```

---

## 🔧 **TECHNICAL BREAKTHROUGHS**

### **1. OCR for Scanned PDFs**

**Problem:** Many Slovenian legal documents are scanned PDFs without text layer.

**Solution:** Integrated Tesseract OCR with Slovenian language support.

**Implementation:**
```python
def extract_text_with_ocr(pdf_path: str) -> str:
    images = convert_from_path(pdf_path)
    text = ""
    for image in images:
        text += pytesseract.image_to_string(image, lang='slv')
    return text
```

### **2. Table Extraction**

**Problem:** Legal documents contain important tables (fees, deadlines, requirements).

**Solution:** Detect and preserve table structure during extraction.

**Implementation:**
```python
def extract_tables(pdf_path: str) -> List[Table]:
    tables = camelot.read_pdf(pdf_path, pages='all')
    return [table.df for table in tables]
```

### **3. Semantic Chunking**

**Problem:** Fixed-size chunks break context and reduce relevance.

**Solution:** Chunk by semantic boundaries (paragraphs, sections, topics).

**Implementation:**
```python
def semantic_chunk(text: str, max_size: int = 1000) -> List[str]:
    # Split by paragraphs
    paragraphs = text.split('\n\n')
    
    chunks = []
    current_chunk = ""
    
    for para in paragraphs:
        if len(current_chunk) + len(para) < max_size:
            current_chunk += para + "\n\n"
        else:
            chunks.append(current_chunk.strip())
            current_chunk = para + "\n\n"
    
    if current_chunk:
        chunks.append(current_chunk.strip())
    
    return chunks
```

### **4. Multi-Tenant Isolation**

**Problem:** Different tenants should not access each other's Admin RAG.

**Solution:** Tenant ID filtering in all RAG queries.

**Implementation:**
```python
.with_where({
    "path": ["tenant_id"],
    "operator": "Equal",
    "valueText": tenant_id
})
```

---

## 🐛 **ERROR HANDLING**

### **No Documents Found**

**Scenario:** RAG search returns 0 results

**Behavior:**
- System logs warning
- AI proceeds with general knowledge only
- User sees: "⚠️ No relevant documents found in knowledge base"
- Response includes disclaimer: "Based on general knowledge only"

### **Upload Failure**

**Scenario:** User document upload fails

**Behavior:**
- User sees error message: "❌ Upload failed: [reason]"
- Suggestions: Check file size, format, or try again
- Document not added to conversation

### **Weaviate Connection Error**

**Scenario:** Vector database is unreachable

**Behavior:**
- System falls back to general knowledge
- Admin receives alert
- User sees: "⚠️ Knowledge base temporarily unavailable"

---

## 🔮 **FUTURE ENHANCEMENTS**

1. **Multi-Language Support:** English, German, Italian legal documents
2. **Image Analysis:** Extract information from diagrams and blueprints
3. **Cross-Document Linking:** Automatically link related regulations
4. **Version Control:** Track changes in legal documents over time
5. **Smart Chunking:** Use AI to determine optimal chunk boundaries

---

**The Dual RAG Architecture is the foundation of Moj AI's accuracy and trustworthiness, combining authoritative legal knowledge with user-specific context.**

