# n8n-nodes-memu

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This is an n8n community node package that integrates [MemU's](https://memu.so) agentic memory capabilities into n8n workflows. MemU is an agentic memory framework that processes multimodal inputs (conversations, documents, images, videos, audio) into structured, retrievable memory for your AI agents.

## Table of Contents

- [Installation](#installation)
- [Nodes](#nodes)
- [Credentials](#credentials)
- [Configuration Examples](#configuration-examples)
- [Workflow Integration Patterns](#workflow-integration-patterns)
- [Resources](#resources)
- [License](#license)

## Installation

Since this package is built for n8n, you can install it for local development or testing directly from GitHub.

### 1. Install n8n and the MemU node package
Use `pnpm` to install n8n and this package into your local project:

```bash
pnpm install n8n
pnpm install github:username/repo-name
```

### 2. Configure n8n to load the custom node
To run n8n and make it recognize the MemU nodes, you need to set the `N8N_CUSTOM_EXTENSIONS` environment variable. 

Add a start script to your `package.json`:

```json
"scripts": {
  "start": "N8N_CUSTOM_EXTENSIONS=./node_modules/n8n-package/n8n-nodes-memu npx n8n start"
}
```

Then run:
```bash
pnpm start
```

## Nodes

This package provides nodes to bridge the gap between your data and AI memory:

### 🧠 MemU Memorize
Register a memorization task to extract and store memories from a conversation

*   **Modality**: Supports `conversation`
*   **User Scoping**: Crucial for multi-tenant agents. Attach a `user_id` so the memory remains private to that user.

### 🔍 MemU Retrieve
Retrieve memories using semantic search. This endpoint uses embedding-based similarity search to find relevant memories based on your query.

### 📋 MemU List Categories
Retrieve all memory categories for a specific user and agent combination.

## Credentials

To use these nodes, you need a **MemU Cloud API** credential.

1.  **Base URL**: Use `https://api.memu.so` (default).
2.  **API Key**: Your secret key from the MemU dashboard.

## Configuration Examples

### Use Case: Building a Research Assistant
Instead of manually reading 10 PDFs, use the **Memorize** node to process them into your agent's brain.

**Memorize Configuration:**
```json
{
  "resourceSource": "manual",
  "resourceUrl": "https://example.com/research-paper.pdf",
  "modality": "document",
  "userScoping": {
    "user_id": "researcher_01"
  }
}
```

**Retrieve Configuration:**
When the researcher asks a question later, use the **Retrieve** node:
```json
{
  "queryText": "What were the main conclusions of the climate study?",
  "method": "rag",
  "filters": { "user_id": "researcher_01" }
}
```

## Workflow Integration Patterns

### Auto-Updating Knowledge Base
Connect a **Google Drive Trigger** to a **MemU Memorize** node. Every time you drop a new file in your drive, your n8n agent automatically "reads" it and updates its knowledge.

### Persistent Chat Experience
1.  **Webhook** receives a chat message.
2.  **MemU Retrieve** fetches what the user said in previous conversations.
3.  **AI Node** generates a personalized response using that context.
4.  **MemU Memorize** (Modality: Conversation) stores the new interaction so it's never forgotten.

## Resources

- [MemU Official Website](https://memu.so)
- [n8n Community Nodes Documentation](https://docs.n8n.io/integrations/community-nodes/)
- [Report Issues](https://github.com/NevaMind-AI/memU-n8n-plugin/issues)

## License

[MIT](LICENSE) © MemU Team
