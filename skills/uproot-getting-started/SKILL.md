---
name: uproot-getting-started
description: This skill should be used when users ask about getting started in uproot; it prioritizes documentation references and then source inspection only for unresolved details.
---

# uproot: Getting Started

## High-Signal Playbook
### Route conditions
- Use this skill for first-contact workflows: install, open a ROOT file, discover objects, inspect branches, read first arrays/histograms, and simple write setup (`docs-sphinx/index.rst`, `docs-sphinx/basic.rst`).
- Route to `uproot-build-and-install` for scale-up patterns (`iterate`, `concatenate`, `dask`, `step_size`, executors, multi-file planning).
- Route to `uproot-docs-sphinx` for migration or docs-tooling questions (`docs-sphinx/uproot3-to-4.rst`, `docs-sphinx/requirements.txt`).

### Triage questions
- Is the user reading, writing, or both?
- Is input local, HTTP(S), S3, XRootD, or a Python file-like object?
- Do they already know the object path (`file.root:events`)?
- Do they want `library="ak"`, `library="np"`, or `library="pd"`?
- Are branches flat or jagged/nested?
- Is this interactive/small-data or memory-limited processing?

### Canonical workflow
1. Install `uproot` (+ `awkward`) via pip or conda (`docs-sphinx/index.rst`).
2. Open with a context manager (`with uproot.open(...) as file`) (`docs-sphinx/basic.rst`).
3. Discover objects with `keys()` and `classnames()` before loading payloads (`docs-sphinx/basic.rst`).
4. Open the target object via `file["..."]` or `uproot.open("file.root:obj")` (`docs-sphinx/basic.rst`).
5. Inspect schema with `typenames()` or `show()` (`docs-sphinx/basic.rst`).
6. Read arrays with `array()`/`arrays()` and explicit `library=` when needed (`docs-sphinx/basic.rst`).
7. For histograms, use `to_numpy`, `axis().edges()`, and `values()` to validate binning/content (`docs-sphinx/basic.rst`).

### Simulation quickstart (local/offline)
1. Install the package into the current environment:
```bash
python -m pip install -e .
```
2. Run a local smoke simulation:
```bash
python - <<'PY'
from pathlib import Path
import numpy as np
import uproot

path = Path("getting_started_local.root")
with uproot.recreate(path) as f:
    f.mktree("events", {"px1": np.float64, "py1": np.float64, "M": np.float64})
    f["events"].extend(
        {
            "px1": np.array([1.0, 2.0, 3.0, 4.0]),
            "py1": np.array([0.5, 0.4, 0.3, 0.2]),
            "M": np.array([91.2, 90.8, 89.9, 92.1]),
        }
    )

with uproot.open(f"{path}:events") as tree:
    arrays = tree.arrays(["px1", "M"], library="np")
    assert tree.num_entries == 4
    assert arrays["px1"].tolist() == [1.0, 2.0, 3.0, 4.0]
    assert len(tree.keys()) == 3

print("getting-started simulation check: OK")
PY
```
3. Validation checkpoint: output must end with `getting-started simulation check: OK`.

### Focused validation commands
- Local/offline parser check: `python -m pytest -q tests/test_0976_path_object_split.py`
- Full read-path regression (requires `skhep_testdata`): `python -m pytest -q tests/test_0022_number_of_branches.py tests/test_0081_dont_parse_colons.py`

### Pitfalls/fixes
- Missing optional package for output conversion (`pd`, `hist`): install the package named in the ImportError hint (`docs-sphinx/basic.rst`, `src/uproot/extras.py`).
- File-like objects support `read`/`seek`, but they are not parallel-read sources (`docs-sphinx/basic.rst`).
- Filenames containing `:` can be misread as `file:object`: use `pathlib.Path` or `{file: object}` syntax (`docs-sphinx/uproot3-to-4.rst`, `src/uproot/reading.py`).
- Calling `arrays()` without expressions/filters can read all branches; constrain with names or filters (`docs-sphinx/basic.rst`).
- For writing, tiny `WritableTree.extend` chunks are overhead-heavy; target >= 100 kB per branch per extend (`docs-sphinx/basic.rst`).

### Convergence/validation checks
- `file.classnames()` shows expected ROOT classes before extraction.
- `tree.show()`/`tree.typenames()` match expected schema.
- Full-branch read length matches `tree.num_entries`.
- Histogram sanity: `len(edges) == len(values) + 1`.
- Write sanity: `num_entries` increases after each `extend`.

## Scope
- Handle questions about initial setup, quickstarts, and core concepts.
- Keep responses architectural first; expand to per-function detail only when needed.

## Primary documentation references
- `docs-sphinx/index.rst`
- `docs-sphinx/basic.rst`
- `docs-sphinx/uproot3-to-4.rst`
- `dev/custom-interpretation/README.md`

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
- `src/uproot/reading.py`
- `src/uproot/behaviors/TBranch.py`
- `src/uproot/_util.py`
- `src/uproot/extras.py`
- `src/uproot/interpretation/custom.py`
- `tests/test_0009_nested_directories.py`
- `tests/test_0022_number_of_branches.py`
- `tests/test_0081_dont_parse_colons.py`
- `tests/test_0099_read_from_file_object.py`
- `tests/test_0976_path_object_split.py`
- `tests/test_0167_use_the_common_histogram_interface.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src tests`).
