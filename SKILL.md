---
name: daily-research-digest
description: >
  Mine M365 emails, meetings, and Teams chats to generate a daily research digest.
  Extracts top themes from communications, creates 3 focused research questions,
  and invokes the @Researcher agent in parallel to answer each one.
  Keywords: daily research, research digest, what should I research today,
  research from my emails, signal-driven research, morning research brief,
  mine my M365, generate research questions from my calendar, trending in my comms.
argument-hint: "[lookback period] [focus area]"
user-invocable: true
disable-model-invocation: false
compatibility: >
  Requires WorkIQ MCP server (@microsoft/workiq), Node.js 18+,
  M365 Entra ID authentication, and @Researcher agent enabled in tenant.
  Network access needed for M365 Graph API calls.
license: MIT
allowed-tools: ask_work_iq accept_eula bash
metadata:
  author: donlee
  version: "1.2.0"
  created: "2026-04-06"
  persona: "SAS / Principal Engineer"
  default-lookback: "24 hours"
  max-questions: "3"
---

# Daily Research Digest

Mine your M365 signals → extract themes → invoke @Researcher × 3 → compile digest.

> **First-time setup?** See [references/SETUP.md](references/SETUP.md) for WorkIQ MCP installation, EULA acceptance, and M365 auth.

## When to Use
- Start of day: `/daily-research-digest` or "What should I research today?"
- After a busy period: `/daily-research-digest 3 days`
- Before strategy meetings: `/daily-research-digest 1 week Foundry Agents`
- Exploring new topics surfaced in team conversations

## Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| lookback | `24 hours` | How far back to scan signals (e.g., "3 days", "1 week") |
| custom_focus | _(none)_ | Bias question generation toward a specific topic |
| output_dir | `./prompts/daily-research-results` | Where to save the digest |

## Workflow

### Phase 1: Gather M365 Signals

Collect context from three channels via `ask_work_iq`. Run all three in parallel. Use the query templates in [assets/signal-queries.md](assets/signal-queries.md).

- **Emails** — themes, decisions, open questions, action items (skip noise)
- **Meetings** — agendas, discussion topics, follow-ups, unresolved questions
- **Teams Chats** — technical discussions, tags, recurring themes (skip casual)

If any signal call fails, continue with available data and note failures.

### Phase 2: Analyze & Generate 3 Research Questions

Analyze signals through a **Senior Architecture Specialist / Principal Engineer** lens. Look for:

- Topics appearing across multiple channels
- Open questions or unresolved technical debates
- Upcoming decisions needing research backing
- Areas where the user was asked for expertise
- New technologies, announcements, or competitive moves

Generate 3 **specific, actionable** research questions. Questions should target recent sources (2025–2026), comparisons, or frameworks.

**Agent routing:** For each question, select the best-fit M365 Copilot agent:

| Route to | When the question is about |
|----------|---------------------------|
| **@Researcher** | Trends, comparisons, landscape analysis, best practices, "what's new", competitive intel, broad synthesis |
| **@Analyst** | Metrics, KPIs, data analysis, calculations, cost modeling, "how much/many", quantitative reasoning, structured data |

Prefix each question with the selected agent (`@Researcher` or `@Analyst`). If unclear, default to `@Researcher`.

If a `custom_focus` was provided, weight questions toward that topic. If all signals are empty, fall back to current Azure AI / agent architecture trends with a clear note.

### Phase 3: Invoke Agents (3 Parallel Calls)

Send each question to its assigned agent via `ask_work_iq`:

```
ask_work_iq: "@Researcher {research_question}"   # for exploration/synthesis
ask_work_iq: "@Analyst {research_question}"       # for data/quantitative
```

Run all 3 in parallel. Allow 1 retry per question. If @Analyst is unavailable (tenant not enabled), fall back to @Researcher and note the fallback. On complete failure, record the original question for manual execution.

### Phase 4: Compile & Save Digest

Use the digest template in [assets/digest-template.md](assets/digest-template.md). Save to `{output_dir}/{date}-daily-research-digest.md`.

## Guardrails
- Do not fabricate M365 data — only use what WorkIQ returns.
- If WorkIQ fails, state what failed and offer manual fallback.
- Do not skip Phase 1 — signal-driven research is the core value.
- Do not expose raw email content or transcripts — only synthesized themes.
- Questions must be specific enough for deep @Researcher answers.

## Examples

- **`/daily-research-digest`** → Full pipeline (24h lookback).
- **`/daily-research-digest 3 days`** → `lookback = 3 days`.
- **`/daily-research-digest 24 hours Foundry Agents`** → Weighted toward Foundry Agents.
- **"Run my daily research but skip Teams chats"** → Email + meetings only.
- **"Just show me the questions first"** → Phase 1–2 only, pause for review.
