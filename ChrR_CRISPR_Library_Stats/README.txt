Validation of the ChrR CRISPRa and CRISPRi Plasmid and Fungal Libraries

This notebook checks how well the sgRNA libraries are represented, both in the plasmid libraries and in the
fungal (SC5314) libraries at the start of the screens (TP0). Many steps (FastQC, read processing, and sgRNA
counting) were copied into standalone .sh / .py files and submitted to SLURM on the ComputeCanada cluster.

Samples

| Library             | Sample tags            | Replicates |
|---------------------|------------------------|------------|
| Plasmid (CRISPRa)   | R1, R2, R3             | 3          |
| Plasmid (CRISPRi)   | CR1, CR2, CR3          | 3          |
| SC5314 (CRISPRa)    | A1a, A2a, A3a, A4a     | 4          |
| SC5314 (CRISPRi)    | A1i, A2i, A3i, A4i     | 4          |

Sample metadata and read counts are stored in ChrR_datasheet.csv, and the sgRNA library is in sgrna_list.csv.

Reads are trimmed (VSEARCH), merged (PEAR), and dereplicated, then counted by exact match of the sequence
between the flanking anchors (AATTTCGA...GTTTTAGA) to the sgRNA library. An sgRNA is considered present in a
fungal library if it has at least 50 counts in every replicate. The plasmid libraries were sequenced much less
deeply, so there an sgRNA must have at least 10 counts per million (CPM) in every replicate instead.

Outputs: sgrna_list.csv (counts are written back into this file), sgrna_list_no_NaN.csv, frequencies.csv,
counting_qc_stats.csv, fig_dropouts.png, fig_sgrna_cpm_distributions_by_library.png

Directory layout

ChrR_CRISPR_Library_Stats/
├── QC_and_Barcoding.ipynb
├── ChrR_datasheet.csv
├── sgrna_list.csv
├── data/                    # raw fastq.gz (available on SRA)
└── logs/, fastqc_outputs/, pear_output/, vsearch_trim/, vsearch_aggregate/   # created by the notebook

Before running anything, replace --account=your-account in the SLURM headers.
