---
aliases: ["PI Pods API Index", "packages_pods_src_index", "Packages Pods Src Index"]
type: api
package: pods
topics:
  - pods
tags:
  - pkg/pods
  - doc/api
cssclass: api-note
---



# index.ts


Related: [[../../../00-start/home]]


> Auto-generated documentation for `packages/pods/src/index.ts`

## Overview

Main entry point for the `@mariozechner/pi-pods` package. Re-exports all types for deploying and managing vLLM on GPU pods. This is a minimal barrel file.

## API / Exports

Re-exports all from `./types.js`:
- `GpuInfo` - GPU information
- `ModelConfig` - Model configuration with vLLM settings
- `PodConfig` - Pod connection and storage configuration
- `RunningModel` - Information about a running model instance

## See Also

- `./types.ts` - Type definitions
- `./cli.ts` - Command-line implementation
- `README.md` - Full CLI documentation