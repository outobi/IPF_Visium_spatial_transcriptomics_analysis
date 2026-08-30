# Data

The notebooks in this repository use a previously processed human IPF Visium `AnnData` object.

The large `.h5ad` file is intentionally not tracked in Git.

## Expected local structure

```text
data/
├── processed_ipf_visium.h5ad
└── README.md
```

The notebooks assume the processed object is available locally at:

```python
data/processed_ipf_visium.h5ad
```

Adjust the path in the notebook if your local file is stored elsewhere.

## AnnData contents used in this project

The processed object includes:

### `adata.obs`

Sample and spatial metadata:

- `sample`
- `patient`
- `treatment`
- `array_row`
- `array_col`
- `leiden_25`
- `Niche_NMF`

Cell2location-derived abundance estimates for 37 cell populations, including:

- AT1
- AT2
- Aberrant basaloid
- Alveolar fibroblast
- Myofibroblast
- Macrophage C1Q hi
- Macrophage SPP1-associated populations
- Smooth Muscle
- Pericyte
- T cells
- and additional lung cell populations

Pathway activity features include:

- TGFb
- EGFRsignaling
- MAPK
- NFkB
- PI3K
- TNFa
- Hypoxia
- WNT
- VEGF
- JAK-STAT
- and others

### `adata.obsm`

Relevant matrices include:

- `X_scVI`
- `X_umap`
- `spatial`
- `q05_cell_abundance_w_sf`
- `mlm_estimate`

### Other data

The object also contains:

- spatial gene-expression data
- tissue-image metadata
- Visium spatial coordinates
- precomputed neighborhood graphs
- NMF-related annotations

## Data provenance

This project reuses a processed public IPF Visium dataset for downstream methodological demonstration.

The repository does not redistribute the original large processed data object.
