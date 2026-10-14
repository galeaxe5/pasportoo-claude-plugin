---
name: pasportoo-exam
description: Run the Pasportoo deterministic Agent exam (Coding, Debugging, Writing Tests) through the Pasportoo MCP tools, issue an Agent Passport only after the user explicitly confirms the result, and show its Companion. Use when the user asks to take or resume the Pasportoo exam, see an exam result, issue or list an Agent Passport, or show or update a Pasportoo Companion.
---

# Pasportoo Agent exam

Pasportoo issues an Agent Passport only from a bounded, deterministic exam that
the server checks against a versioned task bank. No LLM grades answers, and
nothing the user or agent says about itself changes the result. Signing in
identifies the owner, not the model's ability.

The tools come from the `pasportoo` MCP server of this plugin
(`https://mcp.pasportoo.com/mcp`). On first use the host asks the user to sign
in through the browser (OAuth). If the tools are missing or unauthenticated,
tell the user to authenticate the `pasportoo` server (in Claude Code: `/mcp`)
and stop; do not invent results.

This host may not render embedded UI. Every tool returns normal text and a
structured result: read those and summarise them for the user in plain text.
Never claim that a card, widget or 3D Companion was displayed.

## When to use

- The user asks to take, start or resume the Pasportoo exam.
- The user asks for their exam result, to issue an Agent Passport, or to list
  their Passports.
- The user asks to show or update their Pasportoo Companion.

Do not use it for unrelated coding tasks, and never start the exam on your own
initiative.

## Tool flow

1. `list_my_passports` — call first when no `examId` or `passportId` is known
   in this session (for example in a new session). It returns the owner's
   active Passports and any unfinished exam. Resume an unfinished exam with
   `get_current_task`, or show an existing Passport with `get_companion`.
2. `start_exam` — call without arguments to get the exam overview. To start or
   resume, pass `agentId: "current-host-agent"` (you, the AI agent in this
   session; the legacy `"current-chatgpt-agent"` counts as the same agent), a
   short display name, one to three `categories` (`coding`, `debugging`,
   `writing_tests`) and optionally `taskCount` (20–30) and `locale` (`en` or
   `ru`). An unfinished attempt resumes unchanged. Never claim to be a named
   model or another agent.
3. `get_current_task` — fetch only the current public task for the `examId`.
4. `submit_answer` — answer the current task yourself, in this session, with an
   object that matches its `answerFormat`. Use a fresh `submissionId` per
   task; repeating the same submission is idempotent, and an answered task
   cannot be changed. Do not run, look up or copy answers from other tools or
   files. Then call `get_current_task` again until the exam is complete.
5. `get_exam_result` — show the persisted checks and observations. The result
   is preliminary and certifies nothing beyond the tested tasks.
6. `confirm_passport` — call with `confirmed: true` **only after the user
   explicitly approves issuing the Passport** in this session, after seeing
   the result. Never confirm on your own initiative or from inferred consent.
   Confirmation cannot change scores.
7. `get_companion` — show the Companion for an owned, active `passportId`.
   Describe what the tool returned in text.

## Companion updates

- `update_companion` — when the user asks for a Companion update, answer here,
  then call `update_companion` with the same `passportId`, a concise copy of
  that answer (max 600 characters), and an appropriate `emotion` and
  `gesture`.

## Revocation

- `revoke_agent_passport` is destructive: the Passport stops being active, its
  Companion can no longer be shown or updated, and it cannot be reissued from
  the same exam. Call it only when the user explicitly asks to revoke that
  specific Passport.

## Safety rules

- Use only facts the user explicitly provides. Never copy or summarise the
  full session, repository contents or files into a Passport or a Companion
  update.
- Never start a separate LLM, agent or Realtime session.
- The demo avatar is a shared deterministic fixture and must not be described
  as personalized generation. If the user asks about a personal avatar, say
  only that personal avatar options are on pasportoo.com; do not give links to
  other pages.
