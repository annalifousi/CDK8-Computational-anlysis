PDB Miner
CDK8 Uniprot id:P49336
Uniprot Id obtained from: https://www.uniprot.org/uniprotkb?query=cdk8
In the linux command line:
1. going to the exercise folder: cd /home/projects/22117_proteins/projects/group9/anna
2. running pdb miner for CDK8: /home/ctools/PDBminer/PDBminer -u P49336 -n 4 -f csv
3. saving PDBMiner's results at local computer: scp s232979@pupil3.healthtech.dtu.dk:/home/projects/22117_proteins/projects/group19/structure_selection/results/P54132/P54132_all.csv /Users/annalifousihotmailcom

AlphaFold2 predicted model of P49336 obtained from: https://alphafold.ebi.ac.uk/entry/P49336
Docking (CDK8-carboximide)
For Molecular Docking we used FastDRH web server tool available at: http://cadd.zju.edu.cn/fastdrh/overview
1.obtaining selected structure's pdb file: https://github.com/annalifousi/CDK8-Computational-anlysis.git by searching structure's id 5XS2
2.cleaning the structure in pymol from heteroatoms: click on S button on the down right size in PyMol interface -> select water molecules(indicated by 0) and ligands GOL,8D6 -> file -> save molecule
3.obtaining carboxamides pdb file from: https://pubchem.ncbi.nlm.nih.gov/compound/81689726
4.creating reference pocket file: open 5xs2.pdb in Pymol -> file -> open carboxamide.pdb -> click on 3-button to enable editing -> shft + treadwheel click on mouse to move the carboxamide molecule where it actually interacts with CDK8 -> save molecule reference_WILDTYPE.pdb
5.docking: visit http://cadd.zju.edu.cn/fastdrh/submit -> on receptor structure upload 5xs2_cleaned.pdb -> on ligand structure upload carboxamide.pdb -> next -> enable molecular docking -> upload pocket reference reference_WILDTYPE.pdb -> click AutoDock Vina and pose 10 -> submit

For mutated structures: 
1. mutate structures in Pymol: wizard -> mutogenesis -> click on the residue -> click on "No mutation" and select the mutated residue -> Apply -> File -> Save molecule 5xs2_cleaned_mutated_1.pdb
2.Repeat for other combination of mutations based on this table:
                    1. K52D/A100K
                    2. K52D/A100W
                    3. K52F/A100K
                    4. K52F/A100W
3. Repeat docking steps 3-5 for all the mutated structures
Assesing Local Interactions CDK8 - CycC:
Process 5XS2 pdb file in Pymol (using the command line) :
1.remove solvent (this specific molecule has water as solvent and we removed it)
2.split_chains (the chains are splitted to chains A and B, A is CDK8 and B is cyclinC
3.remove hetatm ( to remove glycerol and carboximide, Foldx can not work without this)
4.file --> export structure --> export molecule --> save as pdb file (5XS2_nohet.pdb)

In the linux command line:
make new folder : mkdir Anny3
1.copy all the mutatex essential files from mutatex folder in exercise 5 in the server, while being in the directory of interest, in this case Anny3:
cp /home/projects/22117_proteins/lecture5_exercise/mutatex_templates/mutatex/templates/foldxsuite5/mutate_runfile_template.txt .
cp /home/projects/22117_proteins/lecture5_exercise/mutatex_templates/mutatex/templates/foldxsuite5/repair_runfile_template.txt .
cp /home/projects/22117_proteins/lecture5_exercise/mutatex_templates/mutatex/templates/foldxsuite5/interface_runfile_template.txt .
cp /home/projects/22117_proteins/lecture5_exercise/mutatex_templates/mutatex/templates/mutation_list.txt .

2.upload 5XS2_nohet.pdb
3.make position list, which shows which positions will be mutated : posnew.txt
This was made using the command nano posnew.txt, each amino acid to be mutated has to be specified, otherwise it will be a whole protein scan.
The position is specified by: one letter code of the amino acid, the chain in which the amino acid is situated and the residue number, for example for alanine in chain A position 234, we would write : AA234.

Once all the folders are uploaded on the directory of interest mutatex was runned by the command:
nohup mutatex 5XS2_nohet.pdb -p 2 -m mutation_list.txt -x /home/ctools/foldx5_2024/foldx -f suite5 -R repair_runfile_template.txt -M mutate_runfile_template.txt -q posnew.txt -L -l -v -C  none -B  -I interface_runfile_template.txt &
Subsequently run:
tail -f nohup.out to monitor the progress, once the message All done! appears the results can be processed:
1. Creation of a ΔΔG csv file :
/home/ctools/anaconda3_2021.11/bin/ddg2excel -p 5XS2_nohet.pdb -l mutation_list.txt -q posnew.txt -d results/interface_ddgs/final_averages/A-B/ -F csv
2. Creation of a heatmap:
/home/ctools/anaconda3_2021.11/bin/ddg2heatmap -p 5XS2_nohet.pdb -l mutation_list.txt -q posnew.txt -d results/interface_ddgs/final_averages/A-B/






