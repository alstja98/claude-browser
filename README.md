# claude-browser

CLI tool for Claude Code frontend development. Launch a browser, visually inspect elements, detect React/Vue/Svelte components, and monitor console/network/performance — all from your terminal.

All heavy output (screenshots, logs, JSON data) is saved to files. Only file paths and summaries appear in chat, keeping context usage ~10x lower than MCP.

## What it does

- **Visual element selection** — Hover over any element to see its component name, props, source file, and styles
- **"→ Claude Code" button** — Click to auto-type `[Component #N]` into your Claude Code chat. Full component info is saved to a file that Claude reads automatically
- **Component detection** — React, Vue 3, Svelte components with props and source file mapping
- **Browser monitoring** — Console logs, network requests, Core Web Vitals (LCP, FCP, CLS, INP)
- **Screenshots** — Capture page state for visual debugging
- **Headless mode** — Claude can autonomously monitor your app in the background

## Installation

### Claude Code Plugin Marketplace

Register the marketplace, then install:

```bash
/plugin marketplace add tnsqjahong/claude-browser
/plugin install claude-browser
```

### Manual

Register the local repo as a marketplace, then install:

```bash
git clone https://github.com/tnsqjahong/claude-browser.git
cd claude-browser
npm install    # installs Playwright Chromium
npm run build
/plugin marketplace add /path/to/claude-browser
/plugin install claude-browser
```

## Usage

### Quick start

```
/claude-browser:inspect http://localhost:3000
```

This launches the browser and enables element selection mode.

### Element selection workflow

1. `/claude-browser:inspect http://localhost:3000`
2. Hover over elements to see component info in a tooltip
3. Click **"→ Claude Code"** button on the tooltip
4. `[Component #1: <Header>]` is auto-typed into your chat
5. Claude reads the full component details from `.claude-browser/selections/`

### Natural language (in Claude Code)

```
Launch the browser at http://localhost:3000 and take a screenshot
```

```
Check the console errors and failed network requests on localhost:3000
```

## CLI Commands

| Command | Description |
|---------|-------------|
| `claude-browser launch <url> [--headless]` | Launch Chromium and navigate |
| `claude-browser navigate <url>` | Navigate to a new URL |
| `claude-browser screenshot [--fullpage]` | Save screenshot as PNG |
| `claude-browser close` | Close browser and daemon |
| `claude-browser inspect <selector>` | Inspect element by CSS selector |
| `claude-browser select start` | Enable visual selection overlay |
| `claude-browser select wait [--timeout=N]` | Wait for user to click element |
| `claude-browser select stop` | Disable selection overlay |
| `claude-browser logs [--type=error\|warn\|all] [--limit=N]` | Console logs |
| `claude-browser network [--failed] [--limit=N]` | Network requests |
| `claude-browser perf` | Core Web Vitals: LCP, FCP, CLS, INP |
| `claude-browser components [--selector=<sel>]` | React/Vue component tree |
| `claude-browser find-component <name> [--root=<path>]` | Find component source file |
| `claude-browser status` | Check daemon/browser status |

## Output Files

All output is saved under `.claude-browser/`:

| Type | Path |
|------|------|
| Screenshots | `.claude-browser/screenshots/{timestamp}.png` |
| Console logs | `.claude-browser/logs/{timestamp}.json` |
| Network data | `.claude-browser/network/{timestamp}.json` |
| Performance | `.claude-browser/perf/{timestamp}.json` |
| Inspections | `.claude-browser/inspections/{timestamp}.json` |
| Components | `.claude-browser/components/{timestamp}.json` |
| Selections | `.claude-browser/selections/{timestamp}.json` |

## Architecture

```
claude-browser launch http://localhost:3000
    │
    ├─ CLI spawns a daemon process (detached, background)
    │   └─ Daemon runs Playwright + HTTP server on 127.0.0.1:{random port}
    │      Saves session info to .claude-browser/session.json
    │
    └─ CLI sends HTTP commands to daemon
       Daemon executes browser operations
       CLI saves results to .claude-browser/ and prints summary to stdout
```

## Example Workflows

### Visual debugging
```bash
claude-browser launch http://localhost:3000
claude-browser select start
claude-browser select wait
# User clicks element → saved to .claude-browser/selections/
claude-browser close
```

### Performance audit
```bash
claude-browser launch http://localhost:3000
claude-browser perf           # Core Web Vitals
claude-browser network        # Request timing
claude-browser screenshot     # Visual state
claude-browser close
```

### Autonomous monitoring
```bash
claude-browser launch http://localhost:3000 --headless
# ... make code changes ...
claude-browser navigate http://localhost:3000    # reload
claude-browser logs --type=error                 # check errors
claude-browser network --failed                  # check API failures
claude-browser close
```

## Supported Frameworks

| Framework | Component Name | Props | Source File |
|-----------|:-:|:-:|:-:|
| React | ✓ | ✓ | ✓ (dev mode) |
| Vue 3 | ✓ | ✓ | ✓ |
| Svelte | ✓ | — | — |
| Plain HTML | tag + classes | — | — |

Element selection (tag, classes, styles, selector) works on **any** website regardless of framework.

## License

MIT
