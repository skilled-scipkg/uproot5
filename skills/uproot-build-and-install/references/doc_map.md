# uproot documentation map: Build and Install

Use this map when the user asks for API-level behavior, environment setup, or scale-up patterns.

## Core docs
- `docs-sphinx/index.rst` | installation channels (`pip`, `conda-forge`) and package entry points.
- `docs-sphinx/basic.rst` | read/iterate/concatenate workflows, backend selection (`ak`/`np`/`pd`), write APIs.
- `README.md` | project-level install notes and contributor-oriented context.

## Migration and compatibility doc
- `docs-sphinx/uproot3-to-4.rst` | semantics that still affect current usage (`Path`, colon parsing, API replacements).

## Companion behavior checks
- `tests/test_0043_iterate_function.py` | iterate semantics, `report=True`, `num_entries_for` expectations.
- `tests/test_0044_concatenate_function.py` | concatenate behavior across backends.
- `tests/test_0609_num_enteries_func.py` | top-level `uproot.num_entries` behavior.
- `tests/test_0603_dask_delayed_open.py` | delayed open and chunk accounting for dask workflows.
- `tests/test_1406_improved_rntuple_methods.py` | local iterate/concatenate checks without external ROOT files.
