---
name: uproot-build-and-install
description: This skill should be used when users ask about build and install in uproot; it prioritizes documentation references and then source inspection only for unresolved details.
---

# uproot: Build and Install

## High-Signal Playbook
### Route conditions
- Use this skill when the user needs API-level behavior for reading trees/arrays, batching, backend selection, write-path setup, or multi-file workflows (`docs-sphinx/basic.rst`, `src/uproot/behaviors/TBranch.py`).
- Route to `uproot-getting-started` for first-file onboarding and basic object navigation.
- Route to `uproot-docs-sphinx` for migration and docs-toolchain issues.

### Triage questions
- Single file or many files?
- Can full arrays fit memory, or must it stream in batches?
- Desired output container: Awkward, NumPy, or Pandas?
- Need filters/cuts/aliases or all branches?
- Need deterministic entry accounting (`report=True`)?
- Local/remote access, and any handler/worker constraints?

### Canonical workflow
1. Confirm environment install (`pip install uproot awkward` or conda) (`docs-sphinx/index.rst`).
2. Open target tree (`uproot.open("file.root:events")`) and inspect schema (`show`, `typenames`) (`docs-sphinx/basic.rst`).
3. Use `array()`/`arrays()` for single-file eager reads (`docs-sphinx/basic.rst`, `src/uproot/behaviors/TBranch.py`).
4. Use `iterate()` for memory-bounded processing and `concatenate()` only when full materialization is acceptable (`docs-sphinx/basic.rst`).
5. Express `step_size` in memory units and tune with `num_entries_for` (`docs-sphinx/basic.rst`, `src/uproot/behaviors/TBranch.py`).
6. Set `library="ak"|"np"|"pd"` explicitly for predictable downstream types (`docs-sphinx/basic.rst`).
7. Validate chunk accounting with `report=True` before scaling to full datasets (`tests/test_0043_iterate_function.py`).

### Simulation quickstart (local/offline)
1. Install the package into the current environment:
```bash
python -m pip install -e .
```
2. Run a local multi-file simulation smoke test:
```bash
python - <<'PY'
from pathlib import Path
import numpy as np
import uproot

for i, offset in enumerate((0, 100)):
    path = Path(f"build_sim_{i}.root")
    with uproot.recreate(path) as f:
        f.mktree("events", {"px1": np.float64, "py1": np.float64})
        vals = np.arange(offset, offset + 10, dtype=np.float64)
        f["events"].extend({"px1": vals, "py1": vals * 0.1})

files = ["build_sim_0.root:events", "build_sim_1.root:events"]

seen = 0
expected_start = 0
for batch, report in uproot.iterate(files, ["px1", "py1"], step_size=4, library="np", report=True):
    assert report.global_entry_start == expected_start
    expected_start = report.global_entry_stop
    seen += len(batch["px1"])

full = uproot.concatenate(files, ["px1", "py1"], library="np")
assert seen == len(full["px1"]) == 20
print("build-and-install simulation check: OK")
PY
```
3. Validation checkpoint: output must end with `build-and-install simulation check: OK`.

### Focused validation commands
- Local/offline iterate/concatenate check: `python -m pytest -q tests/test_1406_improved_rntuple_methods.py::test_iterate_and_concatenate`
- Full API regression set (requires `skhep_testdata` and optional dask deps): `python -m pytest -q tests/test_0043_iterate_function.py tests/test_0044_concatenate_function.py tests/test_0609_num_enteries_func.py tests/test_0603_dask_delayed_open.py`

### Pitfalls/fixes
- `concatenate()` on large datasets can exhaust memory; switch to `iterate()` and accumulate partial results.
- Multi-file specs need object paths (`"*.root:events"` or `{path: "events"}`), not only filenames (`docs-sphinx/basic.rst`, `src/uproot/behaviors/TBranch.py`).
- Entry-count `step_size` can mis-tune memory; prefer byte units and `num_entries_for` (`docs-sphinx/basic.rst`).
- Missing optional backends (`pandas`, `awkward-pandas`) causes import errors for `library="pd"` on jagged data (`docs-sphinx/basic.rst`).
- `arrays()` with no expressions can read too much; pass explicit branches or filters.
- `allow_missing=False` (default) raises when a file lacks the requested tree; enable only when intentional (`src/uproot/behaviors/TBranch.py`).

### Convergence/validation checks
- For a sample, `sum(len(batch))` from `iterate()` matches `concatenate()` length.
- `report.global_entry_start/stop` are monotonic and gap-free.
- Returned container type matches `library` (`dict` for `np`, Awkward record array for `ak`, DataFrame for `pd`).
- Filtered branch names from `keys(...)` match columns read in `arrays(...)`.
- Spot-check first values against known test expectations before broad runs (`tests/test_0043_iterate_function.py`).

## Scope
- Handle environment setup and API behavior for scaling read/write workloads.
- Keep responses architectural first; drop to function-level behavior when needed.

## Primary documentation references
- `docs-sphinx/index.rst`
- `docs-sphinx/basic.rst`
- `README.md`
- `docs-sphinx/uproot3-to-4.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `dev`
- `docs-sphinx`

## Test references
- `tests`
- `tests-cuda`
- `tests-wasm`

## Optional deeper inspection
- `src`

## Source entry points for unresolved issues
- `src/uproot/behaviors/TBranch.py`
- `src/uproot/reading.py`
- `src/uproot/_dask.py`
- `src/uproot/models/TTree.py`
- `src/uproot/writing/writable.py`
- `src/uproot/__init__.py`
- `tests/test_0043_iterate_function.py`
- `tests/test_0044_concatenate_function.py`
- `tests/test_0609_num_enteries_func.py`
- `tests/test_0603_dask_delayed_open.py`
- `tests/test_0578_dask_for_numpy.py`
- `tests/test_1406_improved_rntuple_methods.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src tests`).
