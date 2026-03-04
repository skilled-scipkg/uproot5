# uproot source map: Docs Sphinx

Use this map only after the docs in `doc_map.md` are insufficient.

## Fast source navigation
- `rg -n "file_object_path_split|def open\(|def dask\(" src/uproot`
- `rg -n "uproot3|colon|pathlib|dask|lazy" docs-sphinx src/uproot tests`

## Function-level source entry points
- `src/uproot/_util.py` | `file_object_path_split` (line 297), the core of colon/path parsing behavior.
- `src/uproot/reading.py` | `open` (line 27), including `pathlib.Path` and object-path dispatch semantics.
- `src/uproot/behaviors/TBranch.py` | `iterate`/`concatenate` API behavior referenced by migration docs.
- `src/uproot/_dask.py` | `dask` (line 37), modern replacement path for legacy lazy workflows.
- `src/uproot/extras.py` | optional dependency gateways and actionable import errors.
- `docs-sphinx/conf.py` | Sphinx configuration used in local docs builds.
- `docs-sphinx/prepare_docstrings.py` | preprocessing step invoked by docs configuration.

## Behavior-level check files
- `tests/test_0976_path_object_split.py` | parser edge cases across local paths, URLs, and protocols.
- `tests/test_0081_dont_parse_colons.py` | regression checks for colon-safe input handling.
- `tests/test_1127_fix_allow_colon_in_key_names.py` | object-name/path colon behavior (local and dataset-backed cases).
- `tests/test_0603_dask_delayed_open.py` | deferred reading semantics used in migration guidance.

## Practical validation commands
- Setup once in repo: `python -m pip install -e .`
- Local/offline migration parser checks: `python -m pytest -q tests/test_0976_path_object_split.py`
- Docs build sanity check: `python -m pip install -r docs-sphinx/requirements.txt && python -m sphinx -b html docs-sphinx docs-sphinx/_build/html`
