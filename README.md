# MORLD
MORLD is a molecule optimization method based on reinforcement learning and docking. This repository provides the source code of the main part of the MORLD software and its usage.

To run a demo, you need to prepare the enviroment described below.

Or simply go to MORLD web service (http://morld.kaist.ac.kr) and see the Tutorial page. The demo prepared at MORLD web service takes 1~2 days to get the result.

## Prepare

#### Enviroment setting
1. MolDQN and its requirements (RL framework): 
https://github.com/google-research/google-research/tree/master/mol_dqn

>The MORLD is based on MolDQN. 
Therefore, to run MORLD in standalone, you must have an environment that you can run MolDQN.
Usage of the MORLD is similar with MolDQN too. 


2. rdkit (QED score and molecule modification): https://www.rdkit.org/docs/Install.html
3. gym-molecule library (SA score): https://github.com/bowenliu16/rl_graph_generation/tree/master/gym-molecule
4. QuickVina2 (docking score): https://github.com/QVina/qvina
5. open babel (converting file types of a molecule): https://openbabel.org/docs/dev/Installation/install.html
6. mgltools for linux (preprocessing a target protein): http://mgltools.scripps.edu/downloads

#### Verified dependencies
The working of MORLD has been verified in the versions below.

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


#### Preprocessing of a target protein


For running QuickVina2, the PDB file should have no ligand molecules.
You need to remove the ligand molecules with tools like pymol before docking.
And PDB file should be protonated to appropriately.
You can use PDB2PQR server to protonate PDB file.

Also, the target protein is given as a pdbqt file format.
Please follow the intruction of the below link to convert a pdb file format to a pdbqt file format.

http://autodock.scripps.edu/faqs-help/how-to/how-to-prepare-a-receptor-file-for-autodock4
or
https://bioinformaticsreview.com/20200716/prepare-receptor-and-ligand-files-for-docking-using-python-scripts/)

We provide an example pdbqt file of the protein DDR1 (discoidin domain receptor 1) ```3zosA_prepared.pdbqt``` for demo in this repository.

#### Configuration file for docking
For running QuickVina2, you need a configuration file.
Make the configuration file looks like below as "config.txt". 
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

>Fill the file name of the receptor at the placeholder of receptor. (You do not need to change the name of ligand file.)
Fill the binding pocket information with the coordinate and the size of the grid box in Angstrom (Å). 

An example configuration file for demo is also provides as ```config.txt``` in this repository.

#### Place the required files
MORLD works inside the MolDQN. 
Place the below files into ```mol_dqn/chemgraph/``` directory.
1. ```optimized_BE.py``` file
2. ```3zos.pdbqt``` the receptor file with pdbqt format.
3. ```config.txt``` file

## Usage
#### Choose the output directory
<pre><code>export OUTPUT_DIR="./save"</code></pre>

#### Set the initial molecule (lead molecule)
<pre><code>export INIT_MOL="C1CC2=CC=CC=C2N(C1)C(=O)CN3CCC(CC3)NC4=NC(=CC(=O)N4)C(F)(F)F"</code></pre>
> Set your own initial molecule with a SMILES representation.
The example SMILES is ZINC12114041 which is found by virtual screening against the protein DDR1 (3zos).

#### Set the hyperparameters
At ```mol_dqn/configs/``` directory, there are json files for the hyperparameters.
You can change thoes hyperparameters as your desire. 

#### Optimization of binding affinity
<pre><code>python optimize_BE.py --model_dir=${OUTPUT_DIR} --start_molecule=${INIT_MOL} --hparams="./configs/bootstrap_dqn_step1.json"</code></pre>
> ```hparams``` could be a custom json file.

## Output
The output file ```optimized_result_total.txt``` contains the optimized molecules with SMILES, docking score, SA score, and QED score by tab delimiter.

MORLD web server (http://morld.kaist.ac.kr), in addition, provides files of docking pose of each optimized molecules. 
