## Step 4: Generating Visualizations

Once the data has been successfully loaded and the main clump identified, the next step is to visualize the galaxy using the tools provided in the Mini-RAMSES codebase.

---

### 4.1 Using the `visu()` Function

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

![Initial XY-plane projection of the galaxy using the visu() function](https://github.com/carlossarkis7/miniramses/blob/carlossarkis7-2025project/1.3%20Photo%20initial%20YZ%20not%20zoomed%20section%204.png?raw=true)
*Figure: Initial XY-plane projection of the galaxy using the `visu()` function*
