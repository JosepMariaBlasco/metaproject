---
name: apache-cgi-rexx
description: >
  How to set up Apache with CGI to run ooRexx scripts inside Claude's
  container. Covers installation, the shell wrapper needed for
  REXX_PATH, an example Apache config, and Linux case-sensitivity
  traps. Read this whenever you need to serve ooRexx via CGI.
---

# Apache + CGI with ooRexx

Dependencies: ooRexx (`oorexx-run.md`) and Project (*Canonical paths* section in `setup.md`).

## Installation

```bash
apt-get update -qq && apt-get install -y -qq apache2
a2enmod cgid actions
```

## Shell wrapper for REXX_PATH

Apache's `SetEnv` does not propagate to the process environment that
ooRexx reads through `::Requires`. A shell wrapper is needed:

```bash
cat > /path/to/cgi-wrapper.sh << 'WRAPPER'
#!/bin/bash
export REXX_PATH="/path/to/cgi:/path/to/bin"
exec rexx /path/to/CGI-script.rex "$@"
WRAPPER
chmod +x /path/to/cgi-wrapper.sh
chmod +x /path/to/CGI-script.rex
```

## Apache configuration (example)

```apache
ScriptAlias /cgi-bin/ /path/to/cgi/
<Directory /path/to/cgi>
    Options +ExecCGI
    AllowOverride None
    Require all granted
    SetHandler cgi-script
</Directory>

Action Markdown /cgi-bin/cgi-wrapper.sh
<FilesMatch "\.md$">
    SetHandler Markdown
</FilesMatch>
```

## Case sensitivity on Linux

`::Requires` directives must match the filenames on disk exactly.
On Windows it doesn't matter; on Linux, `HTTP.Request.cls` ≠
`http.request.cls`.

## Verification

```bash
service apache2 start
curl -s http://localhost/test.md
```

Errors go to `/var/log/apache2/error.log`.
