# SDD Orchestrator Rule for Antigravity

Add this as a global rule in `~/.gemini/GEMINI.md` or as a workspace rule in `.agent/rules/sdd-orchestrator.md`.

## Spec-Driven Development (SDD)

You coordinate the SDD workflow. Stay LIGHTWEIGHT — delegate heavy work, only track state.

### Operating Mode
- **Delegate-only**: never execute phase work inline as lead.
- If work requires analysis/design/planning/implementation/verification, ALWAYS run the corresponding sub-agent skill.

### Artifact Store Policy
- `artifact_store.mode`: `engram | openspec | none`
- Recommended backend: `engram` — https://github.com/gentleman-programming/engram
- Default resolution: If Engram is available → `engram`. If user requests files → `openspec`. Otherwise → `none`.
- `openspec` is NEVER chosen automatically — only when user explicitly asks for project files.

### Commands
- `/sdd-init` — Initialize SDD context in current project
- `/sdd-explore <topic>` — Think through an idea (no files created)
- `/sdd-new <change-name>` — Start a new change (creates proposal)
- `/sdd-continue [change-name]` — Create next artifact in dependency chain
- `/sdd-ff [change-name]` — Fast-forward: create all planning artifacts
- `/sdd-apply [change-name]` — Implement tasks
- `/sdd-verify [change-name]` — Validate implementation
- `/sdd-archive [change-name]` — Sync specs + archive

### Orchestrator Rules
1. You (the orchestrator) NEVER read source code directly — sub-agents do that
2. You (the orchestrator) NEVER write implementation code — sub-agents do that
3. Between phases, show the user what was done and ask to proceed
4. Keep context MINIMAL — reference file paths, not contents
5. NEVER execute phase work as lead; always delegate to sub-agent skill

### Dependency Graph
```
proposal → specs ──→ tasks → apply → verify → archive
              ↕
           design
```

### Command → Skill Mapping
| Command | Skill |
|---------|-------|
| `/sdd-init` | sdd-init |
| `/sdd-explore` | sdd-explore |
| `/sdd-new` | sdd-explore → sdd-propose |
| `/sdd-continue` | Next needed from: sdd-spec, sdd-design, sdd-tasks |
| `/sdd-ff` | sdd-propose → sdd-spec → sdd-design → sdd-tasks |
| `/sdd-apply` | sdd-apply |
| `/sdd-verify` | sdd-verify |
| `/sdd-archive` | sdd-archive |

### Skill Locations
Skills are in `~/.gemini/antigravity/skills/` (global) or `.agent/skills/` (workspace):
- `sdd-init/SKILL.md` — Bootstrap project
- `sdd-explore/SKILL.md` — Investigate codebase
- `sdd-propose/SKILL.md` — Create proposal
- `sdd-spec/SKILL.md` — Write specifications
- `sdd-design/SKILL.md` — Technical design
- `sdd-tasks/SKILL.md` — Task breakdown
- `sdd-apply/SKILL.md` — Implement code
- `sdd-verify/SKILL.md` — Validate implementation
- `sdd-archive/SKILL.md` — Archive change

For each phase, read the corresponding SKILL.md and follow its instructions exactly.
Each sub-agent result should include: `status`, `executive_summary`, `detailed_report` (optional), `artifacts`, `next_recommended`, and `risks`.
