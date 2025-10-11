# Coding Standards

## Critical Rules

These are the MINIMAL but CRITICAL standards that prevent common mistakes. All developers (AI and human) must follow these rules.

- **No External Dependencies:** Never add npm packages, additional CDN scripts, or libraries. The tech stack (Tailwind + Alpine.js) is final and sufficient.

- **Preserve Single-File Architecture:** All code must remain in `index.html`. Do not create separate `.js` or `.css` files unless explicitly approved.

- **Alpine.js Directives Only:** Use Alpine.js directives (`x-data`, `x-model`, `x-for`, `x-show`, etc.) for DOM manipulation. Never use vanilla DOM APIs like `querySelector`, `innerHTML`, or `addEventListener` except in edge cases.

- **Validate Before Generation:** Always call `validateInputs()` before running the algorithm. Display errors via `errorMessage` property.

- **Independent Role Tracking:** Track `driverCount` and `loaderCount` separately. Never combine into a single "opportunityCount" - this would break fairness for the 2:1 ratio.

- **No Persistence:** Do not implement localStorage, sessionStorage, IndexedDB, or any persistence mechanism without explicit approval (deferred to Phase 2).

- **Variance Constraint:** Algorithm must maintain variance ≤ 1 for both driver and loader roles independently. Log console warning if perfect balance is mathematically impossible.

- **Print Optimization Required:** Use Tailwind print utilities (`print:*` classes) to ensure schedule is readable when printed or projected.

- **No Build Tools:** Do not introduce Vite, Webpack, Parcel, ESBuild, or any bundler/transpiler. The single-file architecture is intentional.

- **Console Logging for Debugging:** Use `console.log()` and `console.warn()` liberally during development. These help coaches understand edge cases.

---

## Naming Conventions

| Element | Convention | Example |
|---------|-----------|---------|
| **Alpine.js Component** | camelCase function | `scheduleApp()` |
| **Data Properties** | camelCase | `studentNames`, `numRounds`, `isGenerated` |
| **Methods** | camelCase | `generateSchedule()`, `selectStudentForRole()` |
| **HTML IDs** | kebab-case | `student-input`, `schedule-table` |
| **CSS Classes (Tailwind)** | Tailwind utilities | `bg-blue-500`, `p-4`, `text-center` |
| **Variables (JavaScript)** | camelCase | `minCount`, `assignedThisRound` |
| **Constants** | SCREAMING_SNAKE_CASE | `MAX_STUDENTS` (if needed) |

---
