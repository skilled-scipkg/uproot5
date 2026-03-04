# uproot source map: Build and Install

Use this map only after the docs in `doc_map.md` are insufficient.

## Fast source navigation
- `rg -n "def (iterate|concatenate|num_entries_for|dask|recreate|update|mktree)" src/uproot`
- `rg -n "report=|step_size|open_files|library=" tests/test_*.py`

## Function-level source entry points
- `src/uproot/behaviors/TBranch.py` | top-level `iterate` (line 52), `concatenate` (line 238), `HasBranches.iterate` (line 1258), `num_entries_for` (line 1869), `arrays` (line 735).
- `src/uproot/reading.py` | `open` dispatch and handler selection (line 27).
- `src/uproot/_dask.py` | `dask` graph construction entry point (line 37).
- `src/uproot/models/TTree.py` | `num_entries` helper used by `uproot.num_entries` (line 945).
- `src/uproot/writing/writable.py` | `recreate` (line 86), `update` (line 144), `mktree` (line 1256), `WritableTree.extend` (line 1871).
- `src/uproot/__init__.py` | public API exports (`iterate`, `concatenate`, `dask`, `num_entries`).

## Behavior-level check files
- `tests/test_0043_iterate_function.py` | iterate chunking/report semantics and `num_entries_for` checks.
- `tests/test_0044_concatenate_function.py` | concatenate backend parity (`np`, `ak`, `pd`).
- `tests/test_0609_num_enteries_func.py` | top-level `uproot.num_entries` API behavior.
- `tests/test_0603_dask_delayed_open.py` | dask delayed-open behavior and chunk/division checks.
- `tests/test_0578_dask_for_numpy.py` | dask vs eager array consistency for numpy backend.
- `tests/test_1406_improved_rntuple_methods.py` | local iterate/concatenate smoke checks without external files.

## Practical validation commands
- Setup once in repo: `python -m pip install -e .`
- Local/offline iterate+concatenate check: `python -m pytest -q tests/test_1406_improved_rntuple_methods.py::test_iterate_and_concatenate`
- Full API regression set (requires `skhep_testdata` and optional dask deps): `python -m pytest -q tests/test_0043_iterate_function.py tests/test_0044_concatenate_function.py tests/test_0609_num_enteries_func.py tests/test_0603_dask_delayed_open.py`
