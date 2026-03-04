---
name: uproot-index
description: This skill should be used when users ask how to use uproot and the correct generated documentation skill must be selected before going deeper into source code.
---

# uproot Skills Index

## Fast intent routing
- `Read/open/navigate ROOT files`: use `uproot-getting-started`.
- `Write/update ROOT files` (`recreate`, `update`, `mktree`, `extend`, compression): start in `uproot-getting-started`; escalate to `uproot-build-and-install` for API edge behavior.
- `Convert/extract arrays/histograms` (`library="ak"|"np"|"pd"`, `to_numpy`, `to_hist`): start in `uproot-getting-started`; escalate to `uproot-build-and-install` for batching/performance.
- `Distributed or large-scale I/O` (`iterate`, `concatenate`, `dask`, remote handlers, `step_size` tuning): use `uproot-build-and-install`.
- `Troubleshooting/migration/docs build` (Uproot 3 -> 4/5 changes, docs requirements): use `uproot-docs-sphinx`.

## Skills map
- `uproot-getting-started`: onboarding, first-file workflows, object navigation, first array/hist extraction, first write workflows.
- `uproot-build-and-install`: core read APIs (`array`, `arrays`, `iterate`, `concatenate`), backend/library choices, scale-up patterns.
- `uproot-docs-sphinx`: migration and docs-grounded troubleshooting (`uproot3-to-4.rst`, docs requirements).

## Docs-first escalation protocol
1. Start in the selected skill's `Primary documentation references`.
2. If unresolved, scan the routed skill doc map:
   `../uproot-getting-started/references/doc_map.md`,
   `../uproot-build-and-install/references/doc_map.md`, or
   `../uproot-docs-sphinx/references/doc_map.md`.
3. Escalate to the matching source map only when docs are insufficient:
   `../uproot-getting-started/references/source_map.md`,
   `../uproot-build-and-install/references/source_map.md`, or
   `../uproot-docs-sphinx/references/source_map.md`.
4. Validate claims against runnable tests/examples before finalizing guidance.

## Evidence roots
- Tutorials/examples: `docs-sphinx`, `dev`
- Behavior/regression checks: `tests`, `tests-cuda`, `tests-wasm`
- Implementation inspection: `src`

## Source escalation command
- Use targeted symbol search once routed: `rg -n "<symbol_or_keyword>" src tests docs-sphinx`
