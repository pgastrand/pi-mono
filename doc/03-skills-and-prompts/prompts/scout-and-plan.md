---
aliases: ["Scout and Plan Prompt", "scout-and-plan"]
type: prompt
package: docs
topics:
  - workflow
cssclass: prompt-note
tags:
  - pkg/docs
  - doc/prompt
  - topic/workflow
---

aliases: ["Scout - And - Plan", "scout-and-plan"]
tags:
  - doc/prompt



Related: [[../../00-start/home]]

Use the subagent tool with the chain parameter to execute this workflow:

1. First, use the "scout" agent to find all code relevant to: $@
2. Then, use the "planner" agent to create an implementation plan for "$@" using the context from the previous step (use {previous} placeholder)

Execute this as a chain, passing output between steps via {previous}. Do NOT implement - just return the plan.