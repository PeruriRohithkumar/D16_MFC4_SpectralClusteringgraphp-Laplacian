# MFC4 Project: Spectral Clustering based on the Graph p-Laplacian

## 1. Team Members
* **Koushik** (CB.SC.U4AIE24330)
* **Rohith Kumar** (CB.SC.U4AIE24342)
* **Rohith Kanna** (CB.SC.U4AIE24349)
* **G. Vishal** (CB.SC.U4AIE24363)
* **Group ID:** D-16

## 2. Base/Reference Paper
* **Title:** *Spectral Clustering based on the Graph p-Laplacian*
* **Key Concept:** Generalizing the standard Graph Laplacian (p=2) to the nonlinear p-Laplacian (p < 2) to approximate the optimal Cheeger Cut.

## 3. Project Outline
Our project aims to improve clustering accuracy on non-convex, noisy datasets by moving from linear to nonlinear spectral clustering.
* **Objective:** To solve the "Over-smoothing" problem of standard spectral clustering by implementing the p-Laplacian operator ($p \to 1$).
* **Methodology:**
    1.  **Graph Construction:** Built a k-nearest neighbor graph ($k=10$) using a Gaussian Kernel.
    2.  **Initialization (Warm Start):** Solved the standard Generalized Eigenvalue Problem ($p=2$) to get an initial smooth eigenvector.
    3.  **Homotopy Continuation:** Implemented a strategy to slowly decrease $p$ in the sequence $\{1.8, 1.6, 1.4, 1.2\}$ to guide the solver.
    4.  **Optimization:** Used Gradient Descent to minimize the p-Rayleigh quotient $F_p(f)$ at each step.
* **Dataset:** Validated on the synthetic "Two Moons" dataset ($N=500$, Noise $\sigma=0.1$).

## 4. Updates
* **Baseline Implementation:** Successfully implemented standard Spectral Clustering ($p=2$), achieving a baseline accuracy of ~75.4%.
* **Algorithm Development:** Completed the code for `calculate_Fp.m` and `get_p_gradient.m` to handle the nonlinear p-Laplacian calculations.
* **Homotopy Loop:** Successfully integrated the outer optimization loop that updates the eigenvector $f$ as $p$ decreases.
* **Results:** Achieved a final accuracy of **91.2%** at $p=1.2$, demonstrating a **+15.8% improvement** over the standard method.
* **Visualization:** Generated comparative plots showing the "Sharpening Effect"—transforming the fuzzy decision boundary of $p=2$ into a sharp, quasi-binary cut at $p=1.2$.

## 5. Challenges / Issues Faced
* **Non-Convexity:** The optimization problem for $p < 2$ is non-convex. Direct optimization failed, requiring us to implement the "Warm Start" strategy using the $p=2$ solution.
* **Singular Matrix Warnings:** We encountered MATLAB warnings regarding singular/badly scaled matrices during the initial eigenvector computation, which required careful tuning of the sigma/similarity parameters.
* **Noise Sensitivity:** The standard algorithm struggled significantly with the "bridge" of noise connecting the two moons, requiring precise implementation of the nonlinearity to "snap" the values apart.

## 6. Future Plans
* **Real-World Application:** Apply the p-Spectral algorithm to image segmentation tasks (e.g., MNIST digits or medical imaging) to test scalability.
* **Cut Comparisons:** Perform a deeper quantitative comparison between Ratio Cuts, Normalized Cuts, and Cheeger Cuts on different graph topologies.
* **Performance Optimization:** Optimize the gradient descent step size ($\eta$) dynamically to speed up convergence.

## 7. References
* Bühler, T., & Hein, M. (2009). Spectral Clustering based on the Graph p-Laplacian. *Proceedings of the 26th International Conference on Machine Learning (ICML)*, 81–88.
