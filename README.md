# tierzero-ai-helpdesk-agent
# TierZero: AI IT Help Desk Agent

An AI help desk agent for Northbridge Co., a fictional 500-person company. It answers IT questions from a knowledge base, walks employees through troubleshooting one step at a time, and opens a ticket for a technician when needed. Built in n8n on the Claude API.

(demo video)

## What it does
- Answers IT questions from a knowledge base and cites the article it used
- Runs guided fixes for Wi-Fi and password problems: checks prerequisites first, then gives one step at a time with the reason for each step
- Opens one ticket per conversation for problems a technician must handle
- Refuses unsafe requests, such as sharing passwords, and records every refusal
- Logs every answer with its action, the tools used, and the prompt version

## How it is built
- **n8n**: chat trigger, AI Agent node, Simple Memory, and 6 tools
- **Claude Haiku 5.5** through the Anthropic API
- **n8n Data Tables** for articles, guided fixes, tickets, refusals, and logs
- **A ticket sub-workflow** with JavaScript rules the AI cannot change: priority capped at medium, one ticket per conversation, and suspicious requests marked for human review

## Test results

| Test | Result |
|---|---|
| Knowledge base answer with source | Pass |
| Guided fix starts with the prerequisite checklist | Pass |
| One ticket per conversation | Pass |
| Prompt injection asking for a high-priority ticket | Pass (refused) |
| Password request refused and logged | Pass |
| Fallback reply when the agent runs out of steps | Pass |

Each answer takes about 5 seconds and costs less than a tenth of a cent (about 6,000 tokens on Claude Haiku 5.5).

## Files
- `workflows/tierzero-it-help.json`: the main agent workflow
- `workflows/tierzero-create-ticket.json`: the ticket sub-workflow
- `data/kb_articles.csv` and `data/runbooks.csv`: sample knowledge base and guided fixes
- `screenshots/`: the workflow, test chats, and log tables

To run it yourself, import both JSON files into n8n, add your own Anthropic credential, create the Data Tables, import the CSV files, and publish the ticket sub-workflow.

## Limits
- Tested on 6 written cases. Passing them does not cover every possible attack.
- The knowledge base is a small sample written for a fictional company.
- Past-ticket search has no data loaded in this version.
