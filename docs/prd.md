# VexIQ Team Pairing Tool Product Requirements Document (PRD)

---

## Goals and Background Context

### Goals

- **Deliver a fair, instant schedule generator** that creates role assignments for VexIQ tournaments in under 2 minutes
- **Eliminate manual scheduling burden** reducing coach time from 2-4 hours to under 5 minutes per tournament
- **Ensure mathematical fairness** with variance ≤ 1 for both driver and loader roles across all students
- **Prevent student forfeits** through clear advance communication of role assignments
- **Maintain team morale** by providing perceivably fair role distribution that supports collaborative learning
- **Enable quick regeneration** allowing coaches flexibility to create multiple fair schedules instantly
- **Work offline-first** with zero setup, no accounts, and no external dependencies after initial page load

### Background Context

VexIQ coaches face a significant logistical challenge when preparing for robotics tournaments: manually creating equitable role assignments across multiple rounds. Each team requires 2 drivers and 1 loader per round, and ensuring fairness across these two distinct roles becomes mathematically complex and time-consuming. Current solutions (spreadsheets, generic schedulers) don't understand the 2:1 driver-to-loader ratio or track role equity independently, leading to hours of error-prone manual work.

The impact extends beyond logistics. When students perceive unfairness or don't know their schedule in advance, they may disengage or miss their assigned slots, resulting in forfeits. These forfeits damage team morale and undermine the core purpose of VexIQ participation: the collaborative learning experience. This tool addresses the root cause by providing instant, mathematically fair, role-specific schedules that coaches can generate, regenerate, and share within seconds—allowing them to focus on coaching rather than spreadsheet mathematics.

### Change Log

| Date       | Version | Description                       | Author          |
| ---------- | ------- | --------------------------------- | --------------- |
| 2025-10-10 | v1.0    | Initial PRD creation from Project Brief | John (PM Agent) |

---

## Requirements

### Functional Requirements

**FR1:** The system shall accept student names via a multi-line textarea input (one name per line)

**FR2:** The system shall accept numeric input for number of rounds (1-99 range)

**FR3:** The system shall accept numeric input for number of teams (1-2 range)

**FR4:** The system shall generate role assignments using a greedy fill algorithm that assigns roles round-by-round, always selecting students with the fewest opportunities for that specific role

**FR5:** The system shall track driver opportunities and loader opportunities independently to handle the 2:1 ratio fairly

**FR6:** The system shall use random tiebreaking when multiple students have equal fewest opportunities for a role

**FR7:** The system shall enforce variance ≤ 1 constraint, ensuring no student receives 2+ more opportunities than another student in the same role

**FR8:** The system shall prevent physically impossible assignments by ensuring no student is assigned to both teams in the same round

**FR9:** The system shall display generated schedules in a clear table/grid format showing Round × Team × Role → Student Name

**FR10:** The system shall provide a "Generate Schedule" button that executes the algorithm and displays results

**FR11:** The system shall provide a "Regenerate" button that re-runs the algorithm with the same inputs to produce a different fair schedule

**FR12:** The system shall handle edge cases where perfect balance is mathematically impossible without generating errors

### Non-Functional Requirements

**NFR1:** Schedule generation shall complete in under 1 second for typical tournament sizes (≤30 students, ≤15 rounds)

**NFR2:** The system shall work offline after initial page load, with no external API calls required for core functionality

**NFR3:** The system shall function without build tools, npm dependencies, or backend infrastructure

**NFR4:** The system shall be compatible with modern browsers (Chrome, Firefox, Safari, Edge - last 2 versions)

**NFR5:** The system shall use only CDN-based dependencies (Tailwind CSS, Alpine.js) for zero-setup deployment

**NFR6:** The system shall be implementable and testable within 4-5 hours (same-day delivery constraint)

**NFR7:** The generated schedule display shall be readable when projected or printed without additional styling

**NFR8:** The system shall not collect, store, or transmit any user data (privacy by design)

**NFR9:** The UI shall remain responsive during algorithm execution with no freezing or blocking

**NFR10:** The system shall be deployable as a single HTML file or via static file hosting (GitHub Pages, Netlify, etc.)

---

## User Interface Design Goals

### Overall UX Vision

The VexIQ Team Pairing Tool embraces radical simplicity: a single-screen interface where coaches can input data, generate schedules, and view results without navigation, tabs, or cognitive overhead. The design philosophy is "instant utility"—every element serves the core workflow of getting from names to fair schedule in under 2 minutes. Visual hierarchy guides users naturally from inputs (top) to action (middle) to results (bottom), mimicking the logical flow of the task.

### Key Interaction Paradigms

- **Paste-and-go workflow:** Coaches paste student lists from existing documents (rosters, spreadsheets) directly into the textarea, eliminating manual typing
- **Generate-regenerate pattern:** Primary action generates the initial schedule; regenerate button allows instant iteration without re-entering data
- **Immediate feedback:** Results appear instantly below inputs without page refresh, maintaining context
- **Print-friendly output:** Generated schedule is designed for immediate projection or printing without additional steps

### Core Screens and Views

- **Single-page application:** All functionality on one screen (no navigation required)
  - **Input Section:** Student names textarea, round/team number inputs
  - **Action Section:** Generate and Regenerate buttons
  - **Results Section:** Schedule table/grid display

### Accessibility: None

Given the same-day delivery constraint and niche user base (coaches in controlled environments), accessibility enhancements are deferred to Phase 2. MVP focuses on core functionality for standard desktop/tablet browsers.

### Branding

Minimal branding approach using clean, professional Tailwind CSS defaults. No custom branding required—tool prioritizes utility over visual identity. Design should feel like a professional coaching tool, not a consumer app.

### Target Device and Platforms: Web Responsive (Desktop and Tablet primary)

Optimized for desktop and tablet use during tournament preparation and execution. Mobile viewable but not optimized—coaches typically use larger screens for projection/printing workflows.

---

## Technical Assumptions

### Repository Structure: Monorepo (single-file initially)

The MVP will be a single `index.html` file containing all HTML, CSS (via Tailwind CDN), and JavaScript (Alpine.js via CDN). Documentation lives in `/docs` directory. This ultra-simple structure enables same-day delivery and maximum portability.

**Rationale:** No build process, no dependencies, instant deployment. Can evolve to proper monorepo structure in Phase 2 if needed.

### Service Architecture

**Client-side only (Static Single-Page Application)**

- No backend services or APIs
- All logic executes in the browser via Alpine.js
- Zero server-side dependencies or infrastructure
- Algorithm runs entirely client-side using JavaScript

**Rationale:** Aligns with same-day delivery, zero-cost hosting, offline-first requirements, and privacy-by-design (no data transmission).

### Testing Requirements

**Manual testing with real tournament data**

Given the 4-5 hour delivery constraint, testing will be manual and focused:
- Test with Jon's actual student roster and tournament parameters
- Validate algorithm fairness with edge cases (few students, many rounds)
- Verify cross-browser compatibility (Chrome, Firefox, Safari)
- Test print/projection readability

**No automated test suite for MVP.** Testing framework would consume significant development time that should be spent on core functionality.

### Additional Technical Assumptions and Requests

- **Tech Stack:**
  - HTML5 for structure
  - Tailwind CSS via CDN for styling
  - Alpine.js via CDN for reactive UI and algorithm logic
  - No bundlers, transpilers, or build tools

- **Algorithm Implementation:**
  - Greedy fill algorithm implemented in vanilla JavaScript within Alpine.js component
  - Randomization uses `Math.random()` for tiebreaking
  - Data structures: Arrays for students, objects for tracking opportunities

- **Browser Storage:**
  - No localStorage or persistence in MVP
  - All data ephemeral (cleared on page refresh)

- **Hosting:**
  - Static file hosting (GitHub Pages, Netlify, or local file open)
  - No server configuration required

- **Performance:**
  - Synchronous algorithm execution (no Web Workers needed for MVP scale)
  - Expected O(n*r) complexity acceptable for target sizes

---

## Epic List

**Epic 1: Core Schedule Generation Engine**
Build the foundational scheduling algorithm and basic UI to generate fair role assignments, delivering immediate value as a functional tournament scheduler.

**Epic 2: Enhanced Usability and Refinement**
Add regeneration capability, improve visual presentation, and refine the user experience for real-world tournament use.

---

## Epic 1: Core Schedule Generation Engine

**Epic Goal:** Establish the project foundation and implement the core greedy fill scheduling algorithm with independent role tracking and variance enforcement, delivering a working scheduler that generates mathematically fair role assignments for VexIQ tournaments.

### Story 1.1: Project Setup and Basic HTML Structure

**As a** coach,
**I want** to open a web page with input fields for my tournament data,
**so that** I can begin using the scheduling tool immediately.

#### Acceptance Criteria

1. Single `index.html` file exists with proper HTML5 structure
2. Tailwind CSS loaded via CDN and functional
3. Alpine.js loaded via CDN and initialized
4. Page displays a textarea for student names (placeholder: "Enter student names, one per line")
5. Page displays number input for rounds (label: "Number of Rounds", min=1, max=99)
6. Page displays number input for teams (label: "Number of Teams", min=1, max=2)
7. Page has clear heading: "VexIQ Team Pairing Tool"
8. Basic responsive layout works on desktop and tablet
9. File can be opened locally in browser without server

---

### Story 1.2: Implement Greedy Fill Algorithm with Role Tracking

**As a** developer,
**I want** to implement the core scheduling algorithm,
**so that** the system can generate fair role assignments.

#### Acceptance Criteria

1. Alpine.js component created with reactive data properties for inputs
2. Algorithm maintains separate opportunity counters for driver and loader roles per student
3. Algorithm processes round-by-round, team-by-team assignment
4. For each driver slot (2 per team): algorithm selects student with fewest driver opportunities
5. For each loader slot (1 per team): algorithm selects student with fewest loader opportunities
6. When multiple students are tied for fewest opportunities, algorithm randomly selects one using `Math.random()`
7. Algorithm enforces hard constraint: no student assigned to both teams in same round
8. Algorithm handles edge cases where perfect balance is impossible (best-effort assignment)
9. Generated schedule stored in reactive Alpine.js data structure
10. Algorithm completes in < 1 second for 30 students × 15 rounds

---

### Story 1.3: Variance Enforcement and Fairness Validation

**As a** coach,
**I want** the algorithm to ensure no student gets significantly more opportunities than others,
**so that** all students perceive the schedule as fair.

#### Acceptance Criteria

1. Algorithm tracks opportunity variance per role (driver variance, loader variance)
2. After each assignment, algorithm verifies variance ≤ 1 for both roles
3. If variance would exceed 1, algorithm backtracks or selects alternative student
4. Final schedule guarantees: max(driver_opportunities) - min(driver_opportunities) ≤ 1
5. Final schedule guarantees: max(loader_opportunities) - min(loader_opportunities) ≤ 1
6. Algorithm logs warnings (console) if perfect balance is mathematically impossible
7. Tested with edge cases: 5 students × 20 rounds, 25 students × 5 rounds, 8 students × 10 rounds

---

### Story 1.4: Display Generated Schedule

**As a** coach,
**I want** to see the generated schedule in a clear, readable format,
**so that** I can use it during the tournament.

#### Acceptance Criteria

1. "Generate Schedule" button triggers algorithm and displays results
2. Schedule displayed as HTML table with columns: Round, Team, Driver 1, Driver 2, Loader
3. Table uses Tailwind CSS styling for readability (borders, padding, alternating row colors)
4. Each cell shows student name clearly
5. Table is responsive and readable on desktop/tablet screens
6. Empty state message shown before generation: "Enter student names and click Generate"
7. If inputs are invalid (empty names, 0 rounds), display error message instead of generating
8. Table suitable for projection (readable from distance) and printing (clear text, good contrast)

---

## Epic 2: Enhanced Usability and Refinement

**Epic Goal:** Improve user experience by adding schedule regeneration capability, refining the visual presentation for tournament use, and ensuring the tool handles real-world usage patterns gracefully.

### Story 2.1: Regenerate Schedule Functionality

**As a** coach,
**I want** to regenerate a new fair schedule without re-entering data,
**so that** I can explore different student pairings while maintaining fairness.

#### Acceptance Criteria

1. "Regenerate" button appears after initial schedule generation
2. Clicking "Regenerate" re-runs algorithm with same inputs (names, rounds, teams)
3. New schedule uses different randomization seed, producing different pairings
4. New schedule maintains all fairness guarantees (variance ≤ 1, no conflicts)
5. Regeneration completes in < 1 second
6. Button is visually distinct from "Generate" (different color or style)
7. Multiple regenerations produce varied results (not cycling through fixed set)

---

### Story 2.2: Visual Refinement and Print Optimization

**As a** coach,
**I want** the schedule to look professional and print cleanly,
**so that** I can share it with students and use it during tournaments.

#### Acceptance Criteria

1. Table styling refined with clear visual hierarchy (header row stands out)
2. Round numbers are bold or highlighted for easy scanning
3. Team names/numbers clearly distinguished
4. Font sizes appropriate for both screen viewing and printing
5. Page margins and padding optimized for 8.5×11 printing
6. Tailwind print utilities applied to hide/show appropriate elements when printing
7. Color scheme works in both screen and print contexts (no critical info lost in grayscale)
8. Page title and metadata included in print output

---

### Story 2.3: Input Validation and User Feedback

**As a** coach,
**I want** helpful feedback when I enter invalid data,
**so that** I can quickly correct mistakes and generate a valid schedule.

#### Acceptance Criteria

1. Validation triggers on "Generate" button click
2. If student names are empty or whitespace-only, show error: "Please enter at least 3 student names"
3. If rounds = 0, show error: "Number of rounds must be at least 1"
4. If teams = 0, show error: "Number of teams must be 1 or 2"
5. If number of students < 3, show warning: "Very few students - schedule may be repetitive"
6. Error messages displayed prominently above results area with red/warning styling
7. Error messages clear when user fixes input and regenerates
8. Generate button disabled while validation errors present

---

### Story 2.4: Edge Case Handling and Algorithm Robustness

**As a** developer,
**I want** the algorithm to handle unusual inputs gracefully,
**so that** the tool doesn't crash or produce invalid schedules.

#### Acceptance Criteria

1. Algorithm handles 2 students (minimum viable tournament size)
2. Algorithm handles 50+ students (large tournament)
3. Algorithm handles 1 round (minimal tournament)
4. Algorithm handles 50+ rounds (extended tournament)
5. Algorithm handles team=1 configuration (single-team practice mode)
6. If mathematically impossible configuration (e.g., 4 students, 100 rounds, 2 teams), algorithm produces best-effort schedule and logs console warning
7. No JavaScript errors thrown for any valid input combination
8. Algorithm never produces physically impossible assignments (student in both teams same round) under any circumstances

---

## Checklist Results Report

### PRD Validation Report

#### Executive Summary

- **Overall PRD Completeness:** 92%
- **MVP Scope Appropriateness:** Just Right
- **Readiness for Architecture Phase:** Ready
- **Most Critical Gaps:** Minor - No blocking issues identified

#### Category Analysis Table

| Category                         | Status  | Critical Issues                                      |
| -------------------------------- | ------- | ---------------------------------------------------- |
| 1. Problem Definition & Context  | PASS    | None                                                 |
| 2. MVP Scope Definition          | PASS    | None                                                 |
| 3. User Experience Requirements  | PASS    | None                                                 |
| 4. Functional Requirements       | PASS    | None                                                 |
| 5. Non-Functional Requirements   | PASS    | None                                                 |
| 6. Epic & Story Structure        | PASS    | None                                                 |
| 7. Technical Guidance            | PASS    | None                                                 |
| 8. Cross-Functional Requirements | PARTIAL | Data requirements implicit (client-side only)        |
| 9. Clarity & Communication       | PASS    | None                                                 |

#### Top Issues by Priority

**BLOCKERS:** None

**HIGH:** None

**MEDIUM:**
- Data requirements section could be more explicit about the ephemeral nature of client-side data structures
- Integration section N/A but not explicitly stated

**LOW:**
- Could add user flow diagram for visual learners
- Post-MVP roadmap could be more detailed

#### MVP Scope Assessment

**Scope is appropriate for same-day delivery:**
- ✅ Two epics with 4 stories each = manageable 4-5 hour implementation
- ✅ No feature bloat - strict adherence to Brief's MVP boundaries
- ✅ Each story is independently valuable and testable
- ✅ Algorithm stories properly sequenced (core → validation → display)

**Nothing should be cut** - this is truly minimal viable product.

**Nothing is missing** - all core requirements from Brief are captured.

**Complexity Concerns:**
- Story 1.3 (Variance Enforcement) is the most algorithmically complex - may need developer attention
- Edge case handling (Story 2.4) could surface unexpected scenarios

**Timeline Realism:** 4-5 hour delivery is aggressive but achievable given single-file architecture and no backend.

#### Technical Readiness

**Clarity of Technical Constraints:** Excellent
- Single-file HTML/CSS/JS explicitly specified
- CDN-only dependencies clearly stated
- No build process requirement well-documented

**Identified Technical Risks:**
- Algorithm edge cases with very small/large student counts
- Browser Math.random() quality for "fairness perception"
- Print CSS behavior across browsers

**Areas Needing Architect Investigation:**
- Optimal data structure for tracking opportunities (arrays vs objects)
- Algorithm backtracking strategy if variance enforcement fails
- Alpine.js reactive performance with large schedules

#### Recommendations

1. **Consider adding:** A brief technical spike story at beginning of Epic 1 to validate algorithm approach with pseudocode/prototype before full implementation (optional - may save time if algorithm proves tricky)

2. **Documentation:** Consider adding inline code comments requirement to Story 1.2 for algorithm maintainability

3. **Testing:** Story 1.3 AC #7 calls for edge case testing - ensure Jon has test datasets ready

4. **Phase 2 Planning:** Once MVP deployed, immediately capture user feedback to inform Phase 2 priorities

#### Final Decision

**✅ READY FOR ARCHITECT**

The PRD is comprehensive, properly structured, and provides clear guidance for architectural design. The epic/story breakdown is logical, sequential, and appropriately sized for AI agent execution. Technical constraints are well-documented. No blocking issues identified.

The architect can proceed with confidence.

---

## Next Steps

### UX Expert Prompt

```
I'm handing off the VexIQ Team Pairing Tool PRD for UX design. Please review docs/prd.md and create the UI/UX architecture focusing on:

1. Single-page layout design (input section → action buttons → results table)
2. Tailwind CSS component specifications for inputs, buttons, and schedule table
3. Responsive design considerations for desktop/tablet
4. Print-friendly styling approach
5. Visual hierarchy and information architecture

This is a same-day delivery MVP with radical simplicity as core design principle. No navigation, tabs, or complex interactions. The workflow is: paste names → set parameters → generate → view results.

Please create the UX architecture document at docs/ux-architecture.md.
```

### Architect Prompt

```
I'm handing off the VexIQ Team Pairing Tool PRD for technical architecture. Please review docs/prd.md and create the technical architecture covering:

1. Single-file HTML structure with Alpine.js component design
2. Algorithm implementation approach (greedy fill with variance enforcement)
3. Data structures for student tracking and opportunity counting
4. Edge case handling strategies
5. Performance optimization for client-side execution

Key constraints: Single index.html file, CDN-only dependencies (Tailwind/Alpine.js), no build process, 4-5 hour implementation timeline, must work offline.

Please create the architecture document at docs/architecture.md and prepare for story-by-story implementation.
```

---

*Generated using BMAD-METHOD™ framework*
*PM Agent: John (pm)*
*Date: 2025-10-10*
