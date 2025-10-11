# API Specification

**Status:** N/A - No API layer

This project is client-side only with no backend services or API endpoints. All logic executes in the browser:

- **No REST API** - No HTTP endpoints to document
- **No GraphQL** - No schema or resolvers
- **No tRPC** - No router definitions

**Internal "API":**

While there's no network API, the Alpine.js component exposes internal methods that function as the application's "interface":

```javascript
// Schedule Generation Methods
generateSchedule()     // Parse inputs, run algorithm, populate rounds[]
regenerateSchedule()   // Re-run algorithm with same inputs, different randomization

// Validation Methods
validateInputs()       // Check for empty/invalid inputs, return boolean
parseStudents()        // Convert textarea string to Student[] array

// Algorithm Core (internal)
assignRoles(round, team)  // Greedy fill logic for one team in one round
selectStudent(role)       // Find student with fewest opportunities for role
checkVariance(role)       // Validate variance ≤ 1 constraint
```

---
