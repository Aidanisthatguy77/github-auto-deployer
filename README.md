# 🌉 GitHub Auto-Deployer

One repo to rule all free deployment platforms. Push code → Get deployed! 🚀

## ⚡ Quick Start

1. **Choose a platform** (see below)
2. **Copy an example** to `.github/workflows/deploy.yml`
3. **Add secrets** to GitHub (Settings → Secrets → Actions)
4. **Push** → Watch it deploy!

## 🎯 Pick Your Platform

| Platform | Best For | Free Tier | Docs |
|----------|----------|-----------|------|
| **Vercel** | Next.js, React, Vue | 100GB bandwidth | [examples/vercel/](examples/vercel/) |
| **Netlify** | Static sites, JAMstack | 100GB bandwidth | [examples/netlify/](examples/netlify/) |
| **GitHub Pages** | Simple static sites | 1GB storage | [examples/pages/](examples/pages/) |
| **Cloudflare Pages** | Global CDN, Workers | Unlimited builds | [examples/cloudflare/](examples/cloudflare/) |
| **Fly.io** | Docker containers | 3 VMs | [examples/fly/](examples/fly/) |
| **Render** | APIs, web services | 750hrs + PostgreSQL | [examples/render/](examples/render/) |

## 📋 Required Secrets

| Platform | Secrets to Add |
|----------|---------------|
| **Vercel** | `VERCEL_TOKEN`, `VERCEL_ORG_ID`, `VERCEL_PROJECT_ID` |
| **Netlify** | `NETLIFY_AUTH_TOKEN`, `NETLIFY_SITE_ID` |
| **GitHub Pages** | *(none! just enable in settings)* |
| **Cloudflare** | `CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ACCOUNT_ID`, `CLOUDFLARE_PROJECT_NAME` |
| **Fly.io** | `FLY_API_TOKEN`, `FLY_APP_NAME` |
| **Render** | `RENDER_API_KEY`, `RENDER_BLUEPRINT_ID` |

## 🐳 Docker Example (Already Included!)

Check out `examples/docker-example/docker-compose.yml` for a Node.js app with health checks.

## 📖 Full Setup Guide

See [SETUP.md](SETUP.md) for detailed step-by-step instructions.

## 🌐 Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        YOU                                  │
│                   Push to GitHub                            │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│                   GitHub Actions                           │
│               (Builds & Tests)                             │
└─────────────────────┬───────────────────────────────────────┘
                      │
        ┌─────────────┼─────────────┬─────────────┐
        │             │             │             │
        ▼             ▼             ▼             ▼
    ┌────────┐   ┌────────┐   ┌────────┐   ┌────────┐
    │Vercel  │   │Netlify │   │ Cloudflare │ Fly.io │
    │  ✅    │   │  ✅    │   │    ✅      │  ✅    │
    └────────┘   └────────┘   └────────┘   └────────┘
```

## 💡 Example Workflows

Copy any of these to `.github/workflows/deploy.yml`:

### Vercel (Next.js/React)
```yaml
name: Deploy to Vercel
on: [push]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
```

### GitHub Pages (Zero Config!)
```yaml
name: Deploy to GitHub Pages
on: [push]
jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions: contents: read, pages: write, id-token: write
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20' }
      - run: npm ci && npm run build
      - uses: actions/configure-pages@v4
      - uses: actions/upload-pages-artifact@v3
        with: { path: ./dist }
      - uses: actions/deploy-pages@v4
```

## 🆘 Troubleshooting

**Workflow not found?** → Ensure file is in `.github/workflows/`

**Secret not found?** → Check Settings → Secrets → Actions

**Build fails?** → Verify build command in workflow matches package.json

## 📚 Resources

- [Full Setup Guide](SETUP.md)
- [Deploy Bridge Skill](../deploy-bridge/SKILL.md)
- [GitHub Actions Docs](https://docs.github.com/en/actions)

---

**No more manual deploys. Just push and ship. 🚀**