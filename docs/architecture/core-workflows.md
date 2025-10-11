# Core Workflows

## Workflow: Generate Initial Schedule

This sequence shows the complete flow from user input to displaying the generated schedule.

```mermaid
sequenceDiagram
    participant User
    participant InputForm
    participant ActionButton
    participant Algorithm
    participant Display
    participant ErrorDisplay

    User->>InputForm: Enters student names
    User->>InputForm: Sets numRounds, numTeams
    User->>ActionButton: Clicks "Generate Schedule"

    ActionButton->>Algorithm: generateSchedule()

    Algorithm->>Algorithm: parseStudents()
    Note over Algorithm: Split textarea by newlines<br/>Create Student objects<br/>Initialize counters to 0

    Algorithm->>Algorithm: validateInputs()

    alt Validation Fails
        Algorithm->>ErrorDisplay: Set errorMessage
        ErrorDisplay->>User: Show error (e.g., "Enter at least 3 students")
    else Validation Passes
        Algorithm->>Algorithm: Clear previous rounds[]

        loop For each round
            loop For each team
                Algorithm->>Algorithm: assignTeam(round, team)
                Note over Algorithm: Select 2 drivers<br/>Select 1 loader<br/>Track opportunities

                Algorithm->>Algorithm: checkVariance()

                alt Variance > 1
                    Algorithm->>Algorithm: Backtrack or skip student
                end
            end
        end

        Algorithm->>Display: Update rounds[] (reactive)
        Display->>User: Show schedule table

        Algorithm->>ActionButton: Set isGenerated = true
        ActionButton->>User: Show "Regenerate" button
    end
```

---

## Workflow: Regenerate Schedule

This sequence shows how regeneration reuses inputs but produces different results via randomization.

```mermaid
sequenceDiagram
    participant User
    participant RegenerateButton
    participant Algorithm
    participant Display

    User->>RegenerateButton: Clicks "Regenerate"

    RegenerateButton->>Algorithm: regenerateSchedule()

    Note over Algorithm: Reuse existing studentNames,<br/>numRounds, numTeams

    Algorithm->>Algorithm: Reset all student counters to 0
    Algorithm->>Algorithm: Clear rounds[]

    loop For each round/team
        Algorithm->>Algorithm: assignTeam()
        Note over Algorithm: Math.random() produces<br/>different tiebreaking
    end

    Algorithm->>Display: Update rounds[] (reactive)
    Display->>User: Show new schedule

    Note over User: Same fairness guarantees,<br/>different student pairings
```

---

## Workflow: Error Handling - Invalid Input

```mermaid
sequenceDiagram
    participant User
    participant InputForm
    participant Algorithm
    participant ErrorDisplay

    User->>InputForm: Enters 2 students (too few)
    User->>Algorithm: Clicks Generate

    Algorithm->>Algorithm: validateInputs()

    alt Empty student names
        Algorithm->>ErrorDisplay: "Please enter at least 3 student names"
    else numRounds = 0
        Algorithm->>ErrorDisplay: "Number of rounds must be at least 1"
    else numTeams < 1 or > 2
        Algorithm->>ErrorDisplay: "Number of teams must be 1 or 2"
    else Too few students
        Algorithm->>ErrorDisplay: "Warning: Very few students - schedule may be repetitive"
        Note over Algorithm: Continue with generation
    end

    ErrorDisplay->>User: Show error message (red banner)

    User->>InputForm: Corrects input
    User->>Algorithm: Clicks Generate again

    Algorithm->>ErrorDisplay: Clear errorMessage
    Algorithm->>Algorithm: Proceed with generation
```

---

## Workflow: Edge Case - Impossible Perfect Balance

```mermaid
sequenceDiagram
    participant User
    participant Algorithm
    participant Console
    participant Display

    User->>Algorithm: Generate (4 students, 100 rounds, 2 teams)

    Note over Algorithm: Configuration requires<br/>600 slots (100 rounds × 2 teams × 3 roles)<br/>but only 4 students available

    Algorithm->>Algorithm: Begin greedy fill

    loop Round 1-50
        Algorithm->>Algorithm: Assign roles
        Note over Algorithm: Students accumulate<br/>many opportunities
    end

    Algorithm->>Algorithm: checkVariance()

    alt Variance constraint violated
        Algorithm->>Algorithm: Best-effort assignment
        Algorithm->>Console: console.warn("Perfect balance impossible...")
    end

    Algorithm->>Display: Show generated schedule
    Note over Display: Schedule is "as fair as possible"<br/>but variance may exceed 1

    Algorithm->>Console: Log final opportunity counts
```

---
