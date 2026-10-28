# TM-D710_Firmware

An analysis of the Kenwood TM-D710 firmware. The goals are to understand the radio, find undocumented PC-control commands, and
eventually build custom firmware.

The method comes from the FT-710 analysis, [Reid-n0rc/FT-710_Firmware](https://github.com/Reid-n0rc/FT-710_Firmware): get the official
update files, extract and decode each image, decompile it in Ghidra, commit the decompiled output, and document the code with figures and
evidence tags.

**Agents and contributors: start with [AGENTS.md](AGENTS.md).** The task board is in [docs/tasks/README.md](docs/tasks/README.md).

## Status

Just started: nothing is decoded yet. Firmware updaters, decoded images, manuals and Ghidra projects are never committed; see
[AGENTS.md](AGENTS.md#sources-not-in-git).
