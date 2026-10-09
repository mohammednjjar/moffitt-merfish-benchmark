# Data

The data are not included in this repository. Download the three raw files
below into `data/raw/`, keeping these exact file names:

| File | Contents | Source |
|---|---|---|
| `Moffitt_and_Bambah-Mukku_et_al_merfish_all_cells.csv` | Cell-by-gene table, 1,027,848 cells (≈1.03 GB) | Dryad, https://doi.org/10.5061/dryad.8t8s248 |
| `cellboundaries_example_animal.csv` | Segmentation polygons, one animal (73,655 rows) | | `cellboundaries_example_animal.csv` | Segmentation polygons, one animal (73,655 rows) | Zenodo, inside `supplemental_data_ssam_2019.zip` (31.5 GB), supplementary data of Park et al. (2021): https://doi.org/10.5281/zenodo.3478502 |
| `merfish_barcodes_example.csv` | Decoded molecules, one section (3,739,360 rows) | Same Zenodo archive as above | |
| `merfish_barcodes_example.csv` | Decoded molecules, one section (3,739,360 rows) | | `cellboundaries_example_animal.csv` | Segmentation polygons, one animal (73,655 rows) | Zenodo, inside `supplemental_data_ssam_2019.zip` (31.5 GB), supplementary data of Park et al. (2021): https://doi.org/10.5281/zenodo.3478502 |
| `merfish_barcodes_example.csv` | Decoded molecules, one section (3,739,360 rows) | Same Zenodo archive as above | |

Then run `notebooks/01_data_cleaning.ipynb`, which writes to `data/clean/`:

| File | Rows | Used by |
|---|---:|---|
| `cells_cleaned.csv` | 874,768 | `02_benchmark.ipynb` |
| `cells_boundaries_clean.csv` | 35,522 | `02_benchmark.ipynb` |
| `boundaries_cleaned.csv`, `cells_with_boundaries.csv` | — | intermediate |
| `barcodes_cleaned.csv` | 1,920,853 | not used for modelling |
