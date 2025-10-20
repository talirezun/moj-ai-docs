# Mode Selector - Lightning vs Frontier

**Component:** `frontend/components/chat/ModeSelector.tsx`  
**Last Updated:** 2025-10-20

---

## 🎯 **OVERVIEW**

The Mode Selector is a critical UI component that allows users to choose between two AI orchestration modes: Lightning (⚡) and Frontier (🚀). This choice fundamentally affects response quality, processing time, tools engaged, and question cost.

**Key Principle:** Empower users to make informed decisions about speed vs comprehensiveness.

---

## 👤 **USER PERSPECTIVE**

### **What Users See**

**Location:** Above the message input area in the chat interface

**Visual Design:**
- Two toggle buttons side-by-side
- **Lightning Button:** Yellow background, ⚡ icon, "0.5" badge
- **Frontier Button:** Purple background, 🚀 icon, "1.0" badge
- **Info Button:** ℹ️ icon next to mode selector
- Active mode is highlighted with brighter color

**Interaction:**
1. Click Lightning or Frontier button to select mode
2. Click Info button to open detailed comparison dialog
3. Selected mode persists across queries in the same session

---

### **Mode Comparison Dialog**

**Triggered By:** Clicking the Info (ℹ️) button

**Content:**

```
┌─────────────────────────────────────────────────────────┐
│           Choose Your AI Mode                           │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  ⚡ Lightning Mode          🚀 Frontier Mode            │
│  ─────────────────          ──────────────────          │
│                             [Recommended]               │
│                                                         │
│  💰 Cost: 0.5 questions     💰 Cost: 1.0 questions      │
│  ⏱️  Time: 15-30 seconds     ⏱️  Time: 60-240 seconds    │
│  📄 Length: 2-5k chars      📄 Length: 10-30k chars     │
│                                                         │
│  Tools Engaged:             Tools Engaged:              │
│  ✅ RAG Search              ✅ RAG Search                │
│  ✅ Legal Reasoning         ✅ Legal Reasoning           │
│  ❌ Internet Research       ✅ Internet Research         │
│  ✅ Quality Check           ✅ Quality Check             │
│                                                         │
│  Best For:                  Best For:                   │
│  • Quick answers            • Comprehensive analysis    │
│  • Known regulations        • Market research           │
│  • Simple queries           • Complex scenarios         │
│  • Saving questions         • Multiple perspectives     │
│                                                         │
│  Example Response:          Example Response:           │
│  "To obtain a building      "To obtain a building       │
│  permit in Ljubljana for    permit in Ljubljana for     │
│  a two-unit residential     a two-unit residential      │
│  building, you need to:     building, you need to:      │
│  1. Submit application...   1. Submit application...    │
│  2. Provide documents...    [10x more detailed]         │
│  [2-3 paragraphs]"          [5-10 pages with sources]"  │
│                                                         │
│  [Select Lightning Mode]    [Select Frontier Mode]      │
└─────────────────────────────────────────────────────────┘
```

**User Actions:**
- Click "Select Lightning Mode" to choose Lightning
- Click "Select Frontier Mode" to choose Frontier
- Click outside dialog or X to close without changing

---

## 🎨 **DESIGN SPECIFICATIONS**

### **Colors**

**Lightning Mode:**
- Background: `bg-yellow-50` (light yellow)
- Border: `border-yellow-500` (yellow)
- Text: `text-yellow-900` (dark yellow)
- Icon: ⚡ (yellow)
- Badge: `bg-yellow-500 text-white` (0.5)

**Frontier Mode:**
- Background: `bg-purple-50` (light purple)
- Border: `border-purple-500` (purple)
- Text: `text-purple-900` (dark purple)
- Icon: 🚀 (purple)
- Badge: `bg-purple-500 text-white` (1.0)

### **Typography**

- Mode name: `font-semibold text-sm`
- Cost badge: `text-xs font-bold`
- Dialog title: `text-xl font-bold`
- Section headers: `text-sm font-semibold`
- Body text: `text-sm`

### **Spacing**

- Button padding: `px-4 py-2`
- Badge margin: `ml-2`
- Dialog padding: `p-6`
- Section spacing: `space-y-4`

---

## 🔧 **TECHNICAL IMPLEMENTATION**

### **Component Structure**

```typescript
interface ModeSelectorProps {
  value: boolean;           // true = lightning, false = frontier
  onChange: (lightningMode: boolean) => void;
  disabled?: boolean;       // Disable during query processing
  showComparison?: boolean; // Show comparison dialog by default
}

export function ModeSelector({ value, onChange, disabled, showComparison }: ModeSelectorProps) {
  const [dialogOpen, setDialogOpen] = useState(showComparison || false);
  
  // Render mode buttons
  // Render info button
  // Render comparison dialog
}
```

### **State Management**

**Local State:**
- `dialogOpen`: Controls comparison dialog visibility

**Global State (Zustand):**
- `lightningMode`: Boolean stored in chat store
- Persists across component re-renders
- Resets when starting new conversation

**Props:**
- `value`: Current mode (true = Lightning, false = Frontier)
- `onChange`: Callback to update mode in parent component

### **Event Handlers**

```typescript
const handleLightningClick = () => {
  if (!disabled) {
    onChange(true);
  }
};

const handleFrontierClick = () => {
  if (!disabled) {
    onChange(false);
  }
};

const handleInfoClick = () => {
  setDialogOpen(true);
};

const handleSelectMode = (isLightning: boolean) => {
  onChange(isLightning);
  setDialogOpen(false);
};
```

### **Disabled State**

Mode selector is disabled when:
- Query is being processed (`isLoading = true`)
- Response is streaming (`isStreaming = true`)
- User has 0 questions remaining

**Visual Feedback:**
- Buttons become grayed out
- Cursor changes to `cursor-not-allowed`
- Opacity reduced to 50%

---

## 📊 **MODE COMPARISON DATA**

### **Lightning Mode (⚡)**

```typescript
const lightningMode = {
  id: 'lightning',
  name: 'Lightning Mode',
  icon: '⚡',
  cost: '0.5 questions',
  costValue: 0.5,
  time: '15-30 seconds',
  timeRange: [15, 30],
  length: '2-5k characters',
  lengthRange: [2000, 5000],
  model: 'GPT-4o or higher',
  provider: 'OpenAI',
  features: [
    { name: 'RAG Search', enabled: true, icon: '📚' },
    { name: 'Legal Reasoning', enabled: true, icon: '⚖️' },
    { name: 'Internet Research', enabled: false, icon: '🌐' },
    { name: 'Quality Check', enabled: true, icon: '✓' }
  ],
  bestFor: [
    'Quick answers to known regulations',
    'Simple procedural questions',
    'Saving questions for later',
    'Fast turnaround needed'
  ],
  exampleQuery: 'How do I get a building permit in Ljubljana?',
  exampleResponse: 'To obtain a building permit in Ljubljana for a two-unit residential building, you need to: 1. Submit application to administrative unit...'
};
```

### **Frontier Mode (🚀)**

```typescript
const frontierMode = {
  id: 'frontier',
  name: 'Frontier Mode',
  icon: '🚀',
  cost: '1.0 questions',
  costValue: 1.0,
  time: '60-240 seconds',
  timeRange: [60, 240],
  length: '10-30k characters',
  lengthRange: [10000, 30000],
  model: 'Claude Sonnet 4 or higher',
  provider: 'Anthropic',
  features: [
    { name: 'RAG Search', enabled: true, icon: '📚' },
    { name: 'Legal Reasoning', enabled: true, icon: '⚖️' },
    { name: 'Internet Research', enabled: true, icon: '🌐' },
    { name: 'Quality Check', enabled: true, icon: '✓' }
  ],
  bestFor: [
    'Comprehensive legal analysis',
    'Market research and trends',
    'Complex scenarios with multiple factors',
    'Detailed economic analysis'
  ],
  exampleQuery: 'How do I get a building permit and what are current construction costs?',
  exampleResponse: 'To obtain a building permit in Ljubljana for a two-unit residential building, you need to: [10x more detailed with sources, market data, examples]...'
};
```

---

## 🎯 **USER DECISION SUPPORT**

### **Recommendation Logic**

**Recommend Lightning (⚡) when:**
- Query is short (<50 words)
- Query asks about specific regulation
- Query doesn't mention market/trends/prices
- User has limited questions remaining

**Recommend Frontier (🚀) when:**
- Query is long (>50 words)
- Query mentions market research, trends, prices
- Query asks for comprehensive analysis
- Query has multiple sub-questions
- User has sufficient questions remaining

**Visual Indicator:**
- "Recommended" badge appears on suggested mode
- Badge color matches mode color
- Tooltip explains why it's recommended

---

## 🔄 **MODE PERSISTENCE**

### **Session Persistence**

Mode selection persists within the same browser session:
- Stored in Zustand store
- Survives page refreshes (via localStorage)
- Resets when user logs out

### **Conversation Persistence**

Mode selection does NOT persist across conversations:
- Each new conversation starts with default mode (Frontier)
- User can change mode at any time during conversation
- Mode used for each query is stored in message metadata

---

## 📱 **MOBILE RESPONSIVENESS**

### **Mobile View (<768px)**

**Compact Layout:**
- Buttons stack vertically instead of horizontally
- Icon and badge remain visible
- Info button moves below mode buttons
- Dialog becomes full-screen overlay

**Touch Optimization:**
- Larger touch targets (min 44x44px)
- Increased spacing between buttons
- Swipe gesture to close dialog

---

## ♿ **ACCESSIBILITY**

### **Keyboard Navigation**

- Tab to focus on Lightning button
- Tab to focus on Frontier button
- Tab to focus on Info button
- Enter/Space to activate focused button
- Escape to close dialog

### **Screen Reader Support**

```html
<button
  aria-label="Lightning Mode: 0.5 questions, 15-30 seconds, 2-5k characters"
  aria-pressed={value === true}
  role="radio"
>
  ⚡ Lightning
</button>

<button
  aria-label="Frontier Mode: 1.0 questions, 60-240 seconds, 10-30k characters"
  aria-pressed={value === false}
  role="radio"
>
  🚀 Frontier
</button>
```

### **Color Contrast**

- All text meets WCAG AA standards (4.5:1 contrast ratio)
- Icons have sufficient contrast against backgrounds
- Focus indicators are clearly visible

---

## 🐛 **EDGE CASES & ERROR HANDLING**

### **No Questions Remaining**

**Behavior:**
- Both mode buttons disabled
- Tooltip: "No questions remaining. Purchase more to continue."
- Link to purchase page

### **API Error**

**Behavior:**
- Mode selector remains functional
- Error message appears in chat
- User can retry with different mode

### **Network Offline**

**Behavior:**
- Mode selector disabled
- Offline indicator appears
- Queued queries wait for reconnection

---

## 📈 **ANALYTICS & TRACKING**

### **Events Tracked**

1. **Mode Selection:**
   - Event: `mode_selected`
   - Properties: `{ mode: 'lightning' | 'frontier', previous_mode: string }`

2. **Comparison Dialog Opened:**
   - Event: `mode_comparison_viewed`
   - Properties: `{ current_mode: string }`

3. **Mode Changed from Dialog:**
   - Event: `mode_changed_from_comparison`
   - Properties: `{ from: string, to: string }`

### **Metrics**

- Lightning vs Frontier usage ratio
- Comparison dialog open rate
- Mode switches per session
- Average questions per mode

---

## 🔮 **FUTURE ENHANCEMENTS**

1. **Smart Mode Suggestion:** AI analyzes query and suggests best mode
2. **Custom Modes:** Users create custom modes with specific tool combinations
3. **Mode Presets:** Save favorite mode configurations
4. **A/B Testing:** Test different mode descriptions and layouts
5. **Mode History:** Show which mode was used for each past query

---

**The Mode Selector is the gateway to Moj AI's dual-mode orchestration system, empowering users to balance speed and comprehensiveness based on their needs.**

