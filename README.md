# Tableau Data Quality Sentinel

**A Claude skill that profiles published Tableau datasources for schema hygiene, naming violations, type mismatches, and metadata gaps. Built for [Claude Desktop](https://claude.ai/download) and the [Tableau MCP](https://github.com/tableau/tableau-mcp). v.2.0**

## How It Works

Say "DQ scan" or "check my datasources" and the skill pulls metadata for every published datasource you can access, runs checks across 6 domains, and returns a letter-graded health report with prioritized fixes.

```
Data Quality Scan -- All Published Datasources

B- -- 81.0/100
"Metadata gaps are the recurring theme. Types are mostly clean."

Schema: A  |  Naming: C  |  Types: B  |  Calcs: B  |  Metadata: D  |  Freshness: B

Quick Wins (raise to ~87):
1. Add descriptions to 5 datasources -- 10 min
2. Add tags to 6 datasources -- 5 min
3. Fix 5 Count fields stored as STRING -- 15 min

Long-Term Goals:
1. Alias 33 Salesforce __c fields (user-facing readability)
2. Certify the 2 datasources with downstream workbooks
3. Break apart 391-field datasource into logical tables
```

## Check Domains

| Domain | What It Catches |
|--------|-----------------|
| Schema Hygiene | Oversized field counts, missing dimensions/measures, orphan tables, excessive calc ratios |
| Field Naming | Salesforce __c suffixes, SCREAMING_CASE, CamelCase, cryptic abbreviations, generic names |
| Type & Role | Dates stored as STRING, numeric fields as STRING, IDs as MEASURE, booleans as STRING |
| Calc Complexity | Deep nesting, hardcoded values, cross-table references, nested LODs, very long formulas |
| Metadata | Missing descriptions, no tags, no field docs, uncertified datasources |
| Freshness | Stale datasources (90+ days), no extracts under load, orphaned content, default project |

## Output Formats

| Format | Use Case |
|--------|----------|
| Governance-style | Letter grade, domain scores, quick wins, findings. The default. |
| Scorecard | Per-datasource pass/fail table. No narrative. |
| Alert-style | Problems only. No scores, no positives. |
| Control Tower | Interactive HTML artifact with filters |
| Spreadsheet | Shareable .xlsx with one row per finding |
| Word | Formal .docx report for leadership |

## Quick Scan Modes

Not every scan needs to be a full pass. Three modes for when you want speed over depth.

| Mode | What It Does |
|------|-------------|
| Changed-Only | Re-scans only datasources modified since last scan. Default when memory exists. |
| Top Issues Only | Checks HIGH/CRITICAL rules only. Skips the small stuff. |
| Prior Failures Only | Re-scans datasources that scored below B last time. |

Modes stack. "Quick scan, just critical on the failures" runs Mode 3 + Mode 2.

## Chunked Scanning

Sites with more than 20 datasources get scanned in chunks of 20. The skill offers to run all chunks sequentially or pause between them. Memory tracks progress, so if a session ends mid-scan, the next session picks up where you left off.

```
Chunk 2 of 4 -- 20 datasources profiled

Running total: 40 of 78
Site average so far: B- (80.2)
Chunks remaining: 2
```

## Scheduled Monitoring

After any full scan, the skill offers to schedule recurring checks. You pick the cadence and alert threshold. Scheduled runs use alert-style output and only notify you when scores drop or new issues appear.

## Key Features

- **Zero-config execution.** Infers scope, format, and scan mode from natural language.
- **Memory caching.** Remembers metadata schemas, prior scores, and 403 blocklists. Skips unchanged datasources entirely.
- **Delta comparison.** Shows what improved, what degraded, and what's new since last scan.
- **Side-by-side mode.** Compare two datasources against each other domain-by-domain.
- **Chunked scanning.** Handles sites with 100+ datasources across resumable chunks.
- **Scheduled scans.** Set it and forget it. Get alerted only when something breaks.
- **Graceful degradation.** Skips 403s and timeouts without failing the scan. Caches them to avoid retrying.
- **Systemic aggregation.** 33 __c fields become one finding, not 33.
- **Validation phase.** Arithmetic, deduplication, severity cross-checks before output.

## Requirements

- Claude Desktop with the Tableau MCP connected
- Tableau Cloud or Server REST API access
- Minimum permissions: `list-datasources` + `get-datasource-metadata`

## Installation

1. Save `SKILL.md` to your Claude skills directory
2. Trigger with: "DQ scan", "data quality check", "profile this datasource", "schema issues", or "field hygiene"

## Companion Skills

| Skill | Handoff |
|-------|---------|
| Governance Scanner | Site-wide content hygiene (hands off flagged datasources to Sentinel) |
| Tableau Scribe | Auto-generate docs for undocumented datasources |
| Tableau Auto Query | Query actual values for flagged fields |
| Tableau Calc Master | Optimize complex calcs surfaced by the scan |

## Limitations

- Metadata-only. Cannot assess null rates, row counts, value distributions, or duplicates.
- No PII detection (would require querying actual values).
- `updatedAt` is a proxy for freshness. Actual extract refresh schedules are not exposed.
- Downstream workbook count may not be available in all API responses.
- 20-datasource cap per chunk (by design, to stay within context limits).
