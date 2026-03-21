---
aliases: ["Coding Agent Dashboard", "coding-agent-dashboard"]
type: index
package: coding-agent
topics:
  - coding-agent
  - navigation
cssclass: moc
tags:
  - pkg/coding-agent
  - doc/index
  - topic/coding-agent
  - topic/navigation
---

# Coding Agent Dashboard

> [!summary]
> Main entry points: [[home]] · [[../../10-mirrors/coding-agent/packages_coding-agent_README]] · [[guides/sdk]] · [[guides/extensions]]

Related: [[../../00-start/home]] · [[../../00-start/dashboards]] · [[home]]

## Core docs

- [[../../10-mirrors/coding-agent/packages_coding-agent_README]]
- [[guides/providers]]
- [[../pods/guides/models]]
- [[guides/sdk]]
- [[guides/extensions]]
- [[guides/skills]]
- [[guides/prompt-templates]]
- [[guides/settings]]
- [[guides/session]]
- [[guides/custom-provider]]

## Dataview

```dataview
TABLE file.link AS Note, type, topics
FROM ""
WHERE package = "coding-agent"
SORT type ASC, file.name ASC
```

```dataview
TABLE file.link AS API, topics
FROM ""
WHERE package = "coding-agent" AND type = "api"
SORT file.name ASC
```

```dataview
TABLE file.link AS Guide, topics
FROM ""
WHERE package = "coding-agent" AND (type = "guide" OR type = "readme")
SORT file.name ASC
```
