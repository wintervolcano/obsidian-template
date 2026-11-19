---
type: method
created: 2026-01-20
tags:
  - cookbook
---
# Example cookbook

A label line, then the block. No explanation of what the tool is, no surrounding prose.
This is a reference you copy and paste from at 1am, not an essay.

To separate archive files by centre frequency:

```bash
psrstat -c freq *.ar 2>/dev/null | awk -F= '{print $2, $0}' | sort -n
```

To see what is actually in a filterbank header:

```bash
header raw.fil
```

Labels start with `To ...:` and end with a colon. Group with `### Topic` once the file
gets long. Pasting a tool's own `--help` output verbatim is correct, that is the point.
