# Question Allowance System

**Component:** `frontend/components/chat/QuestionAllowanceDisplay.tsx`  
**Last Updated:** 2025-10-20

---

## 🎯 **OVERVIEW**

The Question Allowance System is Moj AI's **unit-based pricing model** that allows fractional question deduction (0.5 for Lightning, 1.0 for Frontier). This innovative approach gives users flexibility to balance cost and quality based on their needs.

**Key Innovation:** Fractional question deduction - users can ask twice as many simple questions with the same budget.

---

## 💰 **QUESTION TYPES**

### **1. Premium Questions (💎)**

**Source:** Purchased by user via Stripe

**Characteristics:**
- **Color:** Purple (#A855F7)
- **Priority:** Used first (highest priority)
- **Expiration:** Never expires
- **Transferable:** No
- **Refundable:** No

**Purchase Options:**
- 5 questions: Free plan
- 15 questions: €20
- 50 questions: €50
- 100 questions: €75

**Stripe Integration:**
```typescript
const purchaseQuestions = async (quantity: number) => {
  const response = await fetch('/api/stripe/create-checkout', {
    method: 'POST',
    body: JSON.stringify({
      quantity,
      mode: 'payment',
      success_url: '/chat?purchase=success',
      cancel_url: '/pricing?purchase=cancelled'
    })
  });
  
  const { url } = await response.json();
  window.location.href = url;
};
```

---

### **2. Bonus Questions (🎁)**

**Source:** Granted by admin

**Characteristics:**
- **Color:** Green (#10B981)
- **Priority:** Used second (after premium)
- **Expiration:** Set by admin (default: 90 days)
- **Transferable:** No
- **Refundable:** No

**Admin Grant Process:**
1. Admin navigates to User Management
2. Selects user
3. Clicks "Grant Bonus Questions"
4. Enters quantity and expiration date
5. Adds optional note (e.g., "Welcome bonus", "Apology for downtime")
6. Confirms grant

**Use Cases:**
- Welcome bonus for new users (5 questions)
- Apology for service issues (10 questions)
- Referral rewards (15 questions)
- Contest prizes (50 questions)
- VIP perks (100 questions)

---

### **3. Free Questions (🆓)**

**Source:** Automatically granted to all new users

**Characteristics:**
- **Color:** Blue (#3B82F6)
- **Priority:** Used last (lowest priority)
- **Expiration:** Never expires
- **Transferable:** No
- **Refundable:** No

**Default Allocation:**
- New users: 5 free questions
- Cannot be replenished
- One-time grant per user

---

## 📊 **FRACTIONAL DEDUCTION**

### **How It Works**

**Lightning Mode (⚡):**
- Cost: **0.5 questions** (half unit)
- Example: 10 questions → 20 Lightning queries

**Frontier Mode (🚀):**
- Cost: **1.0 questions** (full unit)
- Example: 10 questions → 10 Frontier queries

**Mixed Usage:**
- Start with: 10.0 questions
- Use Lightning: 10.0 - 0.5 = 9.5 questions
- Use Frontier: 9.5 - 1.0 = 8.5 questions
- Use Lightning: 8.5 - 0.5 = 8.0 questions

### **Deduction Priority**

Questions are deducted in this order:
1. **Premium Questions** (💎) - User paid for these
2. **Bonus Questions** (🎁) - Admin granted
3. **Free Questions** (🆓) - Last resort

**Example:**
```
User has:
- 5.0 Premium questions
- 3.0 Bonus questions
- 2.0 Free questions
Total: 10.0 questions

User sends Frontier query (1.0 cost):
- Premium: 5.0 - 1.0 = 4.0
- Bonus: 3.0 (unchanged)
- Free: 2.0 (unchanged)
Total: 9.0 questions

User sends Lightning query (0.5 cost):
- Premium: 4.0 - 0.5 = 3.5
- Bonus: 3.0 (unchanged)
- Free: 2.0 (unchanged)
Total: 8.5 questions

User sends 7 more Frontier queries (7.0 cost):
- Premium: 3.5 - 3.5 = 0.0 (depleted)
- Bonus: 3.0 - 3.5 = 0.0 (depleted, used 3.0)
- Free: 2.0 - 0.5 = 1.5
Total: 1.5 questions
```

### **Technical Implementation**

**Database Schema:**
```sql
CREATE TABLE user_usage (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users(id),
  tenant_id UUID REFERENCES tenants(id),
  purchased_questions DECIMAL(10, 1) DEFAULT 0.0,
  bonus_questions DECIMAL(10, 1) DEFAULT 0.0,
  free_questions DECIMAL(10, 1) DEFAULT 5.0,
  total_questions_used DECIMAL(10, 1) DEFAULT 0.0,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

**Deduction Logic:**
```python
def deduct_questions(user_id: str, cost: float) -> Dict:
    usage = get_user_usage(user_id)
    
    remaining = cost
    
    # Deduct from premium first
    if usage.purchased_questions >= remaining:
        usage.purchased_questions -= remaining
        remaining = 0
    else:
        remaining -= usage.purchased_questions
        usage.purchased_questions = 0
    
    # Deduct from bonus second
    if remaining > 0 and usage.bonus_questions >= remaining:
        usage.bonus_questions -= remaining
        remaining = 0
    elif remaining > 0:
        remaining -= usage.bonus_questions
        usage.bonus_questions = 0
    
    # Deduct from free last
    if remaining > 0 and usage.free_questions >= remaining:
        usage.free_questions -= remaining
        remaining = 0
    elif remaining > 0:
        remaining -= usage.free_questions
        usage.free_questions = 0
    
    # If still remaining, user has insufficient questions
    if remaining > 0:
        raise InsufficientQuestionsError(f"Need {remaining} more questions")
    
    # Update total used
    usage.total_questions_used += cost
    
    # Save to database
    save_user_usage(usage)
    
    return {
        "success": True,
        "cost": cost,
        "remaining": {
            "premium": usage.purchased_questions,
            "bonus": usage.bonus_questions,
            "free": usage.free_questions,
            "total": usage.purchased_questions + usage.bonus_questions + usage.free_questions
        }
    }
```

---

## 🎨 **UI/UX DESIGN**

### **Compact Display**

**Location:** Header area, next to UsageMonitor

**Visual:**
```
┌─────────────────────────┐
│  💎 8.5 questions       │
│  Next: -0.5 (⚡)        │
└─────────────────────────┘
```

**Hover Tooltip:**
```
Your Question Bank:
💎 Premium: 5.0
🎁 Bonus: 3.0
🆓 Free: 0.5
Total: 8.5 questions

Next question cost:
⚡ Lightning: -0.5
🚀 Frontier: -1.0
```

---

### **Full Display (Dialog)**

**Triggered By:** Clicking on compact display

**Visual:**
```
┌─────────────────────────────────────────────────────────┐
│           Your Question Bank                            │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  Total Questions: 8.5                                   │
│                                                         │
│  💎 Premium Questions: 5.0                              │
│  [████████████████████░░░░░░░░░░] 5.0 / 10.0           │
│  Purchased • Never expires                              │
│                                                         │
│  🎁 Bonus Questions: 3.0                                │
│  [████████████████████████████░░] 3.0 / 3.0            │
│  Admin granted • Expires in 45 days                     │
│                                                         │
│  🆓 Free Questions: 0.5                                 │
│  [██░░░░░░░░░░░░░░░░░░░░░░░░░░░░] 0.5 / 5.0            │
│  Welcome bonus • Never expires                          │
│                                                         │
│  ─────────────────────────────────────────────────────  │
│                                                         │
│  Next Question Cost:                                    │
│  ⚡ Lightning Mode: -0.5 questions                      │
│  🚀 Frontier Mode: -1.0 questions                       │
│                                                         │
│  💡 Pro Tip: Use Lightning Mode for simple questions    │
│     to save questions for complex analysis!             │
│                                                         │
│  [Buy More Questions]  [View Usage History]             │
└─────────────────────────────────────────────────────────┘
```

---

### **Low Balance Warning**

**Triggered When:** Total questions < 2.0

**Visual:**
```
┌─────────────────────────────────────────────────────────┐
│  ⚠️  Low Question Balance                               │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  You have 1.5 questions remaining.                      │
│                                                         │
│  This is enough for:                                    │
│  • 3 Lightning queries (⚡ 0.5 each)                    │
│  • 1 Frontier query (🚀 1.0 each)                       │
│                                                         │
│  [Buy More Questions]                                   │
└─────────────────────────────────────────────────────────┘
```

---

### **Out of Questions**

**Triggered When:** Total questions = 0.0

**Visual:**
```
┌─────────────────────────────────────────────────────────┐
│  ❌ No Questions Remaining                              │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  You've used all your questions.                        │
│                                                         │
│  Purchase more to continue using Moj AI:                │
│                                                         │
│  • 15 questions: €20.00                                  │
│  • 50 questions: €50.00                   │
│  • 100 questions: €75.00                                │
│                                                         │
│  [Buy Questions Now]                                    │
└─────────────────────────────────────────────────────────┘
```

**Behavior:**
- Message input disabled
- Mode selector disabled
- Send button disabled
- Prominent "Buy Questions" CTA

---

## 📱 **MOBILE RESPONSIVENESS**

### **Compact Mobile Display**

```
┌──────────────┐
│ 💎 8.5       │
│ Next: -0.5   │
└──────────────┘
```

### **Full Mobile Display**

- Full-screen overlay
- Swipe down to close
- Larger touch targets
- Simplified layout

---

## 📊 **ANALYTICS & TRACKING**

### **Events Tracked**

1. **Question Deducted:**
   - Event: `question_deducted`
   - Properties: `{ cost: 0.5 | 1.0, mode: 'lightning' | 'frontier', type: 'premium' | 'bonus' | 'free' }`

2. **Low Balance Warning Shown:**
   - Event: `low_balance_warning`
   - Properties: `{ remaining: number }`

3. **Out of Questions:**
   - Event: `out_of_questions`
   - Properties: `{ last_query_mode: string }`

4. **Purchase Initiated:**
   - Event: `purchase_initiated`
   - Properties: `{ quantity: number, price: number }`

5. **Purchase Completed:**
   - Event: `purchase_completed`
   - Properties: `{ quantity: number, price: number, payment_method: string }`

### **Metrics**

- Average questions per user per month
- Lightning vs Frontier usage ratio
- Purchase conversion rate
- Average purchase size
- Bonus question redemption rate
- Free question depletion time

---

## 🔧 **TECHNICAL BREAKTHROUGHS**

### **1. Decimal Precision**

**Problem:** Floating-point arithmetic errors (0.1 + 0.2 = 0.30000000000000004)

**Solution:** Use DECIMAL(10, 1) in database and Python Decimal type

```python
from decimal import Decimal

cost = Decimal('0.5')
remaining = Decimal('8.5')
new_remaining = remaining - cost  # Exactly 8.0
```

### **2. Atomic Transactions**

**Problem:** Race condition when multiple queries sent simultaneously

**Solution:** Database transactions with row-level locking

```python
@transaction.atomic
def deduct_questions(user_id: str, cost: float):
    usage = UserUsage.objects.select_for_update().get(user_id=user_id)
    # Deduction logic here
    usage.save()
```

### **3. Real-Time Balance Updates**

**Problem:** UI shows stale balance after query

**Solution:** SSE updates + optimistic UI updates

```typescript
// Optimistic update
setQuestions(prev => prev - cost);

// Listen for server confirmation
eventSource.addEventListener('balance_updated', (event) => {
  const { remaining } = JSON.parse(event.data);
  setQuestions(remaining);
});
```

---

## 🔮 **FUTURE ENHANCEMENTS**

1. **Question Packages:** Subscription plans with monthly question allowances
2. **Rollover Questions:** Unused questions roll over to next month
3. **Question Sharing:** Share questions with team members
4. **Question Gifting:** Gift questions to other users
5. **Dynamic Pricing:** Adjust question costs based on complexity
6. **Question Predictions:** Estimate questions needed for project

---

**The Question Allowance System is Moj AI's innovative pricing model that gives users flexibility and control over their AI usage costs.**

