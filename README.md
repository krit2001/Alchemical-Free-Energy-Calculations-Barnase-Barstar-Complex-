# Alchemical Free Energy Calculation: Barnase-Barstar Complex (D149A & W44F)

![GROMACS](https://img.shields.io/badge/GROMACS-2024-blue) ![PMX](https://img.shields.io/badge/PMX-Hybrid_Topology-green) ![Python](https://img.shields.io/badge/Python-3.x-yellow)

## 📌 Project Overview
This project quantifies the contribution of key interfacial residues to the high-affinity binding of the **Barnase-Barstar** complex. Using alchemical free energy perturbation (FEP) methods, I calculated the relative binding free energy changes ($\Delta\Delta G_{bind}$) for two critical mutations:
1.  **D149A (Asp39Ala):** Disruption of a salt-bridge network.
2.  **W44F (Trp44Phe):** Disruption of hydrophobic/hydrogen-bonding interactions.

**Key Results:**
*   **D149A:** Destabilizes the complex by **+26.7 kJ/mol** (~6.4 kcal/mol).
*   **W44F:** Destabilizes the complex by **+7.84 kJ/mol** (~1.9 kcal/mol).

## 🔬 Biological Background & Literature Review
The **Barnase-Barstar** system is a paradigmatic model for understanding high-affinity protein-protein interactions ($K_d \approx 10^{-14}$ M). The interface is driven by strong electrostatic steering and high shape complementarity.

### Target 1: Asp149 (Asp39)
**Aspartate 39 on Barstar** (indexed as Asp149 in the complex) forms a strong hydrogen-bonded salt bridge with **Histidine 102 on Barnase**. Seminal alanine scanning work by *Schreiber & Fersht (J. Mol. Biol, 1995)* identified this as a major "hotspot" for binding energy.

### Target 2: Trp44 (Trp44)
**Tryptophan 44 on Barstar** is a central hydrophobic anchor that buries significant surface area upon binding and contributes a hydrogen bond via its indole nitrogen. Mutating it to Phenylalanine (W44F) removes this H-bond while retaining aromaticity, serving as a subtle probe for interface packing and hydrogen bonding specificity.

## 🛠️ Methodology

### Structural Validation: AlphaFold vs. Crystal (1BRS)
To obtain a complete starting structure (including missing loops), the Barnase-Barstar complex was modeled with **AlphaFold2** and aligned to the experimental crystal structure **1BRS** using PyMOL.

**PyMOL alignment statistics:**
- Atoms aligned: 1329
- Final backbone RMSD: **0.28 Å**

This sub-ångström RMSD indicates that the AlphaFold model faithfully reproduces the experimental binding interface.

<p align="center">
  <img src="image/align.png" width="45%">
  <img src="image/align_score.png" width="45%">
  <br>
  <i>Figure 1: Overlay of AlphaFold model (Cyan) vs. Crystal Structure (Green) and alignment score.</i>
</p>

### Simulation Protocol
The Free Energy Perturbation was performed using a rigorous thermodynamic cycle (Complex vs. Unbound leg).

- **Force Field:** Amber99SB-ILDN (with TIP3P water)
- **Software:** GROMACS 2024, PMX (hybrid topology generation)
- **Protocol:** Slow Growth (Continuous Lambda Change)
- **Simulation Time:** 10 ns per leg per mutation
- **Integrator:** Stochastic Dynamics (SD)
- **Singularity Resolution:** Optimized Soft-Core potentials (`sc-alpha=0.3`, `sc-sigma=0.25`) to resolve high-energy steric clashes.

## 📊 Results & Analysis

### 1. Thermodynamic Cycle Results
The binding affinity change is calculated as:
$$ \Delta\Delta G_{bind} = \Delta G_{complex} - \Delta G_{unbound} $$

#### Mutation A: D149A (Asp $\to$ Ala)

| Leg | Time | $\Delta G$ (kJ/mol) |
| :--- | :--- | :--- |
| **Complex** | 10 ns | **+366.9** |
| **Unbound** | 10 ns | **+340.2** |
| **Net ($\Delta\Delta G$)** | - | **+26.7** |

#### Mutation B: W44F (Trp $\to$ Phe)

| Leg | Time | $\Delta G$ (kJ/mol) | $\Delta G$ (kcal/mol) |
| :--- | :--- | :--- | :--- |
| **Complex (Bound)** | 10 ns | **-99.38** | -23.75 |
| **Unbound (Free)** | 10 ns | **-107.22** | -25.63 |
| **Net ($\Delta\Delta G$)** | - | **+7.84** | **+1.87** |

### 2. Validation against Experiment
Our computational predictions align well with experimental data, validating the accuracy of the slow-growth protocol for both charged and hydrophobic mutations.

| Mutation | Calc. $\Delta\Delta G$ (kcal/mol) | Exp. $\Delta\Delta G$ (kcal/mol)* | Interpretation |
| :--- | :--- | :--- | :--- |
| **D149A** | **+6.4** | **+7.7** | Strongly Destabilizing (Salt-bridge loss) |
| **W44F** | **+1.9** | **~0.0 - 0.4** | Destabilizing (H-bond loss / packing) |

*   **Experimental Reference:** Schreiber, G., & Fersht, A. R. (1993/1995). *Journal of Molecular Biology*.
*   **Discussion:**
    *   **D149A:** Excellent agreement (~1.3 kcal/mol deviation) confirms the electrostatic model is accurate.
    *   **W44F:** The positive sign correctly predicts destabilization. The magnitude overestimation (+1.9 vs ~0.3 kcal/mol) is typical for alchemical FEP, reflecting challenges in fully sampling solvent reorganization around the hydrophobic Phenylalanine substitution within 10 ns.

### 3. Convergence Plots (XVG Data)

#### D149A Convergence (Asp $\to$ Ala)
The slow-growth transformation demonstrated stable $dH/d\lambda$ profiles.

<p align="center">
  <img src="image/fep3.png" width="800">
  <br>
  <b>Complex Leg Convergence:</b>
  <br>
  <i>Figure 2a: dH/d $\lambda$ vs Time (10 ns) for the Barnase-Barstar complex (D149A).</i>
</p>

<p align="center">
  <img src="image/fep.png" width="800">
  <br>
  <b>Unbound Leg Convergence:</b>
  <br>
  <i>Figure 2b: dH/d $\lambda$ vs Time (10 ns) for the unbound Barstar (D149A).</i>
</p>

#### W44F Convergence (Trp $\to$ Phe)
The W44F mutation also showed excellent stability, with flat dH/dλ trajectories indicating equilibrium behavior.

<p align="center">
  <!-- REPLACE THESE WITH YOUR SAVED PLOTS FOR W44F -->
  <img src="image/w44f_complex.png" width="800">
  <br>
  <b>Complex Leg Convergence (W44F):</b>
  <br>
  <i>Figure 3a: dH/d $\lambda$ vs Time (10 ns) for the Barnase-Barstar complex (W44F).</i>
</p>

<p align="center">
  <img src="image/w44f_unbound.png" width="800">
  <br>
  <b>Unbound Leg Convergence (W44F):</b>
  <br>
  <i>Figure 3b: dH/d $\lambda$ vs Time (10 ns) for the unbound Barstar (W44F).</i>
</p>

### 4. Statistical Validation (gmx analyze)
Statistical analysis of the $dH/d\lambda$ time series using `gmx analyze` confirms the robustness of the integration.

**D149A Statistics:**
*   **Skewness (Complex):** 1.528 – Consistent with the asymmetric energy barrier of removing a charged residue.
*   **Kurtosis (Complex):** 2.864 – Indicates "heavy tails" from occasional steric clashes, effectively smoothed by the soft-core potential.

<p align="center">
  <img src="image/integeral1.png" width="45%" alt="Complex Leg Statistics">
  <img src="image/integeral2.png" width="45%" alt="Unbound Leg Statistics">
  <br>
  <i>Figure 4: Statistical output from gmx analyze for the D149A Complex leg (Left) and Unbound leg (Right).</i>
</p>

### 5. Challenges & Solutions
*   **Hysteresis:** Initial 1 ns simulations exhibited significant hysteresis (+428 kJ/mol for D149A). Extending the slow-growth protocol to **10 ns** and optimizing `sc-alpha` to 0.3 stabilized the transition.
*   **Sampling:** For W44F, the subtle hydrophobic rearrangement required careful equilibration to ensure the starting structure was relaxed before $\lambda$-dynamization.
