# MYOB API integration and MCP server: connect MYOB to ChatGPT, Claude and any AI agent

MYOB API integration and MCP server, unofficial, by Cotlet. Your AI works in your own signed-in MYOB account: invoices, bills, bank feeds and reconciliation, contacts, inventory, payroll and reports, on the same screens your login opens. No developer and no MYOB API key needed. Cotlet is independent and is not affiliated with MYOB.

**Does MYOB have an API or an MCP server?** MYOB has an official connector for Claude and ChatGPT, but it is read-only, in beta in Australia (announced 17 Aug 2026) and returns account-level numbers only. The MYOB Business API is paid: its Developer Program costs $110 to $630 a month (checked 23 Nov 2026), and MYOB says it does not support webhooks. MYOB has no Zapier app and no Make app.

## What small business owners and bookkeepers ask their AI to do in MYOB

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Bank feed coding | "Code this week's uncoded bank feed transactions to the right accounts and show me the matches before you save." | Weekly |
| Duplicate bank transactions | "Find duplicate transactions from the bank feed this month and list them for me to remove." | Monthly |
| Bills from supplier invoices | "Create bills in MYOB from these supplier invoices and receipts. Show me before you save." | When invoices come in |
| Invoices from a spreadsheet | "Enter the 35 invoices in this Excel sheet into MYOB. Show me the first three before you save the rest." | Weekly |
| Overdue invoices | "List overdue invoices by customer and draft a reminder for each. Send nothing yet." | Weekly |
| Transactions behind an account | "Show the transactions behind this quarter's motor vehicle expenses and flag anything odd." | When you review the books |
| BAS figures | "Prepare this quarter's BAS figures from MYOB and list what needs checking before I lodge." | Quarterly |
| Bank statement into MYOB | "Turn this bank CSV into a file MYOB will import." | When a feed is missing |
| Invoices with line items to Excel | "Export all invoices from this financial year with their line items to Excel." | When you need it |
| Numbers in Google Sheets or Power BI | "Put daily sales, receivables and bank balance in my Google Sheet." | Your own endpoint, called from Google Sheets or Power BI |
| Moving to Xero | "Export customers, suppliers, items, invoices and bills before I move to Xero." | Once |

Your AI asks you before it saves, sends or changes anything in your books. Payments stay in your hands: Cotlet refuses payments, and you lodge your BAS yourself.

## Your own MYOB API, built with Cotlet

MYOB's API needs a paid Developer Program and a developer. With Cotlet you build your own: your AI does the job once in your Cloud Browser, you save it as an endpoint, and you call it from Google Sheets, Power BI, Shopify, Pipedrive, Zapier, Make, n8n, your own code.

Examples you can build in a few minutes (none of them exists until you build it):

| Endpoint you build | What it returns or does |
|---|---|
| `uncoded-transactions` | bank feed transactions not yet coded, with date, amount and description |
| `overdue-invoices` | overdue invoices by customer with amount and days overdue |
| `account-transactions` | transactions behind an account for a period |
| `invoices-export` | invoices with line items for a period |
| `bas-figures` | BAS totals for a quarter, from your MYOB reports |
| `create-bill` | creates a bill from the lines you send; nothing is paid (writes only after you allow it) |

Call it with your Cotlet API key, JSON in and JSON out:

```bash
curl -X POST https://cotlet.dev/api/v1/custom/uncoded-transactions \
  -H "Authorization: Bearer $COTLET_API_KEY" -H "Content-Type: application/json" -d '{}'
```

Limits: one call runs up to 90 seconds, writes wait for your consent, payments are blocked. Start here: https://cotlet.dev/app/custom-api/new?app=myob

## Connect in 2 minutes

Sign up at https://cotlet.dev, sign in to MYOB once in your Cloud Browser, then add Cotlet to your AI.

### Claude

Settings → Connectors → Add custom connector, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev.

### ChatGPT

Open Plugins in ChatGPT, choose Create as a plugin, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev. No API key needed.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

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
**Does MYOB have an official connector for Claude or ChatGPT?**

Yes, read-only and in beta in Australia (announced 17 Aug 2026). It returns account-level numbers and cannot change your books. Cotlet works in your own MYOB login, reads the transactions behind each account and makes changes after your OK.

**Does MYOB have an API or a Zapier app?**

MYOB's API needs a paid Developer Program ($110 to $630 a month, checked 23 Nov 2026) and has no webhooks, and MYOB has no Zapier or Make app. With Cotlet you build your own endpoints in your signed-in MYOB account.

**Can AI code and reconcile my MYOB bank feed?**

Yes. Your AI codes uncoded bank feed transactions to the right accounts, finds duplicates and shows you every match before it saves.

**Can AI create bills in MYOB from supplier invoices?**

Yes. It reads supplier invoices and receipts and creates the bills, which you check before they are saved. Paying them stays with you.

**Can AI prepare and lodge my BAS from MYOB?**

It prepares the quarter's BAS figures from your MYOB reports and lists what needs checking. You lodge the BAS yourself.

**How do I bulk import invoices from Excel into MYOB?**

Your AI enters the invoices from your spreadsheet into MYOB and shows you the first ones before it saves the rest.

**How do I get everything out of MYOB before I move to Xero?**

Your AI exports customers, suppliers, items, invoices with line items and bills from the screens your login opens.

## Find it on

- [MYOB MCP server on Cotlet.dev](https://cotlet.dev/integrations/myob-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=myob&utm_content=find)
- [MYOB API with Cotlet](https://cotlet.dev/integrations/api/myob?utm_source=github&utm_medium=readme&utm_campaign=myob&utm_content=find)
- [Does MYOB have an API or MCP server?](https://cotlet.dev/api-mcp-status/myob?utm_source=github&utm_medium=readme&utm_campaign=myob&utm_content=find)
- [PyPI package myob-mcp](https://pypi.org/project/myob-mcp/)

## More accounting apps on Cotlet

- [Tradify MCP](https://github.com/cotlet-connect/tradify-mcp)
- [TaxDome MCP](https://github.com/cotlet-connect/taxdome-mcp)
- [Invoice Simple MCP](https://github.com/cotlet-connect/invoice-simple-mcp)
- [Joist MCP](https://github.com/cotlet-connect/joist-mcp)
