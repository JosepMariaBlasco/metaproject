---
name: pagedjs-cli
description: >
  How to install and run pagedjs-cli inside Claude's container to
  render HTML with paged.js and produce PDF. Covers the pre-built
  bundle workaround (npm is blocked by the proxy), the Chromium
  wrapper, common errors, and a note on performance. Read this
  whenever you need to generate PDFs from HTML via paged.js.
---

# pagedjs-cli in Claude's Container

Dependencies: ooRexx (`oorexx-run.md`) and Project (*Canonical paths* section in `setup.md`).

pagedjs-cli uses Chromium (via puppeteer) to render HTML with
paged.js and produce PDF. It cannot be installed via `npm` in this
environment because the proxy blocks `storage.googleapis.com`. A
pre-built bundle is used instead.

## Installation

```bash
curl -o /tmp/pagedjs-bundle.tar.gz https://rexx.epbcn.com/pagedjs-bundle-barcelona2026.tar.gz
mkdir -p /home/claude/pagedjs
tar xzf /tmp/pagedjs-bundle.tar.gz -C /home/claude/pagedjs

cat > /usr/local/bin/pagedjs-cli << 'WRAPPER'
#!/bin/bash
export PUPPETEER_EXECUTABLE_PATH=/home/claude/pagedjs/linux-115.0.5790.98/chrome-linux64/chrome
exec node /home/claude/pagedjs/pagedjs-cli/src/cli.js --browserArgs="--no-sandbox" "$@"
WRAPPER
chmod +x /usr/local/bin/pagedjs-cli
```

If the download fails, ask the user to upload the bundle.

Verify: `pagedjs-cli --help`

## The wrapper

It does three things: points `PUPPETEER_EXECUTABLE_PATH` to the
Chromium inside the bundle, passes `--no-sandbox` (the container
runs as root), and forwards the arguments.

## Common errors

- **"pagedjs-cli: not found"** — wrapper not created. See installation.
- **"Failed to launch the browser process"** — `--no-sandbox` is
  missing, or `PUPPETEER_EXECUTABLE_PATH` points to the wrong place.
  Check that it exists:
  `ls /home/claude/pagedjs/linux-115.0.5790.98/chrome-linux64/chrome`
- **Missing shared libraries** —
  `apt-get install -y libnss3 libatk1.0-0 libatk-bridge2.0-0 libcups2
  libdrm2 libxkbcommon0 libxcomposite1 libxdamage1 libxrandr2 libgbm1
  libpango-1.0-0 libasound2`

## Note

The bundle (~149 MB) is heavy. Only install it when the session
requires generating PDFs. PDF tests are very slow in the container;
suggest trying this on WSL if at all possible.
