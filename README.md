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

If `ghosttrace_data.dat` exists next to `main.py`, GhostTrace loads that encrypted data file first. Otherwise it uses `private_puzzles.py` when present, and falls back to `sample_puzzles.py`.

## Test

```bash
python3 -m pytest -q
```

## Preferred macOS Delivery

Use the Python-source delivery mode to avoid PyInstaller/Gatekeeper malware warnings.

```bash
chmod +x build_macos.sh
./build_macos.sh
```

The build script:

- Clears `~/.ghosttrace_save.json`.
- Runs tests.
- Generates `ghosttrace_data.dat` from local private puzzle content.
- Stores answers as hashes only.
- Creates `~/Desktop/GhostTrace_Delivery`.
- Creates `~/Desktop/GhostTrace.zip`.

Delivery zip contents:

```text
GhostTrace_Delivery/
  Launch_GhostTrace.command
  main.py
  requirements.txt
  ghosttrace_data.dat
```

Do not send the repo, tests, README, `private_puzzles.py`, `sample_puzzles.py`, build folders, dist folders, or raw `.py` puzzle datasets.

The launcher:

- Changes into its own directory.
- Checks that `python3` exists.
- Installs requirements quietly.
- Uses a local save file at `GhostTrace_Delivery/.ghosttrace_save.json`.
- Runs `python3 main.py`.
- Keeps Terminal open after exit.

## Optional PyInstaller Build

PyInstaller remains available, but it is not the preferred delivery method unless the app is properly Developer ID signed and notarized.

```bash
chmod +x build_pyinstaller.sh
./build_pyinstaller.sh
```

## Public Repo Safety

The public repo contains the engine and a fake sample puzzle dataset in `sample_puzzles.py`.

Real proposal content belongs only in ignored local files such as:

```text
private_puzzles.py
COMMANDS_REFERENCE.local.md
ghosttrace_data.dat
```

`ghosttrace_data.dat` is generated for delivery from private content. It should not be committed to a public engine repo.

## Save Data

Progress is saved automatically after each recovered fragment.

Default local development save:

```text
~/.ghosttrace_save.json
```

Launcher delivery save:

```text
GhostTrace_Delivery/.ghosttrace_save.json
```

The save file contains the current stage, solved puzzle IDs, timestamp, and an HMAC SHA256 checksum. If the file is modified outside the game, GhostTrace aborts recovery with an archive corruption warning.
