## Step 3: Loading and Exploring the Data

Once the environment has been set up and the simulation data extracted, the next step is to load the data into Python and explore the galaxy's structure.

---

### 3.1 Loading the AMR Cell Data

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

### 3.2 Loading the Clump Catalog

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
