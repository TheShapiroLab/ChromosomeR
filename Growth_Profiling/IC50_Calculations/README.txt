--IC50 Analysis--

The code for calculating IC50s for all experiments is available and annotated at each step in the Jupyter notebook (IC50s.ipynb).

All data were collected via broth microdilution assays, followed by taking OD600 measurements after 24h of growth. 
The code fits a four-parameter logistic dose-response model to each replicate and outputs the corresponding fit parameters, and some rough plots for quick visualization.

Dependencies: pandas, numpy, matplotlib, seaborn, scipy, openpyxl

The notebook expects data to be organized in sub-directories relative to the working directory, one per experiment (exactly how the current repository is structured). For example:

IC50_Calculations/
├─ IC50s.ipynb
├─ ComboA_FLZ/
│   ├─ plate1.csv
│   ├─ plate2.csv
│   └─ plate3.csv
└─ ...

Each CSV represents one replicate plate or combined set of plates. 
Rows are strains (lab names are listed ["fRS] but common names corresponding to these can be seen in the Supplemental Tables, and columns are the drug concentrations in µg/mL. 
Cell values are raw OD600 readings.

To analyze a new dataset, change only the single line in Cell 3 to reflect the sub-directory with your data in it:
DATA_DIR = 'ComboA_FLZ'

Also make sure to update the drug_columns variable in Cell 4 to reflect your drug concentration gradient values:
drug_columns = ['64', '32', '16', '8', '4', '2', '1', '0.5', '0.25', '0.125', '0.0625', '0']

IC50 values were estimated by fitting the four-parameter logistic (4PL) model: f(x) = c + (d−c) / (1 + (x/e)^b). 
Here, c and d are the lower and upper asymptotes (ie. the concentrations that elicit the maximum and maximally-inhibited OD600 responses, respectively), e is the IC50 (concentration at half-maximal inhibition), and b is the Hill slope describing curve steepness. 
Model fitting is performed independently for each replicate, and IC50 values that fall outside of the tested concentration range are excluded as implausible. The actual reported IC50 values represent the mean across three biological replicates. 
This is mathematically equivalent to the four-parameter logistic model used by GraphPad Prism (https://www.graphpad.com/guides/prism/6/curve-fitting/reg_dr_inhibit_variable.htm) that handles raw drug concentrations rather than log-transformed values as the input.
