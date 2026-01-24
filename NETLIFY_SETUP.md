# Netlify Deployment Setup

Instructions for deploying the Obsidian AI Project Management documentation to Netlify.

---

## Prerequisites

- GitHub account with access to `ostanlabs/obsidian_docs` repository
- Netlify account (free tier is sufficient)

---

## Netlify Configuration Steps

### 1. Connect Repository to Netlify

1. Log in to [Netlify](https://app.netlify.com/)
2. Click **"Add new site"** → **"Import an existing project"**
3. Choose **"Deploy with GitHub"**
4. Authorize Netlify to access your GitHub account
5. Select the repository: **`ostanlabs/obsidian_docs`**

### 2. Configure Build Settings

Netlify should auto-detect the settings from `netlify.toml`, but verify:

**Build settings:**
- **Branch to deploy:** `main`
- **Build command:** `pip install -r requirements-docs.txt && mkdocs build`
- **Publish directory:** `site`
- **Python version:** `3.11` (set in netlify.toml)

### 3. Deploy Site

1. Click **"Deploy site"**
2. Netlify will:
   - Clone the repository
   - Install Python dependencies
   - Run MkDocs build
   - Deploy to CDN

### 4. Configure Custom Domain (Optional)

If you want a custom domain instead of `*.netlify.app`:

1. Go to **Site settings** → **Domain management**
2. Click **"Add custom domain"**
3. Follow instructions to configure DNS

---

## Automatic Deployments

Once configured, Netlify will automatically:

- **Deploy on push to `main`** - Any commit to main branch triggers rebuild
- **Deploy previews for PRs** - Pull requests get preview URLs
- **Rollback support** - Can rollback to previous deployments

---

## Build Configuration

The build is configured in `netlify.toml`:

```toml
[build]
  command = "pip install -r requirements-docs.txt && mkdocs build"
  publish = "site"

[build.environment]
  PYTHON_VERSION = "3.11"
```

### What Happens During Build:

1. **Install dependencies:**
   - mkdocs >= 1.5
   - mkdocs-material >= 9.0
   - pymdown-extensions >= 10.0
   - mkdocs-mermaid2-plugin >= 1.0

2. **Build documentation:**
   - Processes all markdown files in `docs/`
   - Renders Mermaid diagrams
   - Generates static HTML site
   - Outputs to `site/` directory

3. **Deploy:**
   - Uploads `site/` to Netlify CDN
   - Configures redirects and headers
   - Makes site live

---

## Site Features

### Enabled Features:

- ✅ **Material Theme** - Modern, responsive design
- ✅ **Dark/Light Mode** - Automatic theme switching
- ✅ **Search** - Full-text search across all docs
- ✅ **Mermaid Diagrams** - Interactive diagrams
- ✅ **Code Highlighting** - Syntax highlighting for code blocks
- ✅ **Navigation Tabs** - Organized navigation structure
- ✅ **Mobile Responsive** - Works on all devices

### Security Headers:

Configured in `netlify.toml`:
- X-Frame-Options: DENY
- X-Content-Type-Options: nosniff
- Referrer-Policy: strict-origin-when-cross-origin

### Caching:

- Static assets cached for 1 year
- HTML pages cached with smart invalidation

---

## Verifying Deployment

### Check Build Status:

1. Go to **Deploys** tab in Netlify dashboard
2. View build logs for any errors
3. Check deploy preview before going live

### Test the Site:

1. Visit the deployed URL (e.g., `https://obsidian-ai-pm.netlify.app`)
2. Verify:
   - All pages load correctly
   - Mermaid diagrams render
   - Search works
   - Navigation functions
   - Dark/light mode toggles

---

## Troubleshooting

### Build Fails

**Check build logs in Netlify:**
- Look for Python dependency errors
- Check MkDocs build errors
- Verify all markdown files are valid

**Common issues:**
- Missing dependencies in `requirements-docs.txt`
- Invalid markdown syntax
- Broken internal links
- Invalid Mermaid diagram syntax

### Site Not Updating

1. Check that commit was pushed to `main` branch
2. Verify build triggered in Netlify dashboard
3. Clear browser cache
4. Check deploy status

### Mermaid Diagrams Not Rendering

1. Verify `mkdocs-mermaid2-plugin` is installed
2. Check diagram syntax is valid
3. Look for JavaScript errors in browser console

---

## Local Testing

Before pushing, test locally:

```bash
# Install dependencies
pip install -r requirements-docs.txt

# Build site
mkdocs build

# Serve locally
mkdocs serve
```

Visit `http://127.0.0.1:8000` to preview.

---

## Maintenance

### Updating Documentation:

1. Edit markdown files in `docs/`
2. Test locally with `mkdocs serve`
3. Commit and push to `main`
4. Netlify auto-deploys

### Updating Theme/Plugins:

1. Update versions in `requirements-docs.txt`
2. Test locally
3. Commit and push
4. Netlify rebuilds with new versions

---

## Site URL

After deployment, your documentation will be available at:

**Default:** `https://[site-name].netlify.app`  
**Custom:** Configure in Netlify domain settings

---

## Support

- **Netlify Docs:** https://docs.netlify.com/
- **MkDocs Docs:** https://www.mkdocs.org/
- **Material Theme:** https://squidfunk.github.io/mkdocs-material/

---

## Summary

✅ Repository configured with `netlify.toml`  
✅ Build command and settings defined  
✅ Python dependencies specified  
✅ Mermaid diagrams supported  
✅ Material theme with dark mode  
✅ Automatic deployments on push to main  

**Next:** Connect repository in Netlify dashboard and deploy!

