# Step 7: Aligning the Disk with the Coordinate Axes

> ⚠️ **Advanced Section**  
> This step involves more complex operations like angular momentum computation and coordinate rotation.  
> It is intended for advanced users with a strong background in astrophysical simulation analysis.  

After visualizing the galaxy in multiple planes, it becomes clear that the disk is not perfectly aligned with the simulation box. To create consistent, interpretable visualizations, the galaxy must be rotated so that its disk lies flat in the XY plane, with its angular momentum vector pointing along the z-axis.

---

### 7.1 Center the Data Around the Clump

First, all cell positions are shifted so that the center of the most massive clump is at the origin:

```python
x0, y0, z0 = x - x_c, y - y_c, z - z_c

# Next, subtract the clump’s velocity from each component so that velocities are measured in the clump’s rest frame.
vx0 = vx - vx_c
vy0 = vy - vy_c
vz0 = vz - vz_c  # subtract clump velocity components
```
---

### 7.2 Calculate the Angular Momentum Vector

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

### 7.3 Build and Apply the Rotation

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

### 7.4 Visualize the Aligned Galaxy with `visu()`

After rotation, the disk lies in the new X′Y′ plane. Visualizations confirm the alignment:

```python
visu(x1, y1, dx, rho, log=1, cmap='inferno')
plt.show()
```

Side views can also inspected:

```python
visu(x1, z1, dx, rho, log=1)
plt.show()

visu(y1, z1, dx, rho, log=1)
plt.show()
```


### 7.5 Visualizing the Disk from Multiple Angles (Hexbin View)

Hexbin plots are useful for density comparisons across projections:

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
![Face-on view of aligned disk (X′Y′)](https://github.com/carlossarkis7/miniramses/blob/carlossarkis7-2025project/4.1%20Photo%20X'Y'%20aligned%20axis%20section%207.png?raw=true)
*Figure: Face-on view of aligned galactic disk (X′Y′ projection).*


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
![Side view of aligned disk (Y′Z′)](https://github.com/carlossarkis7/miniramses/blob/carlossarkis7-2025project/4.3%20Photo%20Y'Z'%20aligned%20axis%20section%207.png?raw=true)
*Figure: Side view of aligned galactic disk (Y′Z′ projection).*



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
![Edge-on view of aligned disk (X′Z′)](https://github.com/carlossarkis7/miniramses/blob/carlossarkis7-2025project/4.2%20Photo%20X'Z'%20aligned%20axis%20section%207.png?raw=true)
*Figure: Edge-on view of aligned galactic disk (X′Z′ projection).*

### 7.6 Visualizing the Improved Appearance of the Disk Using make_image_2d()

To improve the appearance of the image, we can use the make_image_2d() function from miniramses. By artificially increasing the cell size using dx_factor, we simulate a smoothing effect that improves the visual clarity. This technique is especially helpful when the grid is very refined and produces noisy images.

```
from miniramses import make_image_2d

img = make_image_2d(x1, y1, rho, dx_factor=10)
plt.imshow(np.log10(img + 1e-6), origin='lower', cmap='inferno')
plt.colorbar(label='log Density')
plt.title("Smoothed Disk View (XY′ plane)")
plt.tight_layout()
plt.show()
```

*Placeholder: Face-on view of aligned disk (XY′ projection)*
