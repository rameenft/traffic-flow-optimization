**Dataset:** Stanford SNAP California Road Network (`roadNet-CA`) — 1.96M intersections, 2.77M road segments

---

## 📌 Problem Overview

How do you route thousands of drivers through a shared road network — each going somewhere different — without blowing past capacity on any single road?

This project tackles the **Multi-Commodity Flow (MCF) problem** applied to real-world urban traffic routing. Multiple traffic flows (commodities) share a road network with hard capacity constraints. The goal is to maximize total vehicles successfully routed while respecting those constraints.

**Real-world applications modeled:**
- Urban congestion management
- Emergency evacuation planning
- Delivery fleet routing
- Public transit optimization

---

## 🧮 Mathematical Formulation

**Given:** Road network G = (V, E), road capacities u_ij, and K commodities each with an origin, destination, and demand.

**Decision variable:** x_ij^k = vehicles of commodity k using road (i, j)

**Objective:** Maximize total vehicles routed across all commodities

**Subject to:**
- Flow conservation at every intersection for every commodity
- Shared road capacity constraints across all commodities
- Non-negativity of all flow variables

---

## ⚙️ Methods Compared

| Method | Approach | Speed | Solution Quality |
|---|---|---|---|
| Greedy Shortest Path | Route each commodity via Dijkstra sequentially | Very fast (<1s) | ~70–85% optimal |
| Sequential Max Flow | Apply Edmonds-Karp per commodity, update residuals | Fast (1–5s) | ~75–90% optimal |
| Gurobi IP Solver | Solve full integer program directly | Slow (10–300s) | 100% optimal |
| Column Generation | Path-based decomposition (master + pricing subproblem) | Medium (2–30s) | ~98–100% optimal |

Column generation is the key novel method — it decomposes the problem into a master problem (select which paths to use) and a pricing subproblem (find new profitable paths via shortest path with modified costs). This makes it far more scalable than direct IP for large K.

---

## 📂 Notebooks

### `INDENG240 Final Project.ipynb`
Full project notebook covering problem formulation, dataset preparation, algorithm implementation, computational experiments, and analysis. Includes application discussion across infrastructure planning, emergency response, network resilience testing, and logistics.

### `Edmonds-Karp.ipynb`
Standalone implementation of the Edmonds-Karp max flow algorithm (BFS-based augmenting paths). Used as the Sequential Max Flow baseline in the main project.

### `Frank-Wolfe Algorithm for MCF.ipynb`
Application of the Frank-Wolfe algorithm to the Minimum Cost Flow problem. Covers linearization, descent direction computation, and convergence analysis.

---

## 📊 Experimental Design

Experiments varied across:
- Number of commodities K: 5, 10, 20, 50, 100
- Network size: 500 to 5,000 nodes (subsampled from roadNet-CA)
- Capacity tightness: uniform, degree-based, and random assignments
- Demand patterns: uniform vs. distance-based

Metrics evaluated: total throughput, demand satisfaction rate, computation time, and optimality gap vs. Gurobi.

---

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![NumPy](https://img.shields.io/badge/numpy-%23013243.svg?style=for-the-badge&logo=numpy&logoColor=white)
![Jupyter](https://img.shields.io/badge/jupyter-%23FA0F00.svg?style=for-the-badge&logo=jupyter&logoColor=white)

**Key concepts:** Max flow / min cut theorem, Dijkstra's algorithm, Edmonds-Karp, Frank-Wolfe, column generation, LP/IP formulation, graph theory

---

## 👩‍💻 Author

**Rameen Faisal** — Master of Analytics, UC Berkeley  
[LinkedIn](https://www.linkedin.com/in/rameen-faisal/) · [rameen@berkeley.edu](mailto:rameen@berkeley.edu)
