# One-Dimensional Bin Packing for E-commerce Fulfillment Using Ant Colony Optimization (ACO)

## Project Description

The "ACO.ipynb" notebook implements an Ant Colony Optimization  (ACO) algorithm to solve the one-dimensional bin packing problem. 
The objective is to pack items into bins so that no bin exceeds its capacity, and the total number of bins is minimized.

### Dataset

The dataset is located in the folder: Data/1-D Bin Packing.

Dataset source: https://people.brunel.ac.uk/~mastjjb/jeb/orlib/binpackinfo.html

Data files format: 
- Number of test problems on the first line
- For each test problem:
    - Problem identifier
    - Bin capacity, number of items, number of bins in current best known solution
    - Item sizes listed on separate lines


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

## Algorithm Description
The ACO algorithm has been implemented based on Singh and Baidya (2013):
- Path Construction: Each ant constructs a path containing pairs (i,j), representing the movement from one bin (i) to another one (j).
- Pheromone Update: Pheromones are updated based on a fitness function that considers the summed bin weights relative to capacity, raises the sum to a power m, and then divides it by the number of bins used by an ant.
- Heuristics: Items are selected based on pheromone intensity and item fit to the current bin.

## How to Run
1. Open the "ACO.ipynb" notebook.
2. Select Run All to execute all cells.

**Python version:** 3.12.4

**Libraries used:**
- \_\_future\_\_
- numpy
- random
- matplotlib
- multiprocessing
- time

**How to install the libraries:**
- pip install numpy pandas matplotlib


##  References
N. K. Singh and S. Baidya, "A novel work for bin packing problem by ant colony optimization," International Journal of Research in Engineering and Technology (IJRET), vol. 2, Special Issue 2, pp. 71–73, Dec. 2013. 
