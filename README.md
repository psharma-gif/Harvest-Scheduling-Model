# Harvest-Scheduling-Model
# Forest Harvest Scheduling Optimization

An Excel linear programming project that develops a harvest schedule for a hypothetical 5,800-acre forest. The model allocates acreage among two harvest periods and a no-harvest option to maximize discounted net harvest revenue while meeting forest management constraints.

## Project objective

Determine how many acres from each site and initial age class should be:

- Harvested during years 0–10.
- Harvested during years 10–20.
- Left unharvested throughout the 20-year planning horizon.

## Data and assumptions

The numerical inputs come from Exercise 2 in Chapter 12, “A Basic Harvest Scheduling Model,” of the supplied Forest Resource Management teaching material, printed page 258.

| Parameter | Assumption |
|---|---:|
| Forest area | 5,800 acres |
| Site classes | 2 |
| Initial age classes | 0–10, 11–20, and 21–30 years |
| Planning horizon | 20 years |
| Planning period | 10 years |
| Assumed harvest timing | Years 5 and 15 |
| Real discount rate | 4% |
| Stumpage price | $30 per cord |
| Timber sale preparation | $15 per acre + $0.30 per cord |
| Replanting cost | $150 per harvested acre |

The data are hypothetical textbook inputs, not measurements from an actual property.

## Optimization approach

The model has 18 continuous decision variables:

2 site classes × 3 initial age classes × 3 management options.

Each decision variable represents acres assigned to a management option.

### Objective function

Maximize total discounted net harvest revenue:

PV net revenue per acre =
[(stumpage price × yield) − sale preparation costs − replanting cost]
÷ (1 + discount rate)^harvest year

Total discounted net revenue =
SUMPRODUCT(PV net revenue per acre, acres assigned)

### Constraints

1. **Land availability:** Acreage assigned to the three options equals the available acreage in each site–age group.
2. **Harvest flow:** Each decade’s timber volume must fall between 90% and 110% of the other decade’s volume.
3. **Ending forest age:** Acreage-weighted average age at the end of year 20 must be at least 17 years.
4. **Non-negativity:** All acreage decisions must be zero or greater.

The harvest-flow wording is applied in both directions. Fractional acreage is permitted.

## Base results

| Output | Optimized result |
|---|---:|
| Discounted net harvest revenue | $1,789,932.17 |
| Timber harvested in period 1 | 56,500.89 cords |
| Timber harvested in period 2 | 51,364.44 cords |
| Ending average forest age | 17.00 years |

The ending-age constraint and the upper limit on period-1 harvest volume relative to period 2 are binding.

![Optimized harvest schedule](images/harvest_schedule.png)

## Scenario analysis

Five cases were solved independently:

- Base assumptions.
- A 2% real discount rate.
- A 6% real discount rate.
- A stumpage price of $24 per cord.
- A stumpage price of $36 per cord.

The scenario comparison shows how changes in financial assumptions affect the optimized results.

![Scenario comparison](images/scenarios.png)

## How to use the Excel model

1. Open `harvest_scheduling.xlsx` in desktop Excel.
2. Enable the Solver add-in.
3. Follow the workbook’s `Solver guide` sheet.
4. Set the objective to `Results!B11` and select **Max**.
5. Set changing cells to `Model!M7:M24`.
6. Add the listed acreage, harvest-flow, ending-age, and non-negativity constraints.
7. Select **Simplex LP** and solve.

Detailed settings and formulas are in `SOLVER_AND_FORMULAS.md`.

Changing inputs recalculates financial results for the existing schedule. Rerun Solver to obtain a new optimized schedule.

## Validation and reproduction

The saved solution was independently calculated using Python’s SciPy/HiGHS linear programming solver.

Validation checked:

- Acreage balances for all six land groups.
- Harvest-flow inequalities.
- Minimum ending average age.
- Nonnegative acreage decisions.
- Agreement between workbook calculations and the independent solution.

Excel’s native Solver was not run during package creation.

To reproduce the independent optimization, run:

python solve.py

Required Python packages: NumPy, SciPy, and Matplotlib.

## Repository contents

- `harvest_scheduling.xlsx` — Excel model and results.
- `SOLVER_AND_FORMULAS.md` — Solver setup and formula guide.
- `data/` — Input CSVs, optimized schedule, and scenario results.
- `images/` — Charts and workbook previews.
- `solve.py` — Independent optimization and chart generation.
- `solution.json` — Full-precision base solution.

## Limitations

This model uses aggregated acreage and fixed yield tables. It excludes whole-stand harvest restrictions, spatial adjacency, road access, operational capacity, taxes, annual overhead, and terminal inventory value.

The financial output is discounted net harvest revenue over the planning horizon, not total timberland market value or land expectation value.

This is a learning project demonstrating harvest scheduling concepts and Excel optimization. It does not represent professional experience with Remsoft or Woodstock.
