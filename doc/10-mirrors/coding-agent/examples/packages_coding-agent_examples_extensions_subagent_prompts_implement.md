---
aliases: ["Packages Coding Agent Examples Extensions Subagent Prompts Implement", "packages_coding-agent_examples_extensions_subagent_prompts_implement"]
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

aliases: ["Coding - Agent Examples Extensions Subagent Prompts Implement", "packages_coding-agent_examples_extensions_subagent_prompts_implement"]
tags:
  - pkg/coding-agent
  - doc/prompt
  - topic/extensions
  - topic/prompts



Related: [[../../../00-start/home]]

Use the subagent tool with the chain parameter to execute this workflow:

1. First, use the "scout" agent to find all code relevant to: $@
2. Then, use the "planner" agent to create an implementation plan for "$@" using the context from the previous step (use {previous} placeholder)
3. Finally, use the "worker" agent to implement the plan from the previous step (use {previous} placeholder)

Execute this as a chain, passing output between steps via {previous}.