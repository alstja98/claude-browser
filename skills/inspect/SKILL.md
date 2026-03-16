---
name: inspect
description: Open a browser and start visual element inspection mode for frontend development
argument-hint: "<url>"
allowed-tools:
  - Bash(claude-browser *)
  - Read(.claude-browser/*)
---

# Inspect

Opens a Chromium browser, navigates to the URL, and enables visual element selection mode.

## Instructions

1. Run `claude-browser launch $ARGUMENTS` to start the browser
2. Tell the user the browser is open and they can hover over elements and click "→ Claude Code" to select them
3. When the user selects an element (indicated by `[Component #N]` in chat), read the selection file from `.claude-browser/selections/N.txt` for full component details

## Available Commands

```bash
claude-browser launch <url> [--headless]     # Launch browser
claude-browser navigate <url>                # Navigate
claude-browser screenshot [--fullpage]       # Save screenshot
claude-browser close                         # Close browser
claude-browser inspect <selector>            # Inspect by CSS selector
claude-browser select start                  # Start visual selection
claude-browser select wait [--timeout=N]     # Wait for user click
claude-browser select stop                   # Stop selection
claude-browser logs [--type=error|warn|all]  # Console logs
claude-browser network [--failed]            # Network requests
claude-browser perf                          # Core Web Vitals
claude-browser components [--selector=<s>]   # Component tree
claude-browser find-component <name>         # Find source file
claude-browser status                        # Check status
```

## Output Locations

All data is saved to `.claude-browser/` as files. Use Read tool for full data.

| Command | Output |
|---------|--------|
| screenshot | `.claude-browser/screenshots/{timestamp}.png` |
| logs | `.claude-browser/logs/{timestamp}.json` |
| network | `.claude-browser/network/{timestamp}.json` |
| perf | `.claude-browser/perf/{timestamp}.json` |
| inspect | `.claude-browser/inspections/{timestamp}.json` |
| components | `.claude-browser/components/{timestamp}.json` |
| select wait | `.claude-browser/selections/{timestamp}.json` |
