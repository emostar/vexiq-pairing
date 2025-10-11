# Additional Considerations

## Browser Compatibility

**Target Browsers (per PRD):**

- Chrome 90+ (released April 2021)
- Firefox 88+ (released April 2021)
- Safari 14+ (released September 2020)
- Edge 90+ (released April 2021)

**Required JavaScript Features:**

- ES2020 syntax (arrow functions, destructuring, spread operator, optional chaining)
- Array methods: `map()`, `filter()`, `reduce()`, `find()`, `some()`
- Template literals
- `const` and `let`
- `Math.random()`, `Math.min()`, `Math.max()`, `Math.floor()`

**All supported by target browsers.** No transpilation needed.

---

## Performance Considerations

**Algorithm Complexity:** O(n × r × t) where:
- n = number of students
- r = number of rounds
- t = number of teams (1 or 2)

**Expected Performance:**

- 10 students × 5 rounds × 2 teams = 100 assignments → <10ms
- 30 students × 15 rounds × 2 teams = 900 assignments → <50ms
- 50 students × 20 rounds × 2 teams = 2000 assignments → <100ms

**Performance Targets (per NFR1):**

- Generation must complete in <1 second for typical sizes (≤30 students, ≤15 rounds)
- UI must remain responsive (no browser freezing)

**Optimization Strategy:**

- Synchronous execution acceptable for MVP (no Web Workers needed)
- No UI blocking expected with typical data sizes
- Console timing logs can validate performance: `console.time('generation')`

**Phase 2 Optimizations (if needed):**

- Move algorithm to Web Worker for 100+ student scenarios
- Add loading spinner during generation
- Debounce input validation

---

## Security Considerations

**Threat Model:** Minimal - no backend, no data persistence, no user accounts

**Security Measures:**

1. **No Data Transmission:** All data stays in browser memory (privacy by design)
2. **No Code Injection:** No `eval()`, no `innerHTML` with user input (Alpine.js handles escaping)
3. **No XSS:** Tailwind + Alpine.js templates automatically escape output
4. **No Dependencies:** Zero npm packages = zero supply chain risk
5. **No Authentication:** No credentials to leak

**Not Needed for MVP:**

- ❌ Content Security Policy (no inline scripts restriction)
- ❌ HTTPS enforcement (works via file:// protocol)
- ❌ Input sanitization (no server-side processing)
- ❌ CSRF protection (no state-changing requests)
- ❌ Rate limiting (no API)

**Phase 2 Considerations:**

If localStorage is added, consider:
- Clear localStorage on browser close
- Don't store sensitive data (student names may be PII in some contexts)

---

## Accessibility

**MVP Status:** Basic accessibility only (per PRD)

Given the same-day delivery constraint and niche user base (coaches in controlled environments), accessibility enhancements are deferred to Phase 2.

**MVP Accessibility Features:**

- ✅ Semantic HTML (`<form>`, `<table>`, `<label>`)
- ✅ Proper heading hierarchy (`<h1>`, `<h2>`)
- ✅ Color contrast meets WCAG AA for main text
- ✅ Keyboard navigable (tab through inputs, Enter to submit)

**Not Included in MVP:**

- ❌ ARIA labels and roles
- ❌ Screen reader testing
- ❌ High contrast mode support
- ❌ Reduced motion preferences
- ❌ Focus indicators optimization

**Phase 2 Recommendations:**

- Add `aria-label` to form inputs
- Add `role="alert"` to error display
- Test with NVDA/JAWS screen readers
- Add skip navigation links

---
