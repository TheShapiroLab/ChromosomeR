# Chromosome R

ChromosomeR: Code and Data for the Chromosome R Project in Candida albicans (Gervais et al. 2026)

This repository contains the analysis code (and processed data, where relevant) for the Chromosome R (ChrR) project,
which investigated how ChrR aneuploidy and the ORFs on ChrR contribute to fluconazole tolerance in Candida albicans. 
The analyses are split into four main directories. Each directory has its own README.txt
describing the experimental design, the pipeline, the directory layout, and how to run the code.

ChrR_CRISPR_Library_Stats/
    Validation of the ChrR CRISPRa and CRISPRi sgRNA libraries. Checks how well the sgRNAs were represented in the plasmid
    libraries and fungal libraries at the start of the screens.

CRISPR_Screens/
    Analysis of the pooled ChrR CRISPRa and CRISPRi screens in YPD and in low (1ug/mL) and high (64ug/mL) fluconazole.
    Each screen has its own three-step pipeline (read processing and sgRNA counting, filtering and statistics, and
    plotting), followed by a notebook comparing the two screens.

ChrR_Seq/
    RNA-seq analysis of euploid and ChrR-trisomic (AAB and ABB) strains in YPD and in fluconazole, all the way from read alignment
    to differential expression and plotting.

Growth_Profiling/
    Growth curve (AUC) and broth microdilution (IC50) analyses used to validate and follow up on hits from the screens,
    in fluconazole, posaconazole, and other stress conditions. All growth profiling data that did not involve any analysis/code
    can be viewed in the supplementary information in the manuscript.


-Raw sequencing reads are available on SRA and are not included in this repository. 
-Most of the sequencing analyses were run on the ComputeCanada cluster. Many of the heavy steps in those notebooks were
  copied into standalone .sh / .py / .R files and submitted to SLURM, so the notebooks just keep the whole workflow in one
  readable narrative.-
-Code is written in Python (Jupyter notebooks), with R used for some small parts of the RNA-seq analysis.

This repository is released under the MIT License (see LICENSE).
