# Components

The architecture is organized into logical components, though all exist within a single HTML file.

## Input Form

**Responsibility:** Capture tournament parameters (student names, rounds, teams) and validate user input

**Key Interfaces:**
- `x-model` bindings for textarea and number inputs to Alpine.js data
- `@submit.prevent` event handler to trigger generation
- Data binding: `studentNames`, `numRounds`, `numTeams`

**Dependencies:**
- Alpine.js for reactive two-way binding
- Tailwind CSS for form styling

**Technology Stack:**
- Native HTML5 `<form>`, `<textarea>`, `<input type="number">`
- Alpine.js `x-model` directive
- Tailwind utility classes for layout and styling

---

## Algorithm Engine

**Responsibility:** Core greedy fill scheduling algorithm with independent role tracking and variance enforcement

**Key Interfaces:**
- `generateSchedule()` - Main entry point, orchestrates parsing → validation → generation
- `selectStudentForRole(role)` - Returns student with fewest opportunities for specified role
- `assignTeam(roundNum, teamNum)` - Assigns 2 drivers + 1 loader for one team
- `validateVariance()` - Ensures no student has 2+ more opportunities than another in same role

**Dependencies:**
- Input Form component for data (`studentNames`, `numRounds`, `numTeams`)
- Student data model with opportunity counters
- Math.random() for tiebreaking

**Technology Stack:**
- Pure JavaScript (ES6+) within Alpine.js component
- Array methods: `filter()`, `sort()`, `reduce()` for opportunity calculations
- Console logging for edge case warnings

**Algorithm Pseudocode:**
```javascript
for each round (1 to numRounds):
  for each team (1 to numTeams):
    // Assign 2 drivers
    for i = 1 to 2:
      student = selectStudentForRole('driver')
      assign student as driver
      increment student.driverCount

    // Assign 1 loader
    student = selectStudentForRole('loader')
    assign student as loader
    increment student.loaderCount

selectStudentForRole(role):
  available = students.filter(not already assigned this round)
  minCount = min(available[].roleCount)
  candidates = available.filter(roleCount === minCount)

  if variance would exceed 1:
    candidates = candidates.filter(keeps variance ≤ 1)

  return random(candidates)
```

---

## Schedule Display

**Responsibility:** Render generated schedule as HTML table with clear visual hierarchy for projection/printing

**Key Interfaces:**
- `x-show` directive to toggle visibility based on `isGenerated`
- `x-for` loops to iterate over `rounds` and `teams` arrays
- Data consumption: `rounds[]` with nested `teams[]`

**Dependencies:**
- Algorithm Engine component (consumes its output)
- Tailwind CSS for table styling and print optimization

**Technology Stack:**
- Semantic HTML `<table>` structure
- Alpine.js `x-for` for list rendering
- Tailwind table utilities (`border`, `p-4`, alternating row colors)
- Print-specific Tailwind classes (`print:text-lg`)

---

## Action Buttons

**Responsibility:** Trigger schedule generation and regeneration with appropriate UI state management

**Key Interfaces:**
- `@click` handlers for `generateSchedule()` and `regenerateSchedule()`
- `x-show` conditional rendering (Generate vs Regenerate)

**Dependencies:**
- Algorithm Engine component (invokes its methods)
- Input Form component (reads its data)

**Technology Stack:**
- HTML `<button>` elements
- Alpine.js `@click` event binding and `x-show` directive
- Tailwind button styling (primary/secondary colors)

---

## Error Display

**Responsibility:** Show validation errors and edge case warnings to user

**Key Interfaces:**
- `x-show` directive keyed to `errorMessage` presence
- `x-text` binding to display `errorMessage` string

**Dependencies:**
- Input Form validation (consumes validation results)
- Algorithm Engine warnings (displays edge case messages)

**Technology Stack:**
- HTML `<div>` with conditional rendering
- Alpine.js `x-show` and `x-text` directives
- Tailwind alert styling (red background, padding)

---

## Component Diagram

```mermaid
graph TD
    InputForm[Input Form Component]
    ActionButtons[Action Buttons Component]
    AlgoEngine[Algorithm Engine Component]
    ScheduleDisplay[Schedule Display Component]
    ErrorDisplay[Error Display Component]

    InputForm -->|studentNames, numRounds, numTeams| AlgoEngine
    ActionButtons -->|trigger generateSchedule| AlgoEngine
    ActionButtons -->|trigger regenerateSchedule| AlgoEngine
    AlgoEngine -->|rounds[] data| ScheduleDisplay
    AlgoEngine -->|errorMessage| ErrorDisplay
    InputForm -->|validation errors| ErrorDisplay

    style AlgoEngine fill:#2196F3
    style InputForm fill:#4CAF50
    style ScheduleDisplay fill:#FF9800
    style ActionButtons fill:#9C27B0
    style ErrorDisplay fill:#F44336
```

---
