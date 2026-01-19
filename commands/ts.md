---
description: Document troubleshooting sessions in PHARL format and optionally upload to Notion
---

# Troubleshoot Log Command

Document troubleshooting discussions with Claude Code in PHARL format.

**IMPORTANT: Always respond in the same language the user is using.**

## Workflow

### Step 1: Analyze Conversation

Review the conversation to identify:
- What problem occurred
- What hypotheses were formed
- What solutions were attempted
- What the results were

### Step 2: Generate PHARL Document

Create a markdown document in this format:

```markdown
# [Title: One-line problem summary]

📅 **Date**: YYYY-MM-DD

---

## 🐛 Problem

### Symptoms
- Specific problem behavior experienced
- Error messages (if any)

### Reproduction Steps
- When does it occur vs when does it work normally
- Specific conditions that trigger the issue

### Environment
- Relevant tech stack, library versions
- Context of the problematic code

## 🔍 Hypothesis

### Hypothesis 1: [Title]
- **Evidence**: Why this hypothesis was formed (code analysis, logs, etc.)
- **Suspected Cause**: What might be happening internally
- **Verification Method**: How to test this hypothesis

### Hypothesis 2: [Title]
- (Same format)

## 🔧 Attempt

### Attempt N: [Title] (Proposed by: Agent/User)

**Approach**: Why this method was tried

```code
// Only the key changes (not entire files)
// Comments explaining what each part does
```

**Result**: Success/failure and specific behavior
- What worked
- What didn't work

**Limitations**: (If failed) Why this approach had limitations

---

### (Separate attempts with horizontal rules)

## 📊 Result

| Item | Before | After |
|------|--------|-------|
| Issue | State | State |
| Side effects | State | State |

- **Use metrics** (build time, error count, performance, etc.)
- If not quantifiable, use Before/After comparison

## 💡 Lesson

### Technical Insights
- Technical knowledge gained from this issue
- Characteristics of the library/framework

### Design Principles
- Patterns to apply in similar situations
- How to prevent this issue

### Action Items
- Specific next steps
```

### Step 3: User Review

Show the generated document to the user and confirm:

1. **Title**: Is the title appropriate?
2. **Content**: Any additions or corrections needed?
3. **Notion Upload**: Should this be uploaded to Notion?

### Step 4: Notion Upload (Optional)

If the user wants to upload to Notion, use a **2-step question process**:

#### 4-1. Get Notion Link First
Ask the user for the relevant Notion page/database URL.

#### 4-2. Confirm Specific Upload Method
After receiving the link, **use the fetch tool to understand the page structure first**, then ask how they want to upload:

**Example Questions:**
- "How would you like to add this to the page?"
- "Is there a button that opens a specific page?"
- "Should I add it as a new item to a database?"
- "Should I add content directly to the page body?"

**Possible Upload Methods:**
1. **Add as new database item**: If the page has a database, add as a new row
2. **Create as sub-page**: Create a new page under the target page
3. **Append to page body**: Add content at the end of existing page
4. **Custom method**: Add to a specific location as described by user

**Important**: Notion structures vary widely. Don't use simple "new page/existing page" binary questions.
Use fetch to understand the structure first, then present specific options available for that page.

**Extracting Page ID:**
```
https://www.notion.so/Page-Name-2de27f29d15b8170892af488ed82bf62
                              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                              This 32-character string is the page ID
```

**Notion MCP Workflow:**
1. `notion-fetch`: Understand page structure (databases, sub-pages, etc.)
2. `notion-create-pages`: Create new pages/database items
3. `notion-update-page`: Add content to existing pages

## Writing Guidelines

**Core Principle**: The document should allow readers to understand **"why this approach was taken and why it became the solution"** when read later.

### Section-by-Section Guide

#### 🐛 Problem
- **Symptoms**: Specific problem behavior experienced
- **Reproduction Steps**: When it occurs, differences from normal behavior
- **Environment**: Tech stack, context of problematic code

#### 🔍 Hypothesis
For each hypothesis:
- **Evidence**: Why this hypothesis was formed (code analysis, logs, etc.)
- **Suspected Cause**: Internal reasoning about what's happening
- **Verification Method**: How to test this hypothesis

#### 🔧 Attempt
For each attempt:
- **Approach**: Background on why this method was tried
- **Code**: Only key changes (not entire files), with comments explaining each part
- **Result**: Success/failure and specific behavior
- **Limitations**: (If failed) Why this approach had limitations
- Separate attempts with `---` horizontal rules

#### 📊 Result
- Before/After table format recommended
- **Use metrics** (build time, error count, performance, etc.)
- If not quantifiable, use qualitative comparison

#### 💡 Lesson
- **Technical Insights**: Technical knowledge gained
- **Design Principles**: Patterns to apply in similar situations
- **Action Items**: Specific next steps

## Notes

- If the conversation is not related to troubleshooting, inform the user
- If results cannot be quantified, still provide qualitative results
- If Notion upload fails, suggest saving as a local markdown file
