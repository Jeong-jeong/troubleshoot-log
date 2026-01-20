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
🏷️ **Tags**: `#TAG1`, `#TAG2`, `#TAG3`

---

## 🐛 Problem

### Symptoms
- Specific problem behavior experienced
- **Include actual error messages verbatim** (copy-paste, not summarize)

```
// Example: Include the ACTUAL error output
TypeError: Cannot read property 'data' of undefined
    at fetchUser (/src/api/user.js:42:15)
    at async handleRequest (/src/handlers/request.js:18:20)
```

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

### Attempt 1: [Title]

Write naturally about how this attempt came about. For example:
- "While analyzing the error stack trace, the agent noticed that..."
- "The user suspected that X might be the cause because..."
- "Based on the hypothesis above, we decided to try..."

**Do NOT use "Proposed by: Agent/User" format.** Instead, weave the context naturally into the narrative.

**Approach**

Explain the reasoning in detail:
- What was the core idea behind this attempt?
- Why did this seem like a viable solution given the evidence?
- What specific change or action was taken?

```code
// Include ACTUAL code changes, not pseudocode
// Before:
const data = response.data.user;

// After:
const data = response?.data?.user ?? null;
```

**Result**

Include **actual outputs** - copy-paste real terminal output, error messages, or logs:

```
// GOOD: Actual output
$ npm run build
✓ Compiled successfully in 1.2s

// BAD: Summarized
"Build succeeded"
```

- What specific behavior changed after the attempt?
- Did the original error/issue disappear?
- Were there any new errors or side effects?

**Limitations** (If failed)
- Why didn't this approach work?
- What new information did we learn from this failure?

---

### (Separate attempts with horizontal rules)

## 📊 Result

Use **concrete, measurable values** whenever possible. Avoid vague descriptions.

| Item | Before | After |
|------|--------|-------|
| Error message | `TypeError: Cannot read property 'data' of undefined` | No errors |
| Build output | `✗ Build failed at webpack step` | `✓ Compiled successfully` |
| API response | `500 Internal Server Error` | `200 OK` |
| Test results | `Tests: 8 passed, 2 failed` | `Tests: 10 passed, 0 failed` |

**Bad examples** (avoid these):
- ❌ "Confusion → Clear understanding"
- ❌ "Broken → Fixed"
- ❌ "Error occurred → Error resolved"

**Good examples**:
- ✅ Actual error message → Actual success message
- ✅ Specific metric before → Specific metric after

## 💡 Lesson

**IMPORTANT**: This section should NOT be a brief summary. Write in enough detail that someone reading this months later can understand:
1. What you learned
2. Why it matters
3. How to apply it

### Technical Insights

Don't just state facts - explain the "why" and "how":

**Bad example**:
- "marketplace.json is needed for plugin registration"

**Good example**:
- "Claude Code의 플러그인 마켓플레이스는 분산 구조로 설계되어 있다. 각 개발자가 자신의 GitHub 저장소에 `marketplace.json`을 생성하면 해당 저장소가 하나의 마켓플레이스가 된다. 다른 사용자가 이 플러그인을 검색하려면 먼저 `/plugin marketplace add [owner/repo]` 명령어로 해당 마켓플레이스를 자신의 환경에 등록해야 한다. 이는 중앙 집중식 패키지 매니저(npm, pip 등)와 다른 접근 방식이며, 커뮤니티 공유 마켓플레이스(예: team-attention/plugins-for-claude-natives)에 PR을 보내면 더 넓은 사용자층에게 노출될 수 있다."

### Design Principles

Explain patterns that can be applied to similar situations:

**Bad example**:
- "Use AskUserQuestion for better UX"

**Good example**:
- "사용자에게 여러 옵션 중 선택을 요청할 때, 텍스트로 '어떤 것을 선택하시겠습니까?'라고 물어보면 사용자가 직접 타이핑해야 하므로 토큰이 소모되고 오타 가능성도 있다. AskUserQuestion 도구를 사용하면 선택지를 버튼으로 제공할 수 있어서 (1) 사용자 경험 개선 (2) 토큰 절약 (3) 입력 오류 방지 효과가 있다. 단, 선택지가 2개 이상이어야 하며, 사용자가 자유롭게 입력하고 싶은 경우 'Other' 옵션을 통해 가능하다."

### Action Items

Specific, actionable next steps:
- [ ] Concrete task 1
- [ ] Concrete task 2
```

### Step 3: Auto-generate Tags

Automatically add relevant tags to the document (no user confirmation needed):
- **Problem type**: `#NULL_CHECK`, `#RACE_CONDITION`, `#ASYNC`, `#TYPE_ERROR`, `#CONFIG`, `#DEPENDENCY`
- **Domain**: `#API`, `#DATABASE`, `#UI`, `#AUTH`, `#BUILD`, `#TEST`
- **Technology**: Framework/library names like `#REACT`, `#NODE`, `#TYPESCRIPT`

**Tag format**: `#UPPER_SNAKE_CASE`

### Step 4: Ask Where to Save (Use AskUserQuestion)

After showing the generated document, **immediately use AskUserQuestion tool**:

```
Question: "어디에 저장할까요? (수정사항 있으면 'Other'로 입력해주세요)"
Header: "저장 위치"
Options:
  - "Notion" : "Notion 페이지/데이터베이스에 업로드"
  - "로컬 파일" : "마크다운 파일로 저장"
  - "저장 안 함" : "지금은 저장하지 않음"
multiSelect: false
```

User can select "Other" to request modifications or specify a different destination.

### Step 5: Handle Response & Save

#### If User Selected "Notion"
1. Ask for Notion page/database URL (via AskUserQuestion with "이전에 사용한 페이지" option + Other for new URL)
2. Use `notion-fetch` to understand structure
3. Save using appropriate method (`notion-create-pages` or `notion-update-page`)

#### If User Selected "로컬 파일"
1. Use AskUserQuestion to ask for save path
   - Provide default path option: `./troubleshoot-logs/YYYY-MM-DD-[title-slug].md`
   - Allow custom path via "Other"
2. Create file using Write tool at user-specified path
3. Confirm saved path

#### If User Selected "Other" (custom input)
Handle based on what they typed:
- **Modification request**: Apply changes, then ask where to save again
- **Other platform** (Obsidian, GitHub, etc.): Check MCP availability, proceed or offer local fallback

#### If User Selected "저장 안 함"
End the workflow

### Notion-Specific Instructions

When saving to Notion:

1. Ask for Notion page/database URL
2. Use `notion-fetch` to understand the page structure
3. Determine appropriate upload method:
   - **Database**: Add as new row
   - **Page**: Create sub-page or append to body

**Extracting Page ID:**
```
https://www.notion.so/Page-Name-2de27f29d15b8170892af488ed82bf62
                              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                              This 32-character string is the page ID
```

**Notion MCP Workflow:**
1. `notion-fetch`: Understand page structure
2. `notion-create-pages`: Create new pages/database items
3. `notion-update-page`: Add content to existing pages

## Writing Guidelines

**Core Principle**: The document should allow readers to understand **"why this approach was taken and why it became the solution"** when read later.

### DO's and DON'Ts

#### DO:
- Include actual error messages, terminal outputs, and code snippets (copy-paste)
- Explain the reasoning behind each decision
- Write Lesson section in detail with context and examples
- Describe how ideas evolved during the troubleshooting process

#### DON'T:
- Summarize error messages (e.g., "an error occurred")
- Use vague descriptions (e.g., "fixed the issue")
- Write one-liner lessons without explanation
- Use "Proposed by: Agent/User" format

### Section-by-Section Guide

#### 🐛 Problem
- **Symptoms**: Include ACTUAL error messages verbatim
- **Reproduction Steps**: Specific conditions
- **Environment**: Tech stack, versions

#### 🔍 Hypothesis
For each hypothesis:
- **Evidence**: Specific clues from code/logs
- **Suspected Cause**: Detailed reasoning
- **Verification Method**: How to test

#### 🔧 Attempt
Write naturally about how each attempt came about:
- Who noticed what, and why they thought it might help
- Actual code changes (not pseudocode)
- Actual outputs (not summaries)
- If failed, what was learned

#### 📊 Result
- Before/After with **actual values**
- Error messages, metrics, observable behaviors
- No vague adjectives

#### 💡 Lesson
**This is the most important section for future reference.**
- **Technical Insights**: Explain concepts in detail with context
- **Design Principles**: Patterns that can be reused, with reasoning
- **Action Items**: Specific next steps

## Notes

- If the conversation is not related to troubleshooting, inform the user
- Always offer local save as a fallback option
- Respect user's choice of save destination
