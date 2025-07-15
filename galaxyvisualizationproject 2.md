## Step 1  
### 1. Project Goals

The main objective of this project is to develop a clean and reusable Python-based workflow for analyzing galaxy simulation data produced by the **RAMSES** code.

Specifically, the goals are to:

- Gain hands-on experience working with RAMSES galaxy simulation outputs.  
- Understand how to visualize astrophysical data using Python tools.  
- Create a method to identify and focus on the central galaxy in a simulation.  
- Explore the galaxy’s structure from multiple angles and zoom levels.  
- Apply coordinate transformations to align the galactic disk with the axes for cleaner visualizations.  
- Build a step-by-step, well-documented process that could be reused in similar research contexts.


## Step 2  
### 2. Setting Up the Environment

Before working with the RAMSES simulation data, the first step is to prepare the coding environment and ensure access to the necessary tools and files.

---

### Installing Git

Git is a version control system that allows you to track changes in your code, collaborate with others, and download ("clone") complete project repositories from the internet. It is widely used in software development and scientific computing.

To install Git on Windows:

- Visit [https://gitforwindows.org](https://gitforwindows.org)  
- Download and install Git using the default options.  
- After installation, you will have two new applications:  
  - **Git Bash** – a terminal that allows you to run Git commands.  
  - **Git GUI** – a visual interface for basic Git tasks.  

---

### Creating a Project Folder and Opening Git Bash

1. Choose or create a folder on your computer where you want to store all files for this project (e.g., `GalaxyProjectSummer2025`).  
2. Navigate to that folder in Windows Explorer.  
3. Right-click inside the folder and select **“Git Bash Here”** — this opens a terminal window pointed directly at your working directory.  

---

### Cloning the Mini-RAMSES Repository

In the Git Bash terminal, run:

```bash
git clone https://bitbucket.org/rteyssie/mini-ramses.git
```

This command creates a local copy of the Mini-RAMSES codebase in a folder named `mini-ramses`, which contains all the necessary Python scripts.

---

### Downloading and Extracting the Simulation Data

1. Download the file `output_00011.tar` from the link provided by Professor Teyssier.  
2. Move the file into your project folder.  
3. Extract the contents by running the following command in Git Bash or a terminal:

```bash
tar xvf output_00011.tar
```

---

### Python Environment Setup

- Ensure Python (version 3.8 or higher) is installed on your computer.  
- Install required Python libraries:

```bash
pip install numpy matplotlib
```

- Other dependencies may be required depending on the version of the code (e.g., `scipy`, `h5py`).  

---

### Final Project Structure

At this stage, your project folder should include:

- `mini-ramses/` – the Python codebase  
- `output_00011/` – the simulation data  
- Your own script(s) or Jupyter notebooks for visualization  


## Step 3  
### 3. Loading and Exploring the Data

Once the environment has been set up and the simulation data extracted, the next step is to load the data into Python and explore the galaxy's structure.

---

### Loading the AMR Cell Data

RAMSES simulations divide space into **adaptive mesh refinement (AMR)** cells. To load the data for output number 11:

```python
from miniramses import load_cell

output_path = "C:/path/to/project"  # replace with your actual path
data = load_cell(11, path=output_path)
```

This command loads all the AMR cells from `output_00011`, including positions (`data.x`), sizes (`data.dx`), and physical quantities like density (`data.u[0]`).

Once loaded, you can inspect basic information:

```python
print(data.x[0].shape)               # Number of cells
print(data.u[0].min(), data.u[0].max())  # Density range
```

---

### Loading the Clump Catalog

To identify individual dark matter clumps (e.g., halos), use the function:

```python
from miniramses import rd_clump

clumps = rd_clump(output=11, path=output_path)
```

This returns a catalog of detected clumps, including their masses, positions, and radii. Each row in the catalog corresponds to one clump.

You can then locate the most massive clump:

```python
import numpy as np

index = np.argmax(clumps["mass"])
center_coords = clumps["pos"][index]
```

This `center_coords` value is later used to center the visualization on the main galaxy.

---

![Data Loading Visualization](images/data_loading_overview.png)  
*Figure: Overview of data loading and clump identification.*


## Step 4  
### 4. Generating Visualizations

Once the data has been successfully loaded and the main clump identified, the next step is to visualize the galaxy using the tools provided in the Mini-RAMSES codebase.

---

### Using the `visu()` Function

Mini-RAMSES includes a custom visualization function called `visu()` defined in `miniramses.py`. It can be used to create 2D projections of physical quantities (like density) in any plane.

A typical usage:

```python
from miniramses import visu
import matplotlib.pyplot as plt

visu(data.x[0], data.x[1], data.dx, data.u[0], log=1, vmin=-6, vmax=2, cmap='inferno')
plt.show()
```

This command plots the density field (`data.u[0]`) in the XY plane, using:

- a logarithmic scale (`log=1`)
- the `"inferno"` color map
- defined limits for visualization (`vmin` and `vmax`)

Once this is done, the first image of the galaxy should successfully appear.

---

![Galaxy Visualization](images/galaxy_visualization_step4.png)  
*Figure: Initial XY-plane projection of the galaxy using the `visu()` function.*


## Step 5  
### 5. Zooming into the Galaxy

After generating an initial wide-field view of the simulation, the next step is to zoom in on the central galaxy. This allows for a more detailed analysis of its internal structure.

---

### Focusing on the Most Massive Clump

Using the coordinates of the most massive clump identified in Step 3 (`center_coords`), a zoomed-in region can be extracted using `rd_cell()`:

```python
from miniramses import rd_cell

# Example: zoom with radius 0.005
c = rd_cell(11, path=output_path, center=center_coords, radius=0.005)
```

---

### Extracting Physical Fields

The `rd_cell()` function returns a data object containing all the physical quantities within the zoomed region. From it, we extract:

```python
# Positions
x, y, z = c.x[0], c.x[1], c.x[2]

# Cell sizes
dx = c.dx

# Physical fields
rho = c.u[0]         # density
vx, vy, vz = c.u[1], c.u[2], c.u[3]   # velocity components
```

This step provides access to:

- The spatial position of each cell  
- The density field (`rho`)  
- The velocity field (`vx`, `vy`, `vz`)  

These fields will later be used to compute properties like angular momentum and align the disk.

---

### Exploring Different Zoom Levels

The `radius` parameter in `rd_cell()` can be varied to explore the structure at different scales:

- `0.05` → large-scale view of galaxy surroundings  
- `0.005` → central galactic disk  
- `0.001` → high-resolution view of core  

Each time, the resulting region is visualized using `visu()`:

```python
visu(x, y, dx, rho, log=1, cmap='inferno')
plt.show()
```

These views reveal increasing detail — from the overall shape to disk asymmetries and central density enhancements.

---

![Zoomed Galaxy View](images/zoomed_galaxy_step5.png)  
*Figure: Progressive zoom into the galaxy revealing increasing structural detail.*


## Step 6  
### 6. Multi-Angle Visualization

After zooming in on the galaxy, the next step is to explore its three-dimensional structure by visualizing it in different planes. This allows for a clearer understanding of the galaxy's shape, orientation, and vertical structure.

---

### Changing the Projection Plane

The `visu()` function can display any 2D projection of the data by changing the first two spatial coordinates passed to it. Using the extracted variables from Step 5, different views can be generated:

- **XY Plane (face-on view):**
```python
visu(x, y, dx, rho, log=1, cmap='inferno')
```
*Placeholder: XY projection image*

- **XZ Plane (side view):**
```python
visu(x, z, dx, rho, log=1, cmap='inferno')
```
*Placeholder: XZ projection image*

- **YZ Plane (side view from another angle):**
```python
visu(y, z, dx, rho, log=1, cmap='inferno')
```
*Placeholder: YZ projection image*

All views are displayed using:

```python
import matplotlib.pyplot as plt
plt.show()
```

---

### Why Multi-Angle Visualization Matters

Each projection provides unique insights:

- **XY**: reveals the disk structure, overall shape, and possible spiral arms.  
- **XZ** and **YZ**: show the disk’s vertical thickness, symmetry, and possible tilts or warps.

These comparisons are essential for assessing how well-aligned the galaxy is with the coordinate axes — and they lead directly into the next step: realigning the galaxy to face-on orientation.



# Step 7

## 7. Aligning the Disk with the Coordinate Axes

After visualizing the galaxy in multiple planes, it becomes clear that the disk is not perfectly aligned with the simulation box. To create consistent, interpretable visualizations, the galaxy must be rotated so that its disk lies flat in the XY plane, with its angular momentum vector pointing along the z-axis.

---

### Step 1: Center the Data Around the Clump

First, all cell positions are shifted so that the center of the most massive clump is at the origin:

```python
x0, y0, z0 = x - x_c, y - y_c, z - z_c
vx0, vy0, vz0 = vx, vy, vz  # velocities are already relative to the clump
```

---

### Step 2: Calculate the Angular Momentum Vector

The angular momentum vector is computed explicitly from the positions, velocities, and mass of each AMR cell:

```python
m = rho * dx**3  # mass of each cell

Lx = np.sum(m * (y0 * vz0 - z0 * vy0))
Ly = np.sum(m * (z0 * vx0 - x0 * vz0))
Lz = np.sum(m * (x0 * vy0 - y0 * vx0))

L = np.array([Lx, Ly, Lz])
L_hat = L / np.linalg.norm(L)  # normalized direction vector
```

This vector is perpendicular to the plane of the disk and will define the new z-axis after rotation.

---

### Step 3: Build and Apply the Rotation

An orthonormal basis is constructed using the angular momentum direction:

```python
# New basis: u3 = disk normal, u1 and u2 = in-plane axes
u3 = L_hat
u1 = np.cross(u3, [0, 0, 1])
if np.linalg.norm(u1) == 0:
    u1 = np.cross(u3, [0, 1, 0])
u1 /= np.linalg.norm(u1)
u2 = np.cross(u3, u1)

# Rotation matrix: columns = new basis vectors
R = np.vstack([u1, u2, u3]).T  # shape (3, 3)

# Apply rotation to all positions
coords = np.vstack([x0, y0, z0])        # shape: (3, N)
rotated = R.T @ coords                  # rotate into new frame

x1, y1, z1 = rotated[0], rotated[1], rotated[2]
```

---

### Step 4: Visualize the Aligned Galaxy with `visu()`

After rotation, the disk lies in the new XY′ plane. Visualizations confirm the alignment:

```python
visu(x1, y1, dx, rho, log=1, cmap='inferno')
plt.show()
```
*Placeholder: Face-on view of aligned disk (XY′ projection)*

Side views were also inspected:

```python
visu(x1, z1, dx, rho, log=1)
plt.show()

visu(y1, z1, dx, rho, log=1)
plt.show()
```
*Placeholder: Side views of aligned disk (XZ′ and YZ′ projections)*

---

### Step 5: Visualizing the Disk from Multiple Angles (Hexbin View)

To enhance visual clarity, hexbin plots are used to inspect the aligned galaxy in all three planes:

#### Disk Top View (x′–y′)
```python
plt.figure(figsize=(8, 6))
plt.hexbin(x1, y1, C=rho, gridsize=200, bins='log', cmap='plasma')
plt.xlabel("x'")
plt.ylabel("y'")
plt.title("Disk Top View")
plt.colorbar(label="log Density")
plt.tight_layout()
plt.show()
```
*Placeholder: Top-down disk image (x′–y′)*

#### Disk Side View (y′–z′)
```python
plt.figure(figsize=(8, 6))
plt.hexbin(y1, z1, C=rho, gridsize=200, bins='log', cmap='inferno')
plt.xlabel("y'")
plt.ylabel("z'")
plt.title("Disk Side View")
plt.colorbar(label="log Density")
plt.tight_layout()
plt.show()
```
*Placeholder: Side view of disk (y′–z′ projection)*

#### Disk Edge/Profile View (x′–z′)
```python
plt.figure(figsize=(8, 6))
plt.hexbin(x1, z1, C=rho, gridsize=200, bins='log', cmap='viridis')
plt.xlabel("x'")
plt.ylabel("z'")
plt.title("Disk Edge View")
plt.colorbar(label="log Density")
plt.tight_layout()
plt.show()
```
*Placeholder: Edge-on view of disk (x′–z′ projection)*


