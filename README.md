# CI2025_lab2
Implementation of Genetic Algorithm (GA) and Evolution Strategy (ES) for solving the Traveling Salesman Problem (TSP)
This repository contains two complete implementations for solving the Traveling Salesman Problem (TSP) using evolutionary computation techniques:
1. Genetic Algorithm (GA)
2. Evolution Strategy (ES)

Both are implemented in Python and adapted to handle the permutation-based nature of TSP, where each candidate solution represents a tour visiting every city exactly once.

### Repository Structure:
📦 TSP-Evolutionary-Algorithms

 ┣ 📁 lab02/                --->  .npy problem instances (distance matrices)
 
 ┣ 📁 CSV Results/          --->  CSV outputs (GA and ES results)
 
 ┣ 📄 genetic_algorithm.py  --->  Complete GA implementation
 
 ┣ 📄 evolution_strategy.py ---> Complete ES implementation (μ+λ / μ,λ)
 
 ┗ 📜 README.md             


## Algorithms Implemented

### 1. Genetic Algorithm (GA)

The Genetic Algorithm (GA) implemented here follows a standard generational model with elitism, focusing on strong selection pressure.

#### Key Characteristics & Operators:

* **Architecture:** **Generational GA with Elitism.** A fixed-size population is maintained. The top 10% of the fittest individuals (elites) are passed directly to the next generation, and the remaining 90% are filled by offspring.
* **Parent Selection:** **Tournament Selection** (Size $k=3$). This introduces strong selection pressure, favoring fitter individuals for reproduction.
* **Crossover:** **Order Crossover (OX)**. Essential for permutation problems like the TSP, ensuring every city is visited exactly once.
* **Mutation:** **Swap Mutation**. Randomly swaps two cities in the tour to introduce variation.
* **Initialization:** **Hybrid Seeding** (80% Greedy, 20% Random) to ensure a strong starting point.

#### Parameters Used:

| Parameter | Value | Description |
| :--- | :--- | :--- |
| **Pop\_Size** | 100 | Total number of individuals in the population. |
| **Generations** | 500 | Total number of generations to evolve. |
| **Tourn\_Size** | 3 | Size of the tournament selection pool ($k=3$). |
| **Cross\_Rate** | 0.9 | Probability that two parents will perform crossover. |
| **Mut\_Rate** | 0.3 | Probability that an offspring will undergo mutation. |
| **Elitism** | 0.1 | The top 10% of the population are preserved as elites. |


### Results Summary
| File Name           | Best Fitness | Time (s) |
| ------------------- | ------------ | -------- |
| problem_g_10.npy    | 1497.66      | 0.3729   |
| problem_g_100.npy   | 4341.47      | 1.2154   |
| problem_g_1000.npy  | 14060.33     | 13.8574  |
| problem_g_20.npy    | 1755.51      | 0.4805   |
| problem_g_200.npy   | 6318.06      | 2.2618   |
| problem_g_50.npy    | 2880.75      | 0.7555   |
| problem_g_500.npy   | 9692.40      | 6.1487   |
| problem_r1_10.npy   | 184.27       | 0.3896   |
| problem_r1_100.npy  | 746.97       | 1.2637   |
| problem_r1_1000.npy | 2552.25      | 14.1457  |
| problem_r1_20.npy   | 340.86       | 0.4684   |
| problem_r1_200.npy  | 1112.71      | 2.2447   |
| problem_r1_50.npy   | 579.56       | 0.7475   |
| problem_r1_500.npy  | 1753.46      | 6.3148   |
| problem_r2_10.npy   | -411.70      | 0.3800   |
| problem_r2_100.npy  | -4686.43     | 1.2697   |
| problem_r2_1000.npy | -49409.79    | 13.6941  |
| problem_r2_20.npy   | -861.67      | 0.4541   |
| problem_r2_200.npy  | -9593.84     | 2.2174   |
| problem_r2_50.npy   | -2232.38     | 0.7639   |
| problem_r2_500.npy  | -24515.12    | 5.9786   |
| test_problem.npy    | 2823.79      | 0.4758   |


---

### 2. Evolution Strategy (ES)

The Evolution Strategy (ES) implemented here follows the canonical **ES ($\mu$ + $\lambda$)** model. Crucially, this implementation uses a "purer" ES philosophy by delaying selection pressure until the survivor selection phase.

#### Key Characteristics & Operators:

* **Architecture:** **ES ($\mu$ + $\lambda$) (Steady-State)**. A population of $\mu$ parents generates $\lambda$ offspring. The next generation consists of the best $\mu$ individuals chosen from the combined pool of $\mu$ parents and $\lambda$ offspring. This is highly elitist.
* **Parent Selection:** **Uniform Random Selection**. Unlike the GA, parents for breeding are chosen randomly, applying **no fitness-based pressure** at this stage. This promotes exploration.
* **Crossover:** **Order Crossover (OX)**. Necessary for the permutation encoding of the TSP.
* **Mutation:** **Swap Mutation**.

However, because the TSP is a permutation-based problem (not continuous), this implementation was carefully adapted:
The ES algorithm was modified to include Order Crossover (OX) and Swap Mutation, both of which preserve valid permutations.
This allows the ES to operate effectively on combinatorial structures like city tours.

### Modes of ES:

This implementation supports two classic Evolution Strategy models:
| Mode                           | Description                                                    | Notation |
| ------------------------------ | -------------------------------------------------------------- | -------- |
| **Steady-State (Elitist)**     | Combines parents and offspring, keeps the best μ individuals   | (μ+λ)    |
| **Generational (Non-Elitist)** | Discards parents each generation, selects new μ from offspring | (μ,λ)    |

You can switch between them easily in the code:

In evolution_strategy.ipynb (inside Main Execution Block):

ES_MODE = "Steady-State"

ES_MODE = "Generational"

Simply comment/uncomment one of these lines to switch the ES mode.

#### Parameters Used:

| Parameter | Value | Description |
| :--- | :--- | :--- |
| **$\mu$ (Mu)** | 30 | Number of parents selected to survive or reproduce. |
| **$\lambda$ (Lambda)** | 60 | Number of offspring generated in each generation. |
| **Generations** | 500 | Total number of generations to evolve. |
| **Mut\_Rate** | 0.3 | Probability that an offspring will undergo mutation. |



### Results Summary:
| File Name           | Best Fitness | Time (s) |
| ------------------- | ------------ | -------- |
| problem_g_10.npy    | 1497.66      | 0.1552   |
| problem_g_100.npy   | 4324.67      | 0.7088   |
| problem_g_1000.npy  | 14146.10     | 7.8079   |
| problem_g_20.npy    | 1755.51      | 0.2173   |
| problem_g_200.npy   | 6261.33      | 1.3327   |
| problem_g_50.npy    | 2879.78      | 0.4005   |
| problem_g_500.npy   | 9653.96      | 3.5275   |
| problem_r1_10.npy   | 184.27       | 0.1595   |
| problem_r1_100.npy  | 765.23       | 0.7047   |
| problem_r1_1000.npy | 2588.51      | 7.7830   |
| problem_r1_20.npy   | 350.38       | 0.2263   |
| problem_r1_200.npy  | 1128.15      | 1.3656   |
| problem_r1_50.npy   | 572.34       | 0.4081   |
| problem_r1_500.npy  | 1765.16      | 3.5604   |
| problem_r2_10.npy   | -411.70      | 0.1587   |
| problem_r2_100.npy  | -4671.90     | 0.7234   |
| problem_r2_1000.npy | -49412.73    | 7.6389   |
| problem_r2_20.npy   | -836.99      | 0.2213   |
| problem_r2_200.npy  | -9595.05     | 1.3663   |
| problem_r2_50.npy   | -2252.10     | 0.4141   |
| problem_r2_500.npy  | -24511.16    | 3.5495   |
| test_problem.npy    | 2823.79      | 0.2171   |
