# Optimize Classic Benchmark Functions using Particle Swarm Optimization (PSO)

## Project Description

The "PSO.ipynb" noteboook implements a Particle Swarm Optimization Algorithm to minimize four continuous functions.

### Benchmark Functions

**1. Sphere Function**

$$
f(x) = \sum_{i=1}^{n} x_i^2
$$

* Global minimum: x = 0, f(x) = 0
* Bounds: $x_i \in$  [-5.12, 5.12]


**2. Rosenbrock Function**

$$
f(x) = \sum_{i=1}^{n-1} [100(x_{i+1} - x_i^2)^2 + (x_i - 1)^2]
$$

* Global minimum: x = 1, f(x) = 0
* Bounds: $x_i \in$ [-5, 10]


**3. Rastrigin Function**

$$
f(x) = 10n + \sum_{i=1}^{n} [x_i^2 - 10\cos(2\pi x_i)]
$$

* Global minimum: x = 0, f(x) = 0
* Bounds: $x_i \in$ [-5.12, 5.12]

**4. Ackley Function**
   $$
   f(x) = -20 \exp\left(-0.2 \sqrt{\frac{1}{n}\sum_{i=1}^{n} x_i^2}\right)
          - \exp\left(\frac{1}{n}\sum_{i=1}^{n}\cos(2\pi x_i)\right)
          + 20 + e
   $$

* Global minimum: x = 0, f(x) = 0
* Bounds: $x_i \in$ [-32.768, 32.768]


**Dimensions tested: 2, 10 and 30.**

## Notebook Overview

The notebook is divided into three sections:

**1. Benchmark Functions Implementation and Example Plot.**

**2. Implement Standard PSO.**

* Run each benchmark function for dimensions 2, 10 and 30.

* Parameters:
   - Inertia weight: w = 0.7
   - Cognitive constant: c1 = 1.5
   - Social constant: c2 = 1.5
   - Swarm size: 
      - 30 for 2 and 10 dimensions
      - 50 for 30 dimensions.
   - Early stopping thresholds:
      - Sphere and Rosenbrock: $f(x) < 10^{(-8)}$
      - Rastigrin and Ackley: $f(x) < 10^{(-4)}$
   - Maximum number of iterations:
      - 300 for 2 dimensions
      - 500 for 10 dimensions
      - 1000 for 30 dimensions


* Results were aggregated over 30 runs.
* Success rate: fraction of runs where best final fitness meets threshold.
* Parameters study:
   - Study the effects of the parameters: 
      - inertia weight w: $w \in [0.4, 0.7, 0.9]$
      - cognitive constant c1: $c1 \in [1.0, 1.5, 2.0]$
      - social constant c2: $c2 \in [1.0, 1.5, 2.0]$


**3. Implement Hybrid PSO-Nedler Mead**
* Based on Konduru et al.(2006):
* Algorithm Steps:
   1. Randomly initialize particle positions and velocities
   2. Randomly initialize k cluster centers.
   3. Update particle positions using standard PSO equations.
   4. Evaluate the objective function for each particle.
   5. Update individual and global bests.
   6. Update velocities using standard PSO equations.
   8. Assign particles to the k clusters based on minimum distance.
   9. Apply the Nelder-Mead simplex method to each cluster.
   10. Repeat 3-8 until maximum iterations or early stop condition reached.
* Tested with the same parameters from section 2.

## Parallel Execution
- The 30 aggregated runs for each function and dimension were executed in parallel using `multiprocessing` to speed up computation. 
- Reproducible with fixed random seeds.


# How to run

1. Open the "PSO.ipynb" notebook.
2. Select Run All to execute all cells.


**Python version:** 3.12.4

**Libraries used:**
- \_\_future\_\_
- random
- math
- numpy
- matplotlib
- multiprocessing
- sympy
- pandas
- itertools

**How to install the libraries:**
- pip install numpy matplotlib sympy pandas






# References

Koduru, Praveen & Das, Sanjoy & Welch, Stephen. (2006). A Particle Swarm Optimization-Nelder Mead Hybrid Algorithm for Balanced Exploration and Exploitation in Multidimensional Search Space.. 2. 457-464. 