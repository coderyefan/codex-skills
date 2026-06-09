---
name: req-review-risk
description: >
  Requirements review risk screening. Analyzes requirement documents for ambiguity,
  logic conflicts, missing constraints, unclear boundaries, missing compatibility
  requirements and other risks. Outputs a structured Markdown review checklist with
  severity labels (P0/P1/P2) and category tags, AND auto-generates a standalone HTML
  report. Use when user pastes requirement text for review, asks for 需求评审,
  requirement quality check, risk screening, or mentions catching requirement issues
  before development.
---

# 需求评审风险筛查

Analyze requirement text, output a labeled Markdown checklist AND an HTML report.

## Risk Categories

Label every finding with exactly one category tag:

| Tag | Category |
|-----|----------|
| `[模糊]` | Ambiguity - vague wording, multiple interpretations |
| `[冲突]` | Logic Conflict - contradictory requirements |
| `[缺失]` | Missing Constraint - no perf target, no data range, no error handling |
| `[边界]` | Unclear Boundary - scope unclear, missing interface definition |
| `[兼容]` | Missing Compatibility - browser/OS/version not specified |
| `[安全]` | Security Gap - auth, encryption, input validation missing |
| `[可测]` | Testability - not verifiable, no acceptance criteria |

## Severity

- **P0 阻断**: Must resolve before dev. Causes rework or system failure.
- **P1 重要**: Should resolve. Significant issues or delays likely.
- **P2 建议**: Nice to resolve. Minor clarity improvement.

## Process

1. Read entire requirement text.
2. Scan each statement against all 7 risk categories.
3. For each finding, extract **exact original text**. One finding per row.
4. Assign category tag, severity, issue description, and specific suggestion.
5. Output a Markdown checklist in the chat.
6. **MUST** also generate a standalone HTML report file (see below).

## HTML Report Generation

Always generate an HTML file alongside the Markdown output.

### Filename Convention

Name the file using the following priority order:

```
需求评审-{产品名}-V{版本号}-{YYYYMMDD}.html
```

**Extraction rules:**
- Scan the requirement text for explicit patterns: `XX系统 V2.0`, `XX模块`, `XX平台 V1`, `XX项目 3.0`
- If found, use: `需求评审-{产品名}-V{版本号}-{YYYYMMDD}.html` (e.g. `需求评审-订单系统-V2.1-20260609.html`)
- If only product name found, omit version: `需求评审-{产品名}-{YYYYMMDD}.html`
- If neither found, use the first meaningful noun from the text as product name
- Fallback: `需求评审-{YYYYMMDD}-{HHmm}.html`
- Always save to current working directory.

### HTML Template Rules

The HTML must be self-contained (inline CSS, no external deps) and include:
1. **Header**: title, review date, document/product name
2. **需求原文**: original text in a styled blockquote
3. **概览**: summary grid with P0/P1/P2/total counts
4. **风险清单**: tables grouped by severity, columns: # | 类别 (tag) | 原文引用 | 问题描述 | 修改建议
5. **评审结论**: one-sentence assessment + prioritized action items
6. **Action bar**: print/save PDF button, select-all copy button
7. Print-friendly CSS (@media print)

Style guidelines: clean modern UI, blue-purple gradient header, color-coded tags per category, severity badges (P0=red, P1=orange, P2=blue).

## Output Format (Markdown)

```markdown
# 需求评审风险筛查报告

## 概览
| 指标 | 数量 |
|------|------|
| P0 阻断 | X |
| P1 重要 | X |
| P2 建议 | X |
| 合计 | X |

## 风险清单

### [P0] 阻断风险
| # | 类别 | 原文引用 | 问题描述 | 修改建议 |
|---|------|----------|----------|----------|

### [P1] 重要风险
...

### [P2] 建议优化
...
```

## Rules

- Cite original text verbatim in `原文引用`, do not paraphrase.
- One finding per row. Multiple issues in one sentence = multiple rows.
- Be specific in suggestions: state exactly what to add, not just "be clearer".
- Skip empty severity sections.
- Output Markdown first, then state the HTML file path.
- Do not add closing remarks beyond the file path.
