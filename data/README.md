# Data

The project uses the **CGMacros v1.0.0** dataset (PhysioNet). The notebooks read the raw
dataset in place and write every derived output to `artifacts/`.

## Default location

Extract the dataset so that this directory exists:

```text
data/raw/cgmacros-v1.0.0/
├── CGMacros/
│   ├── CGMacros-001/
│   │   ├── CGMacros-001.csv
│   │   └── photos/
│   ├── CGMacros-002/
│   │   └── ...
│   ├── ...                         (45 participant directories in total)
│   ├── bio.csv
│   ├── gut_health_test.csv
│   └── microbes.csv
├── DataDictionary.pdf
├── DataDictionary_Bio.csv
├── DataDictionary_CGMacros-00X.csv
├── DataDictionary_Gut_Health_Test.csv
├── DataDictionary_Microbes.csv
├── LICENSE.txt
└── SHA256SUMS.txt
```

This directory is the **dataset root**. All locations are resolved by `src/paths.py`; the
notebooks do not hard-code data paths.

## Using a dataset stored elsewhere

Set `CGMACROS_DATA_DIR` to the dataset root (the directory that contains `CGMacros/` and the
`DataDictionary*` files, not the `CGMacros/` directory itself) before starting Jupyter:

```bash
export CGMACROS_DATA_DIR=/path/to/cgmacros-v1.0.0          # Linux / macOS
```

```powershell
$env:CGMACROS_DATA_DIR = "D:\Datasets\cgmacros-v1.0.0"      # Windows PowerShell
```

If `CGMACROS_DATA_DIR` is unset or empty, `data/raw/cgmacros-v1.0.0/` is used. If required files
are missing, `src.paths.validate_data_root()` raises a `FileNotFoundError` that lists them.

## Keep the raw files unchanged

The raw files are read-only inputs. Do not edit, re-save, re-encode or convert their line
endings: the data audit verifies them against `SHA256SUMS.txt`, and several files (the data
dictionary CSVs and the three static tables) use CRLF line endings in the release. Editors and
spreadsheet programs can silently rewrite these bytes.

To check the files against the release checksums (the release ZIP listed in `SHA256SUMS.txt` is not
part of the extracted dataset and is reported as missing):

```bash
cd data/raw/cgmacros-v1.0.0 && sha256sum -c SHA256SUMS.txt
```

When the raw files are tracked by Git, `.gitattributes` marks `data/raw/**` as `-text` so that
Git never converts their line endings on commit or checkout.
