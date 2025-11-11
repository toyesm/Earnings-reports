# 📊 AI-Powered Earnings Report Analyzer

A modern, single-page web application that analyzes company earnings reports using AI. Get instant insights into financial performance with beautiful visualizations and AI-powered analysis from Claude and Gemini.

![Earnings Analyzer](https://img.shields.io/badge/Status-Live-success)
![License](https://img.shields.io/badge/License-MIT-blue)
![React](https://img.shields.io/badge/React-18-61dafb)
![Chart.js](https://img.shields.io/badge/Chart.js-4.4-ff6384)

## ✨ Features

### 📈 Interactive Financial Visualizations
- **Revenue & Net Income Trends** - Beautiful bar charts showing 5-year financial performance
- **Earnings Per Share (EPS) Evolution** - Line charts tracking EPS growth over time
- **Profit Margins Analysis** - Operating and net profit margin trends
- **Key Metrics Dashboard** - Quick overview cards with the latest financial data

### 🤖 Dual AI Analysis
- **Claude AI Integration** - Deep financial insights powered by Anthropic's Claude
- **Gemini AI Integration** - Alternative analysis using Google's Gemini
- **Smart Insights** - Comprehensive analysis covering:
  - Overall financial health assessment
  - Key trends and patterns identification
  - Strengths and potential concerns
  - Future outlook considerations

### 🎨 Modern Design
- **Glassmorphism UI** - Stunning glass-effect design with backdrop blur
- **Gradient Accents** - Beautiful purple/pink color scheme
- **Fully Responsive** - Optimized for mobile, tablet, and desktop
- **Dark Theme** - Easy on the eyes with a professional dark interface
- **Smooth Animations** - Polished fade-in effects and transitions

### 🔐 Secure & Private
- **Client-Side Only** - All processing happens in your browser
- **Local Storage** - API keys stored securely in your browser
- **No Backend** - Direct API calls, no intermediary servers
- **Privacy First** - Your data never passes through third-party servers

## 🚀 Live Demo

**Coming Soon:** `https://toyesm.github.io/Earnings-reports/`

## 📋 Prerequisites

To use this application, you'll need API keys from the following services:

1. **Financial Modeling Prep** (Required)
   - Get your free API key: [financialmodelingprep.com/developer](https://financialmodelingprep.com/developer/docs/)
   - Free tier includes 250 requests per day

2. **Claude AI** (Optional - for AI analysis)
   - Get your API key: [console.anthropic.com](https://console.anthropic.com/)
   - Paid service with generous free credits for new users

3. **Gemini AI** (Optional - for AI analysis)
   - Get your API key: [makersuite.google.com/app/apikey](https://makersuite.google.com/app/apikey)
   - Free tier available

## 🛠️ Technology Stack

| Technology | Purpose | Version |
|------------|---------|---------|
| **React** | UI Framework | 18.x |
| **Chart.js** | Data Visualization | 4.4.x |
| **Tailwind CSS** | Styling | 3.x (CDN) |
| **Babel Standalone** | JSX Compilation | Latest |
| **Financial Modeling Prep API** | Financial Data | v3 |
| **Claude API** | AI Analysis | Latest |
| **Gemini API** | AI Analysis | Latest |

## 📦 Installation

### Option 1: Direct Download
1. Download or clone this repository
2. Open `index.html` in a modern web browser
3. Configure your API keys in the settings panel
4. Start analyzing!

### Option 2: GitHub Pages (Recommended)
See [DEPLOYMENT.md](./DEPLOYMENT.md) for detailed deployment instructions.

### Option 3: Local Development Server
```bash
# Using Python 3
python -m http.server 8000

# Using Node.js
npx serve .

# Using PHP
php -S localhost:8000
```

Then visit `http://localhost:8000` in your browser.

## 📖 Usage Guide

### Step 1: Configure API Keys
1. Click on the **API Configuration** section
2. Enter your Financial Modeling Prep API key (required)
3. Optionally add Claude or Gemini API keys for AI analysis
4. Keys are automatically saved to your browser's local storage

### Step 2: Search for a Company
1. Enter a stock ticker symbol (e.g., AAPL, MSFT, GOOGL)
2. Click the **Analyze** button or press Enter
3. View the earnings data and visualizations

### Step 3: Get AI Insights
1. After loading company data, scroll to the AI Analysis section
2. Choose between Claude AI or Gemini AI
3. Click **Get AI Insights** for detailed analysis
4. Read comprehensive insights about financial health, trends, and outlook

### Example Stock Tickers to Try
- **AAPL** - Apple Inc.
- **MSFT** - Microsoft Corporation
- **GOOGL** - Alphabet Inc.
- **AMZN** - Amazon.com Inc.
- **TSLA** - Tesla Inc.
- **NVDA** - NVIDIA Corporation
- **META** - Meta Platforms Inc.
- **JPM** - JPMorgan Chase & Co.

## 🎯 Features Breakdown

### Financial Data Displayed
- **Revenue** - Total company revenue (in billions)
- **Net Income** - Profit after all expenses (in billions)
- **Earnings Per Share (EPS)** - Net income divided by shares outstanding
- **Operating Margin** - Operating income as % of revenue
- **Net Profit Margin** - Net income as % of revenue
- **5-Year Historical Trends** - Compare year-over-year performance

### Chart Types
1. **Bar Chart** - Revenue & Net Income comparison
2. **Line Chart** - EPS trends over time
3. **Multi-Line Chart** - Operating and net profit margins

### AI Analysis Features
- Financial health assessment
- Trend identification
- Strength analysis
- Risk identification
- Future outlook considerations
- Actionable insights

## 🔧 Configuration

### API Key Storage
API keys are stored in your browser's localStorage:
```javascript
localStorage.setItem('fmpApiKey', 'your-key-here');
localStorage.setItem('claudeApiKey', 'your-key-here');
localStorage.setItem('geminiApiKey', 'your-key-here');
```

### Clear Stored Keys
To clear your API keys, open browser console and run:
```javascript
localStorage.clear();
```

## 🌐 Browser Compatibility

| Browser | Minimum Version |
|---------|----------------|
| Chrome | 90+ |
| Firefox | 88+ |
| Safari | 14+ |
| Edge | 90+ |

## 📱 Responsive Design

- **Mobile** (< 768px) - Single column layout, optimized touch interactions
- **Tablet** (768px - 1024px) - Two column grid for charts
- **Desktop** (> 1024px) - Full multi-column layout with enhanced visualizations

## ⚠️ Important Notes

### API Rate Limits
- **Financial Modeling Prep Free Tier**: 250 requests/day
- **Claude API**: Pay-per-use (check current pricing)
- **Gemini API**: Free tier available with limits

### Data Accuracy
- Financial data is sourced from Financial Modeling Prep
- Data is updated regularly but may have slight delays
- Always verify critical information from official sources

### Security Best Practices
- Never share your API keys publicly
- Don't commit API keys to version control
- Use environment-specific keys for development/production
- Regularly rotate your API keys

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Ideas for Contributions
- Add more chart types (pie charts, candlestick charts)
- Support for quarterly earnings data
- Comparison mode (compare multiple companies)
- Export functionality (PDF, Excel)
- Historical stock price integration
- Additional AI providers
- Dark/light theme toggle
- Keyboard shortcuts
- Accessibility improvements

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](./LICENSE) file for details.

## 🙏 Acknowledgments

- **Financial Modeling Prep** - For providing comprehensive financial data API
- **Anthropic** - For Claude AI capabilities
- **Google** - For Gemini AI capabilities
- **Chart.js** - For beautiful and responsive charts
- **Tailwind CSS** - For rapid UI development
- **React** - For powerful component-based architecture

## 📞 Support

Having issues? Here's how to get help:

1. **Check the [QUICKSTART.md](./QUICKSTART.md)** - Quick solutions to common issues
2. **Review [DEPLOYMENT.md](./DEPLOYMENT.md)** - Deployment troubleshooting
3. **Open an Issue** - Report bugs or request features on GitHub
4. **API Documentation**:
   - [Financial Modeling Prep Docs](https://financialmodelingprep.com/developer/docs/)
   - [Claude API Docs](https://docs.anthropic.com/)
   - [Gemini API Docs](https://ai.google.dev/docs)

## 🗺️ Roadmap

- [ ] Add quarterly earnings support
- [ ] Multi-company comparison
- [ ] PDF export functionality
- [ ] Stock price charts integration
- [ ] Dividend analysis
- [ ] Analyst estimates comparison
- [ ] News sentiment analysis
- [ ] Portfolio tracking
- [ ] Email alerts for earnings releases
- [ ] Mobile app version

## 💡 Tips & Tricks

### Performance
- Charts are cached - switching between tabs is instant
- API responses are not cached - each search makes fresh API calls
- Close other browser tabs to improve performance with large datasets

### Best Practices
- Start with well-known companies (e.g., AAPL, MSFT)
- Compare year-over-year trends for context
- Use both AI providers to get different perspectives
- Save frequently used API keys for quick access

### Troubleshooting
- **"Failed to fetch"** - Check your API key and internet connection
- **"No data found"** - Verify ticker symbol is correct
- **Charts not showing** - Refresh the page and try again
- **API key not saving** - Check if cookies/localStorage are enabled

## 📊 Example Analysis Output

When you analyze Apple (AAPL), you might see:
- Revenue: $394.33B (2023)
- Net Income: $96.99B (2023)
- EPS: $6.16 (2023)
- Net Margin: 24.6%

AI Analysis might highlight:
- Consistent revenue growth
- Strong profit margins
- Healthy EPS progression
- Potential areas of concern or opportunity

---

**Made with ❤️ for the investment community**

*Disclaimer: This tool is for informational purposes only. Not financial advice. Always do your own research and consult with financial professionals before making investment decisions.*
