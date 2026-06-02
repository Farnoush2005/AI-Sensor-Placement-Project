# AI Sensor Placement & Local Search Optimization

This repository contains a Python-based implementation of various local search and evolutionary algorithms designed to solve the **Sensor Placement Optimization Problem** in a grid map environment. This project has been developed as a **B.Sc. Course Project for Artificial Intelligence** at the **University of Isfahan (UI)**.

The project demonstrates the application of both single-state heuristics and population-based metaheuristics to find the most cost-effective spatial layout of sensors, ensuring maximum target coverage while minimizing infrastructure overhead.

---

## Implemented Algorithms

The project features five distinct optimization strategies, all derived from a unified `LocalSearchBase` class to maintain structural consistency and shared logic:

1. **Hill Climbing**
   * **Type:** Local Search (Greedy / Steepest-Descent)
   * **Behavior:** Explores the immediate neighborhood by taking a stochastic sample of 30 neighbors in each step. It greedily transitions to the neighbor with the lowest cost and terminates immediately upon hitting a local minimum or plateau.
   
2. **Simulated Annealing**
   * **Type:** Metaheuristic (Stochastic / Thermodynamic-inspired)
   * **Behavior:** Escapes local optima by accepting worse moves at high temperatures based on the Boltzmann distribution ($e^{-\Delta / T}$). It transitions from broad exploration to strict exploitation as the system cools down via a geometric cooling rate (0.99).

3. **Genetic Algorithm**
   * **Type:** Population-Based Metaheuristic (Evolutionary)
   * **Behavior:** Mimics natural selection by managing a parallel population of 40 individuals. It employs **Tournament Selection** ($k=4$), dynamic length **Crossover** (handling variable sensor counts), random **Mutation** (linked to neighbor generation), and strict **Elitism** to pass the top two solutions directly to the next generation.

4. **Local Beam Search**
   * **Type:** Multi-State Parallel Local Search
   * **Behavior:** Tracks a beam width of 5 states simultaneously. In each iteration, it generates 12 mutations per state (60 total candidates), sorts them globally, and حریصانه filters the top 5 states across the entire map, effectively discarding unpromising search paths.

5. **Tabu Search**
   * **Type:** Memory-Driven Metaheuristic
   * **Behavior:** Utilizes a short-term memory **Tabu List** (FIFO queue of size 15) to prevent cycling and stagnation in flat regions. It converts sensor coordinates into sorted tuples for immutable hashing and implements the **Aspiration Criterion** to override tabu restrictions if a solution breaks the global record.

---

## Environment & Cost Formulation

The optimization engine evaluates each state using a custom penalty-driven **Cost Function** calculated via **Manhattan Distance**:
$$Cost = (U \times 100) + (S \times 10)$$
* **$U$ (Uncovered Targets):** Incurs a heavy penalty of 100 per target, prioritizing full operational coverage.
* **$S$ (Sensor Count):** Incurs a cost of 10 per sensor, encouraging resource economy.

---

## Code Structure & Uniformity

To ensure high-quality software design and clean object-oriented architecture, all algorithms strictly adhere to a standardized development footprint:
* `initial_state`: The starting layout randomly populated by the base class.
* `get_neighbor()` & `evaluate()`: Shared abstractions from the base class for mutation and fitness scoring.
* `evaluations`: A step-by-step history of costs used to plot convergence curves.
* `states_history`: Tracking positional snapshots of sensor arrays for graphical rendering.
* `to_key`: A lambda expression (`tuple(sorted(s))`) ensuring structural isomorphism in memory tracking.

---

## Environment & Requirements

* **Language:** Python 3.x
* **Core Libraries:** * `matplotlib` (with `TkAgg` backend for stable cross-platform UI rendering)
  * `random`, `math`, `copy`, `re` (Standard Libraries)

---

## Academic Context
* **Institution:** University of Isfahan (دانشگاه اصفهان)
* **Degree:** Bachelor of Science (B.Sc.) in Computer Science
* **Course:** Artificial Intelligence (درس هوش مصنوعی)
* **Professor:** Dr. Faria Nasiri Mofakham

### Group Members
* **Member 1:** Farnoush Pourshaban - [Student ID: 4024023007]
* **Member 2:** Yeganeh Rastegari - [Student ID: 4014013040]
* **Member 3:** Farzaneh Salimi - [Student ID: 4014013059]
