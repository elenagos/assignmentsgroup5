# Assignment 2
## Repository overview 
This folder contains the files used for Assignment 2. The assignment focuses on the analysis and interpretation of maximal reaction activities in the E.coli core metabolic model.
### Files
- `analysis2.ipynb` — main Jupyter Notebook that contains all the data analysis and answers to the tasks
- `e_coli_core_expression.csv` — dataset 
- `README.md` 
## 2. How to run
### Requirements
- Python 3
- Jupyter Notebook/JupyterLab
- pandas
- numpy
- matplotlib
- cobra
## 3. Task 1 — Visualization and interpretation of maximal reaction activities
The provided maximal reaction activity data were visualized on the Escher map of the E. coli core metabolic model.
The values of maximal reaction activities can differ as 2 consequent reactions may have different maximums.
The Escher visualization also showed grey reactions with two different types of values:
- "0.00" — the reaction has a defined maximal activity that is equal to zero;
- "no data" — no maximal reaction activity is determined for this reaction.
## 4. Task 2 — Activity-constrained metabolic model

- Loaded the E. coli core metabolic model from computer practical;
- Applied activity constraints only to reactions for which maximal activity data were available;
- For reversible reactions, set the flux bounds to `[-value, +value]`.
- For irreversible reactions we keep the existing lower bound and set the upper bound to the corresponding maximal activity value(if exists);
- Stored the original lower bound of the ATP maintenance reaction after applying the activity constraints;
- Reset the glucose exchange reaction to the high default bounds `[-1000, 1000]` as was stated in assignment;
- Created a final table containing every reaction ID together with its resulting lower and upper flux bounds to verify the implemented constraints.
### Preview of the table with reaction bounds

| Reaction | Lower bound | Upper bound |
|----------|------------:|------------:|
| PFK      | 0.0         | 13.1        |
| PFL      | 0.0         | 0.0         |
| PGI      | -11.1       | 11.1        |
| PGK      | -24.0       | 24.0        |
| PGL      | 0.0         | 7.3         |
...

