# uproot documentation map: Docs Sphinx

Use this map for migration support, docs-toolchain setup, and doc/source consistency checks.

## Migration and user-facing docs
- `docs-sphinx/uproot3-to-4.rst` | authoritative migration map (Uproot 3 -> 4/5 semantics).
- `docs-sphinx/index.rst` | top-level documentation index and install pointers.
- `docs-sphinx/basic.rst` | current API examples referenced during migration triage.

## Docs build inputs
- `docs-sphinx/requirements.txt` | required Sphinx dependencies.
- `docs-sphinx/conf.py` | docs configuration and extension setup.
- `docs-sphinx/prepare_docstrings.py` | docstring preprocessing used by docs build.
- `docs-sphinx/make_changelog.py` | changelog generation helper for docs releases.

## Companion behavior checks
- `tests/test_0976_path_object_split.py` | path/object split edge cases used in migration questions.
- `tests/test_0081_dont_parse_colons.py` | regression checks for colon parsing behavior.
- `tests/test_0603_dask_delayed_open.py` | modern replacement patterns around deferred reading.
