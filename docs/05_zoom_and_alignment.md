## Step 5: Zooming into the Galaxy

After generating an initial wide-field view of the simulation, the next step is to zoom in on the central galaxy. This allows for a more detailed analysis of its internal structure.

---

### 5.1 Focusing on the Most Massive Clump

Using the coordinates of the most massive clump identified in Step 3 (`center_coords`), a zoomed-in region can be extracted using `rd_cell()`:

```python
from miniramses import rd_cell

# Example: zoom with radius 0.005
c = rd_cell(11, path=output_path, center=center_coords, radius=0.005)
```

---

### 5.2 Extracting Physical Fields

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

### 5.3 Exploring Different Zoom Levels

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

![Zoomed-in XY view of the galaxy](https://github.com/carlossarkis7/miniramses/blob/carlossarkis7-2025project/2.2%20Photo%20XY%20zoomed%2050%20times%20section%205.png?raw=true)
*Figure: Progressive zoom into the galaxy revealing increasing structural detail.*
