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

- Python 3.[x]
- [pymoo](https://pymoo.org) [version], with the modified `pinsga2.py` from this repository (see Installation)
- NumPy, pandas, Matplotlib
- Tkinter (included with most Python installations; needed for the manual DM)

Exact versions are listed in `requirements.txt`.

---

## Installation

```bash
git clone https://github.com/aditisinghxd/Landuse_optimization_using_PI-NSGA-II.git
cd Landuse_optimization_using_PI-NSGA-II
pip install -r requirements.txt
```

**Replace the pymoo PI-NSGA-II file.** This project needs the modified
`pinsga2.py`. Copy it over the file in your pymoo installation:

```bash
python -c "import pymoo, os; print(os.path.join(os.path.dirname(pymoo.__file__), 'algorithms', 'moo'))"
# copy update_pinsga_file_in_pymoo-pi-emo/pinsga2.py into the folder printed above
```

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
experimental design tables (automated DM: Table [3]; manual DM: Table [4]).
Automated DM configurations were repeated 30 times and manual DM scenarios
10 times. [The result files used in the article are provided in `results/`.]

---

## Citation

The associated article is not yet published. Until then, please cite this
repository:

> Singh, A. (2026). *Spatially Explicit PI-NSGA-II for Land-Use Allocation*
> [software]. GitHub. https://github.com/aditisinghxd/Landuse_optimization_using_PI-NSGA-II

The full article citation and DOI will be added here after publication.

---

## License

This project is licensed under the [MIT] License (see `LICENSE`).
The file `update_pinsga_file_in_pymoo-pi-emo/pinsga2.py` is a modified version
of pymoo's PI-NSGA-II implementation and remains under pymoo's Apache License 2.0.

## Contact

Aditi Singh – aditi.singh@ufz.de/aditisinghxds@gmail.com
