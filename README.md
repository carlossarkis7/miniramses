# Galaxy Simulation Project with RAMSES

This repository documents a Python-based workflow for analyzing galaxy simulation data produced by the RAMSES code. The project focuses on building a reproducible, well-documented pipeline for extracting, visualizing, and interpreting galactic structure from AMR simulations.

---

## Project Overview

- Analyze RAMSES output using Python
- Visualize galaxy structure at multiple scales
- Align galactic disks using angular momentum calculations
- Build a clean and modular analysis pipeline

---

## Documentation

The full guide is divided into sections available in the [`docs/`](docs) folder:

1. [Step 1 – Project Goals](docs/01_intro_goals.md)  
2. [Step 2 – Setting Up the Environment](docs/02_environment_setup.md)  
3. [Step 3 – Loading and Exploring the Data](docs/03_data_loading.md)  
4. [Step 4 – Initial Visualization](docs/04_visualization.md)  
5. [Step 5 – Zooming into the Galaxy](docs/05_zoom_and_alignment.md)  
6. [Step 6 – Multi-Angle Visualization](docs/06_multi_angle_visualization.md)  
7. [Step 7 – Disk Alignment and Rotation (Advanced)](docs/07_disk_alignment_and_rotation.md)

---

## Example Outputs

![Initial YZ-plane projection of the galaxy](images/1.3%20Photo%20initial%20YZ%20not%20zoomed%20section%204.png)
*Figure: Initial YZ-plane projection of the galaxy using the `visu()` function*


![Aligned galactic disk (X′Y′)](images/4.1%20Photo%20X'Y'%20aligned%20axis%20section%207.png)  
*Aligned galactic disk after angular momentum rotation*

---

## Author and Acknowledgments

This project was developed by Carlos Sarkis as part of a supervised research initiative during the summer of 2025, under the direct mentorship of Professor Romain Teyssier.

Professor Teyssier is a globally recognized expert in computational astrophysics, and a full professor at Princeton University. His research spans a wide range of areas in theoretical astrophysics, including galaxy formation and evolution, cosmological simulations, and numerical methods for magnetohydrodynamics. He is also the original author of RAMSES, a pioneering Adaptive Mesh Refinement (AMR) code that has become a reference tool in large-scale structure and galactic dynamics research.

All simulation analysis, data processing, and visualization documented in this repository were conducted by the author under Professor Teyssier’s scientific supervision, as part of a focused project on galaxy-scale structure and dynamics using the Mini-RAMSES framework.

---

## Contact

Carlos Sarkis  
carlos.sarkis@outlook.com
