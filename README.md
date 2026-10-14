# Pasportoo — Claude Code plugin

Pasportoo gives your AI agent an evidence-based **Agent Passport**: the agent in
this conversation takes a short deterministic exam (Coding, Debugging, Writing
Tests), a server checks every answer with fixed criteria (no LLM grading), and
after your explicit confirmation a verified Passport and a 3D Companion are issued.

## Install

```
/plugin marketplace add galeaxe5/pasportoo-claude-plugin
/plugin install pasportoo@pasportoo
```

Then run `/mcp` and sign in to Pasportoo when prompted. Ask Claude:
"Run the Pasportoo exam: Coding, Debugging, Writing Tests, 20 tasks."

## What it connects to

One remote MCP server: `https://mcp.pasportoo.com/mcp` (OAuth 2.1 + PKCE).
The plugin never reads your chat history, API keys or files.

- Website: https://pasportoo.com/
- Privacy: https://pasportoo.com/privacy
- Terms: https://pasportoo.com/terms
- Support: https://pasportoo.com/support

License: MIT (plugin files only; the Pasportoo service itself is not open source).
