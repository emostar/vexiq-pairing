# Tech Stack

This is the **DEFINITIVE** technology selection for the entire project. All development must use these exact versions.

## Technology Stack Table

| Category | Technology | Version | Purpose | Rationale |
|----------|-----------|---------|---------|-----------|
| **Frontend Language** | JavaScript (ES6+) | ES2020 | Client-side logic and algorithm implementation | Native browser support, no transpilation needed, sufficient for algorithm complexity |
| **Frontend Framework** | Alpine.js | 3.x (latest from CDN) | Reactive UI and state management | Lightweight (15KB), Vue-like syntax, perfect for single-page reactive forms without build step |
| **UI Component Library** | None (native HTML5) | N/A | Form inputs and table display | HTML5 form elements sufficient for simple inputs; custom table styling via Tailwind |
| **State Management** | Alpine.js reactive data | 3.x | In-memory schedule state | Built into Alpine.js `x-data`, no separate library needed |
| **Backend Language** | N/A | N/A | No backend | Client-side only architecture |
| **Backend Framework** | N/A | N/A | No backend | Client-side only architecture |
| **API Style** | N/A | N/A | No API layer | All logic executes in browser |
| **Database** | None (browser memory only) | N/A | Ephemeral data storage | Data exists only during session; privacy by design |
| **Cache** | Browser HTTP cache | Native | CDN asset caching | Browser automatically caches Tailwind/Alpine.js from CDN |
| **File Storage** | None | N/A | No file persistence | No uploads or downloads in MVP |
| **Authentication** | None | N/A | No user accounts | Single-user local tool, no auth needed |
| **Frontend Testing** | Manual testing | N/A | Algorithm validation | No test framework due to 4-5hr timeline; manual edge case testing |
| **Backend Testing** | N/A | N/A | No backend | N/A |
| **E2E Testing** | Manual cross-browser testing | N/A | Browser compatibility validation | Test in Chrome, Firefox, Safari manually |
| **Build Tool** | None | N/A | No build process | Direct HTML file, no compilation |
| **Bundler** | None | N/A | No bundling needed | CDN-served dependencies |
| **IaC Tool** | None | N/A | No infrastructure | Static hosting requires no IaC |
| **CI/CD** | GitHub Actions (optional) | Latest | Automated deployment to GitHub Pages | Simple workflow to copy index.html to gh-pages branch |
| **Monitoring** | None (MVP) | N/A | No telemetry | Phase 2 could add privacy-respecting analytics |
| **Logging** | Browser console.log | Native | Development debugging and edge case warnings | Algorithm logs warnings for impossible configurations |
| **CSS Framework** | Tailwind CSS | 3.x (latest from CDN) | Utility-first styling | No build step via CDN, excellent print styles, responsive defaults |

---
