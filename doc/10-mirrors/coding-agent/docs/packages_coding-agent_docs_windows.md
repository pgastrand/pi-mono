---
aliases: ["Coding Agent Docs - Windows", "packages_coding-agent_docs_windows", "Packages Coding Agent Docs Windows"]
type: guide
package: coding-agent
topics:
  - agent
tags:
  - pkg/coding-agent
  - doc/guide
---



# Windows Setup


Related: [[../../../00-start/home]]


Pi requires a bash shell on Windows. Checked locations (in order):

1. Custom path from `~/.pi/agent/settings.json`
2. Git Bash (`C:\Program Files\Git\bin\bash.exe`)
3. `bash.exe` on PATH (Cygwin, MSYS2, WSL)

For most users, [Git for Windows](https://git-scm.com/download/win) is sufficient.

## Custom Shell Path

```json
{
  "shellPath": "C:\\cygwin64\\bin\\bash.exe"
}
```