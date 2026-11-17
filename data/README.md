# Data

The files the notebook reads are not committed. When the terms let you republish them, they are assets on the GitHub release tagged `data`. The release is public. When the terms let you download the files but not republish them, do not create that release. Write the command that downloads them from the source, and say what it needs (an account, a key in `.env`, a path).

`OWNER/REPO` is your fork, the form `username/aii600-final-project`. In the clone, once:

```bash
gh repo set-default OWNER/REPO
```

Create the release from the project root, after the files are in this folder:

```bash
gh release create data data/your-file.csv --repo OWNER/REPO --title "Data" --notes "See data/README.md"
```

Add every file the notebook reads. When a file changes:

```bash
gh release upload data data/your-file.csv --repo OWNER/REPO --clobber
```

On another machine:

```bash
gh release download data --dir data --repo OWNER/REPO
```

Record a SHA-256 for each file. Replacing a file with `--clobber` does not show up in git, so update the hash in the same commit:

```bash
shasum -a 256 data/your-file.csv
```

Replace the lines below.



## Source

The dataset consists of New York City Taxi and Limousine Commission (TLC) Yellow Taxi Trip Records from January through August 2026.

Official data source: https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page

The dataset contains yellow taxi trip records, including pickup timestamps, drop-off timestamps, pickup locations, drop-off locations, trip distances, and fare information.

## Files

The following eight Parquet files are required:

- `yellow_tripdata_2026-01.parquet`
- `yellow_tripdata_2026-02.parquet`
- `yellow_tripdata_2026-03.parquet`
- `yellow_tripdata_2026-04.parquet`
- `yellow_tripdata_2026-05.parquet`
- `yellow_tripdata_2026-06.parquet`
- `yellow_tripdata_2026-07.parquet`
- `yellow_tripdata_2026-08.parquet`

## Download Instructions

Download the eight monthly Yellow Taxi Trip Records from the official NYC TLC website and save them in the `data/` directory.

Alternatively, run the following Python script from the project root:

```python
from pathlib import Path
from urllib.request import urlretrieve

data_dir = Path("data")
data_dir.mkdir(exist_ok=True)

base_url = "https://d37ci6vzurychx.cloudfront.net/trip-data"

for month in range(1, 9):
    filename = f"yellow_tripdata_2026-{month:02d}.parquet"
    destination = data_dir / filename

    if not destination.exists():
        urlretrieve(f"{base_url}/{filename}", destination)
        print(f"Downloaded {filename}")
    else:
        print(f"Already exists: {filename}")
```

The notebook reads these files locally from `data/`.

## Data Processing

The analysis filters pickup locations to Manhattan and aggregates individual trips into hourly pickup counts.

January–June 2026 is used for training, July 2026 for validation, and August 2026 for testing.

## Data Verification

Each downloaded file must be verified using SHA-256 checksums. Record the expected checksum for each file after confirming the downloaded data is correct.

### SHA-256 Checksums

The following SHA-256 checksums were generated from the eight NYC TLC Yellow Taxi Trip Records Parquet files used in this project.

| Dataset | SHA-256 Checksum |
|---|---|
| `yellow_tripdata_2026-01.parquet` | `8b3933fe6f0d7b6d8826613c0dd724edc680ff7c49e2bd4c7635c05102728637` |
| `yellow_tripdata_2026-02.parquet` | `5352ca800f29c40a221ed5b0c6392c451fde7fcd236a73651bd5aea74713a26c` |
| `yellow_tripdata_2026-03.parquet` | `d7af794a7ac06cbbb5afb4d1df9e86d84f2e878584de708ff2ffca4bd7d234d9` |
| `yellow_tripdata_2026-04.parquet` | `e03b82d5722a6b21d4fbce011afd8837495202c0b5db77ff0d8d15dd80834ba8` |
| `yellow_tripdata_2026-05.parquet` | `9aa5a1609e2bf07d9051b7d530de05b1019e12a560ecb2c59c137c8b3a8b6750` |
| `yellow_tripdata_2026-06.parquet` | `f340c62a18885c56176433c9db28e97d0a8a14e6e49869276ad8afb20ebf088b` |
| `yellow_tripdata_2026-07.parquet` | `3260bb629c2f334055f8a566cbb02c15476b15120c45d7ad0aae46fc4bb06612` |
| `yellow_tripdata_2026-08.parquet` | `6683a025443afcbb28ac4f238fcbb6334767dc69fab4a16b1eb16a78dcd013cd` |

These checksums allow anyone reproducing the project to verify that their downloaded files match the exact datasets used in the analysis.

## Attribution

Data provided by the New York City Taxi and Limousine Commission (TLC).

The original Parquet files are not committed to the Git repository. They are obtained from the official source using the download instructions above.


---

- Source, and whether the terms let you republish the files.
- File names. On the `data` release, or the command that downloads them from the source.
- SHA-256 of each file.
- Unit of observation, sample size, and time span.
