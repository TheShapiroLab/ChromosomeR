CRISPR Screening Analysis: ChrR CRISPRa and CRISPRi Screens during Growth in Fluconazole in Candida albicans

This folder contains the analysis pipeline for the pooled ChrR CRISPRa and CRISPRi screens. The two screens were
analyzed separately but identically (each has its own set of three notebooks that are run in order:
(QC_and_Barcoding --> Filtering_and_Statistics --> Plotting).

Many steps were not actually executed within the notebooks (the FastQC, read processing, and sgRNA counting
cells were copied into standalone .sh / .py files and submitted to SLURM on the ComputeCanada cluster). Cells that
are meant to run in the notebook are marked, and vice versa. The notebooks basically exist to keep the whole
workflow in one readable narrative.

Experimental design

16 libraries per screen: 4 conditions x 4 biological replicates.

| Group | Sample tags    | Timepoint | Condition     |
|-------|----------------|-----------|---------------|
| A     | A1, A2, A3, A4 | TP0       | YPD           |
| B     | B1, B2, B3, B4 | TP3       | YPD           |
| C     | C1, C2, C3, C4 | TP3       | Low FLZ       |
| D     | D1, D2, D3, D4 | TP3       | High FLZ      |

TP0 = the starting populations, TP3 = the end of the screen (see Methods). Low FLZ = YPD + 1ug/mL fluconazole,
High FLZ = YPD + 64ug/mL fluconazole, YPD = plain YPD.

The sgRNA library (sgrna_list.csv) contains a list of all of the ChrR-targeting sgRNAs, the 60 non-targeting control sgRNAs, 
and one "Uncloned" entry that represents reads from the empty plasmid backbone.

Pipeline

1. QC_and_Barcoding: counts raw reads, runs FastQC, then trims (VSEARCH), merges paired reads (PEAR), and dereplicates the reads.
sgRNAs are then counted by perfect match of the sgRNA sequence plus 5-nt flanking anchors (TTCGA...GTTTT). 
Missing counts are set to 0, and frequencies are calculated after adding a pseudocount of 1 to every count.
Outputs: sgrna_list.csv (counts are written back into this file), sgrna_list_no_NaN.csv, frequencies.csv

2. Filtering_and_Statistics: removes low-abundance sgRNAs. For each replicate, the threshold is the 5th percentile of sgRNA frequencies
plus the frequency of a single count in that replicate. An sgRNA is kept only if it meets the threshold in at least 3 of 4 replicates 
in every condition (TP0 YPD, TP3 YPD, TP3 Low FLZ, TP3 High FLZ). In practice, this works out to roughly 55-321 counts at TP0 and 2 
counts at TP3. For each passing sgRNA, log2 fold changes are calculated as log2(mean TP3 frequency / mean TP0 frequency), using only 
the replicates that passed the threshold. Significance is assessed with a two-sample t-test (TP3 vs. TP0) with Benjamini-Hochberg FDR 
correction applied separately for each condition. 
Outputs: filtered_gRNAs.csv, statistics_df_with_log2fc_and_fdr.csv

3. Plotting: an sgRNA is called significant if it has FDR < 0.05 AND a log2 fold change outside the 95% reference interval of the 
non-targeting sgRNAs (mean +/- 1.96 SD). FLZ hits are marked as "FLZ-unique" unless they are also significant in YPD in the same 
direction. We generate volcano plots for each condition and a summary table of enriched/depleted sgRNAs. The CRISPRa notebook also 
plots log2 fold change against ChrR position for the two FLZ conditions, with selected genes labelled. 
Outputs: volcano_plots/, chromosome_plots/ (CRISPRa only)

Directory layout

CRISPR_Screens/
├── CRISPRa/  (and CRISPRi/)
│   ├── data/                            # raw fastq.gz (available on SRA)
│   ├── ChrR_datasheet.csv               # sample metadata
│   ├── sgrna_list.csv                   # sgRNA library
│   ├── Map_Corrected_Positions.xlsx     # sgRNA positions and gene names (CRISPRa directory only)
│   ├── 1.QC_and_Barcoding/
│   │   ├── QC_and_Barcoding.ipynb
│   │   ├── logs/, fastqc_outputs/, pear_output/, vsearch_trim/, vsearch_aggregate/
│   │   ├── sgrna_list_no_NaN.csv
│   │   └── frequencies.csv
│   ├── 2.Filtering_and_Statistics/
│   │   ├── Filtering_and_Statistics.ipynb
│   │   ├── filtered_gRNAs.csv
│   │   └── statistics_df_with_log2fc_and_fdr.csv
│   └── 3.Plotting/
│       ├── Plotting.ipynb
│       ├── volcano_plots/
│       └── chromosome_plots/            # CRISPRa directory only
└── plotting/                            # Figures comparing the two screens
    ├── replicate_correlations/
    ├── flz_low_vs_high/
    └── screen_similarity/

The logs/, fastqc_outputs/, pear_output/, vsearch_trim/, and vsearch_aggregate/ folders are created by the QC_and_Barcoding
notebook and are not included in this repository.

Before running anything, replace --account=your-account in the SLURM headers.
