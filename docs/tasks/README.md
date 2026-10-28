# Task board

Mirrored to GitHub issues (once `origin` exists) by `tools/issues_sync.py`. Update the status here when you start or finish a task.

| ID | Task | Depends on | Status |
|---|---|---|---|
| T01 | [Tooling: verify Ghidra, JDK 21, unpack tools](T01-tooling.md) | — | todo |
| T02 | [Sources: firmware updaters and manuals](T02-sources.md) | — | todo |
| T03 | [Hardware survey from the service manual](T03-hardware-survey.md) | T02 | todo |
| T04 | [Updater packaging and image encoding](T04-firmware-format.md) | T02 T03 | todo |
| T05 | [Decompile each image (split per image after T04)](T05-decomp.md) | T01 T04 | todo |
| T06 | [Document the code of each image (split per image)](T06-docs-code.md) | T05 | todo |
| T07 | [PC-control command table recovery](T07-pc-commands.md) | T05 | todo |
| T08 | [System-level documentation and figures](T08-system.md) | T06 T07 | todo |
