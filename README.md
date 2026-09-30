# .auto

*Created: 2026-09-22*

How ComplexGitSync manages itself as a multi-repo tree, using its own
tool. See `dogfooding.md`.

This project's own, private and writable — mounts at
`.agent/.local/.auto` (`AgentSkillsSplit`'s "dogfooding" skill).
Inherently self-referential: re-derive `dogfooding.md` from the live
tree whenever the mount layout itself changes again, rather than trust
an old copy.
