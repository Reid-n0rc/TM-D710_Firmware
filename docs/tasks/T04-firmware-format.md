# T04 — Updater packaging and image encoding

**Status:** todo · **Depends on:** T02 T03

## Goal
Get the raw firmware images out of the updater(s) without running them, and document how they are packaged.

## Deliverables
- `docs/firmware-format.md`.
- `tools/<name>_extract.py` that writes each image to `build/`, with a round-trip or checksum test.

## Notes / hints
- Look for embedded resources, S-records/Intel HEX, or an obfuscated blob in the `.exe`; the FT-710's byte-wise key and salted CRC (FT-710 `docs/sfl-format.md`) are an example of what to try, not an assumption.
- The updater's serial protocol to the radio (bootloader commands) is also worth recording here, read-only.

## Plan
_(fill in before starting)_

## Findings
_(fill in)_

## Follow-ups
_(fill in)_
