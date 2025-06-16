# BCRP Series Downloader

Download and consolidate time series from the [Banco Central de Reserva del Perú (BCRP)](https://estadisticas.bcrp.gob.pe/estadisticas/series/ayuda/api) API. This tool scrapes metadata, downloads 16k+ series in async batches, and stores them as Parquet files for fast analytics.

---

## 🚀 Quick Start

1. **Install dependencies**

Clone the repository:
   ```sh
   git clone https://github.com/Tooruogata/bcrp-data-hub.git
   cd bcrp-data-hub
   ```

Set up the docker image:
   ```sh
   docker build -t bcrp-data-hub:latest -f .devcontainer/Dockerfile .
   docker run -dit --name bcrp-data-hub -v "$repopath:/workspace" -w /workspace bcrp-data-hub:latest
   ```

2. **Run script**

   ```python
   control_panel()  # Create control CSV from metadata

   await run_batch(
       df_control=df_control,
       formato="json",
       inicio="2000-1",
       fin="2025-12",
       idioma="esp",
       output_dir="../data/bronze/",
       batch_size=100
   )

   duckdb.sql("""
       COPY (
           SELECT * FROM read_parquet('../data/bronze/*.parquet')
       ) TO '../data/silver/series_all.parquet' (FORMAT PARQUET);
   """)
   ```

---

## 📁 Structure

```text
project/
├── data/
│   ├── bronze/         # Raw per-series files (Parquet)
│   ├── silver/         # Consolidated Parquet dataset
│   └── metadata/       # Control panel CSV
├── pull_data.ipynb     # Notebook to pull and download
└── README.md
```

---

## 🧰 Features

* ✅ Scrapes official BCRP metadata
* ⚡ Async download of 16k+ series
* 📁 Saves individual files to `bronze/`
* 🧹 Merges all into `silver/series_all.parquet`
* ⏱️ \~5 min for 16k series, \~50s to consolidate

---

## 🔍 Query Example

```python
import duckdb

df = duckdb.sql("SELECT * FROM read_parquet('../data/silver/series_all.parquet')").df()
```
