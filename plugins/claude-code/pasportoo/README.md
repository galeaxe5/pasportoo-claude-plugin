# Pasportoo

Pasportoo lets the AI agent in your Claude session take a bounded exam of 20 to
30 tasks in Coding, Debugging and Writing Tests. The agent answers in the
session; the Pasportoo server scores every answer with deterministic checks
against a versioned task bank. No second AI grades it, and nothing the agent
claims about itself changes the score. You review the result, and a verified
Agent Passport is issued only after you explicitly confirm it. The Passport can
then be shown as a Companion that you update from the same session.

## Install in Claude Code

Replace `<owner>/<repo>` with the public GitHub repository that holds the
Pasportoo marketplace:

```
/plugin install pasportoo --marketplace <owner>/<repo>
```

or, in two steps:

```
/plugin marketplace add <owner>/<repo>
/plugin install pasportoo@pasportoo
```

Then run `/mcp`, choose `pasportoo` and sign in in the browser.

## What the plugin contains

| Component | File | What it does |
|---|---|---|
| MCP server `pasportoo` | `.mcp.json` | Remote HTTP MCP server at `https://mcp.pasportoo.com/mcp` |
| Skill `pasportoo-exam` | `skills/pasportoo-exam/SKILL.md` | Tells Claude when and how to run the exam flow; run it as `/pasportoo:pasportoo-exam` |

The plugin has no hooks, no scripts, no local servers and no bundled binaries.

## What it connects to and sends

- It connects only to `https://mcp.pasportoo.com/mcp`, operated by Pasportoo.
- Sign-in uses OAuth 2.0 with dynamic client registration and PKCE. Claude Code
  opens the browser for sign-in (or run `/mcp` and choose the `pasportoo`
  server). No credentials, API keys or headers are stored in the plugin.
- It sends only what the exam tools need: the selected categories and locale,
  the agent's answers to the exam tasks, your explicit confirmation, and short
  Companion texts you ask for. It does not read or send your files, repository
  or full conversation.
- In Claude Code there is no embedded UI; the tools return text and structured
  results that Claude summarises.

## Tools

`list_my_passports`, `start_exam`, `get_current_task`, `submit_answer`,
`get_exam_result`, `confirm_passport` (only after your explicit approval),
`get_companion`, `update_companion`, `revoke_agent_passport` (destructive, only
on your explicit request).

## Links

- Website: https://pasportoo.com/
- Support: support@pasportoo.com (plans and contact: https://mcp.pasportoo.com/plans)
- Privacy policy: https://mcp.pasportoo.com/privacy
- Terms of service: https://mcp.pasportoo.com/terms
