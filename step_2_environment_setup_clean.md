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
