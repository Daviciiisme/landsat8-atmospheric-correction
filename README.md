# Files for `landsat8-atmospheric-correction` (README + env + .gitignore + download script)

---

## 1) `README.md`

```markdown
# Landsat8 Atmospheric Correction & Chl/TSI GUI

**Short**: GUI (PySimpleGUI) + scripts for calculating Chlorophyll-a and TSI from Landsat 8 SR bands.

## Structure
```

landsat8-atmospheric-correction/
├─ src/                 # Python code (main GUI: CTmod.py)
├─ notebooks/           # analysis & demo notebooks
├─ data/                # small sample metadata or links (do NOT push big rasters)
├─ models/              # trained models (optional, consider .gitignore)
├─ docs/
├─ environment.yml
├─ requirements.txt
├─ README.md
└─ .gitignore

````

## Quickstart (conda, recommended)
1. Clone repo
```bash
git clone https://github.com/<USERNAME>/landsat8-atmospheric-correction.git
cd landsat8-atmospheric-correction
````

2. Create conda env

```bash
conda env create -f environment.yml
conda activate env_ngaiK
```

3. Run GUI

```bash
python src/CTmod.py
```

## Quickstart (pip-only)

1. Create venv & install

```bash
python -m venv .venv
source .venv/bin/activate  # or .venv\Scripts\activate on Windows
pip install -r requirements.txt
python src/CTmod.py
```

## What the GUI does

* Crop Landsat SR bands (B2/B3) to an AOI (shapefile)
* Compute Chl-a (from Gr_B2) and TSI
* Export: `DN_value.xlsx`, `Chl.TIF`, `TSI.TIF`
* Optional: train and apply a Dense regressor (predict Chl from Gr_B2)

## Data & large files

**Do not** push large raster files (.tif) to GitHub. Two options:

* Use [Git LFS](https://git-lfs.github.com/) for large binaries (note quota limits).
* Host raw imagery externally (Google Drive / institutional server / Zenodo) and provide a `data/download_data.sh` to fetch sample data.

## Reproducibility

* `environment.yml` provided to recreate conda environment (recommended)
* `requirements.txt` available for pip users

## Contributing

* Fork -> branch -> PR
* Add minimal tests in `tests/` and CI (GH Actions) if possible

## License

MIT (choose license file or adjust as needed)

```
```

---

## 2) `environment.yml`

```yaml
name: env_ngaiK
channels:
  - conda-forge
  - defaults
dependencies:
  - python=3.10
  - pip
  - numpy
  - pandas
  - scipy
  - matplotlib
  - scikit-learn
  - jupyterlab
  - ipykernel
  - pillow
  - opencv
  - xarray
  - dask
  - rioxarray
  - rasterio
  - geopandas
  - shapely
  - pyproj
  - fiona
  - gdal
  - netcdf4
  - h5py
  - scikit-image
  - pyresample
  - cartopy
  - folium
  - tqdm
  - requests
  - openpyxl
  - pip:
    - PySimpleGUI
    - earthpy
    - tensorflow-cpu
```

---

## 3) `.gitignore`

```gitignore
# Python
__pycache__/
*.pyc
*.pyo
*.pyd
*.egg-info/
.env
.venv/

# Conda envs and local envs
env/
venv/
F:/Conda/envs/env_ngaiK/

# VSCode
.vscode/

# Data & outputs (do not commit large rasters)
data/
*.tif
*.TIF
*.img
*.bil
*.vrt

# Models & caches
*.h5
*.npz
models/

# Jupyter
.ipynb_checkpoints

# OS
.DS_Store
Thumbs.db
```

---

## 4) `download_data.sh` (example)

```bash
#!/usr/bin/env bash
# Small helper script to download example data for this project.
# Edit URLs to point to your data storage (Drive, Zenodo, S3, etc.)

set -e

OUT_DIR="data/sample"
mkdir -p "$OUT_DIR"

# Example placeholders - replace with real links
B2_URL="https://example.com/path/to/B2_crop.tif"
B3_URL="https://example.com/path/to/B3_crop.tif"
AOI_URL="https://example.com/path/to/aoi_shapefile.zip"

echo "Downloading sample B2..."
if command -v curl >/dev/null 2>&1; then
  curl -L "$B2_URL" -o "$OUT_DIR/B2_crop.tif"
else
  wget -O "$OUT_DIR/B2_crop.tif" "$B2_URL"
fi

echo "Downloading sample B3..."
if command -v curl >/dev/null 2>&1; then
  curl -L "$B3_URL" -o "$OUT_DIR/B3_crop.tif"
else
  wget -O "$OUT_DIR/B3_crop.tif" "$B3_URL"
fi

echo "Downloading AOI shapefile (zip)..."
if command -v curl >/dev/null 2>&1; then
  curl -L "$AOI_URL" -o "$OUT_DIR/aoi.zip"
else
  wget -O "$OUT_DIR/aoi.zip" "$AOI_URL"
fi

# unzip if present
if command -v unzip >/dev/null 2>&1; then
  unzip -o "$OUT_DIR/aoi.zip" -d "$OUT_DIR"
fi

echo "Done. Files saved to $OUT_DIR"
```

---

## 5) `download_data.ps1` (Windows PowerShell helper)

```powershell
# PowerShell script to download sample files (edit URLs)
$OutDir = "data\sample"
New-Item -ItemType Directory -Path $OutDir -Force | Out-Null

$B2 = "https://example.com/path/to/B2_crop.tif"
$B3 = "https://example.com/path/to/B3_crop.tif"
$AOI = "https://example.com/path/to/aoi_shapefile.zip"

Write-Host "Downloading B2..."
Invoke-WebRequest -Uri $B2 -OutFile (Join-Path $OutDir "B2_crop.tif")
Write-Host "Downloading B3..."
Invoke-WebRequest -Uri $B3 -OutFile (Join-Path $OutDir "B3_crop.tif")
Write-Host "Downloading AOI..."
Invoke-WebRequest -Uri $AOI -OutFile (Join-Path $OutDir "aoi.zip")

# Unzip if possible
if (Get-Command Expand-Archive -ErrorAction SilentlyContinue) {
  Expand-Archive -Path (Join-Path $OutDir "aoi.zip") -DestinationPath $OutDir -Force
}

Write-Host "Done. Files saved to $OutDir"
```

---

## 6) How to use these files

* Copy-paste the file contents into the corresponding files in your repo root (`README.md`, `.gitignore`, `environment.yml`) and `download_data.*` into `data/` or repo root.
* Edit URLs in download scripts to point to your actual data storage.
* Run `conda env create -f environment.yml` to create environment.

---

*File bundle created by ChatGPT for ngài K.*
