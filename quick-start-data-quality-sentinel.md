## Quick Start

### Prerequisites

- [Claude Desktop](https://claude.ai/download) with agent mode enabled
- [Tableau MCP](https://github.com/tableau/tableau-mcp) installed and authenticated
- Access to a Tableau Cloud or Tableau Server site

### Install

1. Clone or download this repository.
2. Copy the skill folder into your Claude skills directory.
3. Restart Claude Desktop.
4. Confirm Tableau MCP is connected (you should see your site's content when you ask Claude about it).

### Example Prompts

```
Check my datasources for quality issues
```

```
Profile the "Segment Consumption" datasource — any naming violations or missing descriptions?
```

```
Run a DQ scan on all datasources in the Analytics project
```

```
Quick scan — just show me what changed since last time
```

### What You Get

A datasource quality report covering: field naming hygiene, missing descriptions, type mismatches, calculated field complexity, metadata gaps, and schema structure. Each finding includes severity, the specific field(s) affected, and what to fix. Output as a governance-style report (letter grade + domain scores), a pass/fail scorecard, or an alert-style list of just the problems. Available as markdown, .docx, .xlsx, or an interactive HTML control tower.
