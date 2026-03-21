---
aliases: ["Coding Agent Docs - Shell Aliases", "packages_coding-agent_docs_shell-aliases", "Packages Coding Agent Docs Shell Aliases"]
type: guide
package: coding-agent
topics:
  - agent
tags:
  - pkg/coding-agent
  - doc/guide
---



# Shell Aliases


Related: [[../../../00-start/home]]


Pi runs bash in non-interactive mode (`bash -c`), which doesn't expand aliases by default.

To enable your shell aliases, add to `~/.pi/agent/settings.json`:

```json
{
  "shellCommandPrefix": "shopt -s expand_aliases\neval \"$(grep '^alias ' ~/.zshrc)\""
}
```

Adjust the path (`~/.zshrc`, `~/.bashrc`, etc.) to match your shell config.