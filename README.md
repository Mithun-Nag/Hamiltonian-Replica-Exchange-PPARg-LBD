# 🧬 PPARγ Hamiltonian Replica Exchange Molecular Dynamics (HREMD)

This repository contains the complete workflow used to **prepare, optimize, validate, and run Hamiltonian Replica Exchange Molecular Dynamics (HREMD)** simulations of the PPARγ ligand-binding domain from three starting structures: **3PRG, 6ONI, and 6ONJ**.

The workflow was adapted from the **PLUMED Masterclass 22.10: Hamiltonian replica exchange with PLUMED and GROMACS**, which demonstrates the development and validation of HREMD using alanine dipeptide in explicit water. Here, the general framework was adapted and optimized through iterative testing for the substantially larger, solvated PPARγ systems and the conformational transitions of interest.

The workflow covers **system preparation and equilibration → temperature screening → Hamiltonian scaling and validation → replica-ladder construction → HREMD setup → production HREMD**.

Each notebook also contains the **analysis and diagnostic plots associated with that stage**, allowing the simulations, scaling behavior, exchange setup, and parameter choices to be evaluated directly as the workflow progresses.

## 📚 Reference Workflow

**PLUMED Masterclass 22.10 — Hamiltonian replica exchange with PLUMED and GROMACS**
Giovanni Bussi, 2022

**Documentation:**
https://www.plumed.org/doc-v2.9/user-doc/html/masterclass-22-10.html

**Original Masterclass repository:**
https://github.com/plumed/masterclass-22-10

---

## 📁 Repository Structure

The workflow was independently applied to **3PRG, 6ONI, and 6ONJ**. Each system directory contains the starting structures, required simulation input, and five sequential notebooks covering the complete workflow.

```text id="4eyjqx"
PPARG_HREMD/
│
├── README.md
├── LICENSE
│
├── 3PRG/
│   ├── Prep/
│   │   ├── 3PRG_apo_clean.pdb
│   │   └── b4md.gro
│   ├── grompp.mdp
│   ├── 0_System_Preparation.ipynb
│   ├── 1_Temperature_Screening.ipynb
│   ├── 2_Hamiltonian_Scaling.ipynb
│   ├── 3_HREMD_Setup.ipynb
│   └── 4_HREMD_Run.ipynb
│
├── 6ONI/
│   ├── Prep/
│   │   ├── 6ONI_apo_clean.pdb
│   │   └── b4md.gro
│   ├── grompp.mdp
│   ├── 0_System_Preparation.ipynb
│   ├── 1_Temperature_Screening.ipynb
│   ├── 2_Hamiltonian_Scaling.ipynb
│   ├── 3_HREMD_Setup.ipynb
│   └── 4_HREMD_Run.ipynb
│
└── 6ONJ/
    ├── Prep/
    │   ├── 6ONJ_apo_clean.pdb
    │   └── b4md.gro
    ├── grompp.mdp
    ├── 0_System_Preparation.ipynb
    ├── 1_Temperature_Screening.ipynb
    ├── 2_Hamiltonian_Scaling.ipynb
    ├── 3_HREMD_Setup.ipynb
    └── 4_HREMD_Run.ipynb
```

## 🚀 Getting Started

The cleaned starting PDB for each system is provided in its corresponding `Prep/` directory.

`0_System_Preparation.ipynb` contains the complete **system preparation and equilibration workflow**, including the MD parameter files required to prepare the solvated system from the starting structure. The full workflow can therefore be reproduced beginning from the provided PDB.

For convenience, the final equilibrated structure, `Prep/b4md.gro`, is also provided for each system. Users who do not need to repeat system preparation can use this structure to **begin directly from Step 1: Temperature Screening**.

The `grompp.mdp` file required for the subsequent HREMD stages is provided in each system directory.

The notebooks should be executed sequentially because intermediate files and directories required by later stages are generated automatically during the preceding steps.

## 📊 Analysis and Diagnostics

Analysis is integrated directly into the workflow rather than separated into a standalone analysis directory.

Each notebook contains the **analysis, validation checks, and plots relevant to that stage**. These outputs were used to assess simulation behavior and guide progression to the next stage of the HREMD workflow.

This includes structural and energetic diagnostics during temperature and Hamiltonian-scaling tests, validation of the generated Hamiltonians and replica-exchange implementation, evaluation of the replica setup, and checks associated with the final HREMD simulations.

The plots and analysis are therefore retained alongside the simulation code so that the reasoning behind the final HREMD setup can be followed and reproduced.

## 💻 Computational Environment

All simulations were carried out on the **Advanced Computing Center for Research and Education (ACCRE) at Vanderbilt University** using **NVIDIA RTX A6000 GPUs** and **GROMACS with PLUMED support**.

From `1_Temperature_Screening.ipynb` onward, the workflow requires a **PLUMED-enabled GROMACS installation**.

The notebooks and SLURM scripts preserve the ACCRE configuration used for the original calculations. HPC-specific settings—including filesystem paths, environment modules, scheduler directives, MPI configuration, and GPU resources—can be modified to run the workflow on other computing environments.

## 💾 Simulation Data

Large simulation files are not included in this repository due to their size. These include production trajectories, checkpoints, energy files, replica outputs, and other large intermediate or production data generated during the workflow.

The notebooks retain the code and analysis required to generate and evaluate these outputs.

**Production trajectories and associated simulation data can be made available upon request.**

---

## 🔄 Workflow

The notebooks should be followed sequentially:

**0. System Preparation → 1. Temperature Screening → 2. Hamiltonian Scaling and Validation → 3. HREMD Setup → 4. HREMD Production**

---

## 🧪 HREMD Workflow at a Glance

```text
🧬 Starting PDB
      │
      ▼
🧱 System Preparation
      │
      ▼
🌡️ Temperature Screening
      │
      ▼
⚙️ Hamiltonian Scaling
      │
      ▼
🧪 Validation
      │
      ▼
📐 λ-Ladder Construction
      │
      ▼
🔁 64-Replica HREMD
      │
      ▼
🚀 Production Sampling
      │
      ▼
📊 Conformational Ensemble
```

**Three starting structures. One validated workflow. Enhanced sampling across the PPARγ conformational landscape.**


*********************

### 🧱 Step 0 — System Preparation and Equilibration

**Notebook:** `0_System_Preparation.ipynb`

Prepares each PPARγ starting structure (**3PRG, 6ONI, or 6ONJ**) as a fully solvated and equilibrated GROMACS system for the subsequent HREMD workflow.

Starting from the cleaned PDB structure provided in `Prep/`, the notebook performs the complete preparation workflow, including:

* generation of the GROMACS topology and coordinates
* definition of the simulation box
* solvation and ion addition
* energy minimization
* temperature equilibration
* pressure equilibration
* generation of the final equilibrated structure

All required `.mdp` parameters for these preparation and equilibration stages are written directly within the notebook, so no additional preparation parameter files are required.

The final equilibrated structure is saved as `b4md.gro` and serves as the starting configuration for **Step 1 — Temperature Screening**. A copy of `b4md.gro` is also provided in `Prep/`, allowing users to begin directly from Step 1 without repeating system preparation.

The notebook includes the relevant equilibration checks and analysis plots so that system stability can be evaluated before proceeding to the HREMD workflow.

**Input:** cleaned PDB structure in `Prep/`
**Main output:** equilibrated `b4md.gro`
**Next:** `1_Temperature_Screening.ipynb`

***************************
### 🌡️ Step 1 — Temperature Screening

**Notebook:** `1_Temperature_Screening.ipynb`

Determines the temperature range needed to promote conformational transitions in each PPARγ system and provides the temperature-based reference for selecting the **Hamiltonian scaling range used in HREMD**.

The screening is performed in two stages.

#### 1a — Initial Temperature Screening

Short independent simulations are performed across a broad temperature ladder from **300 to 1000 K** to identify the approximate temperature at which the system begins to cross conformational barriers.

The notebook automatically creates temperature-specific directories:

```text
Temp/
├── T300/
│   └── ...
├── T400/
│   └── ...
├── T500/
│   └── ...
├── T600/
│   └── ...
├── T700/
│   └── ...
├── T800/
│   └── ...
├── T900/
│   └── ...
└── T1000/
    └── ...
```

Each temperature is simulated independently for **10 ns**. The trajectories are then analyzed using **protein RMSD and radius of gyration (Rg)**, with optional multi-frame PDB generation for structural visualization.

These analyses identify the approximate temperature range where substantial conformational changes and barrier crossing begin to occur.

#### 1b — Narrowing the Temperature Range

Based on the initial screen, a narrower temperature range is examined using longer simulations to better resolve the onset of intermediate-state sampling.

For the workflow shown in the notebook, temperatures from **310 to 530 K in 10 K increments** are generated:

```text
1b/
├── T310/
│   └── ...
├── T320/
│   └── ...
├── T330/
│   └── ...
├── ...
├── T510/
│   └── ...
├── T520/
│   └── ...
└── T530/
    └── ...
```

These simulations are run for approximately **100 ns** and analyzed using **RMSD, radius of gyration, and potential-energy distributions**.

Potential-energy overlap between neighboring temperatures is evaluated to assess the energetic coverage of the selected temperature ladder. Together with the structural analyses, these results are used to determine the temperature range needed to guide the Hamiltonian scaling tests in **Step 2**.

All temperature directories, simulation files, analysis files, and plots are generated by the notebook and are not stored separately in the repository.

**Input:** equilibrated `b4md.gro`
**Purpose:** determine the temperature range required to guide Hamiltonian scaling
**Next:** `2_Hamiltonian_Scaling.ipynb`
*****************

### ⚙️ Step 2 — Hamiltonian Scaling and Validation

**Notebook:** `2_Hamiltonian_Scaling.ipynb`

Builds and validates the scaled Hamiltonians used for HREMD, then tests a broad λ range to identify a stable and sufficiently overlapping Hamiltonian ladder for the final replica-exchange setup.

This notebook contains four main stages.

#### 2a — Generate Scaled Hamiltonians

A self-contained GROMACS topology is first generated from the equilibrated system. The protein atoms are then marked for scaling, and PLUMED `partial_tempering` is used to generate scaled topologies.

The notebook initially compares λ = 1.0 and λ = 0.5 and directly visualizes the expected Lennard-Jones scaling.

```text
2a/
├── processed.top
├── processed_selected.top
├── scaled1.0.top
├── scaled0.5.top
└── ...
```

This establishes the topology framework used for all subsequent Hamiltonian scaling.

#### 2b — Validate the Scaled Topologies

The scaled topologies are checked using GROMACS energy reruns.

First, the original topology is compared with a `partial_tempering` topology generated at **λ = 1.0**. Because no scaling should occur at λ = 1.0, the corresponding energy terms should agree within numerical precision.

A second sanity check scales the **entire system** to verify that the expected Hamiltonian terms respond correctly to scaling.

```text
2b/
├── rerun0/
│   └── ...
├── rerun1/
│   └── ...
├── rerun_whole/
│   └── ...
└── ...
```

Energy differences are plotted directly in the notebook for bond, angle, dihedral, Lennard-Jones, Coulomb, PME, and total potential-energy terms.

#### 2c — Validate Replica-Exchange Acceptance

A short two-replica HREMD test is performed using **equivalent Hamiltonians**.

```text
2c/
├── rep0/
│   └── ...
├── rep1/
│   └── ...
├── plumed.dat
└── ...
```

Because the two Hamiltonians are equivalent, the expected exchange probability is approximately **1.0**. The exchange statistics and PLUMED energy terms are extracted directly from the GROMACS log to confirm that replica-exchange acceptance is being calculated correctly.

#### 2d — Test the λ Range and Spacing

A broad Hamiltonian ladder is then constructed using **30 λ values from 1.00 to 0.30**:

```text
2d/
├── lambda_100/
│   └── ...
├── lambda_98/
│   └── ...
├── lambda_95/
│   └── ...
├── ...
├── lambda_40/
│   └── ...
└── lambda_30/
    └── ...
```

Each λ directory contains its own scaled topology and GROMACS input files.

The 30 λ windows are first run in parallel for **10 ns each**. At this stage, the simulations are used to evaluate the Hamiltonians independently; replica exchange is not yet enabled.

For every λ window, the notebook calculates and plots:

* protein RMSD
* radius of gyration (Rg)
* potential-energy distributions
* potential-energy overlap between neighboring λ windows

Because the initial 10 ns simulations do not provide sufficiently stable structural and energetic distributions, all 30 windows are continued for an additional **90 ns**, giving **100 ns per λ window**.

The same RMSD, Rg, and potential-energy analyses are then repeated. Potential-energy overlap is also visualized using alternate λ windows to reduce crowding and make the overlap easier to assess.

After 100 ns, the λ windows show substantial energetic overlap while lower λ values progressively access conformations farther from the starting structure without strong global expansion of the protein. Selected low-λ trajectories are also inspected structurally.

These results provide the basis for selecting the λ range and designing the denser **64-replica HREMD ladder** used in the next stage.

All directories, scaled topologies, trajectories, energy files, validation outputs, and analysis plots shown above are generated automatically by the notebook.

**Input:** equilibrated system, topology, and `grompp.mdp`
**Purpose:** generate and validate the scaled Hamiltonians and determine the λ range and spacing for the final HREMD replica ladder
**Next:** `3_HREMD_Setup.ipynb`

****************
### 🧬 Step 3 — HREMD Setup

**Notebook:** `3_HREMD_Setup.ipynb`

Builds the final **64-replica REST2/HREMD system** from the Hamiltonian scaling range identified in Step 2.

The notebook first loads the required GROMACS–PLUMED environment and then generates the final λ ladder using the chosen base temperature and maximum REST2 temperature.

For the default setup:

* **64 replicas**
* **base temperature = 310 K**
* **maximum REST2 temperature = 520 K**
* λ values distributed non-uniformly, with **denser spacing near λ = 1.0**

The λ ladder is written to:

```text
rest2_lambdas.dat
```

The notebook then creates the complete replica structure:

```text
Hamiltonian/
├── lambda001/
│   ├── topol.top
│   ├── topol.tpr
│   └── plumed.dat
├── lambda002/
│   ├── topol.top
│   ├── topol.tpr
│   └── plumed.dat
├── lambda003/
│   └── ...
├── ...
└── lambda064/
    ├── topol.top
    ├── topol.tpr
    └── plumed.dat
```

For every replica, PLUMED `partial_tempering` is applied to the processed topology generated in Step 2 using the corresponding λ value. GROMACS `grompp` is then used to generate the replica-specific `topol.tpr`.

The 64 replicas are prepared in parallel, and the notebook verifies that both `topol.top` and `topol.tpr` were generated successfully for every replica.

The final λ ladder is also plotted directly in the notebook to visualize the non-uniform replica spacing.

An empty `plumed.dat` file is generated because the REST2 scaling is already encoded in the replica-specific topologies; no additional PLUMED bias is applied. The file is then copied into every replica directory.

**Input:** `2a/processed_selected.top`, `Prep/b4md.gro`, and `grompp.mdp`
**Purpose:** construct the final 64-replica λ ladder and prepare all replica-specific HREMD inputs
**Main output:** `Hamiltonian/` containing the complete replica setup
**Next:** `4_HREMD_Run.ipynb`

****************
### 🚀 Step 4 — HREMD Run and Production

**Notebook:** `4_HREMD_Run.ipynb`

Tests the complete 64-replica setup, equilibrates all replicas, and launches the final production HREMD simulation.

The notebook first loads the ACCRE GROMACS–PLUMED environment and performs a short multi-replica test to confirm that all 64 λ windows launch correctly together before submitting the full GPU production job.

#### 4a — Test the 64-Replica Setup

All 64 replicas are launched simultaneously as a preliminary test:

```text
Hamiltonian/
├── lambda001/
├── lambda002/
├── lambda003/
├── ...
└── lambda064/
```

This confirms that the complete multi-replica system can be initialized and run correctly before production submission.

#### 4b — Replica Equilibration

The notebook generates and submits an ACCRE SLURM script that runs all **64 replicas simultaneously on four NVIDIA RTX A6000 GPUs**.

Before replica exchange is enabled, every λ window undergoes **20 ns of equilibration**:

```text
64 replicas
    ↓
20 ns equilibration per replica
    ↓
production HREMD
```

This allows each scaled Hamiltonian to relax before exchanges between neighboring replicas begin.

#### 4c — Production HREMD

Following equilibration, Hamiltonian replica exchange is enabled using:

```text
-hrex
-replex 1000
```

Exchange attempts are therefore made every **1,000 MD steps** across neighboring Hamiltonian replicas.

The production simulation is configured for:

* **64 replicas**
* **4 NVIDIA RTX A6000 GPUs**
* **250,000,000 MD steps**
* **2 fs timestep**
* **500 ns production per replica**
* Hamiltonian exchange attempts every **1,000 steps**

The simulation is executed using MPI with GPU acceleration for nonbonded interactions, PME, and bonded interactions.

#### Restart Capability

The notebook also contains a restart workflow for continuing production HREMD from existing replica checkpoints if the production job is interrupted.

The restart preserves the 64-replica setup and resumes production with Hamiltonian replica exchange enabled.

Generated production data are written within the corresponding `lambda*` replica directories:

```text
Hamiltonian/
├── lambda001/
│   └── ...
├── lambda002/
│   └── ...
├── ...
└── lambda064/
    └── ...
```

The notebook generates the required SLURM script automatically, so no separate production script needs to be provided in the repository.

**Input:** completed `Hamiltonian/` replica setup from Step 3
**Purpose:** equilibrate the final replicas and run the production 64-replica HREMD simulation
**Main output:** production HREMD trajectories and associated simulation data for all replicas
***************
```text
╔══════════════════════════════════════════════════════════════════════╗
║             🧬  HAMILTONIAN REPLICA EXCHANGE MD  🧬               ║
╚══════════════════════════════════════════════════════════════════════╝


              🔥  SCALED HAMILTONIANS  🔥

   λ₁        λ₂        λ₃        λ₄                 λ₆₄
  1.00       •         •         •        ···        λmin
   │         │         │         │                    │
  🧬        🧬        🧬        🧬                   🧬
   │         │         │         │                    │
   └───⇄─────┼───⇄─────┼───⇄─────┼──── ··· ────⇄────┘
        🔁        🔁        🔁                      🔁

              Hamiltonians exchange...
                 proteins explore.


                         ⚡
                   ┌───────────┐
                   │  🖥️ GPU  │
                   │   🔥🔥🔥   │
                   └─────┬─────┘
                         │
                         ▼

        🧬  ⇄  🧬  ⇄  🧬  ⇄  🧬  ⇄  🧬
         ↘       ↗   ↘       ↗       ↘
          🌄─────🌄─────🌄─────🌄─────🌄
              CONFORMATIONAL LANDSCAPE


               Replica #37:
          "I started at λ₃₇...
           I have no idea where I am now." 🤷


        ──────────────────────────────────

             🔁 Exchanges accepted
             🧬 Barriers crossed
             🌄 Landscapes explored
             💾 Trajectories saved
             🔥 GPUs survived (hopefully)

                    🚀
             HAPPY HREMD SAMPLING!

        ──────────────────────────────────
```
