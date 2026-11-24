# The install scripts

These are copies of the 2 scripts that install the `runstory` command line tool, so you can read them before you run them.

| File | The live script | Run it with |
| --- | --- | --- |
| [install.sh](install.sh) | `https://runstory.specstory.com/install.sh` | `curl -fsSL https://runstory.specstory.com/install.sh \| sh` |
| [install.ps1](install.ps1) | `https://runstory.specstory.com/install.ps1` | `powershell -ExecutionPolicy Bypass -c "irm https://runstory.specstory.com/install.ps1 \| iex"` |

A [workflow](../.github/workflows/mirror.yml) copies them from the live site once a day. Install from the live addresses, not from this folder.

## What the installer does

- It checks the download against a signed manifest before anything runs.
- It uses no sudo on macOS and Linux, and no administrator rights on Windows.
- It puts `runstory` in `~/.local/bin`. On Windows it puts `runstory.exe` in `%USERPROFILE%\.local\bin` and adds that folder to your user Path.
- On macOS and Linux it needs curl 7.81 or newer and OpenSSL.

The Windows build is not signed yet, so Smart App Control in enforcement mode blocks it.

## Update or remove it

- `runstory update` updates the tool on this machine.
- `runstory uninstall` removes RunStory from a repository, with its hooks. Run it in each repository before you delete the tool itself.
