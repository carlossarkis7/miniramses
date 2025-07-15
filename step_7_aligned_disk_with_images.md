
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
