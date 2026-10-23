MCP is an **open standard** introduced by Anthropic in November 2024 to standardize how AI systems like LLMs integrate and share data with external tools, systems, and data sources.
Think of it as the **"USB-C for AI"** — one universal connector instead of custom integrations everywhere.
---
### ==The Problem It Solves==
- Without MCP: Every AI tool needs a custom integration with every service → **N×M** integrations
- With MCP: GitHub builds one server, Google Drive builds one server, Slack builds one server → complexity drops from **N×M to N+M**
### Key Components
|Component|What It Does|
|---|---|
|**MCP Host**|The AI app (e.g., Claude, ChatGPT, Cursor)|
|**MCP Client**|Sits inside the host, manages connections|
|**MCP Server**|External service exposing its data/tools|
|**Tools**|Actions the AI can invoke (e.g., create GitHub issue)|
|**Resources**|Structured data the AI can read (e.g., files, DB entries)|
|**Prompts**|Predefined templates to guide AI behaviour|
---
### Adoption (as of 2025–26)
97 million monthly SDK downloads, 10,000+ active servers, and first-class support in Claude, ChatGPT, Cursor, Gemini, Microsoft Copilot, and VS Code.
In December 2025, Anthropic donated MCP to the Agentic AI Foundation (AAIF) under the Linux Foundation, with OpenAI, AWS, Google, Microsoft, Cloudflare, and Bloomberg as members.
---
### Risks to Know
- **Security:** Known attack vectors include tool poisoning (malicious tool descriptions), silent/mutated definitions, and cross-server tool shadowing.
- **Fragmentation:** Risk of proprietary extensions breaking interoperability
- **Compliance:** Regulatory frameworks haven't caught up yet