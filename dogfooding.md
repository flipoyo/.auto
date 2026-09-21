# dogfooding — ComplexGitSync manages itself as a multi-repo tree

This is inherently self-referential and will go stale the fastest of any
skill here — it describes the very tree this split is reorganising.
Whoever dispatches this skill should re-derive it from the live tree at
dispatch time rather than trust this copy, made 2026-09-21, mid-split.

## Two specs, two purposes

- `install.cgs` — the **user** install: ComplexGitSync and its
  documentation, and nothing that configures how the project is
  developed. Mounts no private repository.
- `examples/complexgitsync4dev.cgs` — the **developer** install: the
  same two repositories plus every agentic mount (today: `.agentSpec`,
  `.localSpec`, `.claude`, `.memory`; once `AgentSkillsSplit` dispatches,
  the skill repositories themselves). This is what makes ComplexGitSync
  manage its own working tree, and what CI dogfoods.

The two files are not duplicates — one installs the tool, the other
installs the workshop. Only `install.cgs` sits at the repository root:
where a `.cgs` lives never affects the tree it describes, since the root
is CGSHOME resolved from `--output-path`/`$CGSHOME` plus `project.name`.

## The mount layout, as of `AgentMountSplit` (2026-09-21)

`CLAUDE.md` and `AGENT.md` at the tree root are symbolic links into
`.agent/.local/.claude/`, tracked as links, not as the files they point
to. **`.agent/` is never itself declared in any `.cgs`, and is never a
repository** — a plain directory that `.agentSpec` (at `.distant/`) and
`.localSpec`/`.claude` (at `.local/`) happen to nest their own
`relative_path` inside. An earlier design (`ProjectSpecSplit` WP2)
mounted `.agent` as a real repository holding an `agent-mount.cgs`; that
broke, because nesting a private/local repository (one this project
writes to) under a private/distant one (shared, read-only) caps the
nested one's writability at the parent's, with no override. Declaring
each repository directly, answering only to its own
`private`/`writable` flags, has nothing left for that cap to reach
through — see `AgentMountSplit` for the reproduction and the fix.

`.agent/.local/.localSpec/DevTickets/` is **the planning surface, and it
is private** — the public repository holds no tickets at all. Four
things live there: `README.md` (the short-ticket → planning-ticket →
archive loop, in full), `shortTickets/`, `openTickets/` (named
`<branch>_<priority>-<rank>_<Name>_DevPlanTicket.md`), and `archive/`
(stamped `YYYYMMDD_`).

## What changes next, per `AgentSkillsSplit`

The layout above is itself mid-transition. Once `AgentSkillsSplit`
dispatches its mapped skills, `.agentSpec`'s content splits into
`.agent/.distant/{ticket,documentation,dev-sync}` (shared) and
`.localSpec`/`.claude`'s content into
`.agent/.local/{cgitsync-dev,architecture,release,memory,dogfooding}`
(this project's own) — each its own repository, each declared directly,
the same way `AgentMountSplit` already declares `.agentSpec` today. This
file, once that lands, describes whichever of those actually exist —
update it then, don't leave it describing the flat `.localSpec`/`.claude`
shape after it is gone.
