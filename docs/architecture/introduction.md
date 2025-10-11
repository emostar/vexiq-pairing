# Introduction

This document outlines the complete fullstack architecture for **VexIQ Team Pairing Tool**, a client-side only scheduling application. It serves as the single source of truth for AI-driven development, ensuring consistency across the entire technology stack.

This is a unique "fullstack" document for a pure client-side application - there is intentionally NO backend, no database, and no API layer. All computation happens in the browser. This unified approach streamlines development for this ultra-simple static web application delivered as a single HTML file.

## Starter Template or Existing Project

**Status:** N/A - Greenfield single-file project

This is a greenfield project with no starter template. The architecture is intentionally minimal: a single `index.html` file with CDN-based dependencies (Tailwind CSS and Alpine.js). This approach was chosen to meet the same-day delivery constraint (4-5 hours) and eliminate all setup complexity.

**Key Constraints Imposed:**
- Zero build process
- No npm/package management
- No bundling or transpilation
- CDN-only dependencies
- Works offline after initial load
- Can be opened directly in browser via `file://` protocol

## Change Log

| Date       | Version | Description                              | Author           |
| ---------- | ------- | ---------------------------------------- | ---------------- |
| 2025-10-10 | v1.0    | Initial architecture from PRD            | Winston (architect) |

---
