# Document Analysis & Research - User Scenarios

**Last Updated:** 2025-10-20

---

## 🎯 **OVERVIEW**

This document covers scenarios for analyzing architectural plans, reviewing contracts, understanding technical specifications, and comparing multiple documents using Moj AI's multimodal RAG system.

---

## 📋 **SCENARIO 1: Architectural Plan Analysis**

### **User Profile**
- Property buyer reviewing plans before purchase
- Needs to understand if plans comply with regulations
- Non-technical background

### **Example Questions**

**Frontier Mode (🚀 1.0 questions):**
```
[Upload architectural plans PDF]

Analiziraj te arhitekturne načrte in mi povej:
- Ali načrti ustrezajo slovenskim predpisom?
- Kakšne so dimenzije prostorov?
- Ali so izpolnjene zahteve glede svetlobe in prezračevanja?
- Ali so izpolnjene zahteve glede dostopnosti?
- Kakšne so morebitne težave ali pomanjkljivosti?
- Priporočila za izboljšave

(Analyze these architectural plans and tell me:
- Do plans comply with Slovenian regulations?
- What are room dimensions?
- Are light and ventilation requirements met?
- Are accessibility requirements met?
- What are potential issues or deficiencies?
- Recommendations for improvements)
```

### **Expected Response**

**Frontier Mode:**
- Complete plan analysis:
  - Room dimensions extracted from plans
  - Compliance check against regulations
  - Window/light analysis (minimum 10% floor area)
  - Ventilation assessment
  - Accessibility evaluation

- Identified issues:
  - Undersized rooms (if any)
  - Insufficient natural light
  - Missing accessibility features
  - Code violations

- Recommendations:
  - Specific modifications needed
  - Cost estimates for changes
  - Priority of fixes

- Comparison with regulations:
  - Cited specific articles
  - Referenced building codes
  - Highlighted non-compliance

- 8-15 pages with specific references to uploaded plans

### **Pro Tips**
- Upload high-quality PDF scans
- Include all plan sheets (floor plans, elevations, sections)
- Ask specific questions about concerns
- Request cost estimates for modifications

---

## 📋 **SCENARIO 2: Building Permit Document Review**

### **User Profile**
- Homeowner who received building permit
- Wants to understand conditions and requirements
- Needs to know next steps

### **Example Questions**

**Lightning Mode (⚡ 0.5 questions):**
```
[Upload building permit PDF]

Razloži mi to gradbeno dovoljenje:
- Kakšni so pogoji dovoljenja?
- Kakšni so roki?
- Kaj moram storiti pred začetkom gradnje?

(Explain this building permit:
- What are the permit conditions?
- What are the deadlines?
- What must I do before starting construction?)
```

**Frontier Mode (🚀 1.0 questions):**
```
[Upload building permit PDF]

Podrobno analiziraj to gradbeno dovoljenje:
- Vsi pogoji in zahteve
- Roki in veljavnost
- Potrebni naslednji koraki
- Obveznosti med gradnjo
- Obveznosti po gradnji
- Morebitne omejitve
- Kaj se zgodi če ne izpolnim pogojev?

(Analyze this building permit in detail:
- All conditions and requirements
- Deadlines and validity
- Required next steps
- Obligations during construction
- Obligations after construction
- Any restrictions
- What happens if I don't meet conditions?)
```

### **Expected Response**

**Frontier Mode:**
- Permit conditions extracted:
  - Construction start deadline
  - Construction completion deadline
  - Specific technical requirements
  - Neighbor notifications required

- Pre-construction requirements:
  - Construction site setup
  - Contractor registration
  - Insurance requirements
  - Inspection scheduling

- During construction:
  - Required inspections
  - Documentation to maintain
  - Reporting requirements

- Post-construction:
  - Final inspection
  - Occupancy permit process
  - Cadastral registration

- Consequences of non-compliance:
  - Permit expiration
  - Fines
  - Demolition orders

- 6-12 pages

---

## 📋 **SCENARIO 3: Contract Review**

### **User Profile**
- Property owner about to sign construction contract
- Wants to understand terms and identify risks
- Needs legal perspective

### **Example Questions**

**Frontier Mode (🚀 1.0 questions):**
```
[Upload construction contract PDF]

Preglej to gradbeno pogodbo in mi povej:
- Kakšne so glavne obveznosti vsake strani?
- Kakšni so plačilni pogoji?
- Kakšne so garancije in jamstva?
- Kaj se zgodi pri zamudah?
- Kakšne so kazenske klavzule?
- Kakšna so tveganja zame?
- Kaj bi moral spremeniti ali dodati?

(Review this construction contract and tell me:
- What are main obligations of each party?
- What are payment terms?
- What are warranties and guarantees?
- What happens with delays?
- What are penalty clauses?
- What are risks for me?
- What should I change or add?)
```

### **Expected Response**

**Frontier Mode:**
- Contract analysis:
  - Party obligations clearly outlined
  - Payment schedule extracted
  - Warranty terms (typically 2-5 years)
  - Delay penalties (if any)

- Risk assessment:
  - Unfavorable terms identified
  - Missing protections
  - Ambiguous clauses
  - Liability issues

- Recommendations:
  - Suggested modifications
  - Additional clauses to add
  - Terms to negotiate
  - Red flags to address

- Legal framework:
  - Relevant Slovenian contract law
  - Consumer protection rights
  - Construction industry standards

- 10-15 pages

---

## 📋 **SCENARIO 4: Technical Specification Analysis**

### **User Profile**
- Builder comparing material specifications
- Needs to understand technical requirements
- Wants to ensure quality

### **Example Questions**

**Frontier Mode (🚀 1.0 questions):**
```
[Upload technical specifications PDF]

Analiziraj te tehnične specifikacije za gradnjo:
- Ali specifikacije ustrezajo slovenskim standardom?
- Kakšna je kakovost predlaganih materialov?
- Kakšne so alternative (boljše/cenejše)?
- Kakšni so pričakovani stroški?
- Kakšna je dolgoročna vzdržljivost?
- Priporočila za izboljšave

(Analyze these technical specifications for construction:
- Do specifications meet Slovenian standards?
- What is quality of proposed materials?
- What are alternatives (better/cheaper)?
- What are expected costs?
- What is long-term durability?
- Recommendations for improvements)
```

### **Expected Response**

**Frontier Mode:**
- Specification analysis:
  - Compliance with standards verified
  - Material quality assessment
  - Performance characteristics

- Cost analysis:
  - Current market prices
  - Cost per unit/m²
  - Total project cost estimate

- Alternatives comparison:
  - Higher quality options
  - Cost-effective alternatives
  - Performance trade-offs

- Durability assessment:
  - Expected lifespan
  - Maintenance requirements
  - Long-term costs

- Recommendations:
  - Optimal material choices
  - Cost-benefit analysis
  - Quality improvements

- 8-12 pages

---

## 📋 **SCENARIO 5: Comparing Multiple Documents**

### **User Profile**
- Developer comparing multiple contractor bids
- Needs to understand differences
- Wants to make informed decision

### **Example Questions**

**Frontier Mode (🚀 1.0 questions):**
```
[Upload 3 contractor bid PDFs]

Primerjaj te tri ponudbe izvajalcev:
- Kakšne so cenovne razlike in zakaj?
- Kakšne so razlike v materialih in kakovosti?
- Kakšne so razlike v časovnicah?
- Kakšne so garancije in jamstva?
- Katera ponudba je najboljša vrednost?
- Kakšna so tveganja pri vsaki?
- Priporočilo za izbiro

(Compare these three contractor bids:
- What are price differences and why?
- What are differences in materials and quality?
- What are timeline differences?
- What are warranties and guarantees?
- Which bid is best value?
- What are risks with each?
- Recommendation for selection)
```

### **Expected Response**

**Frontier Mode:**
- Detailed comparison table:
  - Price breakdown by item
  - Material specifications
  - Timeline comparison
  - Warranty terms

- Analysis:
  - Price differences explained
  - Quality assessment
  - Value for money evaluation

- Risk assessment:
  - Contractor reliability
  - Hidden costs
  - Completion risks

- Recommendation:
  - Best overall value
  - Negotiation points
  - Decision factors

- 12-18 pages

---

## 📋 **SCENARIO 6: Property Deed Analysis**

### **User Profile**
- Property buyer
- Needs to understand deed restrictions
- Wants to verify building rights

### **Example Questions**

**Frontier Mode (🚀 1.0 questions):**
```
[Upload property deed PDF]

Analiziraj to zemljiškoknjižno izpisek:
- Kakšne so omejitve na parceli?
- Ali obstajajo služnosti?
- Kakšne so gradbene pravice?
- Ali obstajajo hipoteke ali bremena?
- Kakšne so morebitne težave?
- Kaj moram preveriti pred nakupom?

(Analyze this land registry extract:
- What are restrictions on plot?
- Are there any easements?
- What are building rights?
- Are there mortgages or encumbrances?
- What are potential issues?
- What should I verify before purchase?)
```

### **Expected Response**

**Frontier Mode:**
- Deed analysis:
  - Property boundaries
  - Ownership verification
  - Easements identified
  - Encumbrances listed

- Building rights:
  - Zoning designation
  - Permitted uses
  - Building restrictions

- Issues identified:
  - Outstanding mortgages
  - Legal disputes
  - Access rights
  - Utility easements

- Recommendations:
  - Additional verifications needed
  - Legal consultation points
  - Deal-breakers

- 6-10 pages

---

## 💡 **GENERAL TIPS FOR DOCUMENT ANALYSIS**

### **Before Uploading**
- Ensure documents are clear and readable
- Upload complete documents (all pages)
- Use PDF format when possible
- Check file size (max 10MB)

### **When Asking**
- Be specific about what you need to know
- Ask about specific concerns
- Request comparisons if relevant
- Mention your decision timeline

### **After Response**
- Review cited sections carefully
- Ask follow-up questions
- Request clarification on technical terms
- Consider professional consultation for legal documents

### **Mode Selection**
- Lightning: Quick document summary
- Frontier: Detailed analysis and recommendations

---

**These scenarios demonstrate Moj AI's powerful document analysis capabilities. Always verify critical information with professionals.**

