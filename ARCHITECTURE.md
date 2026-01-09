# 🏗️ Architecture Documentation

**Last Updated:** 2026-01-09
**Version:** 1.0.0

This document provides a comprehensive overview of how the AI-Powered Earnings Report Analyzer works internally.

---

## 📐 High-Level Architecture

### App Type
**Single-Page Application (SPA)** - No build process, runs entirely in the browser.

### Technology Stack
```
┌─────────────────────────────────────┐
│         Browser (Client)            │
├─────────────────────────────────────┤
│  React 18 (via CDN)                 │
│  ├── State Management (Hooks)       │
│  ├── Component Rendering            │
│  └── Lifecycle Management           │
├─────────────────────────────────────┤
│  Chart.js 4.4 (via CDN)             │
│  └── Canvas-based Charts            │
├─────────────────────────────────────┤
│  Tailwind CSS (via CDN)             │
│  └── Utility-first Styling          │
├─────────────────────────────────────┤
│  Babel Standalone                   │
│  └── JSX → JavaScript transpiling   │
└─────────────────────────────────────┘
           ↓ API Calls
┌─────────────────────────────────────┐
│       External APIs                 │
├─────────────────────────────────────┤
│  • Financial Modeling Prep          │
│  • Claude AI (Anthropic)            │
│  • Gemini AI (Google)               │
└─────────────────────────────────────┘
```

---

## 📦 Component Structure

### Single Component Architecture
The app uses **ONE main React component**: `App`

**Location:** `index.html` lines 83-733

**Why single component?**
- Simple app with related functionality
- Avoids over-engineering
- Easy to understand and maintain
- Fast development

---

## 🔄 State Management

### React Hooks (useState)
All state is managed using React's `useState` hook at the top level.

**State Variables (Lines 84-94):**
```javascript
ticker          // string  - Current stock ticker input
fmpApiKey       // string  - Financial Modeling Prep API key
claudeApiKey    // string  - Claude AI API key
geminiApiKey    // string  - Gemini AI API key
selectedAI      // string  - 'claude' or 'gemini'
earningsData    // array   - Earnings data from FMP API
aiAnalysis      // string  - AI-generated analysis text
loading         // boolean - Data fetching state
analyzing       // boolean - AI analysis in progress
error           // string  - Error message to display
showApiSettings // boolean - Toggle API settings panel
```

### Chart Refs (useRef)
Canvas references and Chart.js instances stored in refs to avoid re-renders.

**Refs (Lines 96-101):**
```javascript
revenueChartRef      // Canvas DOM element
epsChartRef          // Canvas DOM element
marginsChartRef      // Canvas DOM element
revenueChartInstance // Chart.js instance
epsChartInstance     // Chart.js instance
marginsChartInstance // Chart.js instance
```

**Why refs?**
- Direct DOM access for Chart.js
- Persist across re-renders
- Manual instance management

---

## 🔌 API Integration

### 1. Financial Modeling Prep API

**Purpose:** Fetch earnings data
**Endpoint:** `https://financialmodelingprep.com/api/v3/income-statement/{TICKER}`
**Function:** `fetchEarningsData()` (Lines 110-177)

**Request Parameters:**
- `period=annual` - Annual data only
- `limit=5` - Last 5 years
- `apikey={KEY}` - User's API key

**Response Structure:**
```json
[
  {
    "date": "2023-09-30",
    "calendarYear": "2023",
    "revenue": 383285000000,
    "netIncome": 96995000000,
    "eps": 6.16,
    "operatingIncome": 114301000000
  }
]
```

**Error Handling:**
- HTTP status checks (line 133)
- Response body error detection (line 142) - **IMPORTANT: FMP returns 200 with error in JSON**
- Array validation (line 147)
- Required fields validation (lines 153-157)

### 2. Claude AI API

**Purpose:** Generate financial analysis
**Endpoint:** `https://api.anthropic.com/v1/messages`
**Function:** `analyzeWithAI()` (Lines 220-261)

**Request Headers:**
```javascript
'Content-Type': 'application/json'
'x-api-key': {USER_KEY}
'anthropic-version': '2023-06-01'
```

**Request Body:**
```json
{
  "model": "claude-3-5-sonnet-20241022",
  "max_tokens": 1024,
  "messages": [{
    "role": "user",
    "content": "{PROMPT}"
  }]
}
```

**Response Path:** `data.content[0].text`

**Error Codes:**
- `401` - Invalid API key
- `429` - Rate limit exceeded
- Other - Generic error message

### 3. Gemini AI API

**Purpose:** Alternative AI analysis
**Endpoint:** `https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent`
**Function:** `analyzeWithAI()` (Lines 263-304)

**Request Body:**
```json
{
  "contents": [{
    "parts": [{
      "text": "{PROMPT}"
    }]
  }]
}
```

**Response Path:** `data.candidates[0].content.parts[0].text`

**Error Detection:**
- `400` with 'API_KEY_INVALID' - Invalid key
- `429` - Rate limit exceeded

---

## 📊 Chart Rendering System

### Chart.js Integration
**Location:** `useEffect` hook (Lines 317-457)

### Flow
```
earningsData changes
    ↓
useEffect triggered
    ↓
Destroy old charts (lines 321-329)
    ↓
Extract & transform data (lines 331-340)
    ↓
Create new Chart instances (lines 368-443)
    ↓
Return cleanup function (lines 446-456)
```

### Data Transformation

**Revenue & Net Income (Lines 332-333):**
```javascript
revenues = data.map(d => (d.revenue / 1e9).toFixed(2))    // Billions
netIncomes = data.map(d => (d.netIncome / 1e9).toFixed(2)) // Billions
```

**EPS (Line 334):**
```javascript
eps = data.map(d => d.eps?.toFixed(2) || 0)
```

**Margins (Lines 335-340):**
```javascript
operatingMargins = data.map(d => ((d.operatingIncome / d.revenue) * 100).toFixed(2))
netMargins = data.map(d => ((d.netIncome / d.revenue) * 100).toFixed(2))
```

### Chart Configuration

**Shared Options (Lines 342-365):**
- `responsive: true` - Auto-resize
- `maintainAspectRatio: false` - Use container height
- White labels and axis
- Transparent gridlines

### Three Charts

**1. Revenue & Net Income Bar Chart (Lines 368-392)**
- Type: `bar`
- Datasets: 2 (Revenue, Net Income)
- Colors: Purple gradient

**2. EPS Line Chart (Lines 395-412)**
- Type: `line`
- Dataset: 1 (EPS)
- Color: Pink/Red
- Filled area under line

**3. Margins Line Chart (Lines 415-443)**
- Type: `line`
- Datasets: 2 (Operating, Net Profit)
- Colors: Teal and Yellow
- Filled areas

### Chart Cleanup
**Important:** Charts must be destroyed before creating new ones to prevent memory leaks.
```javascript
return () => {
  if (revenueChartInstance.current) {
    revenueChartInstance.current.destroy();
  }
  // ... same for other charts
};
```

---

## 💾 Local Storage

### Persistence Strategy
API keys are persisted to browser's localStorage for user convenience.

**Implementation:** `useEffect` hook (Lines 104-108)

```javascript
useEffect(() => {
  if (fmpApiKey) localStorage.setItem('fmpApiKey', fmpApiKey);
  if (claudeApiKey) localStorage.setItem('claudeApiKey', claudeApiKey);
  if (geminiApiKey) localStorage.setItem('geminiApiKey', geminiApiKey);
}, [fmpApiKey, claudeApiKey, geminiApiKey]);
```

**Initialization (Lines 85-87):**
```javascript
const [fmpApiKey, setFmpApiKey] = useState(localStorage.getItem('fmpApiKey') || '');
```

**Security Note:** LocalStorage is client-side only. Keys never leave the user's browser except when making direct API calls.

---

## 🎨 Styling System

### Tailwind CSS + Custom CSS

**Tailwind (CDN):** Line 11 - Utility classes in JSX
**Custom CSS:** Lines 12-75 - Glassmorphism and animations

### Key Custom Classes

**`.glass` (Lines 21-26)**
```css
background: rgba(255, 255, 255, 0.1);
backdrop-filter: blur(10px);
border: 1px solid rgba(255, 255, 255, 0.2);
```
- Used for main card backgrounds
- Creates frosted glass effect

**`.glass-dark` (Lines 28-32)**
```css
background: rgba(0, 0, 0, 0.2);
backdrop-filter: blur(10px);
```
- Used for inputs and nested containers
- Darker variant of glass effect

**`.loading-spinner` (Lines 50-57)**
```css
animation: spin 1s linear infinite;
```
- Rotating spinner for loading states

**`.animate-fade-in` (Lines 41-48)**
```css
animation: fadeIn 0.5s ease-in;
```
- Smooth entrance animations for content

### Responsive Design

**Mobile Breakpoint:** `max-width: 768px`
- Chart height reduces to 300px (Line 72)
- Flexbox switches to column layout (Tailwind classes)
- Summary cards stack vertically

---

## 🔐 Error Handling Strategy

### Layered Error Handling

**Level 1: Input Validation**
- Check for empty ticker (Line 111)
- Check for missing API keys (Lines 116, 186)

**Level 2: HTTP Status**
- Check `response.ok` (Lines 133, 240, 282)
- Catch network errors

**Level 3: Response Body Validation**
- Check for API error messages (Line 142)
- Validate array structure (Line 147)
- Validate required fields (Lines 153-157)
- Validate response structure (Lines 256, 298)

**Level 4: Specific Error Messages**
- 401: Invalid API key (Lines 244, 286)
- 429: Rate limit (Lines 246, 288)
- Empty data: Wrong ticker (Line 149)

### Error Display
**Location:** Lines 572-579

```jsx
{error && (
  <div className="border border-red-500">
    <p>⚠️ {error}</p>
    <p>💡 Tip: Press F12 for console details</p>
  </div>
)}
```

### Console Logging
**Debug logs added throughout:**
- Line 129: Fetch start
- Line 139: API response
- Line 143: FMP errors
- Line 221: Claude AI request
- Line 263: Gemini AI request
- Line 306: Analysis complete

---

## 🔀 Data Flow

### Complete User Journey

```
1. USER INPUT
   │
   ├─> Enter ticker
   ├─> Enter API keys (stored in localStorage)
   └─> Click "Analyze"
       │
2. FETCH EARNINGS DATA
   │
   ├─> Validate inputs
   ├─> Call FMP API
   ├─> Handle errors
   ├─> Validate response
   └─> Store in earningsData state
       │
3. RENDER CHARTS
   │
   ├─> useEffect detects earningsData change
   ├─> Transform data (billions, percentages)
   ├─> Destroy old charts
   ├─> Create new Chart.js instances
   └─> Render to canvas elements
       │
4. AI ANALYSIS (Optional)
   │
   ├─> User clicks "Get AI Insights"
   ├─> Select AI provider (Claude/Gemini)
   ├─> Format prompt with earnings data
   ├─> Call AI API
   ├─> Handle errors
   └─> Display analysis text
```

---

## 🎯 Key Design Decisions

### 1. No Build Process
**Why:** Simplicity, instant deployment, easy debugging
**Trade-off:** Larger initial load (CDN scripts)

### 2. Single Component
**Why:** Simple app, related functionality
**Trade-off:** Less reusability (acceptable for this scope)

### 3. Client-Side Only
**Why:** No server costs, privacy, simplicity
**Trade-off:** API keys in browser (user responsibility)

### 4. CDN Dependencies
**Why:** No package manager needed, always latest
**Trade-off:** Network dependency, version control

### 5. Console Logging
**Why:** Mobile debugging (iOS Safari), user transparency
**Trade-off:** Slightly verbose console (acceptable)

---

## 🐛 Known Limitations

### 1. API Key Security
- Keys stored in localStorage (browser-specific)
- Keys visible in network requests
- **Mitigation:** User-provided keys, clear documentation

### 2. No Data Caching
- Each search makes new API call
- **Mitigation:** Fast APIs, acceptable for use case

### 3. Chart Memory
- Charts must be manually destroyed
- **Mitigation:** Cleanup in useEffect return

### 4. Mobile Console Access
- iOS Safari requires desktop mode for console
- **Future:** In-app console viewer (planned)

### 5. No Offline Mode
- Requires internet for APIs
- **Mitigation:** Graceful error messages

---

## 📈 Performance Characteristics

### Load Time
- **First Paint:** < 1s (CDN scripts cached)
- **Interactive:** < 2s
- **Charts Render:** < 500ms after data load

### API Response Times
- **FMP API:** ~500ms - 2s
- **Claude AI:** ~2s - 5s
- **Gemini AI:** ~1s - 3s

### Memory Usage
- **Base:** ~20MB
- **With Charts:** ~30MB
- **Chart Cleanup:** Returns to ~20MB

---

## 🔄 State Update Patterns

### Synchronous Updates
```javascript
setTicker(value)      // Immediate
setError(message)     // Immediate
setLoading(true)      // Immediate
```

### Asynchronous Updates
```javascript
try {
  setLoading(true);
  const data = await fetch(...);
  setEarningsData(data);  // After API response
} finally {
  setLoading(false);       // Always runs
}
```

### Effect-Triggered Updates
```javascript
useEffect(() => {
  // Runs when earningsData changes
  createCharts();
  return () => destroyCharts();
}, [earningsData]);
```

---

## 🧪 Testing Strategy

### Manual Testing Checklist
- [ ] Enter invalid ticker → Shows error
- [ ] Enter valid ticker → Shows charts
- [ ] Missing API key → Shows error + opens settings
- [ ] Invalid API key → Shows specific error
- [ ] Rate limit hit → Shows retry message
- [ ] Switch AI providers → Updates correctly
- [ ] Resize window → Charts resize
- [ ] Refresh page → API keys persist

### Browser Compatibility
- ✅ Chrome 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Edge 90+

---

## 📝 Code Style Guidelines

### Naming Conventions
- **State:** camelCase (`earningsData`, `fmpApiKey`)
- **Functions:** camelCase (`fetchEarningsData`, `analyzeWithAI`)
- **CSS Classes:** kebab-case (`glass-dark`, `loading-spinner`)
- **Constants:** camelCase (`exampleTickers`)

### Function Structure
```javascript
const functionName = async () => {
  // 1. Validation
  if (!input) return;

  // 2. Setup
  setLoading(true);
  setError('');

  // 3. Try/Catch
  try {
    // Main logic
  } catch (err) {
    // Error handling
  } finally {
    // Cleanup
    setLoading(false);
  }
};
```

---

## 🔮 Future Enhancements

### Planned
- [ ] In-app console viewer (for iOS)
- [ ] Data export (CSV, JSON)
- [ ] Quarterly earnings support
- [ ] Multi-company comparison

### Under Consideration
- [ ] Chart customization
- [ ] Dark/light theme toggle
- [ ] Backend proxy for API keys
- [ ] Caching layer

---

## 📚 Dependencies

### Runtime Dependencies (CDN)
```
react@18              - UI framework
react-dom@18          - DOM rendering
@babel/standalone     - JSX transpilation
chart.js@4.4.0        - Charts
tailwindcss           - Styling
```

### API Dependencies
```
financialmodelingprep.com  - Earnings data
api.anthropic.com          - Claude AI
generativelanguage.googleapis.com - Gemini AI
```

---

## 🎓 Learning Resources

### Understanding This Codebase
1. Start with `App` component (line 83)
2. Follow `fetchEarningsData` flow (line 110)
3. Examine chart rendering (line 317)
4. Review error handling patterns

### External Documentation
- [React Hooks](https://react.dev/reference/react)
- [Chart.js](https://www.chartjs.org/docs/)
- [Tailwind CSS](https://tailwindcss.com/docs)
- [FMP API](https://financialmodelingprep.com/developer/docs/)
- [Claude API](https://docs.anthropic.com/claude/reference/)
- [Gemini API](https://ai.google.dev/docs)

---

**End of Architecture Documentation**
