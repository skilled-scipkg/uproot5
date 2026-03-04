# uproot documentation map: Getting Started

Use this map for first-contact ROOT I/O workflows before source inspection.

## Core onboarding docs
- `docs-sphinx/index.rst` | install options, package scope, and API overview.
- `docs-sphinx/basic.rst` | opening files, object discovery, `array()`/`arrays()`, iterate/concatenate basics, writing basics.
- `docs-sphinx/uproot3-to-4.rst` | migration-safe path rules (`file:object`, `pathlib.Path`, dict input) and API renames.

## Advanced extension doc
- `dev/custom-interpretation/README.md` | custom interpretation interface and registration flow.

## Companion behavior checks
- `tests/test_0009_nested_directories.py` | directory traversal and `keys()` behavior.
- `tests/test_0022_number_of_branches.py` | `array()`/`arrays()` filters, expressions, cuts.
- `tests/test_0081_dont_parse_colons.py` | colon handling in file/object parsing.
- `tests/test_0976_path_object_split.py` | parser edge cases for URLs and local paths.
- `tests/test_0167_use_the_common_histogram_interface.py` | histogram extraction and consistency.
