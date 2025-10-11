# Development Workflow

## Local Development Setup

### Prerequisites

```bash
# No prerequisites required!
# Just a modern web browser:
# - Chrome 90+
# - Firefox 88+
# - Safari 14+
# - Edge 90+
```

### Initial Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/vex-team-builder.git
cd vex-team-builder

# That's it! No npm install, no build step.
```

### Development Commands

```bash
# Open the application
# Option 1: Open directly in browser
open index.html  # macOS
xdg-open index.html  # Linux
start index.html  # Windows

# Option 2: Use a simple local server (optional, for testing)
# Python 3
python -m http.server 8000

# Node.js (if you have it installed)
npx serve .

# Then visit: http://localhost:8000
```

**Development Workflow:**

1. Edit `index.html` in your favorite code editor
2. Save the file
3. Refresh browser (Ctrl+R / Cmd+R)
4. Test changes immediately

**No hot reload, no build step, no complexity.**

---

## Environment Configuration

**Required Environment Variables:** None

The application has no environment-specific configuration. It runs identically in all environments (local, staging, production).

**Optional Configuration (Phase 2):**

If analytics or external integrations are added in Phase 2, configuration could be embedded in the HTML:

```html
<script>
  const CONFIG = {
    ENABLE_ANALYTICS: false,  // Set to true in production
    ANALYTICS_ID: 'UA-XXXXX-Y'
  };
</script>
```

---
