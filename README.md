# Spectral Clustering based on the Graph p-Laplacian

**Team Members (Group D-16):**
* Koushik (CB.SC.U4AIE24330)
* Rohith Kumar (CB.SC.U4AIE24342)
* Rohith Kanna (CB.SC.U4AIE24349)
* G. Vishal (CB.SC.U4AIE24363)

**Reference:** *Spectral Clustering based on the graph p-Laplacian* (T. Bühler and M. Hein, ICML 2009).

---

## 1. Introduction & Problem Statement
Standard spectral clustering treats data partitioning as a graph cut problem. However, the standard method (p=2) minimizes squared differences, which forces the solution to change slowly across the graph. In noisy or complex datasets, this "Over-Smoothing" creates fuzzy boundaries and high misclassification rates.

**Our Objective:** We replace the linear Graph Laplacian with the nonlinear p-Laplacian. By lowering p towards 1, we force the eigenvector to behave like a discrete binary switch (an Indicator Function), effectively solving the Cheeger Cut problem and recovering perfectly sharp boundaries.

---

## 2. Mathematical Formulation
To achieve a sharp cut, we minimize the p-Laplacian Energy functional:

$$F_p(f) = \frac{1}{2} \sum_{i,j=1}^{n} W_{ij} |f_i - f_j|^p$$

We solve this using steepest gradient descent. Applying the chain rule and using the identity $x = |x| \cdot \text{sign}(x)$, the gradient is defined as:

$$\frac{\partial F_p}{\partial f_i} = p \sum_{j} W_{ij} |f_i - f_j|^{p-2} (f_i - f_j)$$

### Methodology
Because the optimization of the p-Rayleigh quotient is non-convex, we cannot jump straight to p=1.2.
1. **Initialization (Warm Start):** We solve the standard eigenvalue problem for p=2 to get an initial guess.
2. **Homotopy Continuation:** We optimize the functional sequentially for p = {1.8, 1.6, 1.4, 1.2}, using the solution of the previous step as the initialization for the next.

---

## 3. Execution Details (Performance Profiling)
As requested for the Eval 2 submission, we profiled the execution time of our codebase using `tic` and `toc` in MATLAB.

* **Platform Used:** macOS / MATLAB Online
* **Hardware:** CPU
* **Language/Environment:** MATLAB R2023b
* **Execution Times:**
  * Two Moons & Concentric Circles (`Two_moon_and_circles.mlx`): **7.188294 seconds**
  * Intertwined Spirals (`spiral.mlx`): **3.155872 seconds**
  * Two Norm Statistical Benchmark (`MFC4_Paper_Replication.mlx`): **0.649949 seconds**
  * *Total Compute Time:* **~10.99 seconds**

---

## 4. Important Results

### A. Non-Convex Separation (Two Moons)
The standard method struggles with the noisy gap between the interleaving moons. Our method (p=1.2) ignores the noise and creates a sharp binary cut.
* **Accuracy:** 97.20%

![Two Moons Result](images/moons_result.png)

### B. Topological Separation (Concentric Circles)
Standard linear methods cut this topology in half. Our method perfectly isolates the inner core from the outer ring.
* **Accuracy:** 100.00%

![Circles Result](images/circles_result.png)

### C. Complex Topology (Intertwined Spirals)
Standard methods mix the colors across the arms because points are spatially close. Our algorithm unwinds the manifold and perfectly traces the distinct arms.

![Spirals Result](images/spirals_result.png)

---

## 5. Summary Table: Statistical Replication
To prove mathematical robustness, we replicated the original paper's high-dimensional benchmark on the "Two Norm" dataset.

| Metric | Original Paper Target | Our Achieved Result |
| :--- | :--- | :--- |
| **Error Rate** | 0.0257 (2.57%) | **0.0250 (2.50%)** |
| **Interpretation** | ~10.3 Errors (Avg) | **10 Errors** (Exact) |

---

## 6. Conclusions
The Graph p-Laplacian is a practical, powerful tool for solving "hard" clustering problems where standard methods fail. We successfully demonstrated that:
1. Standard Laplacians (p=2) blur boundaries ("over-smoothing").
2. The p-Laplacian (p=1.2) sharpens boundaries, behaving as a step function.
3. We achieved 100% Accuracy on topological datasets where standard linear methods failed.
