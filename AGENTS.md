# AGENTS.md — TM-D710 Firmware Analysis

Instructions for any agent (Claude, Codex, etc.) working in this repo. `CLAUDE.md` points here.

This project follows the method of the owner's FT-710 analysis, [Reid-n0rc/FT-710_Firmware](https://github.com/Reid-n0rc/FT-710_Firmware):
its rules, tools and documentation standard are the template. The Kenwood TM-D710 is a different maker's radio (an older dual-band
VHF/UHF FM mobile with a built-in TNC/APRS), so **none of the FT-710 findings are assumed to carry over**; reuse only the method.

## Goals (in order)

1. Understand how the owner's Kenwood TM-D710 works internally (operation panel, body, TNC).
2. Find undocumented PC-control commands and settings.
3. Eventually build custom firmware that extends the radio. When that starts, it gets its own companion repository; facts about the
   stock firmware stay here.

The first milestone is decoding every firmware image and committing Ghidra's decompiled output to `decomp/`.

## Ground rules

- **Analyze and plan before acting.** Before you change anything, read the task file, check what the previous tasks produced, and write your approach into the task file's `## Plan` section.
- **Tasks and sub-tasks.** Plan first: write the approach into the task file before executing. Split any task that won't fit in one context window into sub-task files (`T05a`, `T05b`, ...), and add each one as a row on the board so it is tracked as a GitHub sub-issue. Finish, document and commit each (sub-)task before starting the next.
- **Record findings as you go.** Facts go in `docs/`. Status goes in the task file. Never leave a finding only in chat.
- **Repo visibility: not yet decided.** The owner decides before `origin` is created. Never commit raw firmware, updater executables, decoded binaries, manuals or Ghidra projects (`Firmware/`, `build/`, `Manuals/`, `ghidra/` are gitignored). Never publish anything from this project anywhere the owner hasn't approved.
- **Git:** make incremental commits with clear messages. Pushing needs the owner's approval until `origin` exists and the owner records the rule here. Never tag, release, or change visibility.
- **Contributions.** Once on GitHub, anyone other than the repository owner or an admin starts every change with an issue and gets it in through a pull request that links it (`Closes #N`); see [CONTRIBUTING.md](CONTRIBUTING.md). Agents running with the owner's credentials act as the owner.
- **Don't touch the radio.** Anything that writes to the radio (firmware updates, memory/menu writes, untested PC-control set-commands) needs the owner's explicit approval each time. Kenwood updaters run on Windows and write over the PC port: never run one against a connected radio.

## Sources (not in git)

Fill this table in during T02 and record the SHA-256 of every input in `docs/inputs.sha256`.

| Folder | Contents | Where to get it |
|---|---|---|
| `Firmware/` | Kenwood firmware updater(s) for the TM-D710 (A/E variants; the panel and the body may have separate updaters) **[guess]** | kenwood.com support / download pages (T02) |
| `Manuals/` | Instruction manual, in-depth manual, PC-control/command references (Kenwood or community), service manual | Kenwood; service manuals from the usual archives (T02) |

## Repo layout

```
AGENTS.md            this file
docs/
  hardware.md        CPUs, flash, TNC, buses per board                    (T03)
  firmware-format.md updater packaging + image encoding                   (T04)
  memory-maps.md     load addresses / memory maps per image               (T05)
  pc-commands.md     documented vs firmware-discovered PC commands        (T07)
  manuals.md         every manual/datasheet used, with source links
  tooling.md         installed tools and versions
  inputs.sha256      SHA-256 of the downloads and decoded images
  tasks/             one file per task; README.md is the index/status board
tools/               scripts (Python 3, stdlib preferred), catalogued in tools/README.md
decomp/<image>/      committed Ghidra decompiled C output per image
build/               (ignored) extracted/decoded images
ghidra/              (ignored) Ghidra projects TMD710_<IMAGE>
```

## Firmware images

Unknown until T02/T04. Fill in: file name, version, date, target (panel / body / TNC), CPU and role.

## Tooling

- Ghidra 12.1.4 (Homebrew) + `openjdk@21`. Always run headless through `tools/ghidra-headless.sh`. Put projects under `ghidra/`, named `TMD710_<IMAGE>`. Details in `docs/tooling.md`.
- Python 3 for the decoding tools. Run scripts that read firmware data with `python3 -I`. Analyze updater `.exe` files statically; never execute them.
- The owner authorizes installing community tooling when a task needs it. Prefer reputable sources, avoid sudo and system-wide default changes, and record each install in `docs/tooling.md`.

## Task sizing

- One task has one clear deliverable that you can verify (a doc section, a script plus a test, or one image's decompile export).
- Never read a whole PDF or a whole decompile dump into context. Use `pdftotext` with page ranges, `grep`, and targeted reads.
- Process one firmware image per task. Finish each task with: the task file updated, docs updated, and a local commit.

## Documentation standard (owner requirement)

Full documentation of all the code, with figures. Raw decomp output alone is not a deliverable.

- Code documentation lives in `docs/code/<image>/` plus `docs/code/system.md`. Each image has a `README.md` overview (purpose, boot flow, memory map, main-loop structure, subsystem list) and one page per subsystem with function tables (address, Ghidra name, inferred name, purpose), data structures and globals.
- **Figures:** Mermaid (`flowchart`, `sequenceDiagram`, `stateDiagram-v2`), about 25 nodes or fewer each. Every overview needs a block diagram and a boot/control-flow figure.
- Mark each claim **[fact]** (seen in code/data, cite the address), **[inference]** or **[guess]**.
- Keep names in `tools/ghidra/names_<image>.tsv` (applied with `ApplyNames.java`) as the source of truth, and re-export `decomp/<image>/` with `ExportDecomp.java`.

## Issue tracking (owner requirement)

Once `origin` exists, every task is mirrored as a GitHub issue: an epic, one issue per task, sub-tasks as sub-issues. `docs/tasks/` stays the source of truth. After any change to the board or a task's status/findings, the parent agent runs `python3 -I tools/issues_sync.py` and commits `docs/tasks/issues.json`.

## Watchdog / long-running work

- Give every long command a hard timeout (`-analysisTimeoutPerFile` for Ghidra; the decompiler has a 60 s per-function limit in `ExportDecomp.java`).
- Background agents append `<ISO time> <task> <step>` to `$HB_DIR/<task>` at least every ~10 minutes; older than 30 minutes counts as stalled.
- Retry failed tools/connections with backoff; record unrecoverable failures in the task file.
