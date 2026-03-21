---
aliases: ["Packages Coding Agent Examples Extensions Subagent Prompts Implement And Review", "packages_coding-agent_examples_extensions_subagent_prompts_implement-and-review"]
type: prompt
package: coding-agent
topics:
  - extensions
  - prompts
  - agent
cssclass: prompt-note
tags:
  - pkg/coding-agent
  - doc/prompt
  - topic/extensions
  - topic/prompts
  - topic/agent
---

aliases: ["Coding - Agent Examples Extensions Subagent Prompts Implement - And - Review", "packages_coding-agent_examples_extensions_subagent_prompts_implement-and-review"]
tags:
  - pkg/coding-agent
  - doc/prompt
  - topic/extensions
  - topic/prompts



Related: [[../../../00-start/home]]

Use the subagent tool with the chain parameter to execute this workflow:

1. First, use the "worker" agent to implement: $@
2. Then, use the "reviewer" agent to review the implementation from the previous step (use {previous} placeholder)
3. Finally, use the "worker" agent to apply the feedback from the review (use {previous} placeholder)

Execute this as a chain, passing output between steps via {previous}.