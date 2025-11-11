# 🚀 Deployment Guide - GitHub Pages

This guide will walk you through deploying your AI-Powered Earnings Report Analyzer to GitHub Pages in just a few minutes.

## 📋 Prerequisites

Before you begin, make sure you have:
- ✅ A GitHub account ([Sign up here](https://github.com/join))
- ✅ Git installed on your computer ([Download here](https://git-scm.com/downloads))
- ✅ All project files downloaded to your computer

## 🎯 Quick Deploy (5 Minutes)

### Step 1: Create GitHub Repository

1. Go to [GitHub](https://github.com) and log in
2. Click the **"+"** icon in the top right corner
3. Select **"New repository"**
4. Fill in the details:
   - **Repository name**: `Earnings-reports` (or your preferred name)
   - **Description**: "AI-powered earnings report analyzer with Claude and Gemini AI"
   - **Visibility**: Public (required for free GitHub Pages)
   - **Initialize**: Leave unchecked (we have files already)
5. Click **"Create repository"**

### Step 2: Push Your Code to GitHub

Open your terminal/command prompt and navigate to your project folder:

```bash
# Navigate to your project directory
cd path/to/Earnings-reports

# Initialize git repository (if not already initialized)
git init

# Add all files to staging
git add .

# Create your first commit
git commit -m "Initial commit: AI-Powered Earnings Report Analyzer"

# Add your GitHub repository as remote
# Replace 'toyesm' with your GitHub username
git remote add origin https://github.com/toyesm/Earnings-reports.git

# Push to GitHub
git branch -M main
git push -u origin main
```

**Note**: If you encounter authentication issues, you may need to use a Personal Access Token instead of your password. See the [Authentication Troubleshooting](#authentication-troubleshooting) section below.

### Step 3: Enable GitHub Pages

1. Go to your repository on GitHub: `https://github.com/toyesm/Earnings-reports`
2. Click on **"Settings"** tab
3. Scroll down to **"Pages"** in the left sidebar
4. Under **"Source"**, select:
   - **Branch**: `main`
   - **Folder**: `/ (root)`
5. Click **"Save"**

### Step 4: Access Your Live Site

🎉 **Congratulations!** Your site will be live in 1-2 minutes at:

```
https://toyesm.github.io/Earnings-reports/
```

**Note**: Replace `toyesm` with your GitHub username.

## 📝 Detailed Deployment Instructions

### Method 1: Using HTTPS (Recommended for Beginners)

```bash
# Clone if you don't have the files locally
git clone https://github.com/toyesm/Earnings-reports.git
cd Earnings-reports

# Make your changes to files
# ...

# Stage your changes
git add .

# Commit your changes
git commit -m "Update: Description of your changes"

# Push to GitHub
git push origin main
```

### Method 2: Using SSH (Advanced)

If you have SSH keys set up:

```bash
# Add remote using SSH
git remote add origin git@github.com:toyesm/Earnings-reports.git

# Push to GitHub
git push -u origin main
```

## 🔧 Configuration

### Update Repository Name in README

If you used a different repository name, update these files:
- `README.md` - Update the live demo URL
- Any other references to the repository name

### Custom Domain (Optional)

To use a custom domain like `earnings.yourdomain.com`:

1. Buy a domain from a registrar (e.g., Namecheap, GoDaddy, Google Domains)
2. In your repository settings, go to **Pages** section
3. Under **"Custom domain"**, enter your domain
4. In your domain registrar's DNS settings, add a CNAME record:
   - **Type**: CNAME
   - **Name**: earnings (or @ for root domain)
   - **Value**: `toyesm.github.io`
5. Wait for DNS propagation (can take up to 48 hours)

## 🔄 Updating Your Site

After making changes to your code:

```bash
# Stage all changes
git add .

# Commit with a descriptive message
git commit -m "Add new feature: Description"

# Push to GitHub
git push origin main
```

Your site will automatically update within 1-2 minutes!

## 🐛 Troubleshooting

### Issue: "Permission denied (publickey)"

**Solution**: Set up SSH keys or use HTTPS instead.

For HTTPS:
```bash
git remote set-url origin https://github.com/toyesm/Earnings-reports.git
```

### Issue: "Site not loading"

**Possible Solutions**:
1. Wait 2-3 minutes after enabling Pages
2. Check that GitHub Pages is enabled in Settings
3. Verify your branch is set to `main`
4. Clear your browser cache
5. Try accessing in an incognito/private window

### Issue: "404 Page Not Found"

**Solutions**:
1. Ensure `index.html` is in the root directory
2. File names are case-sensitive on GitHub Pages
3. Wait for the build to complete (check Actions tab)

### Issue: "Charts not showing"

**Solutions**:
1. Check browser console for errors (F12)
2. Verify CDN links are accessible
3. Ensure JavaScript is enabled in browser
4. Try a different browser

### Issue: "API calls failing"

**Solutions**:
1. Verify API keys are correct
2. Check API rate limits
3. Ensure you're not on a corporate network blocking API calls
4. Check CORS settings (shouldn't be an issue with this setup)

## 🔐 Authentication Troubleshooting

### Using Personal Access Token (PAT)

If you encounter password authentication issues:

1. **Generate a Personal Access Token**:
   - Go to GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
   - Click "Generate new token (classic)"
   - Give it a name: "Earnings Analyzer Deploy"
   - Select scopes: `repo` (full control of private repositories)
   - Click "Generate token"
   - **IMPORTANT**: Copy the token immediately (you won't see it again!)

2. **Use the token as your password**:
   ```bash
   git push origin main
   # Username: your-github-username
   # Password: paste-your-token-here
   ```

3. **Cache credentials** (optional):
   ```bash
   # Cache for 1 hour (3600 seconds)
   git config --global credential.helper 'cache --timeout=3600'
   ```

### Using SSH Keys (Recommended for Regular Users)

1. **Generate SSH key**:
   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```

2. **Add to SSH agent**:
   ```bash
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519
   ```

3. **Add to GitHub**:
   - Copy your public key: `cat ~/.ssh/id_ed25519.pub`
   - Go to GitHub → Settings → SSH and GPG keys → New SSH key
   - Paste your key and save

4. **Update remote URL**:
   ```bash
   git remote set-url origin git@github.com:toyesm/Earnings-reports.git
   ```

## 📊 Monitoring Deployment

### Check Deployment Status

1. Go to your repository on GitHub
2. Click the **"Actions"** tab
3. You'll see the deployment workflow running
4. Green checkmark = successful deployment
5. Red X = deployment failed (click for details)

### View Deployment Logs

In the Actions tab, click on a workflow run to see:
- Build steps
- Any errors or warnings
- Deployment time
- Deployment URL

## 🌐 Alternative Deployment Options

### Option 1: Vercel (Alternative to GitHub Pages)

1. Go to [vercel.com](https://vercel.com)
2. Sign up with GitHub
3. Import your repository
4. Deploy with one click
5. Get a custom domain automatically

**Advantages**:
- Faster deployment
- Better analytics
- Automatic HTTPS
- Preview deployments for PRs

### Option 2: Netlify

1. Go to [netlify.com](https://netlify.com)
2. Sign up with GitHub
3. Drag and drop your project folder
4. Or connect your GitHub repository
5. Get instant deployment

**Advantages**:
- Drag and drop simplicity
- Form handling
- Serverless functions
- Split testing

### Option 3: Cloudflare Pages

1. Go to [pages.cloudflare.com](https://pages.cloudflare.com)
2. Connect your GitHub account
3. Select your repository
4. Deploy instantly

**Advantages**:
- Cloudflare's global CDN
- Unlimited bandwidth
- DDoS protection
- Analytics

## 📈 Post-Deployment Checklist

After deploying, verify:
- [ ] Site loads correctly at your GitHub Pages URL
- [ ] All images and styles are loading
- [ ] Charts render properly
- [ ] API configuration panel works
- [ ] You can fetch earnings data
- [ ] AI analysis works with API keys
- [ ] Site is responsive on mobile
- [ ] No console errors (press F12 to check)

## 🔒 Security Best Practices

### Environment Variables

Since this is a client-side app, API keys are entered by users. However, for future enhancements:

1. **Never commit API keys** to your repository
2. **Use .gitignore** for sensitive files
3. **Implement rate limiting** in future versions
4. **Consider a backend proxy** for production use

### HTTPS Enforcement

GitHub Pages automatically uses HTTPS. To enforce it:
1. Go to Settings → Pages
2. Check "Enforce HTTPS"

## 🎨 Customization After Deployment

### Update Branding

Edit `index.html`:
```html
<!-- Update title -->
<title>Your Custom Title</title>

<!-- Update header -->
<h1>Your Custom Header</h1>
```

### Change Colors

Modify the gradient in `index.html`:
```css
background: linear-gradient(135deg, #your-color1 0%, #your-color2 100%);
```

### Add Analytics

Add Google Analytics before `</head>`:
```html
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

## 📞 Getting Help

If you run into issues:

1. **Check GitHub Pages Status**: [githubstatus.com](https://www.githubstatus.com/)
2. **GitHub Docs**: [docs.github.com/pages](https://docs.github.com/en/pages)
3. **Git Documentation**: [git-scm.com/doc](https://git-scm.com/doc)
4. **Open an Issue**: Create an issue in your repository

## 🎉 Success!

Once deployed, share your earnings analyzer:
- Tweet about it with #FinTech #AI
- Share on LinkedIn
- Add to your portfolio
- Show it in job interviews!

**Your live URL**: `https://toyesm.github.io/Earnings-reports/`

---

**Next Steps**: Check out [QUICKSTART.md](./QUICKSTART.md) for a quick guide on using your new analyzer!
