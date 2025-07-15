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
