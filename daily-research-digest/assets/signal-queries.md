# Signal Query Templates

Parameterized queries for M365 signal gathering via `ask_work_iq`.
Replace `{lookback}` with the desired time window (e.g., "24 hours", "3 days").

---

## Email Signals

```
Summarize my emails from the last {lookback}. Group them by theme or project.
For each theme, list:
- Key topics discussed
- Decisions made
- Open questions
- Action items
Focus on substantive work threads — skip newsletters, automated notifications, and FYI-only messages.
```

---

## Meeting Signals

```
What meetings do I have today and what meetings did I have in the last {lookback}?
For each meeting, summarize:
- Agenda or stated purpose
- Key discussion topics
- Decisions or follow-ups agreed
- Unresolved questions
If meeting notes or transcripts are available, include highlights.
```

---

## Teams Chat Signals

```
Summarize my Microsoft Teams chat activity from the last {lookback}.
Focus on:
- Substantive technical discussions
- Questions people asked me or tagged me on
- Topics I was tagged on across channels
- Recurring themes or hot topics
Skip casual, social, or off-topic messages.
```

---

## Fallback (when all signals are empty)

If all three signal queries return empty or error, use this prompt for Phase 2 instead:

```
Generate 3 research questions relevant to a Senior Architecture Specialist / Principal Engineer
at Microsoft working on Azure AI, solution accelerators, and agent-based architectures.
Focus on current trends (2025–2026): multi-agent orchestration, Foundry Agents, MCP tooling,
AI evaluation frameworks, and enterprise AI adoption patterns.
Each question should start with @Researcher and request deep, sourced analysis.
```
