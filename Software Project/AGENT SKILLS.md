It is a standardized way to give AI agents new capabilities and expertise.
Agent Skills are a lightweight, open format for extending AI agent capabilities with specialized knowledge and workflows.
At its core, a skill is a folder containing a `SKILL.md` file. This file includes metadata (name and description, at minimum) and instructions that tell an agent how to perform a specific task. Skills can also bundle scripts, reference materials, templates, and other resources.
```markdown
my-skill/
├── SKILL.md          # Required: metadata + instructions
├── scripts/          # Optional: executable code
├── references/       # Optional: documentation
├── assets/           # Optional: templates, resources
└── ...               # Any additional files or directories
```
# ==**How do Agent Skills work?**==
Agents load skills through **progressive disclosure**, in three stages:
1. **Discovery**: At startup, agents load only the name and description of each available skill, just enough to know when it might be relevant.
2. **Activation**: When a task matches a skill’s description, the agent reads the full `**SKILL.md**` instructions into context.
3. **Execution**: The agent follows the instructions, optionally executing bundled code or loading referenced files as needed.
Full instructions load only when a task calls for them, so agents can keep many skills on hand with only a small context footprint.
# ==Who created it?==
`**agentskills.io**` was created by **Anthropic**.
It was originally launched in late 2025 as a core feature inside their command-line developer agent, **Claude Code**. Recognizing that separating prompt instructions across fragmented file types was a massive pain point for developers, Anthropic designed the format to be completely open-source and vendor-agnostic.
They published the open specification under the `**agentskills.io**` domain to ensure it could be adopted globally. Because it is an open standard, major AI platforms—including **Google Gemini (CLI/Antigravity)**, **OpenAI (Codex)**, **Cursor IDE**, and **GitHub Copilot**—now natively read and support the same `SKILL.md` format without vendor lock-in.
# ==References==
- https://agentskills.io/home