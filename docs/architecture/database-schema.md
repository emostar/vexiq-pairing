# Database Schema

**Status:** N/A - No database

This application has no persistent data storage. All data structures exist only in browser memory (JavaScript variables) and are cleared when the page is refreshed or closed.

**Rationale:**
- Privacy by design: No student names stored anywhere
- Matches use case: Coaches regenerate schedules as needed each tournament
- Eliminates infrastructure: No database server, no connection strings, no migrations

**Phase 2 Consideration:**
If coaches request "save my roster" functionality, localStorage could be used for client-side persistence:

```javascript
// Hypothetical Phase 2 localStorage schema
{
  "savedRosters": [
    {
      "id": "uuid",
      "name": "Fall 2025 Team",
      "students": ["Alice", "Bob", "Carol", ...],
      "lastUsed": "2025-10-10T14:30:00Z"
    }
  ]
}
```

---
