# `dataset/` — Sea Green Data, All in One Place

This folder is the single home for everything data-related in this
project: the raw source files, the provenance record for them, and a
small representative sample. Everything downstream (the dashboard, the
model training) is derived from what's in here.

## Layout

```
dataset/
├── README.md            — this file
├── PROVENANCE.md         — source, product ID, coverage, and checksum for every raw file
├── sample_dataset.csv    — 25-row sample of the exact table every model trains on
└── raw/                  — the actual raw source files (gitignored -- large)
    ├── CHECKSUMS.sha256
    ├── Sea Surface temp.nc
    ├── Sea surface salinity.nc
    ├── cmems_mod_glo_bgc-pft_anfc_0.25deg_P1D-m_*.nc
    ├── cmems_mod_glo_phy-cur_anfc_0.083deg_P1D-m_*.nc
    ├── cmems_mod_glo_phy-thetao_anfc_0.083deg_P1D-m_*_0.49-902.34m_*.nc
    ├── cmems_mod_glo_phy-thetao_anfc_0.083deg_PT6H-i_*.nc
    └── MOSDAC/
        └── 3RIMG_*_L2B_SST_V02R00.h5   (17 files)
```

## The flow, end to end

1. **`raw/`** — real MOSDAC (ISRO) + CMEMS (Copernicus Marine) files, downloaded
   directly from the source institutions. See `PROVENANCE.md` for exactly which
   product, institution, coverage, and time range each file is, plus a
   per-file sha256 checksum.
2. **`real/real_data.py`** and **`real/real_training.py`** read straight out of
   `dataset/raw/` (resolved relative to the repo root, so it works no matter
   where a script is run from).
3. **`real/export_real_data.py`** and **`real/export_real_results.py`** compile
   that raw data into the compact JSON the dashboard actually serves
   (`public/data_real.json`, `public/data.json`) — see the root `README.md`'s
   "Real data: MOSDAC and CMEMS" section for the exact commands.
4. **`sample_dataset.csv`** is a 25-row, human-readable sample of the exact
   table produced by step 2/3, small enough to open directly on GitHub without
   downloading anything — see the root `DATASET.md` for what each column means.

## Raw files are not committed

The files in `raw/` are gitignored (100s of MB to ~800 MB each, ~1.9 GB total)
— version control isn't the right place for them. What's committed instead:

- This directory's `PROVENANCE.md` and `raw/CHECKSUMS.sha256`, so the
  real-data claim can be verified without needing the files themselves.
- The compact derived JSON the dashboard actually serves (`public/data.json`,
  `public/data_real.json`).
- `sample_dataset.csv`, a small direct sample of the real training table.

**Full raw dataset download:** <!-- add the Google Drive link here once uploaded -->
_(not yet uploaded — add the link here)_

Once downloaded, verify integrity with:

```bash
cd dataset/raw
sha256sum -c CHECKSUMS.sha256
```
