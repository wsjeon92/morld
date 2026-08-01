# MORLD
MORLD is a molecule optimization method based on reinforcement learning and docking. This repository provides the source code of the main part of the MORLD software and its usage.

Paper: Jeon, W., Kim, D. Autonomous molecule generation using reinforcement learning and docking to develop potential novel inhibitors. Sci Rep 10, 22104 (2020). https://doi.org/10.1038/s41598-020-78537-2

To run a demo, you need to prepare the environment described below.

Or simply visit the MORLD web service (<https://morld.apps.cloud.kbds.re.kr/>) and see the Tutorial page. The demo provided on the MORLD web service takes 1–2 days to produce a result.

## Prepare

#### Environment setting
1. MolDQN and its requirements (RL framework): 
https://github.com/google-research/google-research/tree/master/mol_dqn

>MORLD is based on MolDQN. 
Therefore, to run MORLD standalone, you must have an environment in which you can run MolDQN.
Usage of MORLD is similar to that of MolDQN.


2. rdkit (QED score and molecule modification): https://www.rdkit.org/docs/Install.html
3. gym-molecule library (SA score): https://github.com/bowenliu16/rl_graph_generation/tree/master/gym-molecule
4. QuickVina2 (docking score): https://github.com/QVina/qvina
5. open babel (converting file types of a molecule): https://openbabel.org/docs/dev/Installation/install.html
6. mgltools for linux (preprocessing a target protein): http://mgltools.scripps.edu/downloads

#### Verified dependencies
MORLD has been verified to work with the versions below.

1. python: 3.7.6
2. MolDQN: Latest github version (https://github.com/aksub99/MolDQN-pytorch)
3. rdkit: 2018.09.1 
4. mgltools: mgltools_Linux-x86_64_1.5.7 (https://ccsb.scripps.edu/mgltools/downloads/)
5. gym-molecule: Latest github version (https://github.com/bowenliu16/rl_graph_generation/tree/master/gym-molecule)
6. QuickVina2: Latest github version (https://github.com/QVina/qvina)
7. open babel: 2.4.1
8. pandas: 1.0.1
9. baselines: Latest github version (https://github.com/openai/baselines#installation)
10. absl-py: 0.9.0
11. networkx: 2.4
12. numpy: 1.18.1
13. tensorflow: 1.14.0
14. gast: 0.3.3 (recommended to suppress warnings)


#### Preprocessing of a target protein


QuickVina2 requires a PDB file with no ligands. Remove them with a tool such as PyMOL before docking.
The PDB file should also be properly protonated; you can use the PDB2PQR server for this.

The target protein must also be provided in pdbqt format.
Please follow the instructions in the link below to convert a pdb file to a pdbqt file.

http://autodock.scripps.edu/faqs-help/how-to/how-to-prepare-a-receptor-file-for-autodock4
or
https://bioinformaticsreview.com/20200716/prepare-receptor-and-ligand-files-for-docking-using-python-scripts/

For the demo, we provide an example pdbqt file of the protein DDR1 (discoidin domain receptor 1), ```3zosA_prepared.pdbqt```, in this repository.

#### Configuration file for docking
For running QuickVina2, you need a configuration file.
Create the configuration file as shown in the example below. 
<pre><code>receptor = receptor.pdbqt
ligand = ligand.pdbqt

#binding_pocket
center_x = ###
center_y = ###
center_z = ###

size_x = ###
size_y = ###
size_z = ###
</code></pre>

>Enter the receptor file name in the receptor field. (You do not need to change the ligand file name.)
Fill in the binding pocket information with the coordinates and size of the grid box in angstroms (Å). 

An example configuration file for the demo is also provided as ```config.txt``` in this repository.

#### Place the required files
MORLD runs inside MolDQN. 
Place the following files into the ```mol_dqn/chemgraph/``` directory.
1. ```optimize_BE.py``` file
2. ```3zosA_prepared.pdbqt```, the receptor file in pdbqt format.
3. ```config.txt``` file

## Usage
#### Choose the output directory
<pre><code>export OUTPUT_DIR="./save"</code></pre>

#### Set the initial molecule (lead molecule)
<pre><code>export INIT_MOL="C1CC2=CC=CC=C2N(C1)C(=O)CN3CCC(CC3)NC4=NC(=CC(=O)N4)C(F)(F)F"</code></pre>
> Set your own initial molecule with a SMILES representation.
The example SMILES is ZINC12114041, which was found by virtual screening against the protein DDR1 (3zos).
