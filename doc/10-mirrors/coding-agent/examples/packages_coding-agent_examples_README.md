---
aliases: ["Coding Agent Examples README", "packages_coding-agent_examples_README", "Packages Coding Agent Examples Readme"]
type: readme
package: coding-agent
topics:
  - agent
tags:
  - pkg/coding-agent
  - doc/readme
cssclass: moc
---



# Examples


Related: [[../../../00-start/home]]


Example code for pi-coding-agent SDK and extensions.

## Directories

### [sdk/](sdk/)
Programmatic usage via `createAgentSession()`. Shows how to customize models, prompts, tools, extensions, and session management.

### [extensions/](extensions/)
Example extensions demonstrating:
- Lifecycle event handlers (tool interception, safety gates, context modifications)
- Custom tools (todo lists, questions, subagents, output truncation)
- Commands and keyboard shortcuts
- Custom UI (footers, headers, editors, overlays)
- Git integration (checkpoints, auto-commit)
- System prompt modifications and custom compaction
- External integrations (SSH, file watchers, system theme sync)
- Custom providers (Anthropic with custom streaming, GitLab Duo)

## Documentation

- [SDK Reference](packages_coding-agent_examples_sdk_README.md)
- [Extensions Documentation](../docs/packages_coding-agent_docs_extensions.md)
- [Skills Documentation](../docs/packages_coding-agent_docs_skills.md)