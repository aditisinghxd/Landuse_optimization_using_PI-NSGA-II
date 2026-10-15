# Spatially Explicit PI-NSGA-II for Land-Use Allocation

Python implementation of a spatially explicit, constrained version of the
Progressively Interactive Non-dominated Sorting Genetic Algorithm II (PI-NSGA-II)
for raster-based land-use allocation. Decision maker (DM) preferences are
learned progressively during the optimization and used to steer the search
toward preferred regions of the Pareto front.

> **Status:** The article describing this software is currently under review.
> Citation details will be added here once it is published.

This code accompanies the manuscript:

> Singh, A., Kropp, I., Kaim, A. *Interactive Multi-Objective Optimization
> Algorithm for Spatially Explicit Land-Use Allocation Problems*.
> Manuscript submitted for publication.

It builds on the Master's thesis *"Preference-based Multi-Objective Optimization
of Land-Use Allocation using PI-NSGA-II"* (M.Sc. Data Science,
Friedrich-Alexander-Universität Erlangen-Nürnberg).

---

## Features

- Spatial land-use optimization on raster maps (ASCII grid input)
- Patch-based genome representation for scalability
- Constraint handling via
  - land-use transition matrix
  - minimum–maximum area-share constraints
  - constraint-aware mutation with repair
- Two objectives: crop yield (CY) and forest species richness (SR)
- Two DM modes
  - **Automated DM**: preferences based on Euclidean distance to a reference point
  - **Manual DM**: pairwise comparison of candidate solutions in a graphical interface (Tkinter)

---

## Repository structure

```
landusepinsga2.py                     Main script: configuration, constraints, optimization run
helper/
  toy_models.py                       Objective functions (crop yield, forest species richness)
  genome_to_full_map.py               Converts a patch-based genome back to a land-use map
  gui_input.py                        Decision window for the manual DM
  plot_landuse_share.py               Land-use share plots
  timestamp_utils.py                  Timing utilities
input/
  10x10/ 20x20/ 30x30/ 40x40/ 100x100/
                                      land_use.asc and soil_fertility.asc per map size
  transition_matrix.txt               Allowed land-use transitions
  min_max.txt                         Minimum and maximum area shares per land-use class
processing/                           Intermediate files (original map, patch-ID map)
output/                               Results (Pareto-front plots and CSV files)
update_pinsga_file_in_pymoo-pi-emo/
  pinsga2.py                          Modified PI-NSGA-II implementation for pymoo
```

---

## Requirements

- Python 3.9
- [pymoo-pi-emo](https://github.com/Human-Centered-Optimization-Lab/pymoo-pi-emo), branch `feature/pinsga2`
  (pymoo 0.6.0.1 with PI-NSGA-II), installed with the modified `pinsga2.py` from this repository (see Installation).
  Its dependencies (NumPy, SciPy, Matplotlib, etc.) are installed automatically.
- pandas
- Tkinter (included with most Python installations; needed for the manual DM)
- Recommended: Anaconda or Miniconda for managing the environment

---

## Installation

1. Clone this repository:

   ```bash
   git clone https://github.com/aditisinghxd/Landuse_optimization_using_PI-NSGA-II.git
   cd Landuse_optimization_using_PI-NSGA-II
   ```

2. Create and activate a conda environment:

   ```bash
   conda create -n pinsga2env python=3.9 -y
   conda activate pinsga2env
   ```

3. Clone pymoo-pi-emo and switch to the PI-NSGA-II branch:

   ```bash
   git clone https://github.com/Human-Centered-Optimization-Lab/pymoo-pi-emo.git
   cd pymoo-pi-emo
   git checkout feature/pinsga2
   ```

4. Replace `pymoo-pi-emo/pymoo/algorithms/moo/pinsga2.py` with the file
   `update_pinsga_file_in_pymoo-pi-emo/pinsga2.py` from this repository.
   The modified version passes the candidate genomes to the decision step so
   that the manual DM window can display the corresponding land-use maps.

5. Install the modified pymoo and pandas:

   ```bash
   pip install .
   conda install pandas -y
   cd ..
   ```

For later use, activate the environment (`conda activate pinsga2env`) and run
the script as described below. Step-by-step instructions are also provided in
`Land use optimization using PI-NSGA-II.pdf`.

---

## Usage

1. Open `landusepinsga2.py` and set the parameters at the top of the file:

   | Parameter    | Description                                              | Default   |
   |--------------|----------------------------------------------------------|-----------|
   | `map_size`   | `10x10`, `20x20`, `30x30`, `40x40` or `100x100`          | `10x10`   |
   | `pop_size`   | Population size                                          | `40`      |
   | `max_gen`    | Maximum number of generations                            | `200`     |
   | `dm_period`  | DM period τ (generations between DM calls)               | `50`      |
   | `points`     | Number of candidate points compared per DM call (η)      | `5`       |
   | `type_of_dm` | `'automated'` or `'manual'`                              | `'manual'`|
   | `x, y`       | Reference point for the automated DM (CY, SR)            | `90, 10`  |
   | `seeds`      | Random seed                                              | `6111321` |

2. Run:

   ```bash
   python landusepinsga2.py
   ```

3. With the manual DM, a decision window opens at every DM call. Choose the
   preferred option, or select "Can't make a decision".

4. Results are written to `output/`:
   - `pinsga2plot<map_size>_<timestamp>.png`: final Pareto front
   - `pinsga2<map_size>_<timestamp>.csv`: objective values and solutions

---

## Reproducing the results in the manuscript

The experiments in the manuscript use the parameter settings listed in its
experimental design tables (automated DM: Table 2; manual DM: Table 3).
Automated DM configurations were repeated 30 times and manual DM scenarios
10 times. The results can be reproduced with the provided code and input data.

---

## Citation

The associated article is not yet published. Until then, please cite this
repository:

> Singh, A. (2026). *Spatially Explicit PI-NSGA-II for Land-Use Allocation*
> [software]. GitHub. https://github.com/aditisinghxd/Landuse_optimization_using_PI-NSGA-II

The full article citation and DOI will be added here after publication.

---

## License

Copyright (c) 2025 Aditi Singh. All rights reserved until publication of the
associated article (see `LICENSE`). The code may be used for review and
reproducibility purposes. An open-source license will be applied upon publication.

The file `update_pinsga_file_in_pymoo-pi-emo/pinsga2.py` is a modified version of
`pinsga2.py` from [pymoo-pi-emo](https://github.com/Human-Centered-Optimization-Lab/pymoo-pi-emo)
(a fork of [pymoo](https://github.com/anyoptimization/pymoo)) and is licensed under
the Apache License 2.0 (see `update_pinsga_file_in_pymoo-pi-emo/LICENSE-APACHE`).

## Contact

Aditi Singh – aditi.singh@ufz.de / aditisinghxds@gmail.com
