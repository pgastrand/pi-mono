---
aliases: ["Dashboards", "dashboards"]
type: index
package: docs
topics:
  - navigation
  - metadata
cssclass: moc
tags:
  - pkg/docs
  - doc/index
  - topic/navigation
  - topic/metadata
---

# Dashboards

Related: [[home]] · [[graph-index]] · [[tag-index]]

> [!tip]
> Package dashboards: [[../01-packages/coding-agent/dashboard]] · [[../01-packages/ai/dashboard]] · [[../01-packages/agent/dashboard]] · [[../01-packages/tui/dashboard]] · [[../01-packages/web-ui/dashboard]] · [[../01-packages/mom/dashboard]] · [[../01-packages/pods/dashboard]]

## Manual package dashboards

### Coding agent
- Home: [[../01-packages/coding-agent/home]]
- README: [[../10-mirrors/coding-agent/packages_coding-agent_README]]
- API/types: [[../10-mirrors/coding-agent/api/packages_coding-agent_src_main]], [[../10-mirrors/coding-agent/api/packages_coding-agent_src_core_extensions_types]], [[../02-api/coding-agent-extension-types]]
- Guides: [[../01-packages/coding-agent/guides/sdk]], [[../01-packages/coding-agent/guides/extensions]], [[../01-packages/coding-agent/guides/skills]], [[../01-packages/coding-agent/guides/prompt-templates]], [[../01-packages/coding-agent/guides/settings]], [[../01-packages/coding-agent/guides/session]], [[../01-packages/coding-agent/guides/custom-provider]]

### AI
- Home via [[graph-index]]
- README: [[../10-mirrors/ai/packages_ai_README]]
- API: [[../01-packages/ai/api/api-registry]], [[../01-packages/ai/api/stream]], [[../10-mirrors/ai/api/packages_ai_src_types]], [[../10-mirrors/ai/api/packages_ai_src_models]]

### Agent
- README: [[../10-mirrors/agent/packages_agent_README]]
- API: [[../01-packages/agent/api/agent]], [[../01-packages/agent/api/agent-loop]], [[../10-mirrors/agent/api/packages_agent_src_types]]

### TUI
- README: [[../10-mirrors/tui/packages_tui_README]]
- API: [[../01-packages/tui/api/tui]], [[../01-packages/tui/api/terminal]], [[../10-mirrors/tui/api/packages_tui_src_tui]]

### Web UI
- README: [[../10-mirrors/web-ui/packages_web-ui_README]]
- API: [[../01-packages/web-ui/api/index]], [[../10-mirrors/web-ui/api/packages_web-ui_src_index]]

### Mom
- Home: [[../01-packages/mom/home]]
- README: [[../10-mirrors/mom/packages_mom_README]]
- Docs: [[../01-packages/mom/guides/new]], [[../01-packages/mom/guides/events]], [[../01-packages/mom/guides/sandbox]], [[../01-packages/mom/guides/artifacts-server]], [[../01-packages/mom/guides/slack-bot-minimal-guide]], [[../01-packages/mom/guides/v86]]

### Pods
- Home: [[../01-packages/pods/home]]
- README: [[../10-mirrors/pods/packages_pods_README]]
- Docs: [[../01-packages/pods/guides/models]], [[../01-packages/pods/guides/qwen3-coder]], [[../01-packages/pods/guides/gpt-oss]], [[../01-packages/pods/guides/gml-4.5]], [[../01-packages/pods/guides/kimi-k2]], [[../01-packages/pods/guides/implementation-plan]], [[../01-packages/pods/guides/plan]]

## Dataview examples

If you use the Dataview plugin, these queries can power live dashboards.

### All README notes
```dataview
TABLE file.link AS Note, package, topics
FROM ""
WHERE type = "readme"
SORT file.name ASC
```

### API notes by package
```dataview
TABLE file.link AS Note, package, topics
FROM ""
WHERE type = "api"
SORT package ASC, file.name ASC
```

### Coding-agent notes
```dataview
TABLE file.link AS Note, type, topics
FROM ""
WHERE package = "coding-agent"
SORT type ASC, file.name ASC
```

### Provider/model related notes
```dataview
TABLE file.link AS Note, package, type
FROM ""
WHERE contains(topics, "providers") OR contains(topics, "models")
SORT package ASC, file.name ASC
```

### Workflow prompts
```dataview
TABLE file.link AS Note, topics
FROM ""
WHERE type = "prompt"
SORT file.name ASC
```

### Package summary
```dataview
TABLE rows.file.link AS Notes
GROUP BY package
SORT package ASC
```
