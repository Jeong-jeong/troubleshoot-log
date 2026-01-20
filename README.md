# Troubleshoot Log

> Document your troubleshooting sessions with Claude Code in **PHARL format** and save anywhere you want.

[한국어](./README.ko.md) | English

---

## What is PHARL?

**PHARL** is a structured format for documenting troubleshooting sessions:

| Step | Meaning | Description |
|------|---------|-------------|
| **P** | Problem | The issue encountered - symptoms, reproduction steps, environment |
| **H** | Hypothesis | Suspected causes with evidence and verification methods |
| **A** | Attempt | Solution attempts with code changes and results |
| **R** | Result | Outcome with before/after metrics comparison |
| **L** | Lesson | Technical insights, design principles, action items |

This format ensures you capture **not just what you did, but why you did it** - making your troubleshooting logs valuable for future reference.

---

## Features

- **Automatic Analysis**: Analyzes your troubleshooting conversation with Claude Code
- **Structured Documentation**: Generates comprehensive PHARL-formatted markdown
- **Auto Tag Generation**: Automatically generates relevant tags based on content
- **Multi-language Support**: Responds in your language (English, Korean, etc.)
- **Flexible Save Options**: Save to Notion, Obsidian, local files, or anywhere with MCP support

---

## Installation

```bash
/plugin install Jeong-jeong/troubleshoot-log
```

---

## Usage

### Command

```
/ts
```

or full name:

```
/troubleshoot-log:ts
```

### Trigger Phrases

The plugin recognizes these patterns:
- `troubleshooting`, `트러블슈팅`
- `PHARL format`, `PHARL 형식`
- `document this debugging session`

### Workflow

```
┌─────────────────┐
│  Troubleshoot   │
│  with Claude    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Run /ts        │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Claude analyzes│
│  conversation   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  PHARL document │
│  + auto tags    │
│  generated      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Choose where   │
│  to save        │
└────────┬────────┘
         │
    ┌────┼────┐
    ▼    ▼    ▼
┌──────┐┌──────┐┌──────┐
│Notion││Local ││ Any  │
│      ││ File ││ MCP  │
└──────┘└──────┘└──────┘
```

---

## Example Output

```markdown
# TypeError in API Response Handler

📅 **Date**: 2025-01-20
🏷️ **Tags**: `#NULL_CHECK`, `#API`, `#AXIOS`

---

## 🐛 Problem

### Symptoms
- TypeError: Cannot read property 'data' of undefined
- Occurs intermittently on API calls

### Environment
- Node.js 18.x, axios 1.6.0

## 🔍 Hypothesis

### Hypothesis 1: Race Condition
- **Evidence**: Error only occurs under high load
- **Suspected Cause**: Response arrives before state initialization

## 🔧 Attempt

### Attempt 1: Add Null Check

After analyzing the error stack trace, it was clear that `response.data` could be undefined when the API returns an unexpected response. Since the code was directly accessing nested properties without validation, we decided to apply optional chaining.

**Approach**: Guard against undefined response

```javascript
// Before
const user = response.data.user;

// After
const user = response?.data?.user ?? null;
```

**Result**:
```
$ npm test
✓ All 15 tests passed
```
The TypeError no longer occurs.

## 📊 Result

| Item | Before | After |
|------|--------|-------|
| Error message | `TypeError: Cannot read property 'data' of undefined` | No errors |
| Test results | `Tests: 13 passed, 2 failed` | `Tests: 15 passed, 0 failed` |

## 💡 Lesson

### Technical Insights
When accessing nested properties from external API responses, always use defensive coding practices. The optional chaining operator (`?.`) combined with nullish coalescing (`??`) provides a clean way to handle undefined values without verbose null checks. This is especially important for API responses that may have inconsistent structures.
```

---

## Save Options

After generating and reviewing your document, you'll be asked where to save it.

### Supported Destinations

| Destination | Requirement |
|-------------|-------------|
| **Notion** | Notion MCP configured |
| **Obsidian** | Obsidian MCP configured |
| **Local file** | None (always available) |
| **Others** | Respective MCP configured |

### Local File Save

Always available as a fallback. Default location:
```
./troubleshoot-logs/YYYY-MM-DD-[title-slug].md
```

### Notion Setup (Optional)

1. **Add Notion MCP**
   ```bash
   claude mcp add --transport http notion https://mcp.notion.com/mcp
   ```

2. **Authenticate**
   ```
   /mcp
   ```
   Select Notion and complete authentication in your browser.

3. **Restart Terminal**

   After first-time setup, restart your terminal for changes to take effect.

---

## Tags

Tags help you categorize and find troubleshooting logs later.

### How it works

1. **Auto-generation**: Claude analyzes the troubleshooting content and automatically generates relevant tags
2. **Format**: `#UPPER_SNAKE_CASE` (e.g., `#NULL_CHECK`, `#API`, `#REACT`)
3. **Modifiable**: Request changes via "Other" option before saving

---

## File Structure

```
troubleshoot-log/
├── .claude-plugin/
│   ├── plugin.json        # Plugin metadata
│   └── marketplace.json   # Marketplace configuration
├── commands/
│   └── ts.md              # /ts command definition
├── README.md              # English documentation
└── README.ko.md           # Korean documentation
```

---

## Troubleshooting

### Save to [platform] fails
1. Check if the required MCP is configured (`/mcp`)
2. Re-authenticate if needed
3. Restart terminal
4. Try saving locally as fallback

### Command not found
```bash
/plugin install Jeong-jeong/troubleshoot-log
```

---

## License

MIT

---

## Author

**Jeong-jeong**

- GitHub: [@Jeong-jeong](https://github.com/Jeong-jeong)
