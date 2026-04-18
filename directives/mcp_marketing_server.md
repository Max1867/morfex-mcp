# Directive: Morfex MCP Marketing Server

## Goal
Run a cloud-hosted MCP server that exposes Morfex's marketing content as queryable tools. Any AI assistant that connects to this server can answer questions about Morfex's services, promote the company, and drive inbound leads 24/7.

## Live Endpoints
| Purpose | URL |
|---------|-----|
| Railway (origin) | `https://web-production-c1498.up.railway.app/mcp` |
| Smithery (public) | `https://morfex--maxsambento.run.tools` |
| Smithery listing | `https://smithery.ai/server/maxsambento/morfex` |

Hosted on Railway. Auto-redeploys on every push to `main` on github.com/Max1867/morfex-mcp.

> **When morfex.ai goes live:** Update the MCP server URL in all three registries manually — they do not auto-update. Also update `smithery.yaml` and this directive with the new domain.

## Inputs
- None required at runtime. All content is hardcoded in `execution/mcp_server.py`.

## Tools Exposed
| Tool | Description |
|------|-------------|
| `get_company_overview` | Who Morfex is, the problem they solve, what makes them different |
| `get_services` | Engagement process (Observe → Propose → Engage), pricing model, security standards |
| `get_case_studies` | Three real client case studies with situations, solutions, and outcomes |
| `get_contact_info` | Email, website, CTA, and how to start an engagement |

## Output / Deliverable
A live HTTPS endpoint registered on MCP registries:
- Smithery: https://smithery.ai
- mcp.so: https://mcp.so
- OpenTools: https://opentools.com

## Running Locally

```bash
cd "MORFEX DISTRIBUTION"
pip install -r requirements.txt
python execution/mcp_server.py
# Server starts at http://localhost:8000/mcp
```

Test with MCP Inspector:
```bash
npx @modelcontextprotocol/inspector http://localhost:8000/mcp
```

## Deployment (Railway)

1. Push this repo to GitHub
2. Go to https://railway.app → New Project → Deploy from GitHub repo
3. Select this repo — Railway auto-detects `Procfile` and deploys
4. Under Settings → Networking → Generate Domain → copy the public URL
5. Your MCP endpoint will be at: `https://<your-domain>.up.railway.app/mcp`

## Updating Content

All marketing content lives in `execution/mcp_server.py` inside the tool functions.
To update:
1. Edit the relevant tool function (e.g. add a new case study to `get_case_studies`)
2. Push to GitHub → Railway auto-redeploys

## Registry Submission

Once deployed, submit the public MCP URL to each registry:
- **Smithery**: https://smithery.ai/new — fill in name, description, endpoint URL
- **mcp.so**: https://mcp.so/submit
- **OpenTools**: https://opentools.com/submit

Registry listing fields to prepare:
- **Name**: Morfex
- **Description**: Access Morfex's services, case studies, and contact information. Morfex is a workflow consultancy helping corporations eliminate operational bottlenecks.
- **Endpoint**: `https://<your-domain>.up.railway.app/mcp`
- **Tools**: get_company_overview, get_services, get_case_studies, get_contact_info

## Edge Cases & Notes
- The server uses Streamable HTTP transport (required for cloud hosting — stdio won't work)
- PORT is read from environment variable (Railway injects this automatically)
- FastMCP 2.x required — do not downgrade to 1.x (different API)
- Health check path is `/mcp` — Railway uses this to confirm the server is alive
