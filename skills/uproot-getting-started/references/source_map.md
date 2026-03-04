# uproot source map: Getting Started

Use this map only after the docs in `doc_map.md` are insufficient.

## Fast source navigation
- `rg -n "open\(|array\(|arrays\(|show\(|typenames\(|file_object_path_split\(" src`
- `rg -n "uproot\.open\(|\.arrays\(|\.array\(" tests/test_*.py`

## Function-level source entry points
- `src/uproot/reading.py` | `open` (line 27), `ReadOnlyDirectory.keys` (line 1577), `ReadOnlyDirectory.classnames` (line 1675).
- `src/uproot/behaviors/TBranch.py` | `show` (line 635), `arrays` (line 735), `typenames` (line 1630), `array` (line 2098).
- `src/uproot/_util.py` | `file_object_path_split` (line 297) for `file:object` disambiguation.
- `src/uproot/extras.py` | optional backend import gateways (`pandas`, `hist`, dask extras).
- `src/uproot/interpretation/custom.py` | custom interpretation contracts when default decoding is insufficient.

## Behavior-level check files
- `tests/test_0009_nested_directories.py` | directory key traversal and object lookup.
- `tests/test_0022_number_of_branches.py` | branch expression/filter behavior for `arrays()`.
- `tests/test_0081_dont_parse_colons.py` | colon parsing regression coverage.
- `tests/test_0099_read_from_file_object.py` | file-like object read path.
- `tests/test_0976_path_object_split.py` | exhaustive path/object split edge cases.
- `tests/test_0167_use_the_common_histogram_interface.py` | histogram read behavior (`to_numpy`/axis/value access).

## Practical validation commands
- Setup once in repo: `python -m pip install -e .`
- Local/offline parser check: `python -m pytest -q tests/test_0976_path_object_split.py`
- Full read regression set (requires `skhep_testdata`): `python -m pytest -q tests/test_0022_number_of_branches.py tests/test_0081_dont_parse_colons.py`
