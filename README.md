# 📊 Daily Research Digest

An [Agent Skill](https://agentskills.io) that mines your M365 communications to generate a daily research digest powered by the @Researcher agent.

**Works with:** GitHub Copilot in VS Code · GitHub Copilot CLI · GitHub Copilot coding agent

## What It Does

```
📧 Emails + 📅 Meetings + 💬 Teams Chats
              ↓
    🔍 Extract top 3 themes
              ↓
    🤖 @Researcher × 3 (parallel)
              ↓
    📊 Daily Research Digest
```

1. **Gathers signals** from your M365 emails, meetings, and Teams chats via [WorkIQ MCP](https://github.com/microsoft/work-iq-mcp)
2. **Analyzes themes** through a Senior Architecture Specialist / Principal Engineer lens
3. **Generates 3 research questions** and invokes the @Researcher agent in parallel
4. **Compiles a digest** with findings, takeaways, and action items

## Quick Install

### Option A: Project skill (shared with your team)

```bash
# Copy into your repository
cp -r daily-research-digest/ .github/skills/
```

### Option B: Personal skill (just for you)

```bash
# Copy into your user profile
cp -r daily-research-digest/ ~/.copilot/skills/
```

### Option C: Clone this repo directly

```bash
git clone https://github.com/Dongbumlee/daily-research.git
cp -r daily-research/daily-research-digest/ ~/.copilot/skills/
```

## Prerequisites

| Requirement | How to Get It |
|-------------|---------------|
| **WorkIQ MCP server** | `npm install -g @microsoft/workiq` — [setup guide](daily-research-digest/references/SETUP.md) |
| **M365 authentication** | Entra ID login (browser flow on first use) |
| **@Researcher agent** | Must be enabled in your M365 tenant |
| **Node.js 18+** | `node -v` to check, `nvm install --lts` to upgrade |

See [references/SETUP.md](daily-research-digest/references/SETUP.md) for detailed setup instructions.

## Usage

### As a slash command in VS Code Copilot Chat

```
/daily-research-digest
/daily-research-digest 3 days
/daily-research-digest 1 week Foundry Agents
```

### As a natural language request

```
"What should I research today?"
"Generate research from my last 3 days of communications"
"Run my daily research digest, focus on MCP tooling"
```

### Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `lookback` | `24 hours` | How far back to scan signals |
| `custom_focus` | _(none)_ | Bias questions toward a specific topic |
| `output_dir` | `./prompts/daily-research-results` | Where to save the digest |

## Output

The skill produces a markdown digest saved to `{output_dir}/{date}-daily-research-digest.md`:

```markdown
# 📊 Daily Research Digest — 2026-04-06

## Signal Summary
Today's research was driven by recurring discussions about...

## Research Topic 1: Foundry Agents Tool Optimization
**Question:** @Researcher ...
### Findings
...
### Key Takeaways
- ...

## 🎯 Action Items
...

## Sources & References
...
```

## Skill Structure

```
daily-research-digest/
├── SKILL.md                    # Metadata + workflow instructions
├── references/
│   └── SETUP.md                # Installation & auth guide
├── assets/
│   ├── signal-queries.md       # M365 query templates
│   └── digest-template.md      # Output format template
└── scripts/                    # (reserved for future automation)
```

Follows the [Agent Skills specification](https://agentskills.io/specification) with progressive disclosure — only `SKILL.md` loads at activation; references and assets load on demand.

## How It Works

| Phase | What Happens | Tools Used |
|-------|-------------|------------|
| **1. Gather Signals** | Queries M365 for emails, meetings, Teams chats | `ask_work_iq` × 3 |
| **2. Analyze Themes** | Identifies top 3 themes across all channels | AI reasoning |
| **3. Research** | Sends 3 questions to @Researcher in parallel | `ask_work_iq` × 3 |
| **4. Compile** | Formats digest with findings + action items | AI writing + `bash` |

## Customization

### Change the persona lens

Edit `SKILL.md` Phase 2 to replace "Senior Architecture Specialist / Principal Engineer" with your role.

### Add more signal sources

Add additional `ask_work_iq` calls in Phase 1 (e.g., OneNote, SharePoint, Planner).

### Adjust question count

Change `max-questions` in the metadata and update Phase 2/3 instructions.

## Related

- [Agent Skills specification](https://agentskills.io)
- [VS Code Agent Skills docs](https://code.visualstudio.com/docs/copilot/customization/agent-skills)
- [WorkIQ MCP](https://github.com/microsoft/work-iq-mcp)
- [github/awesome-copilot](https://github.com/github/awesome-copilot) — community skills collection

## License

MIT
