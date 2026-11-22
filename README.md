# ClubReady API integration and MCP server: connect ClubReady to ChatGPT, Claude and any AI agent

ClubReady API integration and MCP server, unofficial, by Cotlet. Your AI works in your own signed-in ClubReady account: prospects, members, class bookings, past dues and reports, on the same screens your login opens. No developer and no ClubReady API key needed. Cotlet is independent and is not affiliated with ClubReady.

**Does ClubReady have an API or an MCP server?** ClubReady has an Add Lead API that a club turns on to add prospects from its website or forms. Its wider APIs and data notifications are for partners that a club authorizes through the ClubReady Partner Portal. ClubReady has no MCP server, no ChatGPT or Claude app and no app of its own in the Zapier directory (checked 20 Nov 2026).

## What studio owners ask their AI to do in ClubReady

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Follow up new leads | "List yesterday's new prospects who have not booked a trial and draft a text for each. Send nothing yet." | Every morning, as a Cotlet automation |
| Past dues | "Build this week's past-due list with amount and days overdue, and draft a reminder for each member." | Every Monday |
| Class bookings | "Who is booked and who is on the waitlist for Saturday 9:00? How many spots are left?" | Before each class day |
| Members going quiet | "Which members have not checked in for 14 days? Sort them by membership type." | Weekly |
| Milestones | "Which members reach their 100th class this week?" | Weekly |
| Leads from ads and forms | "Add the leads in this spreadsheet as prospects, with their source and a note." | When a new file arrives |
| Exports | "Download this week's check-in export and the member list as spreadsheets." | Weekly |
| Payroll and commission | "Run the commission and payroll report for this pay period and total it per coach." | Every pay period |
| Credits and packs | "Which members on a 10-class pack have one class left?" | Weekly |

Your AI asks you before it sends, books or changes anything. Billing, agreements and payments stay in your hands: Cotlet refuses payments.

## Your own ClubReady API, built with Cotlet

ClubReady gives a club owner no self-serve API beyond Add Lead. With Cotlet you build your own: your AI does the job once in your Cloud Browser, you save it as an endpoint, and you call it from Google Sheets, n8n, Make or your own code.

Examples you can build in a few minutes (none of them exists until you build it):

| Endpoint you build | What it returns or does |
|---|---|
| `past-due-members` | name, phone, amount owed, days overdue |
| `new-prospects` | prospects since a date, with source and trial status |
| `class-roster` | booked and waitlisted members for a class and date |
| `checkins` | check-ins between two dates |
| `inactive-members` | members with no check-in for N days |
| `class-milestones` | members who reach a class count, for example 100 |
| `packs-running-low` | members with one or two classes left |
| `add-prospect` | adds a prospect with name, phone, email, source and note (writes only after you allow it) |

Call it with your Cotlet API key, JSON in and JSON out:

```bash
curl -X POST https://cotlet.dev/api/v1/custom/past-due-members \
  -H "Authorization: Bearer $COTLET_API_KEY" -H "Content-Type: application/json" -d '{}'
```

Limits: one call runs up to 90 seconds, writes wait for your consent, payments are blocked. Start here: https://cotlet.dev/app/custom-api/new?app=clubready

## Connect in 2 minutes

Sign up at https://cotlet.dev, sign in to ClubReady once in your Cloud Browser, then add Cotlet to your AI.

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

This repo is a real MCP server. It speaks MCP over stdio, lists its ClubReady tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"clubready":{"command":"uvx","args":["clubready-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t clubready-mcp .
docker run -i --rm -e COTLET_API_KEY clubready-mcp
```

Tools: `clubready_guide`, `clubready_status`, `clubready_open`, `clubready_read`, `clubready_click`, `clubready_type`, `clubready_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install clubready-mcp        # or run it once: uvx clubready-mcp connect
clubready-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 clubready_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 clubready_agent.py status    # is ClubReady ready in your Cotlet.dev account
python3 clubready_agent.py guide     # ClubReady screens and sign-in link
python3 clubready_agent.py read      # open ClubReady, read the page, close the browser
python3 clubready_agent.py tools     # list Cotlet.dev tools
python3 clubready_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from ClubReady owners

**Is there a ChatGPT integration for ClubReady?**

Not from ClubReady. With Cotlet, ChatGPT and Claude work in your own ClubReady account through your Cloud Browser, in their phone apps too.

**Can Claude connect to ClubReady through MCP?**

Yes, through this unofficial MCP server. ClubReady publishes no MCP server of its own.

**How do I get a ClubReady API key?**

ClubReady gives API access to partners a club authorizes, and the Add Lead API only adds prospects. If you want your own API without a partner, build it with Cotlet Custom API (see above).

**Can I connect ClubReady to Zapier?**

ClubReady's help center sets up Zapier through an API authorization form, and ClubReady has no app of its own in the Zapier directory. With Cotlet you can call your own ClubReady endpoint from Zapier, Make or n8n instead.

**How do I get ClubReady data into Google Sheets every day?**

Build a `checkins` or `past-due-members` endpoint once, then call it from Google Sheets or n8n on a schedule. Or ask your AI to download the report as a spreadsheet each week.

**Can AI follow up leads who did not book a trial?**

Yes. Your AI lists the new prospects without a trial, drafts a text for each, and sends nothing until you say yes.

**Can an AI agent build my past-due list and text those members?**

Yes. It builds the list from Manage Past Dues and drafts the messages. Sending waits for your OK, and payments stay with you.

**Does ClubReady work with QuickBooks?**

ClubReady's help center says it does not integrate with QuickBooks. Your AI can download the remit and sales reports you reconcile by hand.

**Is Cotlet affiliated with ClubReady?**

No. Cotlet is independent and not affiliated with ClubReady.

## Find it on

- [ClubReady MCP server on Cotlet.dev](https://cotlet.dev/integrations/clubready-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=clubready&utm_content=find)
- [ClubReady API with Cotlet](https://cotlet.dev/integrations/api/clubready?utm_source=github&utm_medium=readme&utm_campaign=clubready&utm_content=find)
- [Does ClubReady have an API or MCP server?](https://cotlet.dev/api-status/clubready?utm_source=github&utm_medium=readme&utm_campaign=clubready&utm_content=find)
- [PyPI package clubready-mcp](https://pypi.org/project/clubready-mcp/)
- [Odel](https://odel.app/modules/cotlet/clubready)

## More gym and studio apps on Cotlet

- [Pike13 MCP](https://github.com/cotlet-connect/pike13-mcp)
- [Club OS MCP](https://github.com/cotlet-connect/club-os-mcp)
- [Gymdesk MCP](https://github.com/cotlet-connect/gymdesk-mcp)
- [Exercise.com MCP](https://github.com/cotlet-connect/exercise-com-mcp)
- [Eversports Studio Manager MCP](https://github.com/cotlet-connect/eversports-studio-manager-mcp)
- [DaySmart MCP](https://github.com/cotlet-connect/daysmart-mcp)
- [Acuity Scheduling MCP](https://github.com/cotlet-connect/acuity-scheduling-mcp)
- [Zenoti MCP](https://github.com/cotlet-connect/zenoti-mcp)
- [PushPress MCP](https://github.com/cotlet-connect/pushpress-mcp)
- [Trainerize MCP](https://github.com/cotlet-connect/trainerize-mcp)
- [TrueCoach MCP](https://github.com/cotlet-connect/truecoach-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Jane App MCP](https://github.com/cotlet-connect/jane-app-mcp)
