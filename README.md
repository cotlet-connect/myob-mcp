# How to connect MYOB to Claude Code, ChatGPT and any AI agent

**Yes, Claude, ChatGPT and other AI agents can work in your MYOB account.** Cotlet.dev connects your own signed-in MYOB account to AI agents through MCP and REST, so you can automate invoices, bills, payments and customers with your approval.

## What you can ask

> Create an invoice from this billing list and email it to the customer.

> List overdue MYOB invoices every Monday.

> Record a payment against an open invoice.

> Enter these supplier bills without creating duplicates.

> Add a new customer from this form.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its MYOB tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"myob":{"command":"uvx","args":["myob-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t myob-mcp .
docker run -i --rm -e COTLET_API_KEY myob-mcp
```

Tools: `myob_guide`, `myob_status`, `myob_open`, `myob_read`, `myob_click`, `myob_type`, `myob_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install myob-mcp        # or run it once: uvx myob-mcp connect
myob-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 myob_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 myob_agent.py status    # is MYOB ready in your Cotlet.dev account
python3 myob_agent.py guide     # MYOB screens and sign-in link
python3 myob_agent.py read      # open MYOB, read the page, close the browser
python3 myob_agent.py tools     # list Cotlet.dev tools
python3 myob_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from MYOB users

**Does MYOB have an API?**

MYOB has a Business API for approved developers and OAuth apps. Cotlet.dev works in your own signed-in MYOB account through a hosted MCP server, so you do not need to build your own integration.

**How do I connect MYOB to ChatGPT?**

Add `https://cotlet.dev/mcp/v1` as a custom connector in ChatGPT, sign in to MYOB once in Cotlet.dev Cloud Browser, and ask ChatGPT to work with your account.

**Is there an MYOB MCP server?**

MYOB has a limited official connector for invited customers. Cotlet.dev provides a hosted MYOB MCP server for invoices, bills, payments and customers in your own account.

**Can Claude work with MYOB?**

Yes. Add Cotlet.dev as a custom connector in Claude or use the Claude Code MCP command, then approve the connection.

## Find it on

- [MYOB on Cotlet.dev](https://cotlet.dev/integrations/myob-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=myob&utm_content=find)
- [PyPI package myob-mcp](https://pypi.org/project/myob-mcp/)
- [Odel](https://odel.app/modules/cotlet/myob)
- [mcpbeat](https://mcpbeat.com/mcp-servers/cotlet/myob/)
- [Step-by-step guide (PDF) on Internet Archive](https://archive.org/details/cotlet-connect-myob-to-ai-agents-guide)

## Also works with


- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/myob-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=myob

Start free: https://cotlet.dev/signup?app=myob&utm_source=github&utm_medium=readme&utm_campaign=myob
