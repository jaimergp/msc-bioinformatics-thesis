.. role:: cite

.. role:: citein

.. raw:: latex

    \providecommand*\DUrolecite[1]{\citep{#1}}
    \providecommand*\DUrolecitein[1]{\citet{#1}}

============
 Chapter 04
============

------------------------
Case Study II: Aluminium
------------------------

|

1. Metals and life
==================
As it was advanced in the previous chapter, metals play an important role in several biological processes. They are central to a lot of essential biological reactions, but they can also be very toxic. As enunciated by Paracelsus, it's the dose that makes the poison. Life processes are very sensitive to concentration changes, especially when it comes to metal ions. As a result, if a given metal experiments a sudden increase or decrease in blood levels, it can lead to devastating consequences. For example, the most common case of anaemia is caused by iron deficiency: without iron, the cells cannot form enough functional haemoglobin to bind oxygen and support their metabolism.

Besides their metabolic implications, anomalies in the homeostasis of some metals have been reported to participate in some neurodegenerative disorders, such as Parkinson's (PD) or Alzheimer's (AD) disease. While AD is characterized by the accumulation of amyloid-beta plaques in the brain, its true ethiology is still unknown. Aluminium has been in the spotlight since its brain acumulation was first described by :citein:`Crapper1973`, amongst other metals :cite:`Shcherbatykh2007, Gonzalez-Dominguez2014`. Although mainstream science seems to have abandoned the aluminium hypothesis due to some controversy in the previous studies :cite:`Santibanez2007`, it still attracts some scientists :cite:`Lidsky2014`.

1.1. Aluminium as a biometal
----------------------------
Aluminium(III) can coordinate to several different elements, but it particularly leans towards nitrogen and oxygen atoms in biological environments :cite:`Kaim1994`, though oxygen is preferred due to its smaller size and larger negative charge. Thus, in a proteic environment, it will mostly bind to aspartic and glutamic acid, serine residues and the oxygens found in the backbone. The resulting coordinate complexes tend to feature coordination numbers of four, five and six :cite:`GreenWood1997`, which usually means trigonal planar, tetrahedral and trigonal bipyramidal geometries, respectively.

2. The challenge
================
Xabier López (Department of Science & Technology of Polymer, Faculty of Chemistry, UPV/EHU) and coworkers are wondering if both observations can be compatible: does aluminium have some thing to do with amyloid-beta plaque accumulation? This case study tries to shed some light on the matter and discuss the possible results.

Dr. López supplied a set of GRID-optimized systems obtained from a PDB screening essay that looked for observed amyloid-beta structures, resulting in PDB entries 1AMB, 1AMC, 1AML, 1BA4, 1BA6, 1BJB, 1BJC, 1IYT, 1NMJ, 1Z0Q, 2LFM, 2M9R, and 2M9S. However, the GRID protocol used by López et al. only optimized the position of alpha-carbons while looking for energy minima, thus ignoring rotameric alterations and their chemosteric effects. Given the input library, we were asked to find the nearby oxygen-containing residues and optimize their rotameric configuration so they can allocate the aluminium in a feasible coordination geometry. Backbone oxygens were also allowed to participate in the complex, but their position should not be changed in order not to disrupt the tertiary structure of the protein.

3. Current techniques in bioinformatics
=======================================
This case study can be regarded as a docking problem where the ligand is a single atom of aluminium. However, existent docking solutions rarely accept metals in the input, let alone being the naked ligand. Furthermore, rotameric optimization is usually directed towards hydrogen bond forming and Van der Waals contacts, and avoiding clashes, not towards the discovery of suitable coordination geometries. GOLD does implement this feature but is not optimized to handle them as part of the ligand :cite:`Ortega-Carrasco2014`, and so does FlexX :cite:`Seebeck2008`, but it doesn't support naked metal ions as ligands. Since GAUDI can be easily extended to adopt new objectives and chromosomes, we thought of creating an alternative launcher script that would meet the requirements of this essay.

4. Our strategy
===============
GAUDI can treat any Chimera-compatible input file as a ligand, so using a single aluminium atom as a ligand works out of the box. However, the provided dataset already included a tentative position for the aluminium, making that step unnecessary. Instead, the new script only had to detect existent aluminium atoms and register them as ligands. If more than one were found, only the one with the lowest serial number would be taken into account.

Once found, the initial setup was performed. The aluminium was allowed to move to a random point within 2.5A of the starting position, and then search space field was created within 7.0A of the new position. If one or more oxygens were found in the search sphere, their correspondent residues were registered in the rotamers optimization list. It mostly comprised aspartic and glutamic acids, and some serines too. 

Detecting feasible coordination geometries can be an attractive feature and definitely will be implemented in the future, but for this essay it was not necessary. After choosing a new set of rotamers from the Dunbrack's library, the distances of the three nearest oxygens were calculated, as well as the dihedral angle that their two immediate neighbors, themselves, and the aluminium ion formed. GAUDI was then setup to minimize both the distances to 2.2A with a minimum distance threshold of 1.8A, and the absolute sine of each of the dihedrals, seeking to obtain information about the planarity requirements of the coordinate residue, be it mono or bidentate. In order to avoid unfeasible steric conformations, the clashes were calculated and minimized too. Least but not last, an additional scoring was added that accounted for the number of oxygen-containing residues found within 2.0A from the aluminium atom, which would give an approximate idea of the coordination number.

Since each of the 13 PDB entries contained several different poses, which in total accounted for 157 possible solutions, a helper script was designed to run all of them in batch. It simply consisted of a Python subprocess handler to launch sequential instances of UCSF Chimera. Each of the essays started with a population of 300 individuals and ran for 15 generations, with mu, lambda, and crossover probability set to 0.5, and mutation rate set to 0.2.

5. Discussion of results
========================
The Pareto front 

.. raw:: latex

    \newpage

    \bibliographystyle{newapa}

    \bibliography{bibliography}