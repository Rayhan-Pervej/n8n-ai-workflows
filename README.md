# n8n AI Workflows

Two n8n automations that put an LLM between a business tool and WhatsApp. Both run on self-hosted n8n and use Claude on Amazon Bedrock.

| Workflow | What it does |
|---|---|
| [Fathom Meeting Summary to WhatsApp](fathom-meeting-summary-whatsapp/) | When a Fathom meeting ends, turns the transcript into formatted minutes, renders them as a PDF and sends it to a WhatsApp chat. |
| [WhatsApp Task Creation](whatsapp-task-creation/) | A team lead writes a task in a WhatsApp group and mentions the assignees. The task is created in the project management tool and each assignee gets a direct message with the link. |

## Stack

- [n8n](https://n8n.io) (self-hosted, Docker)
- Claude on Amazon Bedrock, through n8n's LangChain nodes
- [Evolution API](https://github.com/EvolutionAPI/evolution-api) for sending and receiving WhatsApp messages
- Community nodes: `n8n-nodes-globals`, `@custom-js/n8n-nodes-pdf-toolkit`

## Setup

1. Copy `.env.example` to `.env` and fill in the values.
2. Start n8n:

   ```bash
   docker compose up -d
   ```

3. Open http://localhost:5678. Under **Settings → Community nodes**, install `n8n-nodes-globals` and `@custom-js/n8n-nodes-pdf-toolkit`.
4. Import a `workflow.json` (**Workflows → Import from file**).
5. Open each node marked with a credential warning and select your own credential. The files contain placeholder credential IDs only.
6. Follow the workflow's own README for the remaining configuration, then activate it.

The workflows read the Evolution API settings with `$env`, so n8n must run with `N8N_BLOCK_ENV_ACCESS_IN_NODE=false`. The included `docker-compose.yml` sets this.

## Environment variables

| Variable | Used by | Purpose |
|---|---|---|
| `EVOLUTION_URL` | both | Base URL of your Evolution API server |
| `EVOLUTION_API_KEY` | both | Evolution API key |
| `EVOLUTION_INSTANCE` | both | Name of the Evolution API instance that sends messages |
| `WHATSAPP_RECIPIENT` | meeting summary | Chat that receives the PDF: a phone number, group ID or LID |

AWS access for Bedrock is stored as an n8n credential, not as an environment variable.
