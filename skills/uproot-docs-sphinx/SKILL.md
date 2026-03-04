---
name: uproot-docs-sphinx
description: This skill should be used when users ask about docs sphinx in uproot; it prioritizes documentation references and then source inspection only for unresolved details.
---

# uproot: Docs Sphinx

## High-Signal Playbook
### Route conditions
- Use this skill for migration from Uproot 3, docs interpretation issues, or docs-toolchain questions (`docs-sphinx/uproot3-to-4.rst`, `docs-sphinx/requirements.txt`).
- Route to `uproot-getting-started` for first-file workflows.
- Route to `uproot-build-and-install` for performance and batching design (`iterate`, `concatenate`, `dask`).

### Triage questions
- Is the user migrating Uproot 3 code or writing fresh Uproot 5 code?
- Which old API shape is failing (`uproot3.http`, `uproot3.pandas.iterate`, `lazy` usage)?
- Is the issue runtime I/O behavior or local docs build?
- Does the filename/path contain a colon (`:`)?
- Which output backend is required (`ak`/`np`/`pd`)?
- Do they need eager arrays or deferred/dask execution?

### Canonical workflow
1. Start with the migration map in `docs-sphinx/uproot3-to-4.rst`.
2. Normalize to current entry points: `uproot.open`, `uproot.iterate`, `uproot.concatenate`, and `uproot.dask` (`docs-sphinx/uproot3-to-4.rst`, `src/uproot/behaviors/TBranch.py`).
3. For `file:object` ambiguity, use `pathlib.Path` or dict form (`docs-sphinx/uproot3-to-4.rst`, `src/uproot/reading.py`).
4. Use context managers so files/threads close cleanly (`docs-sphinx/uproot3-to-4.rst`).
5. Set `library=` explicitly to preserve expected container type.
6. Validate on a small sample before scaling.

### Simulation quickstart (local/offline)
1. Install the package into the current environment:
```bash
python -m pip install -e .
```
2. Run a colon-safe migration sanity script:
```bash
python - <<'PY'
from pathlib import Path
import numpy as np
import uproot

path = Path("real:colon.root")
with uproot.recreate(path) as f:
    f.mktree("events", {"x": np.int32})
    f["events"].extend({"x": np.arange(5, dtype=np.int32)})

with uproot.open(path)["events"] as tree:
    assert tree["x"].array(library="np").tolist() == [0, 1, 2, 3, 4]

with uproot.open({str(path): "events"}) as tree:
    assert tree.num_entries == 5

print("docs-sphinx migration check: OK")
PY
```
3. Optional docs build sanity check:
```bash
python -m pip install -r docs-sphinx/requirements.txt
python -m sphinx -b html docs-sphinx docs-sphinx/_build/html
test -f docs-sphinx/_build/html/index.html
```
4. Validation checkpoints:
- Script output ends with `docs-sphinx migration check: OK`.
- `docs-sphinx/_build/html/index.html` exists after Sphinx build.

### Focused validation commands
- Local/offline parser regression: `python -m pytest -q tests/test_0976_path_object_split.py`
- Dataset-backed migration regressions (requires `skhep_testdata`): `python -m pytest -q tests/test_0081_dont_parse_colons.py tests/test_0603_dask_delayed_open.py`

### Pitfalls/fixes
- Uproot 3 protocol helpers are gone; pass URL schemes directly to `uproot.open(...)` (`docs-sphinx/uproot3-to-4.rst`).
- `uproot3.pandas.iterate` patterns become `uproot.iterate(..., library="pd")` (`docs-sphinx/uproot3-to-4.rst`).
- `uproot.lazy` workflows should migrate to `uproot.dask` (`docs-sphinx/uproot3-to-4.rst`, `src/uproot/_dask.py`).
- Colons in filenames can be misparsed as object separators; use `Path` or dict notation.
- Optional dependencies (`pandas`, `hist`) must be installed for selected conversions (`docs-sphinx/basic.rst`, `src/uproot/extras.py`).
- Docs build needs at least `sphinx>=2.4.4` and `sphinx-rtd-theme` (`docs-sphinx/requirements.txt`).

### Convergence/validation checks
- Updated code paths resolve objects correctly (`keys`, `classnames`) without path-splitting regressions.
- Output container type matches explicit `library`.
- Local docs environment installs and builds cleanly from `docs-sphinx/requirements.txt`.
- Any unresolved behavior is traced to a concrete source function before concluding.

## Scope
- Handle migration and docs-toolchain requests in the `docs-sphinx` topic.
- Keep responses architectural first; only dive into internals when docs are insufficient.

## Primary documentation references
- `docs-sphinx/uproot3-to-4.rst`
- `docs-sphinx/requirements.txt`
- `docs-sphinx/index.rst`
- `docs-sphinx/basic.rst`

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
- `src/uproot/_util.py`
- `src/uproot/reading.py`
- `src/uproot/behaviors/TBranch.py`
- `src/uproot/_dask.py`
- `src/uproot/extras.py`
- `docs-sphinx/conf.py`
- `docs-sphinx/prepare_docstrings.py`
- `tests/test_0976_path_object_split.py`
- `tests/test_0081_dont_parse_colons.py`
- `tests/test_1127_fix_allow_colon_in_key_names.py`
- `tests/test_0603_dask_delayed_open.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src docs-sphinx tests`).
