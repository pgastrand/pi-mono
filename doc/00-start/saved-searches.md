---
aliases: ["Saved Searches", "saved-searches"]
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

# Saved Searches

Related: [[home]] · [[dashboards]] · [[vault-guide]]

## Manual search shortcuts

- All API docs: `type:api`
- All prompts: `type:prompt`
- All plans: `type:plan`
- All coding-agent docs: `package:coding-agent`
- All pods deployment docs: `package:pods topic:deployment`
- All provider/model docs: `topic:providers OR topic:models`

## Dataview: all API docs

```dataview
TABLE file.link AS Note, package, topics
FROM ""
WHERE type = "api"
SORT package ASC, file.name ASC
```

## Dataview: all prompt notes

```dataview
TABLE file.link AS Note, package, topics
FROM ""
WHERE type = "prompt"
SORT file.name ASC
```

## Dataview: all deployment notes

```dataview
TABLE file.link AS Note, package, topics
FROM ""
WHERE contains(topics, "deployment")
SORT package ASC, file.name ASC
```

## Dataview: all readmes

```dataview
TABLE file.link AS Note, package
FROM ""
WHERE type = "readme"
SORT package ASC
```

## Dataview: all index notes

```dataview
TABLE file.link AS Note, package
FROM ""
WHERE type = "index"
SORT package ASC, file.name ASC
```
