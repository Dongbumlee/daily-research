# Setup Guide — Daily Research Digest

## Prerequisites

| Requirement | Details |
|-------------|---------|
| Node.js | v18+ (check: `node -v`) |
| WorkIQ MCP | `@microsoft/workiq` npm package |
| M365 Auth | Entra ID session (browser login) |
| @Researcher | Enabled in your M365 tenant |

## Step 1: Install WorkIQ MCP Server

```bash
# Option A: Global install
npm install -g @microsoft/workiq

# Option B: Use npx (no install, slower first run)
npx -y @microsoft/workiq mcp
```

## Step 2: Configure MCP Server

Add the WorkIQ entry to your MCP config.

**For Copilot CLI** (`~/.copilot/mcp-config.json`):
```json
{
  "mcpServers": {
    "workiq": {
      "command": "npx",
      "args": ["-y", "@microsoft/workiq", "mcp"],
      "type": "stdio"
    }
  }
}
```

**For VS Code** (`settings.json` → `mcp.servers`):
```json
{
  "mcp.servers": {
    "workiq": {
      "command": "npx",
      "args": ["-y", "@microsoft/workiq", "mcp"],
      "type": "stdio"
    }
  }
}
```

Verify:
```bash
cat ~/.copilot/mcp-config.json 2>/dev/null | grep -q "workiq" && echo "✅ Configured" || echo "❌ Not found"
```

## Step 3: Accept EULA

The EULA must be accepted before first use:
- **URL:** https://github.com/microsoft/work-iq-mcp
- **Via CLI tool:** Use the `accept_eula` tool with the EULA URL
- **Via file:** Ensure `~/.workiq.json` contains `{"I-accept-EULA": "true"}`

## Step 4: Authenticate to M365

WorkIQ requires an active Microsoft 365 / Entra ID session:
- The first `ask_work_iq` call may trigger a browser-based login flow
- For CI/headless: set `AZURE_CLIENT_ID`, `AZURE_TENANT_ID`, `AZURE_CLIENT_SECRET`

## Step 5: Verify @Researcher Agent

The @Researcher agent must be enabled in your tenant:
1. Open M365 Copilot Chat (https://copilot.microsoft.com)
2. Type `@Researcher test`
3. If it responds, you're good. If not, contact your M365 admin.

## Quick Smoke Test

```
ask_work_iq: "What is my name and role?"
```

If this returns your profile info, all prerequisites are met.

## Troubleshooting

| Problem | Fix |
|---------|-----|
| `workiq: command not found` | Run `npm install -g @microsoft/workiq` or use npx |
| EULA not accepted | Check `~/.workiq.json` or re-accept via CLI |
| Auth failure / 401 | Re-authenticate: `az login` or browser flow |
| @Researcher not found | Ask M365 admin to enable Copilot agents in tenant |
| Node.js < 18 | Upgrade: `nvm install --lts` |
