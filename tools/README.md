# tools/

Every script here and in `tools/ghidra/`: what it does, inputs → outputs, the task that made it, and where it came from. Python tools are
stdlib only; run them with `python3 -I`. Outputs derived from Kenwood's firmware go to `build/` and are never committed.

| Script | What it does | Inputs → outputs | Task | Origin |
|---|---|---|---|---|
| `ghidra-headless.sh` | Runs Ghidra's `analyzeHeadless` with JDK 21; override with `GHIDRA_JAVA_HOME`, `GHIDRA_HEADLESS` | Ghidra args | T01 | FT-710 |
| `issues_sync.py` | Mirrors `docs/tasks/` to GitHub issues and keeps `docs/tasks/issues.json`. **The parent agent / owner runs it, nobody else**. Refuses to run outside `Reid-n0rc/TM-D710_Firmware` | `docs/tasks/` → GitHub | (board setup) | FT-710 |
| `ghidra/ExportDecomp.java` | Decompiled C per function into `decomp/<image>/*.c` + `functions.tsv` | Ghidra program → `decomp/<image>/` | T05 | FT-710 |
| `ghidra/ApplyNames.java` | Applies names/plate comments from `address<TAB>name[<TAB>comment]` | `tools/ghidra/names_<image>.tsv` → Ghidra | T05 | FT-710 |
