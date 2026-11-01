# OpenCode implementation task

Work in https://github.com/omegakatana92/Street-Fighter-2-The-World-Warrior-Recomp.

Develop a native Street Fighter II: The World Warrior CPS1 port using https://github.com/cheviman/arcade-decomp as the shared analysis/decompilation foundation. Deliver Windows x86-64 and Linux x86-64 / Steam Deck AppImage when the engine works. Carry out implementation, not just a proposal. Never claim a working port based on packaging success alone.

Input archive, available only on the owner's Windows PC:
`C:\Users\omega\Downloads\sf2.zip`

## 1. Inspect and establish a reproducible baseline

- Read repository instructions and preserve existing work and license.
- Clone this repository if needed; use a development branch.
- Inspect upstream before writing integrations. The initial audit used arcade-decomp commit 58f904c1d96940561702805e18ac1386682281fd: no CPS1 runtime and no m68000 lifter. Check whether upstream has since added them.
- Add arcade-decomp as a real git submodule at tools/core, pinned to an inspected commit. Do not create a .gitmodules entry without its gitlink.
- Check archive existence locally. List ZIP members, reject unsafe paths and duplicate members, compute sizes, CRC32 and SHA-1, and identify the exact set revision using an authoritative pinned manifest. Report missing/mismatched members; do not fetch ROMs or assume every sf2.zip is identical.
- Keep ROMs, extraction output, logs containing game data, build directories and private local paths out of tracked output. Use explicit git staging and inspect staged files.

## 2. Implement the missing translation and CPS1 integration

- Inspect upstream APIs and a complete upstream game integration before defining tools/game_profile.py or wrappers. Do not invent unsupported command-line switches.
- Reuse the 68000 decoder and static analysis. Implement missing instruction lifting/translation and execution semantics with meaningful differential tests for arithmetic flags, addressing modes, exceptions, interrupts and indirect control flow.
- Verify the selected set's board configuration and memory map against a pinned primary hardware/reference implementation. Implement program layout/interleaving, work RAM, CPS registers, tile/sprite/palette rendering, priorities, inputs/DIP settings, interrupts and timing.
- Implement its sound-board integration, including Z80 program banking, command handling, YM2151 and OKI sample playback as verified for this set. Audit reusable sound libraries and retain their licenses.
- Build a deterministic headless reference harness, isolating reference CPU emulation from the distributed native game. Reuse upstream verification infrastructure where its contracts apply.
- Do not substitute an emulator executable or a renamed ROM launcher and label it recompilation. A reference-only milestone must be labeled reference-only.
- Reach reset/boot first, then attract mode, character select and playable fights. Record uncovered code paths and unsupported operations explicitly; do not silently return zeros or skip instructions.

## 3. Desktop runtime and game data

- Use a portable frontend such as SDL with keyboard and controller remapping, two players, six attack buttons, coin/start, fullscreen, aspect-correct scaling and clean exit.
- Load user-provided ROM/game data at runtime; support an explicit path and a documented per-user location. Resolve resources relative to the executable or data directory, not the caller's working directory.
- Provide actionable missing-data errors. Do not require placing data inside an AppImage.
- Store settings in appropriate writable per-user directories. Preserve settings across updates.
- Keep copyrighted byte arrays and extracted assets out of public commits, CI artifacts and release packages. If the generation process embeds game bytes, document that blocker and provide a local generation path until redistribution has been resolved.

## 4. Windows build

- Provide a reproducible CMake configuration and PowerShell build/package script for a supported x86-64 compiler toolchain.
- Package the executable, necessary redistributable DLLs, licenses and first-run instructions into a portable ZIP.
- Test launch from Explorer, a directory with spaces, and an unrelated current working directory on a clean Windows machine.
- Do not publish placeholders or claim validation without executing the game.

## 5. Linux and Steam Deck AppImage

- Build with a toolchain/sysroot compatible with the target SteamOS version. Inspect required GLIBC/GLIBCXX versions; do not assume a newer build container will run on the Deck.
- Add reproducible Linux build/package scripts. Construct an AppDir with AppRun, desktop entry, original project icon, runtime dependencies and notices. Do not bundle host graphics drivers.
- Accept an explicit data path and use a writable per-user data directory by default. Support AppImage extraction fallback where FUSE is unavailable.
- Test Desktop Mode and Gaming Mode on an actual Steam Deck: controller navigation, all attack buttons, audio, fullscreen, pacing, repeated launch and exit. Use Steam Input where helpful and document mappings.
- Report unperformed hardware testing honestly.

## 6. Validation and delivery

- Add deterministic scenarios for boot, attract, character selection, a full match and two-player input. Compare native/reference state and video/audio with explicit tolerances or exact hashes as appropriate.
- Measure timing and verify the correct board refresh behavior rather than forcing an arbitrary 60 Hz assumption.
- Add source-only CI checks initially. Enable binary packaging only when the required source/generated inputs exist and the no-game-data distribution boundary has been checked.
- Document prerequisites, exact commands, expected folder layout, first run, controls and known issues.
- Inspect git diff and staged files, run appropriate checks, commit source changes, and push the development branch to the specified repository. Report commit and branch links.
- After successful local gameplay and package tests, prepare Windows ZIP, Steam Deck AppImage, SHA-256 checksums and accurate release notes. Do not mark a playable release until those tests pass.

Work incrementally. At each milestone report implemented behavior, commands actually run, test results and remaining blockers. If the ROM is unavailable or a prerequisite fails, continue independent source work and identify the exact blocked step.
