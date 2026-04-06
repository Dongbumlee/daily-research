# Daily Research Digest

An [Agent Skill](https://agentskills.io) that mines your M365 communications to generate a daily research digest powered by the @Researcher agent.

**Works with:** GitHub Copilot in VS Code · GitHub Copilot CLI · GitHub Copilot coding agent  
**Platforms:** Windows · macOS · Linux

---

> ### One prompt. That's it.
>
> ```
> /daily-research-digest
> ```
>
> Copilot reads your emails, meetings, and Teams chats — finds what matters — researches it — and delivers a digest. No context-switching, no manual searching.
>
> ```
> You:      /daily-research-digest
>
> Copilot:  [1/4] Scanning your M365 signals...
>              - 40 emails scanned, grouped by theme
>              - 3 meetings reviewed for agendas and follow-ups
>              - 6 Teams chats analyzed for technical discussions
>
>           [2/4] Extracting top themes from your communications...
>              - MSRC security incident (CWE-22 path traversal)
>              - RAI test categorization (XPIA vs Direct vs Overreliance)
>              - Dependency management across solution accelerators
>
>           [3/4] Generating 3 research questions and invoking @Researcher...
>              - Question 1: MSRC remediation best practices... done
>              - Question 2: RAI assessment frameworks... done
>              - Question 3: Dependabot + S360 automation patterns... done
>
>           [4/4] Compiling digest with findings, takeaways, and action items...
>              Daily digest saved → prompts/daily-research-results/2026-04-06-daily-research-digest.md
> ```
>
> Want to go deeper? Just say more:
> ```
> /daily-research-digest 3 days Foundry Agents
> ```

---

## What It Does

```
Emails + Meetings + Teams Chats
              |
    Extract top 3 themes
              |
    @Researcher x 3 (sequential)
              |
      Daily Research Digest
```

1. **Gathers signals** from your M365 emails, meetings, and Teams chats via [WorkIQ MCP](https://github.com/microsoft/work-iq-mcp)
2. **Analyzes themes** through a Senior Architecture Specialist / Principal Engineer lens
3. **Generates 3 research questions** and invokes the @Researcher agent sequentially
4. **Compiles a digest** with findings, takeaways, and action items

## Quick Install

### Option A: Project skill (shared with your team)

<details>
<summary><strong>macOS / Linux</strong></summary>

```bash
cp -r daily-research-digest/ .github/skills/
```
</details>

<details>
<summary><strong>Windows (PowerShell)</strong></summary>

```powershell
Copy-Item -Recurse daily-research-digest/ .github/skills/
```
</details>

### Option B: Personal skill (just for you)

<details>
<summary><strong>macOS / Linux</strong></summary>

```bash
cp -r daily-research-digest/ ~/.copilot/skills/
```
</details>

<details>
<summary><strong>Windows (PowerShell)</strong></summary>

```powershell
Copy-Item -Recurse daily-research-digest/ $env:USERPROFILE\.copilot\skills\
```
</details>

### Option C: Clone this repo directly

<details>
<summary><strong>macOS / Linux</strong></summary>

```bash
git clone https://github.com/Dongbumlee/daily-research.git
cp -r daily-research/daily-research-digest/ ~/.copilot/skills/
```
</details>

<details>
<summary><strong>Windows (PowerShell)</strong></summary>

```powershell
git clone https://github.com/Dongbumlee/daily-research.git
Copy-Item -Recurse daily-research\daily-research-digest\ $env:USERPROFILE\.copilot\skills\
```
</details>

## Prerequisites

| Requirement | How to Get It |
|-------------|---------------|
| **WorkIQ MCP server** | `npm install -g @microsoft/workiq` — [setup guide](daily-research-digest/references/SETUP.md) |
| **M365 authentication** | Entra ID login (browser flow on first use) |
| **@Researcher agent** | Must be enabled in your M365 tenant |
| **Node.js 18+** | `node -v` to check — install via [nvm](https://github.com/nvm-sh/nvm) (macOS/Linux) or [nvm-windows](https://github.com/coreybutler/nvm-windows) (Windows) |

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
# Daily Research Digest — 2026-04-06

## Signal Summary
Today's research was driven by recurring discussions about...

## Research Topic 1: Foundry Agents Tool Optimization
**Question:** @Researcher ...
### Findings
...
### Key Takeaways
- ...

## Action Items
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
| **1. Gather Signals** | Queries M365 for emails, meetings, Teams chats | `ask_work_iq` × 3 (parallel) |
| **2. Analyze Themes** | Identifies top 3 themes across all channels | AI reasoning |
| **3. Research** | Sends 3 questions to @Researcher sequentially | `ask_work_iq` × 3 (one at a time) |
| **4. Compile** | Formats digest with findings + action items | AI writing + `bash` |

> **Why sequential?** Concurrent @Researcher calls through WorkIQ can cause timeouts on deep queries. Sequential invocation is more reliable — each question waits for a full response before the next one is sent.

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
