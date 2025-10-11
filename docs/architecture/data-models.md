# Data Models

These are the core data structures that will be maintained in browser memory during schedule generation and display.

## Student

**Purpose:** Represents a single student/participant with opportunity tracking for fair role distribution

**Key Attributes:**
- `name`: string - Student's name as entered by coach
- `driverCount`: number - Total driver role assignments received (initialized to 0)
- `loaderCount`: number - Total loader role assignments received (initialized to 0)
- `id`: number - Unique identifier (array index) for assignment tracking

**TypeScript Interface:**
```typescript
interface Student {
  name: string;
  driverCount: number;
  loaderCount: number;
  id: number;
}
```

**Relationships:**
- Referenced by Round/Team assignments
- Aggregated into `students` array in `ScheduleData`

---

## Team

**Purpose:** Represents a single team's role assignments in a specific round

**Key Attributes:**
- `teamNumber`: number - Team identifier (1 or 2)
- `driver1`: string - First driver's name
- `driver2`: string - Second driver's name
- `loader`: string - Loader's name

**TypeScript Interface:**
```typescript
interface Team {
  teamNumber: number;
  driver1: string;
  driver2: string;
  loader: string;
}
```

**Relationships:**
- Contained within `Round`
- Represents 3 students (2 drivers + 1 loader)

---

## Round

**Purpose:** Represents all team assignments for a single tournament round

**Key Attributes:**
- `roundNumber`: number - Round identifier (1-indexed)
- `teams`: Team[] - Array of team compositions for this round

**TypeScript Interface:**
```typescript
interface Round {
  roundNumber: number;
  teams: Team[];
}
```

**Relationships:**
- Contains array of `Team` objects
- Aggregated into `rounds` array in `ScheduleData`

---

## ScheduleData

**Purpose:** Root data structure containing all schedule state (Alpine.js `x-data` object)

**Key Attributes:**
- `studentNames`: string - Raw textarea input (one name per line)
- `numRounds`: number - Number of tournament rounds
- `numTeams`: number - Number of teams (1 or 2)
- `students`: Student[] - Parsed student list with opportunity counters
- `rounds`: Round[] - Generated schedule organized by round
- `errorMessage`: string - Validation or generation error display
- `isGenerated`: boolean - Whether schedule has been generated (controls button visibility)

**TypeScript Interface:**
```typescript
interface ScheduleData {
  // Input fields
  studentNames: string;
  numRounds: number;
  numTeams: number;

  // Computed/generated data
  students: Student[];
  rounds: Round[];

  // UI state
  errorMessage: string;
  isGenerated: boolean;

  // Methods
  generateSchedule(): void;
  regenerateSchedule(): void;
  parseStudents(): Student[];
  validateInputs(): boolean;
}
```

**Relationships:**
- Root object for entire application state
- Contains all `Student` and `Round` data

---
