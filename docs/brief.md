# Project Brief: VexIQ Team Pairing Tool

**Date:** 2025-10-10
**Author:** Business Analyst Mary
**Status:** Draft v1.0

---

## Executive Summary

The VexIQ Team Pairing Tool is a simple web-based scheduler that generates fair driver/loader role assignments for VexIQ robotics tournaments. Built with HTML, Tailwind CSS, and Alpine.js, this tool solves the most time-consuming part of tournament preparation—creating an equitable initial schedule that prevents forfeits and maintains student morale. The tool targets VexIQ coaches and tournament organizers who need to quickly generate fair role distributions across multiple tournament rounds, ensuring every student gets approximately equal opportunities in each role (driver vs. loader) while respecting physical constraints.

**Key Value Proposition:** Eliminates hours of manual scheduling work while ensuring mathematical fairness (variance ≤ 1 per role), allowing coaches to focus on the collaborative learning experience rather than logistics.

---

## Problem Statement

### Current State and Pain Points

VexIQ coaches currently face a significant logistical challenge when preparing for tournaments: manually creating fair role assignments for students across multiple rounds. Each team requires 2 drivers and 1 loader per round, and with varying numbers of students and rounds, ensuring equitable distribution becomes mathematically complex and time-consuming.

**Specific Pain Points:**
- Manual scheduling takes hours and is error-prone
- Difficult to track fairness across two different roles (drivers get 2x opportunities vs. loaders)
- Students who feel assignments are unfair become disengaged
- Last-minute changes require complete schedule regeneration
- Coaches lack a quick way to visualize who's playing when

### Impact of the Problem

**Quantified Impact:**
- 2-4 hours of coach time per tournament spent on manual scheduling
- Forfeit rates increase when students aren't prepared/present for their assigned slots
- Team morale damage from perceived unfairness or forfeits

**Root Cause Impact:**
When students don't know their schedule in advance or perceive unfairness, they may wander off or miss their slot, resulting in forfeits. Forfeits decrease team morale and damage the collaborative learning environment—the core purpose of VexIQ participation.

### Why Existing Solutions Fall Short

- **Spreadsheets:** Require manual formulas and don't prevent errors; no randomization
- **Generic scheduling tools:** Don't understand the 2-driver + 1-loader constraint or track role equity separately
- **Complete tournament management platforms:** Over-engineered for this simple need; require accounts, setup time, ongoing maintenance

### Urgency

**Same-day delivery requirement.** Jon needs this working for an upcoming tournament, making simplicity and speed critical constraints that drive all design decisions.

---

## Proposed Solution

### Core Concept

A single-page web application that takes three inputs (student names, number of rounds, number of teams) and instantly generates a fair role assignment schedule using a greedy fill algorithm with random tiebreaking.

**Key Approach:**
- **Greedy Fill Algorithm:** Assign roles round-by-round, always selecting students with the fewest opportunities for that specific role
- **Independent Role Tracking:** Track driver opportunities and loader opportunities separately to handle the 2:1 ratio fairly
- **Random Tiebreaking:** When multiple students are tied for fewest opportunities, randomly select to add perceived fairness
- **Variance ≤ 1 Enforcement:** No student gets 2+ more opportunities than another in the same role

### Key Differentiators

1. **Role-Specific Fairness:** Unlike generic schedulers, explicitly tracks driver equity separate from loader equity
2. **Zero Setup:** No accounts, databases, or configuration—just open and use
3. **Instant Regeneration:** Don't like the randomization? Click "Regenerate" for a new fair schedule
4. **Physically Impossible Constraint Handling:** Automatically prevents students from being assigned to both teams in the same round

### Why This Will Succeed

- **Surgical Scope:** Solves exactly one problem (initial schedule generation) and hands off to existing workflow
- **Mathematical Correctness:** Greedy algorithm with variance enforcement guarantees fairness
- **Speed to Value:** Works immediately without learning curve or setup time
- **Built for Reality:** Embraces that coaches will manually execute schedules (tool doesn't try to replace what's already working)

### High-Level Vision

A clean, printable/projectable schedule that coaches can generate in 30 seconds, use during tournaments, and regenerate as needed without friction.

---

## Target Users

### Primary User Segment: VexIQ Coaches & Tournament Organizers

**Profile:**
- Volunteers or educators managing 10-30 students in VexIQ robotics programs
- Technically comfortable but not developers
- Time-constrained, need tools that "just work"
- Focused on educational outcomes over technical sophistication

**Current Behaviors:**
- Manually create schedules in spreadsheets or on paper
- Bring printed schedules to tournaments
- Manage student assignments verbally during events
- Track fairness mentally or through rough tallies

**Specific Needs:**
- Fast schedule generation (minutes, not hours)
- Mathematically fair distribution across roles
- Easy-to-read output for tournament day
- Ability to regenerate quickly if needed

**Goals:**
- Ensure all students get fair opportunities
- Prevent forfeits through clear advance communication
- Maintain positive team morale and collaborative learning environment
- Minimize time spent on logistics vs. coaching

---

## Goals & Success Metrics

### Business Objectives

- **Adoption:** Tool used successfully for at least 5 tournaments within first month
- **Time Savings:** Reduce schedule creation time from 2-4 hours to under 5 minutes per tournament
- **User Satisfaction:** Coaches report the tool "just works" and saves significant time

### User Success Metrics

- **Fairness Achieved:** Generated schedules maintain variance ≤ 1 for both driver and loader roles across all students
- **Forfeit Reduction:** Decreased forfeit rates due to clear advance scheduling (qualitative feedback)
- **Regeneration Usage:** Coaches feel comfortable regenerating schedules when needed (indicates trust in tool)

### Key Performance Indicators (KPIs)

- **Schedule Generation Time:** < 5 minutes from opening tool to having usable schedule (target: < 2 minutes)
- **Fairness Compliance:** 100% of generated schedules meet variance ≤ 1 constraint
- **Error Rate:** Zero schedules with physically impossible assignments (student in both teams same round)
- **Same-Day Delivery:** MVP completed and tested within 4-5 hours

---

## MVP Scope

### Core Features (Must Have)

- **Student Name Input:** Simple textarea accepting one name per line (paste-friendly)
- **Round/Team Configuration:** Two number inputs (1-99 rounds, 1-2 teams)
- **Generate Schedule Button:** Executes greedy fill algorithm and displays results
- **Fair Role Assignment Algorithm:**
  - Greedy fill with random tiebreaking
  - Independent tracking for driver vs. loader opportunities
  - Variance ≤ 1 enforcement per role
  - Hard constraint: no student in both teams same round
- **Clear Results Display:** Table/grid showing Round × Team × Role → Student Name
- **Regenerate Button:** Re-run algorithm with same inputs for new randomization
- **Minimal Styling:** Clean, readable Tailwind CSS styling suitable for projection/printing

### Out of Scope for MVP

- Persistence/saving schedules to localStorage or database
- Export functionality (CSV, PDF, JSON)
- Student attendance tracking during tournament
- Multi-tournament history or cross-tournament analytics
- Real-time tournament dashboard or countdown timers
- Print-specific CSS optimization
- Copy-to-clipboard functionality
- Input validation warnings (e.g., "too many rounds for student count")
- Fairness metrics display (e.g., showing slot counts per student)

### MVP Success Criteria

**The MVP is successful when:**
1. Jon can input student names, specify rounds/teams, and generate a fair schedule in under 2 minutes
2. Generated schedules are mathematically fair (variance ≤ 1 per role) 100% of the time
3. Output is readable enough to use during an actual tournament
4. Regenerate button produces different fair schedules on demand
5. Tool works in a standard modern browser without installation or setup

---

## Post-MVP Vision

### Phase 2 Features

**Immediate Next Priorities (Post-Launch):**
- **Persistence:** Save generated schedules to localStorage for retrieval
- **Export Options:** Download as CSV or printable format
- **Input Validation:** Helpful warnings for edge cases (too many/few students, impossible configurations)
- **Fairness Metrics Display:** Show each student's total driver/loader slot counts before and after generation

**Secondary Enhancements:**
- Student attendance tracking (mark absent, regenerate without them)
- Custom team names instead of "Team 1, Team 2"
- Print-optimized CSS styling

### Long-term Vision

**1-2 Year Vision:**
A lightweight tournament preparation suite that remains simple and fast but adds quality-of-life features:
- Multi-tournament history tracking
- Cross-tournament fairness (ensure students don't always get same roles across events)
- Basic analytics (most common pairings, role preferences)
- Shareable schedule links for parents/students

**Expansion Opportunities:**

**Adjacent Use Cases:**
- Adapted for other competitive robotics formats (FTC, FRC substitution scheduling)
- General sports/activity rotation scheduling with role equity requirements
- Classroom group assignment tools with fairness constraints

**Moonshot Ideas (Transformative but High-Effort):**
- Real-time tournament dashboard with live updates, timers, notifications
- AI-powered team chemistry optimization based on historical performance data
- Mobile app with offline-first architecture for tournament venues with poor connectivity

**Intentional Non-Expansion:**
Do NOT evolve into a full tournament management platform—stay focused on the scheduling problem.

---

## Technical Considerations

### Platform Requirements

- **Target Platforms:** Modern web browsers (Chrome, Firefox, Safari, Edge - last 2 versions)
- **Browser/OS Support:** Desktop and tablet preferred (mobile usable but not optimized)
- **Performance Requirements:**
  - Schedule generation completes in < 1 second for typical tournament size (20 students, 10 rounds)
  - Responsive UI updates (no freezing during algorithm execution)
  - Works offline after initial page load (no external dependencies after assets load)

### Technology Preferences

- **Frontend:** HTML5, Tailwind CSS (CDN), Alpine.js (CDN)
- **Backend:** None (static site, client-side only)
- **Database:** None for MVP (localStorage for Phase 2)
- **Hosting/Infrastructure:** Static file hosting (GitHub Pages, Netlify, Vercel, or simple file serving)

**Rationale for Stack:**
- **Simplicity:** No build process, no npm dependencies, no deployment complexity
- **Speed:** Can be completed in same-day timeframe
- **Portability:** Single HTML file can be copied and used anywhere
- **Maintainability:** Minimal dependencies reduce long-term maintenance burden

### Architecture Considerations

- **Repository Structure:** Single root-level HTML file (index.html) for MVP; documentation in `/docs`
- **Service Architecture:** Client-side only, no API calls
- **Integration Requirements:** None (standalone tool)
- **Security/Compliance:**
  - No data collection or transmission (privacy by design)
  - No PII stored (student names remain client-side only)
  - No authentication/authorization needed

**Algorithm Performance Notes:**
- Greedy fill is O(n*r) where n=students, r=rounds
- Expected to be near-instant for realistic tournament sizes (< 50 students, < 20 rounds)
- No optimization needed for MVP scope

---

## Constraints & Assumptions

### Constraints

- **Budget:** $0 (free hosting, no paid services)
- **Timeline:** Same-day delivery (4-5 hours for implementation + testing)
- **Resources:** Solo developer (Jon + Claude), no dedicated QA team
- **Technical:**
  - Must work without build tools or npm
  - Must work offline after initial load
  - No backend infrastructure available

### Key Assumptions

- Coaches have reliable internet access to initially load the page (CDN dependencies)
- Modern browser availability (no IE11 support needed)
- Student names are manually entered (no integration with roster systems)
- Typical tournament size: 10-30 students, 5-15 rounds, 1-2 teams
- Manual transcription of schedules to physical formats is acceptable (no auto-print needed)
- Randomization variety from regenerate button is sufficient (no manual assignment override needed)
- Coaches understand basic fairness math (don't need explanation of algorithm)
- English language UI is sufficient (no i18n needed)

---

## Risks & Open Questions

### Key Risks

- **Algorithm Edge Cases:** With very few students (< 6) or very many rounds relative to students, the greedy algorithm might struggle to maintain variance ≤ 1. *Mitigation: Test edge cases explicitly; consider displaying warning if impossible configuration detected.*

- **User Expectation Mismatch:** Coaches might expect features beyond MVP scope (persistence, printing, analytics). *Mitigation: Clear communication about MVP scope; Phase 2 roadmap visible.*

- **Randomization Perception:** Users might regenerate many times seeking "perfect" pairings (e.g., friends together), potentially misusing tool. *Mitigation: Accept as edge case; random is intentionally random.*

- **Browser Compatibility:** CDN dependencies might fail or behave differently across browsers. *Mitigation: Test in Chrome, Firefox, Safari before launch; use well-established CDN versions.*

### Open Questions

- Should the tool show fairness metrics (e.g., "Each student: 3-4 driver slots, 2-3 loader slots") after generation?
- How should edge cases where perfect balance is mathematically impossible be handled? (Display warning? Auto-adjust inputs?)
- Would a "preview" mode showing expected slot totals before generating be helpful?
- Should input validation provide warnings (e.g., "You have 20 students and 100 rounds—everyone will play many times")?
- What happens if someone enters 0 students or 0 rounds? (Simple validation needed?)

### Areas Needing Further Research

- **Edge Case Testing:** Systematically test with 2 students, 3 students, 50 students, 1 round, 50 rounds to identify failure modes
- **Visual Design:** Once functional, assess whether Tailwind defaults are sufficient or if custom styling improves usability
- **Algorithm Performance:** Verify greedy fill is fast enough with large student counts (unlikely issue but untested)
- **Error Handling UX:** Determine appropriate user feedback for invalid inputs or algorithm failures

---

## Appendices

### A. Research Summary

**Brainstorming Session (2025-10-10):**
- **Techniques Used:** First Principles Thinking, "Yes, And..." Building, Five Whys
- **Key Finding:** Root purpose is protecting collaborative learning experience by preventing forfeits
- **Core Insight:** Independent role tracking (driver equity ≠ loader equity) is essential due to 2:1 ratio
- **Scope Validation:** Five Whys confirmed tool should only generate initial schedule, not manage tournament execution

**Competitive Analysis (Informal):**
- Generic scheduling tools don't understand VexIQ role constraints
- Tournament management platforms are over-engineered for this narrow need
- No existing free tool specifically addresses VexIQ role equity scheduling

### B. Stakeholder Input

**Jon (Primary Coach):**
- Same-day delivery is critical constraint
- Existing tournament execution workflow (manual) is working well—don't replace it
- Simplicity and speed trump feature richness
- Regenerate functionality important for perceived control

**Students (Indirect):**
- Need clear advance notice of when they're playing (prevents forfeits)
- Fairness perception is important for morale
- Collaborative learning experience is primary value

### C. References

- Brainstorming session results: `/docs/brainstorming-session-results.md`
- VexIQ tournament structure: 2 drivers + 1 loader per team per round
- Greedy algorithm approach: validated during First Principles session

---

## Next Steps

### Immediate Actions

1. **Review and approve this Project Brief** with Jon
2. **Create initial algorithm pseudocode** to validate greedy fill approach
3. **Set up basic HTML/Tailwind/Alpine.js structure** (boilerplate)
4. **Implement core algorithm** with role tracking and variance enforcement
5. **Build minimal UI** (inputs + results display)
6. **Test with real student data** (Jon's actual roster + typical tournament parameters)
7. **Refine styling** for readability (projection/printing consideration)
8. **Final validation** with Jon before tournament use

### PM Handoff

This Project Brief provides the full context for **VexIQ Team Pairing Tool**. The brainstorming session has thoroughly validated scope, approach, and constraints.

**Recommended Next Phase:**
Proceed directly to development given same-day timeline constraint. A full PRD may be unnecessary given the surgical scope and clear requirements. However, if you'd like to create a formal PRD, please start in 'PRD Generation Mode', review this brief thoroughly, and work with Jon to create the PRD section by section as the template indicates, asking for any necessary clarification or suggesting improvements.

---

*Document generated using BMAD-METHOD™ framework*
*Based on brainstorming session conducted 2025-10-10*
