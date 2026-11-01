# Street Fighter II: The World Warrior — recompilation project

Development project targeting Capcom CPS1, Windows x86-64 and Linux x86-64 / Steam Deck AppImage.

## Status

Planning and upstream capability audit only. **There is no playable port or downloadable build yet.**

Audited arcade-decomp commit: `58f904c1d96940561702805e18ac1386682281fd`.

[arcade-decomp](https://github.com/cheviman/arcade-decomp) provides shared analysis and decompilation infrastructure, but this revision does not include a CPS1 board runtime. Its `m68000/` directory contains a decoder/disassembler and static analysis, with no 68000 instruction lifter. Its documented board integrations are System 32 / Multi 32 and Konami boards. Sharing a CPU with a supported board does not supply CPS1 video, audio, memory mapping or timing.

A native SF2 port needs a CPS1 runtime, the missing 68000 translation path, game-specific integration and comparison against a reference before packaging is meaningful.

## Local development

The owner's input ROM archive is on their Windows computer:

```text
C:\Users\omega\Downloads\sf2.zip
```

That path is not accessible from GitHub Actions or a remote development session. Validate archive members, sizes and hashes locally against a pinned authoritative ROM-set manifest; the filename alone does not identify its revision or completeness. Do not download replacement ROMs.

See [OPENCODE_TASK.md](OPENCODE_TASK.md) for the local implementation brief.

## Intended deliverables

- Windows x86-64 portable ZIP containing the executable and required redistributable libraries.
- Linux x86-64 AppImage tested on Steam Deck in Desktop Mode and Gaming Mode.
- Keyboard and two-player controller controls, including all six attack buttons.
- Runtime loading of user-supplied game data with clear errors for missing or incorrect files.
- Reproducible build and packaging scripts, dependency notices and checksums.

These are targets, not implemented features. Release packages must exclude ROMs, extracted game assets and generated files embedding copyrighted game bytes. If translation output requires such bytes, keep generation local until a redistributable architecture is established.

## Acceptance

Before a release: verify boot, attract mode, character select, complete matches, two-player input, graphics, audio, timing and repeated launch. Compare reference and native execution through deterministic scenarios. Then test Windows on a clean machine and the AppImage on an actual Steam Deck; record tested versions and limitations.

## License

The existing repository license is GPL-2.0; it has not been changed. Preserve upstream BSD notices and review each dependency's license before distribution. Game names are used only to identify the compatibility target; no affiliation with Capcom is claimed.
