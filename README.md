# YourTradebase API integration and MCP server: connect YourTradebase to ChatGPT, Claude and any AI agent

**YourTradebase API integration and MCP server by Cotlet.dev: yes, Claude, ChatGPT and other AI agents can work in your YourTradebase account.** YourTradebase has no public API and no official MCP server; it connects to QuickBooks and Xero and exports to CSV. Cotlet.dev connects your own signed-in YourTradebase account to any AI agent, so you ask in plain words and the AI sends quotes and chases invoices for you.

## What you can ask

> List invoices more than 14 days overdue and draft a polite reminder for each. Send nothing yet.

> Which quotes did customers open but not accept? Draft a follow-up for each.

> Turn these site notes into a draft quote from my price list. Do not send it.

> List jobs marked done this week that have no invoice yet.

> Which invoices and payments from this month are not in Xero yet?

## What tradespeople ask their AI to do in YourTradebase

From tradespeople' posts on Reddit, Facebook groups, review sites and the YourTradebase help center (checked 25 Nov 2026).

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Chase overdue invoices | "List invoices more than 14 days overdue and draft a polite reminder for each. Send nothing yet." | Weekly |
| Follow up read quotes | "Which quotes did customers open but not accept? Draft a follow-up for each." | Weekly |
| Quote from site notes | "Turn these notes into a draft quote from my price list for the Smith bathroom. Do not send it." | After a site visit |
| Invoice finished jobs | "List jobs marked done this week that have no invoice yet." | Weekly |
| Check the Xero or QuickBooks sync | "List this month's invoices and payments that are not in Xero yet." | Monthly |
| Import a price list | "Add the prices from this supplier CSV to my materials and labour rates after my OK." | When prices change |

Your AI asks you before it sends, buys, replies or changes anything. Payments stay in your hands.

## YourTradebase API

YourTradebase has no API or webhooks; it syncs with QuickBooks or Xero and exports CSV by hand (checked 25 Nov 2026). With Cotlet.dev you build your own YourTradebase API: show your AI agent a job once in your signed-in YourTradebase account, save it as an endpoint and call it from n8n, Make, Zapier or your own code. How it works: https://cotlet.dev/integrations/api/yourtradebase

Endpoints you can build in a few minutes (none of them exists until you build it):

- `overdue-invoices`: Invoice, customer, total and days overdue
- `quotes-not-accepted`: Quotes read but not accepted, oldest first
- `job-balances`: Balance still owed on each live job
- `week-schedule`: Job, customer and team member for each day
- `jobs-without-invoice`: Jobs marked done that have no invoice yet
- `draft-quote`: A draft quote from your price list, not sent

Call one with your Cotlet API key: `POST https://cotlet.dev/api/v1/custom/overdue-invoices`. One call runs up to 90 seconds, writes wait for your OK, payments are blocked.

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

This repo is a real MCP server. It speaks MCP over stdio, lists its YourTradebase tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"yourtradebase":{"command":"uvx","args":["yourtradebase-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t yourtradebase-mcp .
docker run -i --rm -e COTLET_API_KEY yourtradebase-mcp
```

Tools: `yourtradebase_guide`, `yourtradebase_status`, `yourtradebase_open`, `yourtradebase_read`, `yourtradebase_click`, `yourtradebase_type`, `yourtradebase_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install yourtradebase-mcp        # or run it once: uvx yourtradebase-mcp connect
yourtradebase-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 yourtradebase_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 yourtradebase_agent.py status    # is YourTradebase ready in your Cotlet.dev account
python3 yourtradebase_agent.py guide     # YourTradebase screens and sign-in link
python3 yourtradebase_agent.py read      # open YourTradebase, read the page, close the browser
python3 yourtradebase_agent.py tools     # list Cotlet.dev tools
python3 yourtradebase_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from YourTradebase users

**Does YourTradebase have an API?**

YourTradebase has no public API (checked 25 Nov 2026). It connects to QuickBooks and Xero and exports quotes, invoices, jobs and customers to CSV. Cotlet.dev works in your own YourTradebase account, so your AI agent can send quotes and chase invoices.

**How do I connect YourTradebase to ChatGPT?**

Add Cotlet.dev as an app in ChatGPT (Settings → Apps, connector URL `https://cotlet.dev/mcp/v1`), sign in to YourTradebase once in your Cotlet.dev Cloud Browser, and ask ChatGPT in plain words. No code and no YourTradebase API key.

**Is there a way to use Claude with YourTradebase?**

Yes. In Claude Code run the one-line `claude mcp add` command above; in the Claude app add `https://cotlet.dev/mcp/v1` as a custom connector. Claude then works in your own YourTradebase account.

**Is there a YourTradebase MCP server?**

YourTradebase has no official MCP server. Cotlet.dev is a hosted MCP server that works in YourTradebase for you, 24/7, with Claude, ChatGPT, Cursor, Codex, n8n, Make and Zapier.

**Can AI check that YourTradebase invoices reached Xero or QuickBooks?**

Yes. Your AI reads invoices and payments in YourTradebase, compares them with the list you give it from Xero or QuickBooks and shows what is missing.

**Can AI turn my site notes into a YourTradebase quote?**

It drafts the quote in your account from your saved materials and labour rates and leaves it unsent until you check it.

## Find it on

- [YourTradebase on Cotlet.dev](https://cotlet.dev/integrations/mcp/yourtradebase?utm_source=github&utm_medium=readme&utm_campaign=yourtradebase&utm_content=find)
- [PyPI package yourtradebase-mcp](https://pypi.org/project/yourtradebase-mcp/)
- [Odel](https://odel.app/modules/cotlet/yourtradebase)
- [mcpbeat](https://mcpbeat.com/mcp-servers/cotlet/yourtradebase/)
- [Step-by-step guide (PDF) on Internet Archive](https://archive.org/details/cotlet-connect-yourtradebase-to-ai-agents-guide)

## Also works with

- [MyBuilder MCP](https://github.com/cotlet-connect/mybuilder-mcp)
- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [Wanderlog MCP](https://github.com/cotlet-connect/wanderlog-mcp)
- [SmugMug MCP](https://github.com/cotlet-connect/smugmug-mcp)
- [Garmin Connect MCP](https://github.com/cotlet-connect/garmin-mcp)
- [Housecall Pro MCP](https://github.com/cotlet-connect/housecall-pro-mcp)
- [Medium MCP](https://github.com/cotlet-connect/medium-mcp)
- [Crexi MCP](https://github.com/cotlet-connect/crexi-mcp)
- [DistroKid MCP](https://github.com/cotlet-connect/distrokid-mcp)
- [LinkedIn MCP](https://github.com/cotlet-connect/linkedin-mcp)
- [EstimateOne MCP](https://github.com/cotlet-connect/estimateone-mcp)
- [Google Search Console MCP](https://github.com/cotlet-connect/google-search-console-mcp)
- [Lichess MCP](https://github.com/cotlet-connect/lichess-mcp)
- [Strava MCP](https://github.com/cotlet-connect/strava-mcp)
- [StreamElements MCP](https://github.com/cotlet-connect/streamelements-mcp)
- [Meetup MCP](https://github.com/cotlet-connect/meetup-mcp)
- [AccuRanker MCP](https://github.com/cotlet-connect/accuranker-mcp)
- [GetYourGuide MCP](https://github.com/cotlet-connect/getyourguide-mcp)
- [Idealista MCP](https://github.com/cotlet-connect/idealista-mcp)
- [Riverside MCP](https://github.com/cotlet-connect/riverside-mcp)
- [Udemy MCP](https://github.com/cotlet-connect/udemy-mcp)
- [Faire MCP](https://github.com/cotlet-connect/faire-mcp)
- [Teachable MCP](https://github.com/cotlet-connect/teachable-mcp)
- [Vagaro MCP](https://github.com/cotlet-connect/vagaro-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Zoopla MCP](https://github.com/cotlet-connect/zoopla-mcp)
- [Dribbble MCP](https://github.com/cotlet-connect/dribbble-mcp)
- [Hostelworld MCP](https://github.com/cotlet-connect/hostelworld-mcp)
- [Mashvisor MCP](https://github.com/cotlet-connect/mashvisor-mcp)
- [Rent Manager MCP](https://github.com/cotlet-connect/rent-manager-mcp)
- [ThriveCart MCP](https://github.com/cotlet-connect/thrivecart-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Booksy MCP](https://github.com/cotlet-connect/booksy-mcp)
- [BookingKoala MCP](https://github.com/cotlet-connect/bookingkoala-mcp)
- [Milanote MCP](https://github.com/cotlet-connect/milanote-mcp)
- [Partiful MCP](https://github.com/cotlet-connect/partiful-mcp)
- [Contractor Foreman MCP](https://github.com/cotlet-connect/contractor-foreman-mcp)
- [Pixieset MCP](https://github.com/cotlet-connect/pixieset-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [ZenMaid MCP](https://github.com/cotlet-connect/zenmaid-mcp)
- [AroFlo MCP](https://github.com/cotlet-connect/aroflo-mcp)
- [Simpro MCP](https://github.com/cotlet-connect/simpro-mcp)
- [Workiz MCP](https://github.com/cotlet-connect/workiz-mcp)
- [Kickserv MCP](https://github.com/cotlet-connect/kickserv-mcp)
- [Service Autopilot MCP](https://github.com/cotlet-connect/service-autopilot-mcp)
- [Mindbody MCP](https://github.com/cotlet-connect/mindbody-mcp)
- [DaySmart MCP](https://github.com/cotlet-connect/daysmart-mcp)
- [Acuity Scheduling MCP](https://github.com/cotlet-connect/acuity-scheduling-mcp)
- [Jane App MCP](https://github.com/cotlet-connect/jane-app-mcp)
- [FieldEdge MCP](https://github.com/cotlet-connect/fieldedge-mcp)
- [Phorest MCP](https://github.com/cotlet-connect/phorest-mcp)
- [DINGG MCP](https://github.com/cotlet-connect/dingg-mcp)
- [Invoice Simple MCP](https://github.com/cotlet-connect/invoice-simple-mcp)
- [PhotoShelter MCP](https://github.com/cotlet-connect/photoshelter-mcp)
- [RazorSync MCP](https://github.com/cotlet-connect/razorsync-mcp)
- [ShootProof MCP](https://github.com/cotlet-connect/shootproof-mcp)
- [Invoice2go MCP](https://github.com/cotlet-connect/invoice2go-mcp)
- [Appointy MCP](https://github.com/cotlet-connect/appointy-mcp)
- [Contractor+ MCP](https://github.com/cotlet-connect/contractor-plus-mcp)
- [Petpooja MCP](https://github.com/cotlet-connect/petpooja-mcp)
- [Teachers Pay Teachers MCP](https://github.com/cotlet-connect/teachers-pay-teachers-mcp)
- [Bloom MCP](https://github.com/cotlet-connect/bloom-mcp)
- [hipages MCP](https://github.com/cotlet-connect/hipages-mcp)
- [ServiceSeeking MCP](https://github.com/cotlet-connect/serviceseeking-mcp)
- [MyJobQuote MCP](https://github.com/cotlet-connect/myjobquote-mcp)
- [Yelp for Business MCP](https://github.com/cotlet-connect/yelp-for-business-mcp)
- [Rosie MCP](https://github.com/cotlet-connect/rosie-mcp)
- [Roofr MCP](https://github.com/cotlet-connect/roofr-mcp)
- [FieldPulse MCP](https://github.com/cotlet-connect/fieldpulse-mcp)
- [JobNimbus MCP](https://github.com/cotlet-connect/jobnimbus-mcp)
- [Ascora MCP](https://github.com/cotlet-connect/ascora-mcp)
- [eLocal MCP](https://github.com/cotlet-connect/elocal-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/mcp/yourtradebase?utm_source=github&utm_medium=readme&utm_campaign=yourtradebase

Start free: https://cotlet.dev/signup?app=yourtradebase&utm_source=github&utm_medium=readme&utm_campaign=yourtradebase
