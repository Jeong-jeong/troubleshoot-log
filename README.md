# Troubleshoot Log

Document your troubleshooting sessions with Claude Code in PHARL format and optionally upload to Notion.

---

## PHARL Format

| Step | Meaning | Description |
|------|---------|-------------|
| **P** | Problem | The issue encountered |
| **H** | Hypothesis | Suspected causes |
| **A** | Attempt | Solution attempts |
| **R** | Result | Outcome with metrics |
| **L** | Lesson | Lessons learned |

---

## Installation

```bash
claude plugin add troubleshoot-log
```

---

## Usage

### Command

```
/ts
```

or

```
/troubleshoot-log:ts
```

### What it does

1. **Analyze conversation** - Reviews the troubleshooting discussion
2. **Generate PHARL document** - Creates structured documentation
3. **User review** - Allows edits and confirmation
4. **Upload to Notion (optional)** - Saves to your Notion workspace

### Writing Guidelines

The plugin generates detailed documentation so you can understand the **context and reasoning** later.

Each section includes:
- **Problem**: Symptoms, reproduction steps, environment
- **Hypothesis**: Evidence, suspected cause, verification method
- **Attempt**: Approach, code changes, results, limitations
- **Result**: Before/After comparison (metrics recommended)
- **Lesson**: Technical insights, design principles, action items

---

## Notion Integration (Optional)

### Step 1: Add Notion MCP

```bash
claude mcp add --transport http notion https://mcp.notion.com/mcp
```

### Step 2: Authenticate

```
/mcp
```

Select Notion and complete authentication in your browser.

### Step 3: Restart Terminal

**Important**: After first-time setup, restart your terminal for changes to take effect.

---

## File Structure

```
troubleshoot-log/
├── .claude-plugin/
│   └── plugin.json       # Plugin metadata
├── commands/
│   └── ts.md             # /ts command definition
└── README.md
```

---

## Troubleshooting

### Notion upload fails
1. Run `/mcp` and check Notion connection status
2. Re-authenticate if needed
3. Restart terminal

### Command not found
```bash
claude plugin add troubleshoot-log
```

---

## License

MIT
