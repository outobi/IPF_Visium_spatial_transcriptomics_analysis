# Spatial Niche Dissection in IPF Visium Data

This repository demonstrates downstream spatial transcriptomics analysis of a processed human idiopathic pulmonary fibrosis (IPF) 10X Genomics Visium dataset.

Rather than reproducing the full preprocessing workflow, the analysis starts from a processed `AnnData` object containing spatial gene expression, scVI embeddings, cell2location-derived cell abundances, pathway activity scores, and NMF-defined tissue niches.

## Project goals

The analysis focuses on spatial organization around the fibrotic niche:

1. Explore the processed Visium dataset and existing annotations.
2. Examine cell2location-derived cell-type abundances and NMF-defined tissue niches.
3. Characterize cell, gene, and pathway gradients around fibrotic niches using Visium lattice distance.
4. Separate local fibrotic compositional state (fibrotic niche score in each spot) from spatial proximity to the fibrotic niche.
5. Test whether gene/pathway associations with fibrotic composition depend on local spatial context.

## Repository structure

```text
spatial-niche-dissection-analysis/
├── README.md
├── notebooks/
│   ├── 01_load_and_explore_processed_visium.ipynb
│   ├── 02_cell2location_nmf_niches.ipynb
│   ├── 03_fibrotic_niche_ring_gene_pathway_analysis.ipynb
│   ├── 04_fibrotic_niche_context_gene_pathway_analysis.ipynb
│   └── 05_fibrotic_niche_context_ring_interaction_gene_pathway_analysis.ipynb
├── data/
│   └── README.md
├── figures/
└── .gitignore
```

## Analysis workflow

### 01 — Load and explore processed Visium data

Inspect the processed `AnnData` structure, sample metadata, spatial coordinates, precomputed UMAP/scVI representations, tissue niche labels, and representative spatial gene-expression patterns.

### 02 — Cell2location and NMF-defined tissue niches

Explore cell2location-derived abundances for 37 cell populations and use NMF to characterize multicellular tissue niches, with particular emphasis on the fibrotic niche.

### 03 — Fibrotic niche ring analysis

Construct Visium hexagonal-lattice neighborhoods and define graph-distance rings around fibrotic spots.

Evaluate spatial gradients in:

- cell-type composition
- fibrosis-associated genes
- alveolar and airway markers
- pathway activity

### 04 — Fibrotic compositional context

Use a continuous fibrotic NMF score to quantify local fibrotic composition.

Model molecular readouts while accounting for spatial proximity and tissue section:

```text
gene/pathway ~ fibrotic NMF score + spatial ring + sample
```

### 05 — Spatial-context interaction analysis

Test whether the relationship between fibrotic composition and molecular activity changes with distance from the fibrotic niche:

```text
gene/pathway ~ fibrotic NMF score × spatial ring + sample
```

Representative pathways include TGFβ, EGFR, TNFα, MAPK, and NFκB.

## Data

Large processed `.h5ad` files are not tracked in this repository.

See [`data/README.md`](data/README.md) for information about the expected input object and required data fields.

## Key Python packages

- Scanpy
- AnnData
- pandas
- NumPy
- matplotlib
- scikit-learn
- statsmodels
- scipy

## Notes

This repository is intended as a downstream spatial-omics analysis demonstration. Preprocessing steps such as Space Ranger processing, scVI integration, and cell2location inference are not recomputed here.
