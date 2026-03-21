---
aliases: ["Implement and Review Prompt", "implement-and-review"]
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

aliases: ["Implement - And - Review", "implement-and-review"]
tags:
  - doc/prompt



Related: [[../../00-start/home]]

Use the subagent tool with the chain parameter to execute this workflow:

1. First, use the "worker" agent to implement: $@
2. Then, use the "reviewer" agent to review the implementation from the previous step (use {previous} placeholder)
3. Finally, use the "worker" agent to apply the feedback from the review (use {previous} placeholder)

Execute this as a chain, passing output between steps via {previous}.