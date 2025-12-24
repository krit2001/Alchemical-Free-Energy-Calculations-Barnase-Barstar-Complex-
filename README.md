# Alchemical-Free-Energy-Calculations-Barnase-Barstar-Complex-
# Alchemical Free Energy Calculation: Barnase-Barstar Complex (D149A)

![GROMACS](https://img.shields.io/badge/GROMACS-2024-blue) ![PMX](https://img.shields.io/badge/PMX-Hybrid_Topology-green) ![Python](https://img.shields.io/badge/Python-3.x-yellow)

## 📌 Project Overview
This project quantifies the contribution of the interfacial salt-bridge residue **Asp149** (Asp39 in Barstar numbering) to the binding affinity of the **Barnase-Barstar** complex. Using alchemical free energy perturbation (FEP) methods, I calculated the relative binding free energy change ($\Delta\Delta G_{bind}$) associated with the **Asp $\to$ Ala** mutation.

**Key Result:** The D149A mutation destabilizes the complex by **+26.7 kJ/mol** (~6.4 kcal/mol), confirming the critical role of the Asp149-His102 salt bridge.

## 🔬 Biological Background & Literature Review
The **Barnase-Barstar** system is a paradigmatic model for understanding high-affinity protein-protein interactions ($K_d \approx 10^{-14}$ M). The rapid and tight binding is largely driven by strong electrostatic steering and a highly complementary interface.

### The Target: Asp149 (Asp39)
The interface features a critical salt-bridge network. Specifically, **Aspartate 39 on Barstar** (indexed as Asp149 in the complex) forms a strong hydrogen-bonded salt bridge with **Histidine 102 on Barnase**.

*   **Experimental Context:** Seminal work by *Schreiber & Fersht (J. Mol. Biol, 1995)* utilized alanine scanning mutagenesis to map the energetic epitope of the interface. They determined that removing the charge at this position significantly reduces binding affinity.
*   **Study Objective:** This project aims to computationally replicate this "hotspot" effect. By alchemically transforming Asp $\to$ Ala, I eliminate the specific salt-bridge interaction. A positive $\Delta\Delta G_{bind}$ (destabilization) comparable to experimental ranges validates the accuracy of the chosen force field (Amber99SB) and the non-equilibrium/slow-growth protocol.


## 🛠️ Methodology

### Structural Validation: AlphaFold vs. Crystal (1BRS)

To obtain a complete starting structure (including missing loops), the Barnase-Barstar complex was modeled with **AlphaFold2** and aligned to the experimental crystal structure **1BRS** using PyMOL:


PyMOL alignment statistics:

- Atoms aligned: 1329
- Final backbone RMSD: **0.28 Å**

This sub-ångström RMSD indicates that the AlphaFold model faithfully reproduces the experimental binding interface while restoring missing residues, making it a reliable starting point for subsequent free-energy simulations.

Here is the overlay of the AlphaFold model (Cyan) vs. the Crystal Structure (Green):

![AlphaFold Validation](image/align.png)
![Score](imgage/align_score.png)



The Free Energy Perturbation was performed using a rigorous thermodynamic cycle (Complex vs. Unbound leg).

- **Software:** GROMACS 2024, PMX (for hybrid topology generation).
- **Protocol:** Slow Growth (Continuous Lambda Change).
- **Simulation Time:** 10 ns per leg (20 ns total production).
- **Integrator:** Stochastic Dynamics (SD) for temperature coupling.
- **Singularity Resolution:** Optimized Soft-Core potentials (`sc-alpha=0.3`, `sc-sigma=0.25`) to resolve high-energy steric clashes at end-points.
## 📊 Results & Analysis
### 1. Thermodynamic Cycle
The binding affinity change is calculated as:
$$ \Delta\Delta G_{bind} = \Delta G_{complex} - \Delta G_{unbound} $$

| Leg | Integral ($\int dH/d\lambda$) | Time | $\Delta G$ (kJ/mol) |
| :--- | :--- | :--- | :--- |
| **Complex** | 3,669,250 | 10 ns | **+366.9** |
| **Unbound** | 3,402,263 | 10 ns | **+340.2** |
| **Net ($\Delta\Delta G$)** | - | - | **+26.7** |

The slow-growth transformation ($\lambda: 0 \to 1$) demonstrated stable dH/d$\lambda$ profiles for both thermodynamic legs, confirming that the 10 ns sampling duration was sufficient to overcome initial hysteresis.

**Complex Leg Convergence:**
![Complex Leg dH/dl](image/fep3.png)

*Figure 1: dH/d$\lambda$ vs Time (10 ns) for the Barnase-Barstar complex.*

**Unbound Leg Convergence:**
![Unbound Leg dH/dl](images/unbound_convergence.png)
*Figure 2: dH/d$\lambda$ vs Time (10 ns) for the unbound Barstar.*

### 2. Convergence & Stability
*   **Hysteresis Handling:** Initial short simulations (1 ns) exhibited significant hysteresis (+428 kJ/mol). Extending the slow-growth protocol to **10 ns** and reducing the time step to **1 fs** stabilized the transition and converged the result.

