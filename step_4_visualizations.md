## Step 4  
### 4. Generating Visualizations  

Once the data was successfully loaded and the main clump identified, the next step was to visualize the galaxy using the tools provided in the Mini-RAMSES codebase.

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

This command plots the density field (`data.u[0]`) in the **XY plane**, using:

- a logarithmic scale (`log=1`)  
- the `"inferno"` color map  
- defined limits for visualization (`vmin` and `vmax`)

---

### Common Issue: log10 Warning  

At this stage, a warning may appear:

```
RuntimeWarning: invalid value encountered in log10
```

This occurred because the density field included **zero or negative values**, which are not valid inputs for `log10`.

---

### Fixing the Visualization Error  

To fix this:

- Temporarily remove `vmin` and `vmax`:

```python
visu(data.x[0], data.x[1], data.dx, data.u[0], log=1, cmap='inferno')
```

- Manually add `plt.show()` to force the display if it doesn’t appear automatically:

```python
import matplotlib.pyplot as plt
plt.show()
```

Once this was done, the first image of the galaxy successfully appeared.
