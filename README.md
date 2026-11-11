# Kora FiveM Agent Skill

**Version:** 1.0.0

This repository packages the `kora-fivem-docs` Agent Skill for Codex, Claude,
and Grok coding agents. It teaches agents how to use the current Kora FiveM
documentation when creating resources or adapting existing resources for Kora.

## Install

Copy the complete `skills/kora-fivem-docs` folder, including its `references`
directory, to the skill directory recognized by your agent. Typical user-level
locations are:

| Agent | Skill location |
| --- | --- |
| Codex | `~/.codex/skills/kora-fivem-docs` |
| Claude Code | `~/.claude/skills/kora-fivem-docs` |
| Grok Build | `~/.grok/skills/kora-fivem-docs` |

Project-level skill directories can also be used where supported. Check the
agent's current documentation for its preferred location, then restart or reload
the agent if needed.

## Documentation source

The shared guides and resource catalog live in
[`KoraFiveM/kora_docs_next`](https://github.com/KoraFiveM/kora_docs_next).
Resource documentation is owned by each resource's repository. The skill uses
the catalog to locate that repository and fetches only pages relevant to the
current task. It does not embed a full copy of the docs site.

See [`skills/kora-fivem-docs/references/docs-source.md`](skills/kora-fivem-docs/references/docs-source.md)
for the repository layout and retrieval workflow.
