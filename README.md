# L-Shaped Decomposition Algorithm for Two-Stage Stochastic Programming

This project implements the L-shaped decomposition algorithm to solve two-stage stochastic programming problems. The algorithm iteratively solves the master problem and scenario-based subproblems, generating feasibility/optimality cuts until convergence.

## Introduction

Two-stage stochastic programming problems can become very large and complex, especially with an increasing number of scenarios. The L-shaped algorithm, a type of Benders Decomposition, is an efficient method for solving these kinds of problems. The core idea of this algorithm is to decompose the large problem into two parts:

1.  **Master Problem:** This problem contains only the first-stage variables and an approximation of the second-stage cost.
2.  **Subproblems:** For each scenario, there is a subproblem that calculates the optimal second-stage cost by considering the first-stage decisions as fixed.

The algorithm iteratively moves information between these two components. By solving the subproblems, information in the form of "cuts" is generated and added to the master problem. These cuts refine the approximation of the second-stage cost, making it more accurate. This process continues until an optimal solution is found. The provided code implements this algorithm.

## Mathematical Formulation

### 2.1 Master Problem

The master problem seeks to find the first-stage decisions ($x_1, x_2$) that minimize the sum of the first-stage cost and the expected value of the second-stage cost:

$$
\min \quad 3x_1 + 2x_2 + \theta_1 + \theta_2
$$

*   $x_1, x_2$: First-stage decision variables.
*   $\theta_s$: A variable that estimates the optimal second-stage cost under scenario $s$. Initially, this estimate is not precise and is improved by adding optimality cuts.

### 2.2 Subproblem

For each scenario $s \in \{1, 2\}$, a subproblem is solved to find the second-stage decision variables ($y_1, y_2$), given the values of $x_1, x_2$ from the master problem.

$$
\min \quad 15y_1 + 12y_2
$$
$$
\text{s.t.} \quad 3y_1 + 2y_2 \le x_1
$$
$$
\quad \quad 2y_1 + 5y_2 \le x_2
$$
$$
\quad \quad 0.8\xi_{1s} \le y_1 \le \xi_{1s}
$$
$$
\quad \quad 0.8\xi_{2s} \le y_2 \le \xi_{2s}
$$
$$
\quad \quad x, y \ge 0
$$

*   $y_1, y_2$: Second-stage decision variables.
*   $\xi_{1s}, \xi_{2s}$: Random parameters in scenario $s$, read from the `scenario` dictionary.
