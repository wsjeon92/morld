# MORLD

This repository provides the source code of the main part of the MORLD software.

To run a demo, you need to prepare the enviroment described below or just simply go to MORLD web service (http://morld.kaist.ac.kr) and see the Tutorial page. The demo prepared at MORLD web service takes 1~2 days to get the result.

## Prepare

#### Enviroment setting
1. MolDQN and its requirements (RL framework): 
https://github.com/google-research/google-research/tree/master/mol_dqn

>The MORLD is based on MolDQN. 
Therefore, to run MOLRD in standalone, you must have an environment that you can run MolDQN.
Usage of the MORLD is similar with MolDQN too. 


2. rdkit (QED score): https://www.rdkit.org/docs/Install.html
3. gym-molecule library (SA score): https://github.com/bowenliu16/rl_graph_generation/tree/master/gym-molecule
4. QuickVina2 (docking score): https://github.com/QVina/qvina
5. open babel (converting file types of a molecule): https://openbabel.org/docs/dev/Installation/install.html
6. mgltools for linux (preprocessing a target protein): http://mgltools.scripps.edu/downloads


#### Preprocessing of a target protein


For running QuickVina2, the PDB file should have no ligand molecules.
You need to remove the ligand molecules with tools like pymol before docking.

Also, the target protein is given as a pdbqt file format.
Please follow the intruction of the below link to convert a pdb file format to a pdbqt file format.

http://autodock.scripps.edu/faqs-help/how-to/how-to-prepare-a-receptor-file-for-autodock4

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

The example file for demo is also provides as "config.txt" at this repository.

#### Prepare the required files
MORLD works inside the MolDQN. 
Place the below files into ```mol_dqn/chemgraph/``` directory.
1. ```optimized_BE.py``` file
2. ```receptor.pdbqt``` the receptor file with pdbqt format.
3. ```config.txt``` file

## Usage
#### Choose the output directory
<pre><code>export OUTPUT_DIR="./save"</code></pre>

#### Set the initial molecule (lead molecule)
<pre><code>INIT_MOL="CCCCCC"</code></pre>
> Set your own initial molecule with a SMILES representation.

#### Set the hyperparameters
At ```mol_dqn/configs/``` directory, there are json files for the hyperparameters.
You can change thoes hyperparameters as your desire. 

#### Optimization of binding affinity
<pre><code>python optimize_qed.py --model_dir=${OUTPUT_DIR} --start_molecule=${INIT_MOL} --hparams="./configs/bootstrap_dqn_step1.json"</code></pre>
> ```hparams``` could be a custom json file.

## Output
The output file ```optimized_result_total.txt``` contains the optimized molecules with SMILES, docking score, SA score, and QED score by tab delimiter.
MORLD web server (http://morld.kaist.ac.kr), in addition, provides files of docking pose of each optimized molecules. 
