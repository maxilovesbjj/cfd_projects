# High-Fidelity CFD Analysis of Turbulent Flow in a 90-Degree Elbow

## 1. Project Overview

This repository documents a high-fidelity Computational Fluid Dynamics (CFD) analysis focused on simulating **turbulent flow through a 90-degree pipe elbow**. The primary objective is to accurately determine the **Hydraulic Head Loss Coefficient ($K_{HGL}$)** and analyze the **secondary flow phenomena (Dean Vortices)** that significantly contribute to energy loss.

The analysis was performed using the **OpenFOAM** platform and the RANS **$k-\omega\ SST$** turbulence model. The project demonstrates the full CFD workflow, including grid sensitivity analysis, execution, post-processing, and professional reporting.

### Key Simulation Parameters:
* **Solver Platform:** **OpenFOAM (simpleFoam)**
* **Turbulence Model:** **$k-\omega\ SST$**
* **Target Reynolds Number (Re):** **$6.3 \times 10^5$**
* **Selected Base Case:** `elbow90_18D_medium` (Chosen for numerical stability and grid independence)

---

## 2. Computational Methodology

### 2.1 Domain and Mesh Generation
The computational domain extends **18 pipe diameters (18D) upstream** and **18D downstream** to ensure fully developed flow conditions at the boundaries.

The mesh was generated externally and imported using the `ideasUnvToFoam` utility. A strict near-wall treatment was enforced, targeting **$y^+ < 1$** across all critical surfaces to ensure accurate resolution of the viscous sublayer.

Detailed instructions for mesh setup and conversion are provided in the [MeshGeneration.md](MeshGeneration.md) file.

### 2.2 Boundary Conditions
Standard steady-state boundary conditions were implemented:
* **Inlet:** Fixed Value Uniform Velocity (**3.0 m/s**)
* **Outlet:** Fixed Value Pressure (**0 Pa gauge**)
* **Walls:** **No-slip** boundary conditions.

---

## 3. Results and Discussion

The final solution successfully achieved convergence, with all residuals stabilizing below the $10^{-5}$ threshold. The primary quantitative finding is the Loss Coefficient ($K_{HGL}$), derived from the Hydraulic Gradient Line (HGL) slope analysis.

### 3.1 Grid Sensitivity Analysis
The selection of the base case (`elbow90_18D_medium`) was confirmed by the grid sensitivity study, which showed minimal variation between the `medium` and `fine` resolutions for the 18D length, indicating a robust, grid-independent solution for the final analysis.

| Case Name (Mesh Resolution) | Slope (HGL) [$m^2/s^2/m$] | Loss Coefficient ($K_{HGL}$) |
| :--- | :--- | :--- |
| `elbow90_18D_coarse` | -0.1446 | 0.1162 |
| **`elbow90_18D_medium` (Selected)** | **-0.1521** | **0.0908** |
| `elbow90_18D_fine` | -0.1553 | 0.0907 |

### 3.2 Final Loss Coefficient
The final loss coefficient for the elbow is reported as:

| Metric | Value (Simulated) |
| :--- | :--- |
| **Selected Mesh Slope** | $-0.1521$ |
| **Loss Coefficient ($K_{HGL}$)** | **0.0908** |

### 3.3 Visualizations

The flow field visualizations confirm the physical phenomena driving the hydraulic loss. All high-resolution image outputs are located in the dedicated **`images/`** directory.

* **Convergence History:** Plot confirming the stability and convergence of the numerical solution.
  
  ![Convergence Residuals](images/Convergence_Residuals.png)

* **Secondary Flow Analysis:** Cross-sectional velocity vectors clearly illustrating the formation and development of the Dean Vortices within the bend.
  
  ![Secondary Flow Visualization](images/Secondary_Flow_Visualization.png)

* **Pressure Contour:** Surface pressure coefficient distribution highlighting the high-pressure zone on the outer wall and the low-pressure zone on the inner wall.
  
  ![Pressure Contour Cp](images/Pressure_Contour_Cp.png)

---

## 4. Repository Structure and Usage

The project follows a standardized structure for replicability:

* **Installation:** Prerequisites for the post-processing scripts are listed in `requirements.txt`.
* **Execution:** The case is run by first generating the mesh (`MeshGeneration.md`) and then executing the solver scripts within the `base_case/` directory.

```text
CFD_90DegreeElbow_Analysis/
├── base_case/              # OpenFOAM input files (0/, constant/, system/)
├── python_scripts/         # Post-processing and analysis code (compare_k_hgl.py)
├── documentation/          # Formal report (Report.pdf)
├── images/                 # Final, high-resolution visualization outputs (.png)
├── README.md               # Project overview and key results (this file)
├── requirements.txt        # Python dependencies for post-processing
└── .gitignore              # Ensures large result files are excluded

## 5. Technical Report

A comprehensive technical report detailing the theoretical background, setup, mesh sensitivity study, full validation process, and detailed discussion is available in PDF format.

* [Report.pdf](documentation/Report.pdf)

---
*Developed by: Maximiliano Lyon* | *Contact: maxilyonr@gmail.com*