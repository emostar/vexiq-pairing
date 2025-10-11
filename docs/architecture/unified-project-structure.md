# Unified Project Structure

```
vex-team-builder/
├── index.html                  # ENTIRE APPLICATION (HTML + CSS + JS)
│   ├── <head>
│   │   ├── <meta> tags (charset, viewport)
│   │   ├── <title>VexIQ Team Pairing Tool</title>
│   │   ├── <script> Tailwind CSS CDN (3.x)
│   │   └── <script> Alpine.js CDN (3.x, defer)
│   ├── <body>
│   │   └── <div x-data="scheduleApp()">
│   │       ├── Header section
│   │       ├── Input form section
│   │       ├── Action buttons section
│   │       ├── Error display section
│   │       └── Schedule display section
│   └── <script>
│       └── function scheduleApp() { ... }
│
├── docs/                       # Documentation
│   ├── prd.md                  # Product Requirements Document
│   ├── architecture.md         # This document
│   └── README.md               # Usage instructions
│
├── .github/                    # GitHub configuration
│   └── workflows/
│       └── deploy.yaml         # GitHub Pages deployment workflow
│
├── .gitignore                  # Git ignore rules
├── LICENSE                     # Project license
└── README.md                   # Project overview and quick start
```

**Key Structure Notes:**

- **Single-file application:** Everything in `index.html` - no build output, no dist folder
- **Flat structure:** No src/, public/, or components/ directories
- **Documentation separate:** Docs live in `/docs` but aren't required for app functionality
- **No node_modules:** No npm dependencies, no package.json
- **No build artifacts:** No .next, .output, dist, or build directories

---
