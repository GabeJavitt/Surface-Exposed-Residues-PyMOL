No problem, here is the text of the README presented as a standard response:

## PyMOL Surface Exposed Residues Finder

This is a Python script for PyMOL that automates the identification of surface-exposed residues in protein structures. It calculates the relative Solvent Accessible Surface Area (SASA) for each residue and exports the results to a CSV file, while also visualizing the exposed residues directly in PyMOL.

-----

### Overview

Finding surface-exposed residues is a common task in structural biology, often used for identifying potential distinct epitopes, active sites, or regions susceptible to chemical modification.

This script streamlines the process by:

1.  Loading a PDB structure.
2.  Optionally isolating specific chains of interest.
3.  Cleaning the structure (removing solvent and handling alternate locations).
4.  Calculating the absolute SASA for every residue.
5.  Comparing absolute SASA against theoretical maximums (Tien et al., 2013) to determine relative exposure.
6.  Exporting data to CSV and creating a PyMOL selection for visual inspection.

-----

### Features

  * **Automated SASA Calculation:** Uses PyMOL's `get_area` function with standard parameters (solvent radius 1.4 Å).
  * **Relative Exposure Threshold:** Residues are defined as "exposed" based on a user-definable percentage cutoff (default: 30%).
  * **Chain Selection:** Isolate and analyze specific chains in complex structures (e.g., analyzing just the antigen in an antigen-antibody complex).
  * **Robust Handling of Alt-Locs:** Automatically handles residues with alternate conformations by isolating the 'A' conformer before calculation to prevent double-counting SASA.
  * **CSV Export:** Generates a clean CSV file containing Chain, Residue ID, Residue Name, Absolute SASA, Max Reference SASA, and % Exposure.
  * **In-App Visualization:** Automatically creates a selection named `surface_exposed`, colors it yellow, and shows it as sticks for immediate validation.

-----

### Requirements

  * **PyMOL:** This script must be run within the PyMOL environment (Open-Source or Incentive PyMOL).

-----

### Quick Start

1.  **Download** the `find_exposed_residues.py` script.
2.  **Open** the script in any text editor (Notepad, TextEdit, VS Code, etc.).
3.  **Edit** the `Parameters to Customize` section at the very bottom of the file to match your needs (see Configuration below).
4.  **Run in PyMOL:**
      * Open PyMOL.
      * Go to **File \> Run Script...**
      * Select your modified `find_exposed_residues.py` file.
5.  The script will execute immediately, save a CSV to your specified path, and update the PyMOL 3D view.

-----

### Configuration

All configuration happens at the bottom of the script in the section labeled `# --- Parameters to Customize ---`.

| Parameter | Description | Example |
| :--- | :--- | :--- |
| `PDB_FILE_PATH` | Absolute path to your input PDB file. | `r"C:\Data\protein.pdb"` or `"/home/user/protein.pdb"` |
| `OUTPUT_FILE_PATH` | Desired path for the output CSV file. | `"results.csv"` |
| `CHAINS_TO_ANALYZE` | A list of chains to keep. All others are deleted before calculation. Set to `None` to keep everything. | `["A"]` or `["H", "L"]` or `None` |
| `SASA_CUTOFF_PERCENT` | The threshold for defining a residue as exposed (%). | `30.0` |
| `CREATE_PYMOL_SELECTION` | If `True`, creates a visual selection in PyMOL. | `True` or `False` |

#### Configuration Examples

**Example 1: Analyze only Chain A of a complex**

```python
PDB_FILE_PATH = r"C:\Users\data\complex.pdb"
OUTPUT_FILE_PATH = "chain_A_exposed.csv"
CHAINS_TO_ANALYZE = ["A"]  # Only Chain A is kept
SASA_CUTOFF_PERCENT = 30.0
CREATE_PYMOL_SELECTION = True
```

**Example 2: Analyze the entire structure with a stricter cutoff**

```python
PDB_FILE_PATH = "/home/user/data/structure.pdb"
OUTPUT_FILE_PATH = "/home/user/data/results_strict.csv"
CHAINS_TO_ANALYZE = None   # Analyze ALL chains
SASA_CUTOFF_PERCENT = 50.0 # Only highly exposed residues
CREATE_PYMOL_SELECTION = True
```

-----

### Output Format (CSV)

The output CSV file contains the following columns:

| Column | Description |
| :--- | :--- |
| **Chain** | The chain identifier (e.g., A, B). |
| **ResID** | The residue number (includes insertion codes if present, e.g., 100A). |
| **ResName** | The 3-letter amino acid code (e.g., TRP, ALA). |
| **ResidueSASA** | The calculated absolute SASA for this specific residue in Å². |
| **MaxSASA** | The theoretical maximum SASA for this amino acid type (from Tien et al.). |
| **PercentSASA** | `(ResidueSASA / MaxSASA) * 100`. |

*Note: Only residues meeting or exceeding the `SASA_CUTOFF_PERCENT` are written to the CSV file.*

-----

### Methodology & References

Relative SASA is calculated by dividing the observed SASA by a theoretical maximum SASA for that amino acid type in a tripeptide Gly-X-Gly context.

This script uses the theoretical maximum values from:

> **Tien, M. Z., Meyer, A. G., Sydykova, D. K., Spielman, S. J., & Wilke, C. O. (2013).**
> Maximum allowed solvent accessibilities of residues in proteins.
> *PLoS ONE*, 8(11), e80635. [DOI: 10.1371/journal.pone.0080635](https://www.google.com/search?q=https://doi.org/10.1371/journal.pone.0080635)

-----

### License

This project is open-source and available under the MIT License.
