# Solving 0–1 Knapsack Problem Using Bees Algorithm (BA)

## Project Description

The "BA.ipynb" uses BA (bees algorithm) to solve the 0-1 
multidimensional knapsack problem:
- We are given a number of items, each of them with a certain value.
- We are given a set of constraints for each knapsack dimension. 
- The goal is to maximize the total value, while making sure that 
the knapsack is not overloaded on any dimension.


## Dataset

The Multidimensional Knapsack (MKP) data files were collected from 
the OR library (URL: https://people.brunel.ac.uk/~mastjjb/jeb/orlib/mknapinfo.html), 
and placed inside the "Data/" folder.

The following instances were utilized:
- The "SENTO*.txt" files were created by selecting the data instances corresponding to "SENTO1" and "SENTO2"(Senyu and Toyada, 1967) from the "mknap2.txt" file provided by the OR library.
- The  "WEING*.txt" files were created by selecting the data instances corresponding to "WEING1" and "WEING2"(Weingartner and Ness, 1967) from the "mknap2.txt" file provided by the OR library.
- The other two data files "mknapcb8.txt" and "mknapcb9.txt" contain problems solved in Chu and Beasley(1998), and have also been downloaded 
from the OR library.

### Dataset Description

The "SENTO.txt, "SENTO2.txt", "WEING1.txt" and "WEING2.txt" files have the following 
format:
- On the first line: Number of knapsack dimensions and number of objects.
- A list of values of length equal to the number of items/objects.
- A list equal to the number of knapsack dimensions, which shows the maximum capacity for each dimension.
- All dimensions have a list of constraints, equal to the number of objects. 
- The last line contains the optimal solution.

The "mknapcb8.txt" and mknapcb9.txt" files have the following format:
- The number of problems on the first line. Each file contains multiple problems.
- For each problem:
    - The number of items, number of knapsack dimensions, and optimal solution value (0 if not present) on the same line
    - A list of values of length equal to the number of items/objects.
    - Constraints: All dimensions have a list of constraints, equal to the number of objects. 
    - A list equal to the number of knapsack dimensions, which shows the maximum capacity for each dimension.
The optimal solution for these problems are in "mkbres.txt" file collected from the OR library.

In this implementation, the first problems from "mknapcb8.txt" and mknapcb9.txt" were utilized.

## Notebook/ Implementation Overview
The notebook contains the following sections:
- The BA Algorithm
- Helper Functions:
    - Read data from files.
    - Parallel execution of multiple runs
- Experimental results for 6 text problems: WEING1, WEING2, SENTO1, SENTO2, the first problem from "mknapcb8.txt, the first problem from "mknapcb9.txt"
    - Base execution with the parameters:
        - Size of neighborhood patch: ngh = 3
        - Number of scout bees: ns = 10
        - Number of best sites: nb = 5
        - Number of elite sites: ne = 3
        - Number of bees recruited for elite sites: nre = 5
        - Number of bees recruited for best non-elite sites: nrb = 2
        - Stagnation limit for site: stlim = 10
        - Iterations: max_iter = 200
    - Parameter study: 
        - Sizes of neighborhood patches:  [1, 3, 5]
        - Number of recruited bees for elite sites: [2, 5, 10]
        - Number of recruited bees for best non-elite sites: [1, 2, 3, 5]  
        - Note: valid combinations include only those in which the number of recruited bees for elite sites is higher than the 
        number of recruited bees for non-elite sites (nre > nrb).

Results were aggregated over 10 runs. 
Reproducibility: Each run of the algorithm uses a pseudo-random number generator initialized with seed = run number.

## How to Run
1. Open the "BA.ipynb" notebook.
2. Select Run All to execute all cells.

**Python version:** 3.12.4 (Recommended)

**Packages:**:
- numpy: 1.26.4
- matplotlib: 3.8.4
- pandas: 2.2.2

**How to install the libraries:**
- pip install numpy pandas matplotlib



## References
S. Senyu and Y. Toyada (1967) "An approach to linear programming with 0-1 variables." Management Science, 15:B196-B207.

H.M. Weingartner and D.N. Ness (1967) "Methods for the solution of the multi-dimensional 0/1 knapsack problem." Operations Research, 15:83-103.

P.C.Chu and J.E.Beasley "A genetic algorithm for the multidimensional knapsack problem", Journal of Heuristics, vol. 4, 1998, pp63-86.
