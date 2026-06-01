--AUC Calculations--

The code for calculating AUCs for all growth curve assay experiments is available and annotated at each step in the Jupyter notebook (AUC_Calculations.ipynb).

All data were collected via liquid growth curve assays at 37°C, taking OD600 measurements at 20-minute intervals for 24h. The code uses the trapezoidal rule from numpy (ie. numpy.trapezoid) on the raw OD600 readings to approximate the area under the corresponding growth curve, and generates some rough plots for quick visualization.

Dependencies: pandas, numpy, matplotlib, seaborn, openpyxl, pathlib

The notebook expects data to be organized in sub-directories relative to the working directory, one per experiment (exactly how the current repository is structured). For example:

    AUC_Calculations/
    ├─ AUC_Calculations.ipynb
    ├─ MMS/
    │   ├── Data.xlsx
    │   └── Plan.xlsx
    └─ ...

-Data.xlsx contains the corresponding raw OD readings from the plate reader. Wells are rows (eg. A1-H12),
    timepoints (in hours) are columns.
-Plan.xlsx has the late layout with one row per well. Required columns:
        well: well identifier matching Data.xlsx (eg. A1)
        strain: strain name or label
        medium: condition or growth medium label

MMS = Data from Figure XXXX.
H2O2 = Data from Figure XXXX.
NaCl = Data from Figure XXXX.
SORB = Data from Figure XXXX.

To analyze a new dataset, change only the single line in Cell 5 to reflect the sub-directory with your data in it:
EXPERIMENT_DIR = "MMS"

You also need to set the T_END (usually a couple of hours after growth for most strains plateaus):
    T_END = 18

AUC is calculated using the trapezoidal rule (numpy.trapezoid) on OD readings up to T_END. The trapezoidal rule approximates the area under a curve by summing trapezoids between consecutive timepoints.
