# Adatom Diffusion Workflow (LAMMPS + Python)

![LAMMPS](https://img.shields.io/badge/LAMMPS-Simulations-blue)
![Python](https://img.shields.io/badge/Python-Analysis-yellow)
![Status](https://img.shields.io/badge/Status-Archived-lightgrey)

**A clean, Python-integrated reference implementation for simulating adatom diffusion on FCC surfaces (Cu, Pt).**

This repository provides a complete "input-to-analysis" workflow for Molecular Dynamics (MD). It demonstrates how to set up surface diffusion simulations in LAMMPS, manage potentials (EAM), and automatically parse trajectory data into publication-ready energy plots using Python.

---

## Project Structure

This project is organized by material system. Each directory contains the simulation logic (`.in`), potentials (`.eam`), and analysis scripts specific to that experiment.

| Directory | Description |
| :--- | :--- |
| **`/adatom_cu`** | Copper (Cu) adatom diffusion on Cu(100). |
| **`/adatom_pt`** | Platinum (Pt) adatom diffusion on Pt(100). |

## Quick Start

### 1. Prerequisites
* **LAMMPS**: Ensure `lmp` (or `lmp_komp` for Kokkos) is in your PATH.
* **Python 3.x**: Required for analysis scripts.
    ```bash
    pip install matplotlib
    ```

### 2. Running a Simulation (Example: Copper)
Navigate to the experiment directory and run the LAMMPS input script.

```bash
cd adatom_cu

# Standard run
lmp -in Cu_EAM.in

# High-performance run (Kokkos/OpenMP)
lmp_komp -k on t 8 -sf kk -pk kokkos newton on -in Cu_EAM.in
````

### 3. Analyzing Results

Use the included Python utility to parse the thermodynamic output and generate visualization.

```bash
# Plot Energy Conservation
python plot_energy.py --input energy_avg.txt --output results_energy.jpg --skip 30

# Plot Trajectories (XY and Z planes)
python plot_xy.py --input dump.lammpstrj --output results_xy.jpg
python plot_xy.py --input dump.lammpstrj --output results_z.jpg --do_z
```

-----

## Technical Details

  * **Potential**: Embedded Atom Method (EAM) alloy potentials.
  * **Ensemble**: Initial equilibration via velocity rescaling, followed by NVE integration for energy conservation checks.
  * **Minimization**: Conjugate gradient minimization (`1.0e-4`) applied before dynamics to remove steric overlaps.

## Credits & References

**Author:** Hunter Heidenreich (Harvard University)

This workflow is derived from and inspired by the work of **Eric N. Hahn**.

  * Original Tutorial: [Eric N. Hahn's Adatom Example](https://www.ericnhahn.com/tutorials/lammps-tutorials/adatom)
  * Further Reading: [Hunter's Blog Post on LAMMPS]([http://hunterheidenreich.com/posts/lammps-sim/](https://hunterheidenreich.com/posts/adatom-cu-diffusion/))

## 📄 License

This project is open-source and available for educational and research purposes.
