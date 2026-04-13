# Agent Registry

All agents available in this vault. The system automatically discovers agents in `agents/base/` and `agents/custom/`.

## How agents work

An agent defines a **perspective** — how the LLM positions itself when executing a command. Commands declare which agent to activate. The agent is not a separate process; it is a set of behavioral instructions the LLM follows for the duration of the command.

## Base Agents

| Agent | File | Activated by | Role |
|---|---|---|---|
| librarian | `agents/base/librarian.md` | ingest, propagate, connect, init, lint | Graph keeper — maintains structure and connections |
| analyst | `agents/base/analyst.md` | query, trace, impact, prd-check, ost, explore | Product thinker — reasons across the connection graph |

## Custom Agents

Place custom agent files in `agents/custom/`. They are automatically available to all commands.
See `agents/_agent-template.md` for the required format.

| Agent | File | Activated by | Role |
|---|---|---|---|
| *(none yet)* | | | |
