# Spatial Niche Dissection in IPF Visium Data

This repository presents a downstream spatial-transcriptomics analysis of a processed human idiopathic pulmonary fibrosis (IPF) 10x Genomics Visium dataset.

The workflow starts from a processed `AnnData` object rather than repeating upstream preprocessing. The object contains spatial gene expression, scVI representations, cell2location-derived cell-abundance estimates, transcriptional signaling-pathway activity scores, and precomputed NMF-defined tissue niches.

## Project goals

The analysis examines how cellular and molecular programs are organized around the fibrotic tissue niche:

1. Inspect the processed Visium dataset and its existing annotations.
2. Characterize cell2location-derived cell populations and precomputed NMF tissue niches.
3. Measure cell-type, gene-expression, and signaling-pathway gradients using Visium lattice distance from Fibrotic spots.
4. Separate local fibrotic compositional state from spatial proximity to the Fibrotic niche.
5. Test whether associations between fibrotic composition and molecular activity change across spatial contexts.

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

Inspect the `AnnData` structure, sample metadata, spatial coordinates, scVI/UMAP representations, tissue-niche labels, cell-abundance matrices, signaling-pathway scores, and representative spatial gene-expression patterns.

### 02 — Cell2location abundances and NMF-defined tissue niches

Explore precomputed cell2location abundance estimates for 37 lung cell populations and the existing NMF-derived multicellular tissue niches. Visualize discrete niche assignments, continuous fibrotic NMF intensity, representative cell-abundance maps, and niche-level compositional enrichment.

This notebook does not rerun cell2location or NMF.

### 03 — Fibrotic niche ring analysis

Construct the native six-neighbor Visium lattice separately within each IPF tissue section. Fibrotic-niche spots are assigned distance zero, and all other connected spots are assigned their shortest graph distance from the nearest Fibrotic spot.

The notebook describes spatial gradients in:

- cell-type composition;
- fibrosis-associated, alveolar, and airway genes;
- transcriptional signaling-pathway activity.

Measurements are summarized within `sample × ring` before visualization. Heatmaps use feature-wise z-scores, while line plots retain section-level variation.

### 04 — Additive fibrotic-context models

Quantify local fibrotic composition using the relative contribution of the stored fibrotic-associated NMF factor. Fit spot-level additive models for 10 genes and 14 signaling pathways:

```text
standardized gene/pathway
    ~ standardized fibrotic NMF score
    + standardized ring distance
    + sample
```

These models estimate the average association of each molecular readout with fibrotic composition while adjusting for spatial distance and tissue-section baseline differences. No interaction terms are included in Notebook 04.

### 05 — Fibrotic score × spatial context

Test whether the association between fibrotic composition and molecular activity changes across discrete spatial rings:

```text
standardized gene/pathway
    ~ standardized fibrotic NMF score × spatial ring
    + sample
```

The notebook reports joint interaction tests, ring-specific adjusted slopes with 95% confidence intervals, and nine-point sample-balanced pattern plots. Featured signaling pathways include:

- TGF-β signaling pathway;
- MAPK signaling pathway;
- EGFR signaling pathway;
- TNF-α signaling pathway;
- Hypoxia signaling pathway;
- NF-κB signaling pathway.

## Interpretation

The workflow distinguishes two related biological axes:

- **Fibrotic composition:** the local fibrotic-like cellular state of an individual Visium spot.
- **Spatial proximity:** the spot's lattice distance from the nearest discrete Fibrotic niche.

Notebook 03 describes how molecular features vary across space, Notebook 04 estimates their mutually adjusted additive associations, and Notebook 05 tests whether fibrotic-score coupling itself changes with spatial context.

The analyses are exploratory. Sample fixed effects control section-level baseline differences, but they do not fully account for within-section spatial autocorrelation or pseudoreplication. Results should therefore be interpreted as adjusted spatial associations rather than causal effects.

## Data

Large processed `.h5ad` files are intentionally excluded from Git tracking. The notebooks expect the processed object at:

```text
data/adata_vis_human_spatial_paper.h5ad
```

See [`data/README.md`](data/README.md) for the expected input structure and required fields.

## Running the notebooks

Run the notebooks sequentially from `01` through `05`. Each notebook reads the same processed object and reconstructs the variables needed for its analysis.

Core Python packages include:

- Scanpy and AnnData;
- pandas and NumPy;
- SciPy;
- matplotlib and seaborn;
- statsmodels.

## Scope

This repository is intended as a reproducible downstream spatial-omics demonstration. Upstream steps—including Space Ranger processing, scVI integration, cell2location inference, and the original NMF fitting—are not recomputed here.
