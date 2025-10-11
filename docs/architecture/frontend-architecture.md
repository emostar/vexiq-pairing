# Frontend Architecture

## Component Architecture

### Component Organization

The entire application is a single Alpine.js component defined in `index.html`:

```
index.html
├── <!DOCTYPE html>
├── <head>
│   ├── Tailwind CSS CDN link
│   └── Alpine.js CDN script (defer)
├── <body>
│   └── <div x-data="scheduleApp()">
│       ├── Header section
│       ├── Input form section
│       ├── Action buttons section
│       ├── Error display section
│       └── Schedule display section (table)
└── <script>
    └── function scheduleApp() { ... }
        ├── Reactive data properties
        └── Methods (generateSchedule, etc.)
```

**Component Organization Strategy:**

- **Single `x-data` component:** All state and logic in one `scheduleApp()` function
- **Section-based layout:** HTML organized into logical sections using Tailwind spacing
- **Co-located logic:** JavaScript function defined in `<script>` tag at bottom of `<body>`

### Component Template

```javascript
function scheduleApp() {
  return {
    // ===== DATA PROPERTIES =====
    studentNames: '',
    numRounds: 5,
    numTeams: 2,
    students: [],
    rounds: [],
    errorMessage: '',
    isGenerated: false,

    // ===== METHODS =====

    // Parse textarea into Student objects
    parseStudents() {
      const names = this.studentNames
        .split('\n')
        .map(name => name.trim())
        .filter(name => name.length > 0);

      return names.map((name, index) => ({
        id: index,
        name: name,
        driverCount: 0,
        loaderCount: 0
      }));
    },

    // Validate inputs before generation
    validateInputs() {
      if (!this.studentNames.trim()) {
        this.errorMessage = 'Please enter at least 3 student names';
        return false;
      }

      const studentCount = this.parseStudents().length;
      if (studentCount < 3) {
        this.errorMessage = 'Please enter at least 3 student names';
        return false;
      }

      if (this.numRounds < 1) {
        this.errorMessage = 'Number of rounds must be at least 1';
        return false;
      }

      if (this.numTeams < 1 || this.numTeams > 2) {
        this.errorMessage = 'Number of teams must be 1 or 2';
        return false;
      }

      this.errorMessage = '';
      return true;
    },

    // Main generation entry point
    generateSchedule() {
      if (!this.validateInputs()) return;

      this.students = this.parseStudents();
      this.rounds = [];

      // Run algorithm
      this.runGreedyFillAlgorithm();

      this.isGenerated = true;
    },

    // Regenerate with same inputs
    regenerateSchedule() {
      // Reset counters
      this.students.forEach(s => {
        s.driverCount = 0;
        s.loaderCount = 0;
      });

      this.rounds = [];
      this.runGreedyFillAlgorithm();
    },

    // Core algorithm implementation
    runGreedyFillAlgorithm() {
      for (let r = 1; r <= this.numRounds; r++) {
        const round = { roundNumber: r, teams: [] };
        const assignedThisRound = new Set();

        for (let t = 1; t <= this.numTeams; t++) {
          const team = { teamNumber: t, driver1: '', driver2: '', loader: '' };

          // Assign driver 1
          const d1 = this.selectStudentForRole('driver', assignedThisRound);
          team.driver1 = d1.name;
          assignedThisRound.add(d1.id);
          d1.driverCount++;

          // Assign driver 2
          const d2 = this.selectStudentForRole('driver', assignedThisRound);
          team.driver2 = d2.name;
          assignedThisRound.add(d2.id);
          d2.driverCount++;

          // Assign loader
          const loader = this.selectStudentForRole('loader', assignedThisRound);
          team.loader = loader.name;
          assignedThisRound.add(loader.id);
          loader.loaderCount++;

          round.teams.push(team);
        }

        this.rounds.push(round);
      }
    },

    // Select student with fewest opportunities for role
    selectStudentForRole(role, assignedThisRound) {
      const countKey = role === 'driver' ? 'driverCount' : 'loaderCount';

      // Filter available students
      const available = this.students.filter(s => !assignedThisRound.has(s.id));

      if (available.length === 0) {
        console.warn('No available students - reusing students in same round');
        return this.students[0]; // Fallback
      }

      // Find minimum count
      const minCount = Math.min(...available.map(s => s[countKey]));

      // Get candidates with min count
      const candidates = available.filter(s => s[countKey] === minCount);

      // Random tiebreaking
      return candidates[Math.floor(Math.random() * candidates.length)];
    }
  };
}
```

---

## State Management Architecture

### State Structure

State is managed entirely within the Alpine.js reactive `x-data` object. No external state management library is needed.

```typescript
// State structure (TypeScript notation for clarity)
interface AppState {
  // Input state (bound to form fields)
  studentNames: string;
  numRounds: number;
  numTeams: number;

  // Computed/generated state
  students: Student[];      // Parsed from studentNames
  rounds: Round[];          // Generated by algorithm

  // UI state
  errorMessage: string;     // Validation/error display
  isGenerated: boolean;     // Controls button visibility
}
```

**State Management Patterns:**

- **Two-way binding:** `x-model` on inputs automatically updates state
- **Reactive rendering:** Changes to `rounds[]` automatically update table via `x-for`
- **Computed visibility:** `x-show` directives react to `isGenerated` and `errorMessage`
- **Immutability not enforced:** Direct mutation is acceptable for this simple app

---

## Routing Architecture

**Status:** N/A - Single page application with no routing

This application has no navigation or routes. All functionality exists on a single screen.

**Route Organization:**
```
/ (index.html) - Entire application
```

No router library is needed or used.

---

## Frontend Services Layer

**Status:** N/A - No backend services

There is no API communication layer. All logic is self-contained in the Alpine.js component.

**No API Client:**
- No Axios, Fetch wrappers, or HTTP utilities
- No authentication token management
- No request/response interceptors

---
