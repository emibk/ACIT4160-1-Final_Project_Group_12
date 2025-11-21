# One-Dimensional Bin Packing for E-commerce Fulfillment Using Ant Colony Optimization (ACO)

## Project Description

The "ACO.ipynb" notebook implements an Ant Colony Optimization (ACO) algorithm to solve the one-dimensional bin packing problem. 
The objective is to pack items into bins so that no bin exceeds its capacity, and the total number of bins is minimized.

### Dataset

The dataset is located in the folder: Data/1-D Bin Packing.

The data used for this implementation are the 1-D Bin Packing instances (Falkenauer, 1994) available 
from the OR-Library (URL: https://people.brunel.ac.uk/~mastjjb/jeb/orlib/binpackinfo.html): 
binpack1.txt, binpack2.txt, binpack3.txt, binpack4.txt, binpack5.txt, binpack6.txt, 
binpack7.txt, binpack8.txt.

Data files format: 
- Number of test problems on the first line
- For each test problem:
    - Problem identifier
    - Bin capacity, number of items, number of bins in current best known solution
    - Item sizes listed on separate lines

Exact data instances used in this implementation:
| Problem Identifier | Data File Source | Capacity | Number of Items |
|-------------------|-----------------|---------|----------------|
| u120_00           | binpack1.txt    | 150     | 120            |
| u250_01           | binpack2.txt    | 150     | 250            |
| u500_02           | binpack3.txt    | 150     | 500            |
| u1000_03          | binpack4.txt    | 150     | 1000           |
| t60_04            | binpack5.txt    | 100     | 60             |
| t120_05           | binpack6.txt    | 100     | 120            |
| t249_06           | binpack7.txt    | 100     | 249            |
| t501_07           | binpack8.txt    | 100     | 501            |

## Notebook Overview
The notebook contains the following sections:
- ACO implementation
- Helper Functions
    - Read data from files 
    - Parallel execution of multiple runs
    - Execute runs, aggregate and plot results
- Experimental results for 8 test problems, each selected from one of the data files:
    - Base execution with the parameters:
        - Number of ants: 5
        - Number of best: 5 (to update pheromone)
        - Number of iterations: 50
        - Decay: 0.9
        - Alpha: 1
        - Beta:1
    - Parameter variations:
        - Increased number of ants: 15
        - Increased iterations: 100
        - Decreased decay: 0.5
        - Test alpha values: [0.5, 2]
        - Test beta values: [0.5, 2]

Results were aggregated over 20 runs. Plots contain convergence of best solutions over iterations and load distribution across bins in the best solution over runs.

Reproducibility: Each run of the algorithm uses a pseudo-random number generator initialized with the run number as the seed. 

## Algorithm Description
The ACO algorithm has been implemented based on Singh and Baidya (2013):
- Path Construction: Each ant builds a solution iteratively by constructing a path, in which it moves from one 
item to another. The solution is represented by three components:
    - Edges: The moves of the ant are represented by (i,j) pairs, showing the movement from one item i to another one j. This is a graph-like representation, in which items are nodes connected by edges.
    - Number of bins: Each solution stores the total number of bins utilized to store items.
    - Bin weights: Each solution stores a list of bin weights, which is represented by the sum of item sizes added to each bin.
- Pheromone Update: Pheromones are updated based on a fitness function that considers the summed bin weights relative to capacity, raises the sum to a power m, and then divides it by the number of bins used by an ant.
- Heuristics: Items are selected based on pheromone intensity and item size.

## How to Run
1. Open the "ACO.ipynb" notebook.
2. Select Run All to execute all cells.

**Python version:** 3.12.4 (Recommended)

**Packages:**:
- numpy: 1.26.4
- matplotlib: 3.8.4

**How to install the libraries:**
- pip install numpy matplotlib


##  References

E.Falkenauer (1994) "A Hybrid Grouping Genetic Algorithm for Bin Packing" Working paper CRIF Industrial Management and Automation, CP 106 - P4, 50
av. F.D.Roosevelt, B-1050 Brussels, Belgium, email: efalkena@ulb.ac.be.

N. K. Singh and S. Baidya, "A novel work for bin packing problem by ant colony optimization," International Journal of Research in Engineering and Technology (IJRET), vol. 2, Special Issue 2, pp. 71–73, Dec. 2013. 
