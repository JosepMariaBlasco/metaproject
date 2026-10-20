---
name: oorexx-run
description: >
  How to install and run ooRexx inside Claude's container.
  Covers installation, running programs, the ::Requires search
  path, and common errors. Read this whenever you need to
  execute ooRexx code in the container.
---

# ooRexx — Running in Claude's Container

## Installation

```bash
curl -o /tmp/oorexx.deb https://rexx.epbcn.com/oorexx-barcelona2026.deb
dpkg -i /tmp/oorexx.deb
```

If the download fails (proxy 403), ask the user to upload the
`.deb` and install with `dpkg -i /mnt/user-data/uploads/oorexx-*.deb`.

Verify: `rexx -v` → `Open Object Rexx Version 5.2.0`

## Running programs

Scripts: `rexx script.rex [args...]`

To check that a `.cls` loads without errors:
`rexx -e "Call 'path/to/file.cls'"`

## Dependency lookup (::Requires)

ooRexx searches in this order:
1. Directory of the program that issues the `::Requires`
2. Current directory
3. Directories in `REXX_PATH`
4. Directories in `PATH`

This allows referencing dependencies in the same directory without a path.

To add a directory: `export REXX_PATH="/path/to/libs"`

## Common errors

- **"rexx: command not found"** — `.deb` not installed. `dpkg -i`.
- **"Error 43: Routine not found"** — a `::Requires` is missing.
  Check that the `.cls` exists at the expected path.
- **"Error 98: Execution error"** — a `::Requires` failed to load.
  Read the full error, it usually includes the failing path.
- **Shared library errors** — `apt-get install -f`.
