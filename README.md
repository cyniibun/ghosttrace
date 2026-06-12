# GhostTrace

Retro terminal forensic recovery game built with Python and Rich.

## Local Setup

```bash
python3 -m pip install -r requirements.txt
```

## Run Locally

```bash
python3 main.py
```

## Test

```bash
python3 -m pytest -q
```

## Build On macOS

Do not use `--windowed`; GhostTrace is a terminal game.

```bash
chmod +x build_macos.sh
./build_macos.sh
```

The build script runs tests, builds `dist/GhostTrace`, creates:

```text
~/Desktop/GhostTrace_Delivery/
  GhostTrace
  Launch_GhostTrace.command
```

and zips that folder to:

```text
~/Desktop/GhostTrace.zip
```

The build script also removes `~/.ghosttrace_save.json` before packaging so local testing does not resume from an old developer save.

The launcher sets `GHOSTTRACE_SAVE_FILE` so the delivered game stores progress in the same folder as the executable:

```text
GhostTrace_Delivery/.ghosttrace_save.json
```

That keeps the gift build isolated from any developer save state in the user home directory.

The build script ad-hoc signs the executable with:

```bash
codesign --force --deep --sign - dist/GhostTrace
```

Ad-hoc signing helps, but it is not Apple notarization. A zip downloaded from GitHub may still trigger Gatekeeper. For a fully clean macOS download experience, sign with an Apple Developer ID certificate and notarize. For a one-person gift, the practical fallback is right-clicking `Launch_GhostTrace.command` and choosing Open.

## Package For A Friend

Build from your private local folder, not the public repo:

```bash
cd ~/Desktop/ghosttrace-app
./build_macos.sh
```

Send only:

```text
GhostTrace.zip
  GhostTrace_Delivery/
    GhostTrace
    Launch_GhostTrace.command
```

Do not send the repo, source files, tests, README, private puzzle dataset, sample puzzle dataset, build folders, dist folder, or save files.

## Public Repo Safety

The public repo contains the engine and a fake sample puzzle dataset in `sample_puzzles.py`.

Real proposal content belongs only in ignored local files such as:

```text
private_puzzles.py
COMMANDS_REFERENCE.local.md
```

If `private_puzzles.py` exists, GhostTrace uses it. If it is missing, the app falls back to the sample dataset and still runs.

## Save Data

Progress is saved automatically after each recovered fragment at:

```text
~/.ghosttrace_save.json
```

When launched through `Launch_GhostTrace.command`, the save file is instead stored next to the delivery executable as `.ghosttrace_save.json`.

The save file contains the current stage, solved puzzle IDs, timestamp, and an HMAC SHA256 checksum. If the file is modified outside the game, GhostTrace aborts recovery with an archive corruption warning.
