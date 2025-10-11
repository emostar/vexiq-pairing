# Deployment Architecture

## Deployment Strategy

**Frontend Deployment:**
- **Platform:** GitHub Pages (or any static host)
- **Build Command:** None (no build step)
- **Output Directory:** Root directory (`index.html` in repository root)
- **CDN/Edge:** Automatic via GitHub Pages CDN

**Deployment Methods:**

**Option 1: GitHub Pages (Recommended)**
```bash
# Enable GitHub Pages in repository settings
# Select: Deploy from branch → gh-pages (or main)

# Manual deployment:
git add index.html docs/
git commit -m "feat: update application"
git push origin main

# Automatic deployment via GitHub Actions (see CI/CD Pipeline section)
```

**Option 2: Netlify**
```bash
# Drag and drop deployment:
# 1. Visit https://app.netlify.com/drop
# 2. Drag `index.html` to browser
# 3. Done!

# Or: Connect GitHub repo for auto-deploy
```

**Option 3: Vercel**
```bash
# Install Vercel CLI (optional)
npm i -g vercel

# Deploy
vercel --prod

# Or: Connect GitHub repo in Vercel dashboard
```

**Option 4: Local File**
```bash
# No deployment needed - just open the file
open index.html
```

---

## CI/CD Pipeline

**GitHub Actions Workflow** (`.github/workflows/deploy.yaml`):

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    permissions:
      contents: read
      pages: write
      id-token: write

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

**Pipeline Explanation:**

1. Triggers on push to `main` branch
2. Uploads entire repository as static site artifact
3. Deploys to GitHub Pages
4. No build step, no tests (manual testing approach for MVP)

---

## Environments

| Environment | Frontend URL | Backend URL | Purpose |
|-------------|-------------|-------------|---------|
| **Development** | `file:///path/to/index.html` | N/A | Local development and testing |
| **Staging** | Not needed (optional: `staging.yourdomain.com`) | N/A | Optional pre-production testing |
| **Production** | `https://yourusername.github.io/vex-team-builder/` | N/A | Live environment for coaches |

**Environment Notes:**

- **No staging environment needed for MVP:** Changes can be tested locally before pushing
- **Production URL:** Determined by GitHub Pages settings (username.github.io/repo-name or custom domain)
- **No environment variables:** Application behavior is identical across environments

---
