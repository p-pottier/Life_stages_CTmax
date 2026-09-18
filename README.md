# Warming tolerance of fish is remarkably consistent across life stages

This repository holds the data, code and outputs for Cowan et al. (2026). We measured the
critical thermal maximum (CTmax) of 2,206 individuals from eight fish species at four life
stages: embryo, larva, immature (juvenile or immature adult) and spawner. We then compared our
estimates with the compilation of Dahlke et al. (2020) for the six species common to both datasets:
Atlantic salmon, brook trout, sand goby, three-spined stickleback, black goby and
zebrafish.

**Citation:** Cowan et al. (2026). Full citation and DOI to be added on publication.


## Repository structure

```
Life_stages_CTmax/
├── README.md
├── R/
│   ├── Life_stages_CTmax.qmd
│   └── Life_stages_CTmax.html
├── data/
│   ├── Life_stages_project_all_data.xlsx
│   └── Experimental_and_imputed_tolerance_data_Dahlke_et_al_2020.xlsx
├── output/
│   ├── AllSpecies_clean.csv
│   ├── sample_sizes.csv
│   ├── brms_model.rds
│   ├── model_diagnostics.csv
│   ├── overall_lifestage_means.csv
│   ├── overall_lifestage_contrasts.csv
│   └── species_lifestage_contrasts.csv
└── fig/
    ├── Figure1_CTmax_life_stages.jpg
    ├── Figure1_CTmax_life_stages.pdf
    ├── Figure2_dahlke_comparison.jpg
    └── Figure2_dahlke_comparison.pdf
```

## Files

### `R/`

| File | Contents |
|---|---|
| `Life_stages_CTmax.qmd` | Quarto document that runs the full analysis. It cleans the raw data for each species, fits the model, estimates life-stage means and contrasts, and draws both figures. |
| `Life_stages_CTmax.html` | Rendered version of the document, with code, tables, model diagnostics and figures. It opens in any web browser and needs no other files. |

### `data/`

| File | Contents |
|---|---|
| `Life_stages_project_all_data.xlsx` | Raw CTmax measurements, with one worksheet per species and one row per individual. Its `Readme` worksheet defines every column of every worksheet. |
| `Experimental_and_imputed_tolerance_data_Dahlke_et_al_2020.xlsx` | Thermal tolerance limits of fish species and life stages from Dahlke et al. (2020). The analysis uses the `Tmax` worksheet. |

### `output/`

The analysis document writes all tables. Temperatures and differences are in °C. 

| File | Contents |
|---|---|
| `AllSpecies_clean.csv` | Cleaned data for the eight species, one row per individual (2,206 rows). Columns: `Species`, `LifeStage`, `CTmax`, `UniqueTrialID` and `Ramping_rate` (°C h⁻¹). |
| `sample_sizes.csv` | Number of individuals per species and life stage. |
| `brms_model.rds` | Fitted model, saved as a `brmsfit` R object (31 MB). The analysis document loads this file instead of refitting the model. |
| `model_diagnostics.csv` | Sampler convergence diagnostics across all parameters: R-hat, bulk and tail effective sample sizes, divergent transitions, maximum tree-depth hits and E-BFMI. `Status` reads OK when the value meets its `Threshold` and CHECK otherwise. |
| `overall_lifestage_means.csv` | Mean CTmax of each life stage across species. The column `emmean` holds the posterior median. `lower.HPD` and `upper.HPD` bound the 95% highest posterior density (HPD) interval, the narrowest interval that holds 95% of the posterior. |
| `overall_lifestage_contrasts.csv` | Pairwise differences in CTmax between life stages across species, with 95% HPD intervals. A `contrast` of "Embryo - Larva" gives the embryo value minus the larva value. |
| `species_lifestage_contrasts.csv` | Pairwise differences in CTmax between life stages within each species, with 95% HPD intervals. We did not assay brook trout larvae, so the three brook trout contrasts involving larvae are model predictions based on Bayesian data augmentation |

### `fig/`

| File | Contents |
|---|---|
| `Figure1_CTmax_life_stages.jpg`, `.pdf` | Figure 1. CTmax of each individual across life stages in the eight species. Black points and bars show the mean and 95% confidence interval, calculated from the raw data. Letters show which life stages differ within each species, based on 95% HPD intervals of the model contrasts. |
| `Figure2_dahlke_comparison.jpg`, `.pdf` | Figure 2. Comparison with Dahlke et al. (2020) for the six shared species. Panel a shows posterior medians and 95% credible intervals from our model beside the measured and imputed values of Dahlke et al. Panels b and c summarise differences between life stages across species. |

JPG files are 300 dpi; PDF files are vector graphics.

## Reproducing the analysis

1. Install R, Quarto, CmdStan and the R packages listed under Software. The `cmdstanr` package is
   not on CRAN; follow the installation guide at <https://mc-stan.org/cmdstanr/>.
2. From the repository root, run `quarto render R/Life_stages_CTmax.qmd`, or open the file in
   RStudio and click Render. All paths are relative to the repository root.
3. The render loads the fitted model from `output/brms_model.rds`. It rewrites the other six
   files in `output/` and all files in `fig/`, so copy them first if you want to compare results.
4. brms refits the model when `output/brms_model.rds` is missing, or when the data, formula or
   priors change. The fit took about 2 h 20 min on four cores.

## Software

We produced the results in this repository with the versions below. The last section of
`R/Life_stages_CTmax.html` lists the full session information.

| Software | Version |
|---|---|
| R | 4.4.2 |
| Quarto | 1.9.37 |
| CmdStan | 2.38.0 |
| brms | 2.23.0 |
| cmdstanr | 0.9.0.9000 |
| emmeans | 1.11.0-001 |
| posterior | 1.6.1 |
| coda | 0.19-4.1 |
| multcompView | 0.1.10 |
| readxl | 1.4.3 |
| dplyr | 1.1.4 |
| tidyr | 1.3.1 |
| ggplot2 | 4.0.2 |
| patchwork | 1.3.2 |
| knitr | 1.51 |
| rmarkdown | 2.31 |

## Licence

CC-BY 4.0

## References

Dahlke, F. T., Wohlrab, S., Butzin, M., & Pörtner, H.-O. (2020). Thermal bottlenecks in the life
cycle define climate vulnerability of fish. *Science*, 369(6499), 65–70.
<https://doi.org/10.1126/science.aaz3658>
