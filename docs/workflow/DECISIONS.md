# Architecture Decisions

Append new decisions below. Do not rewrite history; mark old decisions as superseded when needed.

---

## ADR-000: Adopt phase-gated delivery as the execution contract
- **Date:** 2026-02-17
- **Phase:** 1 (Problem Framing and Contract)
- **Status:** Accepted
- **Context:** Agentic sessions are fast but can drift without explicit scope, gates, and decision records.
- **Decision:** Use `PLAN.md`, `GATES.md`, and `DECISIONS.md` as mandatory workflow artifacts.
- **Rationale:** Keeps execution aligned across human and agent collaborators with low ceremony.
- **Consequences:** Teams must maintain phase checklists, gate status, and ADR updates before implementation changes.

---

## ADR-001: Inject workflow status via a Claude Code SessionStart hook
- **Date:** 2026-10-16
- **Phase:** 1 (Problem Framing and Contract)
- **Status:** Accepted
- **Context:** The Session Start step "run `workflow.sh status` and `prompt`" relied on the agent remembering to do it, and phase state was lost from context after compaction.
- **Decision:** Commit `.claude/settings.json` with a `SessionStart` hook (matchers `startup|resume|clear|compact`) that runs both commands, plus an allow rule for `./scripts/workflow.sh *`. Keep the written instruction in `AGENTS.md` for tools without hooks (e.g. Codex). Keep `CLAUDE.md` as a one-line `@AGENTS.md` import even though Claude Code v2.1.277+ reads `AGENTS.md` natively: native loading is skipped whenever any `CLAUDE.md`/`CLAUDE.local.md` exists in the working directory or an ancestor, and the import works on every version and provider without double-loading.
- **Rationale:** Deterministic context injection beats prose instructions; re-injecting on `compact` keeps phase discipline in long sessions.
- **Consequences:** Claude Code users get workflow status automatically; hook failures are non-blocking. The hook is Claude-specific, so `AGENTS.md` remains the canonical contract.

---

<!-- TEMPLATE
## ADR-NNN: [Title]
- **Date:** YYYY-MM-DD
- **Phase:** [Phase number and name]
- **Status:** Accepted | Superseded by ADR-NNN
- **Context:** [Problem statement]
- **Decision:** [Chosen approach]
- **Rationale:** [Why this option]
- **Consequences:** [Ongoing impact]
-->
