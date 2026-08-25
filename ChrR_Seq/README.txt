RNA-Seq Analysis: ChrR Trisomy and the Fluconazole Response in Candida albicans

This notebook contains the RNA-seq analysis pipeline. Many heavy steps were not actually executed inside the notebook (the bash and R cells were copied into standalone .sh / .R files and submitted to SLURM on the ComputeCanada cluster). Cells that are meant to run in the notebook are marked, and vice versa. The notebook basically exists to keep the whole workflow in one readable narrative.

Experimental design

18 libraries: 3 genotypes x 2 conditions x 3 biological replicates.

| Group | Sample tags | Genotype | Condition |
|-------|-------------|----------|-----------|
| A | A1, A2, A3 | WT (euploid) | YPD |
| B | B1, B2, B3 | ChrR AAB (trisomic) | YPD |
| C | C1, C2, C3 | ChrR ABB (trisomic) | YPD |
| D | D1, D2, D3 | WT (euploid) | FLZ |
| E | E1, E2, E3 | ChrR AAB (trisomic) | FLZ |
| F | F1, F2, F3 | ChrR ABB (trisomic) | FLZ |

AAB and ABB represent the two ChrR trisomy configurations (ie. which homolog of ChrR is present in two
copies). FLZ = YPD + 64ug/mL fluconazole, YPD = just YPD.

Directory layout

Paths in this notebook assume the following structure. Python cells run from the project
root, the PCA/PERMANOVA R script runs from plotting/, and the alignment and counting scripts
run from aligning/.

ChrR_Seq/
├── data/                          # raw fastq.gz
├── aligning/
│   ├── star_index/
│   ├── calb_features.gtf
│   ├── calb_genome.fasta
│   ├── results/<SAMPLE>_STAR/     # STAR output, one directory per sample
│   ├── counts.txt                 # featureCounts output
│   └── edgeR_results/             # one .txt per comparison
└── plotting/
    ├── pca_PERMANOVA.R
    ├── pca_permanova_results/
    ├── chrR_box_plots/
    ├── volcano_plots/
    │   ├── ChrR_highlighted/
    │   └── gene_coordinates/
    └── interaction_checks/

Before running anything, replace --account=your-account and --mail-user=your@email in the SLURM headers.
