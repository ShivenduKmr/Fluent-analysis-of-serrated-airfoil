# Aeroacoustic Analysis of a Bio-Inspired Serrated Propeller using ANSYS Fluent

## Project Overview

This repository contains the complete CFD project for the aeroacoustic analysis of a bio-inspired propeller featuring trailing-edge serrations, based on designs inspired by owl wings. The primary objective was to quantitatively evaluate the noise reduction capabilities of the serrated propeller compared to a conventional baseline design. The analysis was conducted using the ANSYS suite (SpaceClaim, Fluent Meshing, and Fluent Solver).

The final results demonstrate a significant noise reduction of **2.8 dB**, validating the effectiveness of the bio-inspired design.

## Objectives
- To model and simulate a baseline propeller and a modified propeller with trailing-edge serrations (SER6).
- To perform a transient CFD analysis to accurately capture the unsteady flow field.
- To use the Ffowcs Williams-Hawkings (FW-H) acoustic analogy to predict far-field noise.
- To quantitatively compare the Overall Sound Pressure Level (OASPL) of both designs to determine the noise reduction.

---

## Methodology & Workflow

The entire analysis was performed using **ANSYS 2023 R1**. The workflow was designed to be robust and efficient for a laptop environment.

### 1. Geometry Preparation (ANSYS SpaceClaim)
- Two models were prepared: a baseline propeller and the SER6 serrated propeller.
- Both models were rotated to align their axis of rotation with the Z-axis, simplifying the setup.
- A cylindrical fluid domain was created, consisting of a small inner **Rotating Domain** and a large outer **Stationary Domain**.
- The propeller geometry was subtracted from the rotating domain, and the **Watertight Geometry Workflow** (`Share Topology` = `None`) was used to prepare the model for robust meshing.

### 2. Meshing (ANSYS Fluent Meshing)
The modern Fluent Meshing "Watertight Workflow" was chosen to bypass persistent errors encountered with the classic `Share Topology` method.
- **Algorithm:** The `Patch Independent Tetrahedral` algorithm was used for its robustness with complex CAD.
- **Refinement:** A `Face Size` control of **5 mm** was applied to the propeller surfaces (`propeller_walls`) to capture geometric details.
- **Boundary Layers:** A simple `Smooth Transition` with **3** layers was added to the propeller walls to improve the near-wall flow prediction.
- **Goal:** This created a lightweight yet sufficiently detailed mesh suitable for running on a memory-constrained system.

### 3. Simulation Setup (ANSYS Fluent)
The "Fastest Possible Acoustics" plan was developed for a stable and time-efficient solution.
- **Solver Type:** `Transient`
- **Turbulence Model:** **SST (Scale-Adaptive Simulation)**. This hybrid model was chosen as a robust alternative to a full LES, providing the necessary unsteady turbulence resolution for acoustics while maintaining stability.
- **Acoustic Model:** The **Ffowcs Williams-Hawkings (FW-H)** analogy was used.
- **Acoustic Data Export:** To prevent memory-related crashes, the acoustic source data was **exported** during the run (`.asd` files) and post-processed separately.
- **Rotation:** `Mesh Motion` was enabled for the rotating domain at **3000 RPM** (314.159 rad/s).
- **Run Parameters:** The simulation was run for **2000 time steps** with a time step size of **1e-04 s**, corresponding to 10 full propeller revolutions.

### 4. Post-Processing
- The exported acoustic data (`.asd` files) was loaded into Fluent.
- The `FFT` (Fast Fourier Transform) tool was used to convert the time-domain pressure signals into a frequency-domain plot of **Sound Pressure Level (SPL) vs. Frequency**.
- The final **Overall Sound Pressure Level (OASPL)** was computed for each case.

---

## Results & Key Findings

The primary finding is a quantifiable reduction in noise due to the trailing-edge serrations.

**Noise Reduction:** **2.8 dB**
- **Baseline Propeller OASPL:** 62.1 dB
- **Serrated Propeller OASPL:** 59.3 dB

### Acoustic Spectra Comparison

**Baseline Propeller Noise Spectrum:**
![Baseline SPL Plot](results/images/baseline_spl_plot.png)

**Serrated Propeller Noise Spectrum:**
![Serrated SPL Plot](results/images/serrated_spl_plot.png)

- **Validation:** The primary noise peak for both propellers was correctly identified at the Blade Passing Frequency of **100 Hz**.
- **Observation:** The serrated propeller shows a noticeable reduction in the amplitude of the primary BPF peak and its harmonics, confirming the physical mechanism of noise reduction.

---

## How to Use This Repository

- **Software Needed:** ANSYS 2023 R1 or a compatible version.
- **Geometry:** The SpaceClaim files are located in the `/geometry` folder.
- **Fluent Setup:** The Fluent case files (`.cas.h5`) are in the `/fluent-setup` folder. You can open these to review all the solver settings. Note that these files do not contain the solution data.
- **To Replicate:**
  1. Open the `.scdoc` file and export a geometry for meshing.
  2. Follow the meshing steps outlined in the methodology.
  3. Open the `.cas.h5` file in Fluent and associate it with the new mesh.
  4. Run the simulation.

---

## Acknowledgments
This project was inspired by the methodologies and findings presented in academic research on bio-inspired aeroacoustics, such as the work by Zang, Azarpeyvand, et al.

---
