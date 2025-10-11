# Monitoring and Observability

**Status:** N/A for MVP

This MVP has no monitoring, analytics, or observability tooling. All debugging happens via browser developer tools.

**Phase 2 Considerations:**

If usage tracking becomes valuable, privacy-respecting options could be added:

- **Frontend Monitoring:** Privacy-focused analytics (e.g., Plausible, Fathom) via simple script tag
- **Error Tracking:** Sentry for JavaScript error reporting (requires adding SDK)
- **Performance Monitoring:** Browser Performance API to log generation time to console

**Why Not in MVP:**

- Adds complexity (analytics script, consent banner)
- Violates privacy-first principle without user consent mechanism
- Not necessary for single-user offline tool
- Manual testing sufficient for MVP validation

---
