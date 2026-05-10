# Code Smells Report

**Scope:** Full `pi-mono` repo — `packages/{agent,ai,coding-agent,mom,pods,tui,web-ui}/src` (~115k LOC, 287 source files excluding `node_modules`, `dist`, `test`, `scripts`)
**Language(s):** TypeScript
**Date:** 2026-05-10

Excluded from analysis: `packages/ai/src/models.generated.ts` (generated), `packages/web-ui/src/utils/test-sessions.ts` (fixture), `packages/ai/scripts/generate-models.ts` (build script), `packages/coding-agent/examples/**` (example extensions — lower severity).

## Summary

| Severity | Count |
|----------|-------|
| Critical | 0     |
| Warning  | 16    |
| Info     | 6     |

No critical-severity issues were detected on this pass. Cross-package circular dependencies are forbidden by the documented chain in `CLAUDE.md` and were not spotted; a true shotgun-surgery audit would require a focused refactor scope rather than a static pass.

## Warning

### God Class — `AgentSession` in `packages/coding-agent/src/core/agent-session.ts:175`

Single class is 3182 lines with **72 public methods/getters** spanning model selection, scoped models, tool registration, message lifecycle, compaction, session switching, extension binding, event subscription, abort handling, retry logic, and persistence. This is the highest-impact structural finding in the repo.

**Suggestion:** Split by responsibility into collaborators owned by `AgentSession`:
- `ToolRegistry` — `getActiveToolNames`, `getAllTools`, `setActiveToolsByName`, `_bindExtensionCore`
- `MessageStore` — `messages`, `appendXXX`-style methods, branch/leaf navigation
- `CompactionController` — `compact`, `isCompacting` (currently 108-line `compact()` lives here)
- `ModelController` — `model`, `scopedModels`, `setScopedModels`, `thinkingLevel`
- `SessionLifecycle` — `dispose`, `switchSession`, persistence

Keep `AgentSession` as a thin facade if the public API surface must remain stable.

### Large Class / Divergent Change — `InteractiveMode` in `packages/coding-agent/src/modes/interactive/interactive-mode.ts:145`

4570 lines, 11 public methods but a sprawling private surface (settings selector, tree selector, theme selector, model selector, hotkeys command, autocomplete setup, editor submit handler, paste handler, key handlers, …). Only 11 public methods masks an SRP violation: the class will be edited for unrelated reasons (new selector, new keybind handler, new command).

**Suggestion:** Extract each `show*Selector()` and `handle*Command()` into its own controller class injected into `InteractiveMode`. Pattern is already partly applied via `components/` directory — finish the split.

### Long Method — `TUI.doRender()` in `packages/tui/src/tui.ts:873`

313 lines. Top of the renderer; mixes viewport calculation, overlay compositing, cursor extraction, full-vs-differential render branching, and line-emission. By far the longest single function in the codebase.

**Suggestion:** Extract `computeViewport()`, `applyOverlays()`, `decideRenderStrategy()`, `emitDifferentialLines()`. Keep `doRender()` as orchestrator only.

### Long Method — `Editor.handleInput()` in `packages/tui/src/components/editor.ts:519`

292 lines. Already organized into stanzas (jump mode, bracketed paste, paste accumulation, …) — each stanza is an extract candidate.

**Suggestion:** Use a small dispatch table keyed by input class (paste / jump / control / printable) and one handler per branch.

### Long Methods (cluster — extract candidates)

| Method | Location | ~Lines |
|---|---|---|
| `setupEditorSubmitHandler` | `interactive-mode.ts:1953` | 169 |
| `prompt` | `agent-session.ts:865` | 139 |
| `setupEventHandlers` | `mom/src/slack.ts:272` | 138 |
| `getFileSuggestions` | `tui/src/autocomplete.ts:527` | 133 |
| `showSettingsSelector` | `interactive-mode.ts:3156` | 132 |
| `reload` | `coding-agent/.../resource-loader.ts:307` | 129 |
| `showTreeSelector` | `interactive-mode.ts:3522` | 126 |
| `addAutoDiscoveredResources` | `core/package-manager.ts:1821` | 124 |
| `init` | `interactive-mode.ts:366` | 124 |
| `process` (`AnsiCodeProcessor`) | `tui/src/utils.ts:315` | 122 |
| `_bindExtensionCore` | `agent-session.ts:2075` | 119 |
| `handleHotkeysCommand` | `interactive-mode.ts:4204` | 115 |
| `compact` | `agent-session.ts:1607` | 108 |
| `createBranchedSession` | `core/session-manager.ts:1156` | 92 |

All exceed the 40-line warning threshold by 2-7×. Apply Extract Method per logical section. The cluster on `interactive-mode.ts` reinforces the God-Class finding above — these private methods want to live in their own classes.

### Large Class — `Editor` in `packages/tui/src/components/editor.ts`

2196 lines, 17 public methods. Holds rendering, input handling, layout, jump mode, paste mode, and clipboard logic in one place.

**Suggestion:** Extract `EditorLayoutEngine` (current `layoutText`, 86 lines), `EditorInputDispatcher`, `EditorClipboard`. Keep `Editor` as the `Component`-implementing shell.

### Large Class — `SessionManager` in `packages/coding-agent/src/core/session-manager.ts`

1410 lines, 29 public methods. The 92-line `createBranchedSession()` plus a wide append/branch API surface suggest two responsibilities (persistence vs. tree navigation).

**Suggestion:** Split into `SessionStore` (read/write JSONL files) and `SessionTree` (branch/leaf navigation). The doc comments at lines 1050–1120 already describe the tree as a separate concept.

### Large Class — `TUI` in `packages/tui/src/tui.ts`

1225 lines, 15 public methods. `doRender()` alone is 313 lines (above). Aside from `doRender`, the file mixes overlay management, terminal lifecycle, and component scheduling.

**Suggestion:** Extract a `Renderer` collaborator that owns `doRender`, `applyLineResets`, `extractCursorPosition`, `compositeOverlays`. `TUI` would keep terminal lifecycle + component tree.

### `any` Overuse — `packages/ai/src/providers/openai-completions.ts`

23 `as any` casts, mostly `(choice.delta as any)[field]` for reasoning fields (lines 188-261). These bypass the typed SDK to read undocumented vendor extensions (`reasoning`, `reasoning_content`, `reasoning_details`).

**Suggestion:** Define a local extension type:
```ts
interface ChoiceDeltaWithReasoning extends OpenAI.Delta {
  reasoning?: string; reasoning_content?: string; reasoning_details?: unknown[];
  usage?: OpenAI.CompletionUsage;
}
```
Cast once at the top of the streaming branch instead of 20+ inline casts. Same pattern occurs in `tool-execution.ts:87,97,113,155` for `args`/`details`/`content` — the `args: any` field should be `unknown` (forces narrowing) or a generic `TArgs`.

### `any` Overuse — `packages/web-ui/src/components/sandbox/ArtifactsRuntimeProvider.ts`

22 `(window as any).<name>` casts to inject sandbox globals. Pattern repeats across `ConsoleRuntimeProvider.ts` (15) and `AttachmentsRuntimeProvider.ts` (6).

**Suggestion:** Declare a single `SandboxWindow` interface with the injected fields and cast `window as SandboxWindow` once per provider. Removes ~40 casts across the three files.

## Info

### Speculative Type Casts (`Model<any>`) in `agent-session.ts`

`Model<any>` appears in 7+ public signatures (e.g. `agent-session.ts:137,173,217,673,779,784,1352`). The generic parameter exists but is consistently `any`. Either thread the model-config type through callers or simplify to `Model` (drop the unused generic).

### TODO without ticket — `packages/web-ui/src/components/SandboxedIframe.ts:564`

`// TODO the font-size is needed, as chrome seems to inject a stylesheet into iframes` — has no issue link. The other repo-wide TODO (`mom/src/agent.ts:26`) does reference issue #63, which is the preferred pattern.

**Suggestion:** Open an issue and link it, or resolve.

### Long Methods in example extensions (lower severity)

`examples/extensions/space-invaders.ts` `render()` (127 lines) and `examples/extensions/snake.ts` `render()` (83 lines) are demo code; flagged for completeness only.

### Repeated Tuple Type — `{ model: Model<any>; thinkingLevel?: ThinkingLevel }`

Appears 5+ times in `agent-session.ts` (lines 137, 173, 217, 779, 784) as parameter, return, and field type. Classic Data Clump.

**Suggestion:** Extract `interface ScopedModel { model: Model; thinkingLevel?: ThinkingLevel }` and reuse.

### `Editor.render()` and `layoutText()` co-located

`render(width)` (123 lines, `editor.ts:394`) calls `layoutText(contentWidth)` (86 lines, `editor.ts:813`). Both manipulate the same line/cursor state — borderline Feature Envy on layout state. Worth merging concerns into a small `EditorLayout` value object.

### Negligible findings

- **No `==`/`!=` misuse** outside intentional `== null` / `!= null` nullish checks (which match both `null` and `undefined` and are idiomatic).
- **No commented-out code blocks** detected at meaningful scale.
- **No `instanceof` chains acting as type-switches** — all `instanceof` usage is narrow `Error` narrowing in `catch` blocks (idiomatic).
- **No bare `any` in test code** worth flagging.

## Headline pattern

Two clusters dominate the warning list and account for almost every long-method finding:

1. **`packages/coding-agent/src/core/agent-session.ts` + `packages/coding-agent/src/modes/interactive/interactive-mode.ts`** are growing into facades that own everything in their layer. Splitting them by responsibility (suggested collaborators above) would simultaneously dissolve ~8 of the long-method warnings.
2. **`packages/tui/src/{tui,components/editor}.ts`** have hot rendering paths (`doRender`, `handleInput`) that have accreted features over time. Extract a `Renderer` from `TUI` and an `EditorInputDispatcher` from `Editor` to bring both under threshold.

Tackle those four files and the report shrinks by more than half.
