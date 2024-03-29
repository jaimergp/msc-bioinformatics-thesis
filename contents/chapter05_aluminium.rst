.. role:: cite

.. role:: citein

.. raw:: latex

    \providecommand*\DUrolecite[1]{\citep{#1}}
    \providecommand*\DUrolecitein[1]{\citet{#1}}

========================
Case Study II: Aluminium
========================


|

5.1. Metals and life
====================
As it was advanced in the previous chapter, metals play an important role in several biological processes. They are central to a lot of essential biological reactions, but they can also be very toxic. As enunciated by Paracelsus, it's the dose that makes the poison. Life processes are very sensitive to concentration changes, especially when it comes to metal ions. As a result, if blood levels of a given metal experiments a sudden increase or decrease of concentration, it can lead to devastating consequences. For example, the most common case of anaemia is caused by iron deficiency: without iron, the cells cannot form enough functional haemoglobin to bind oxygen and support their metabolism.

Besides their metabolic implications, anomalies in the homeostasis of some metals have been reported to participate in some neurodegenerative disorders, such as Parkinson's (PD) or Alzheimer's (AD) disease. While AD is characterized by the accumulation of amyloid-beta plaques in the brain, its true etiology remains unknown. Aluminium has been in the spotlight since its brain accumulation was first described by :citein:`Crapper1973`, amongst other metals :cite:`Shcherbatykh2007, Gonzalez-Dominguez2014`. Although mainstream science seems to have abandoned the aluminium hypothesis due to some controversy in the previous studies :cite:`Santibanez2007`, it still attracts some scientists :cite:`Lidsky2014`.

5.1.1. Aluminium as a biometal
------------------------------
Aluminium, despite its relatively high abundance, is barely used in living organisms. However, the increase of its concentration in our environment had made its interaction with biological material more frequent. In spite of all this, its actual binding capabilities are rather puzzling.

Aluminium(III) can coordinate to several different elements, but it particularly leans towards nitrogen and oxygen atoms in biological environments :cite:`Kaim1994`, though oxygen is preferred due to its smaller size and larger negative charge. Thus, in a proteic environment, it will mostly bind to aspartic and glutamic acids, serine residues and the oxygens found in the backbone. The resulting complexes tend to feature coordination numbers of four, five and six :cite:`GreenWood1997`, which usually means *trigonal planar, tetrahedral, trigonal bipyramidal, and octahedral geometries*.

5.2. The challenge
==================
In collaboration with the group of Prof. Xabier López at the UPV/EHU, our group aims at identifying the mechanism under which aluminium interact with b-amyloids and generate three dimensional models of the most likely binding modes of coordination. After having explored under quantum mechanical studies cluster models limited to the first coordination of the metal that Al(III) prefers, coordination that involved three to four carboxyl containing residues, GAUDI was used to satisfy those criteria under a proteic environment. 

A first filtering of all the b-amyloid structures available in the PDB was performed considering the optimized position of alpha-carbons of all acidic residues and carbonyl side chains of the experimental structures. GRID-optimized systems obtained from the PDB screening resulted in PDB entries 1AMB, 1AMC, 1AML, 1BA4, 1BA6, 1BJB, 1BJC, 1IYT, 1NMJ, 1Z0Q, 2LFM, 2M9R, and 2M9S. GAUDI\ :sub:`ASM` objectives were therefore to find the nearby oxygen-containing residues and optimize their rotameric configuration so they can allocate the aluminium in a feasible coordination geometry. Backbone oxygen atoms were also allowed to participate in the complex.

5.3. Current techniques in bioinformatics
=========================================
This case study can be regarded as a docking problem where the ligand is a single atom of aluminium. However, existent docking solutions rarely accept metals in the input, let alone being the naked ligand. Furthermore, rotameric optimization is usually directed towards hydrogen bond forming, Van der Waals contacts, and avoiding clashes, not towards the discovery of suitable coordination geometries. GOLD does implement this feature but is not optimized to handle them as part of the ligand :cite:`Ortega-Carrasco2014`, and so does FlexX :cite:`Seebeck2008`, but it does not support naked metal ions as ligands. Since GAUDI\ :sub:`ASM` can be easily extended to adopt new objectives and features, we thought of creating an alternative launcher script that would meet the requirements of this essay.

5.4. Our strategy
=================
GAUDI\ :sub:`ASM` can treat any UCSF Chimera compatible input file as a ligand, so using a single aluminium atom as a ligand works out of the box. However, the provided dataset already included a tentative position for the aluminium, making that step unnecessary. Instead, the new script only had to detect existent aluminium atoms and register them as ligands. If more than one were found, only the one with the lowest serial number would be taken into account.

.. figure:: fig/pseudo-al.pdf 
	:width: 200 px
	
	Pseudocode for the aluminium case study. This is slightly different from the GAUDI\ :sub:`ASM` launcher script, but uses the same framework.

Once found, the initial set-up was performed. Since each of the 13 PDB entries contained several different poses, which in total accounted for 157 possible input files, a complementary helper script was designed to run all of them in batch. Each of the essays started with a population of 300 individuals and ran for 15 generations, with mu, lambda, and crossover probability set to 0.5, and mutation rate set to 0.2.

In each essay, the aluminium was allowed to move to a random point within 2.5A of the starting position, and then search space field was created within 7.0A of the new position. If one or more oxygen atoms were found in the search sphere, their correspondent residues were registered in the rotamers optimization list. It mostly comprised aspartic and glutamic acids, and some serines too.

Detecting feasible coordination geometries can be an attractive feature and definitely will be implemented in the future, but for this essay it was not necessary. After choosing a new set of rotamers from the Dunbrack's library, the distances of the three nearest oxygen atoms were calculated, as well as the standard and dihedral angles that their two immediate neighbours, themselves, and the aluminium ion formed. GAUDI\ :sub:`ASM` was then set up to minimize both the distances to 2.2A with a minimum distance threshold of 1.8A, and the absolute sine of each of the dihedrals, seeking to obtain information about the planarity requirements of the coordinate residue, be it mono or bidentate. In order to avoid infeasible steric conformations, the clashes were calculated and minimized too. 

.. figure:: fig/dihedral.png
	:width: 200 px
	
	The directionality of the coordination interaction is expressed as the absolute sine of the dihedral angle formed between the atoms 1, 2, 3, and 4; also, the absolute sine of the angle formed between 1, 2, and 3 is taken into account. An absolute sine of zero means perfect alignment, while an absolute sine of one means perfect perpendicularity.

Last but not least, an additional scoring was added that accounted for the number of oxygen-containing residues found within 2.0A from the aluminium atom, which would give an approximate idea of the coordination number.

5.5. Discussion of results
==========================
The generated Pareto front of solutions comprised more than 19,000 possible candidates, so an obvious filtering was needed. These were the applied restrictions.

 - Clashes < 10 nm³ 
 - At least two oxygen atoms within 2.0A from the aluminium
 
This configuration left us with 11 solutions. Of these 11, a total of five poses were chosen and proposed to Xabier. These were based on PDB entries 1AMC, 1AML, 1BJB, 1BJC, and 1Z0Q, whose details can be read in table Z. As it can be seen, the poses tend to feature three or four proximal oxygen atoms that would suggest an octahedral geometry. These atoms usually come from the carboxylic group of aspartic and glutamic acids, though some oxygen atoms from the atom can be also pointed out.

.. figure:: fig/results-aluminium.png
	:height: 200 px

	Selected solutions from the Pareto front proposed by the GAUDI\ :sub:`ASM` essay. Aluminium ions are depicted in bright magenta.

.. raw:: latex

	\begin{table}[h]
	\centering
	\caption{Details of the scoring of each proposed solutions. \emph{Nearby oxygens} refers to the number of oxygen atoms within 2.2A of the aluminium ion. \emph{Average distance} is calculated from the distance of the three nearest oxygens to the aluminium ion. \emph{Average planarity} is the mean of the absolute sines of the dihedrals formed with aluminium, oxygen, and the next two non-terminal atoms in the peptidic chain. A planarity of zero means that the chains are aligned, while a planarity of one means they are orthogonal. More details are given in Appendix A.}
	\label{my-label}
	\resizebox{\textwidth}{!}{%
	\begin{tabular}{@{}lllll@{}}
	\toprule
	PDB code & Nearby oxygens & Clashes (nm³) & Average distance (nm) & Average planarity \\ \midrule
	1AMC & 2 & 7.8922 & 2.6789 & 0.6433 \\
	1AML & 2 & 6.1656 & 2.3198 & 0.9189 \\
	1BJB & 2 & 7.6941 & 2.2176 & 0.5629 \\
	1BJC & 2 & 8.1273 & 2.3761 & 0.8315 \\
	1Z0Q & 2 & 9.665 & 2.3291 & 0.7289 \\ \bottomrule
	\end{tabular}
	}
	\end{table}


Conclusions
-----------
While this kind of essays was not initially considered to be one of those GAUDI\ :sub:`ASM` would deal with, this side project has proved that it is actually able to handle them. Of course, it required a separate script, but the required modifications are not drastic and could be easily implemented in the core framework. Simple geometric parameters like threshold distances, angles and dihedrals have reported interesting results, raising our interest in this unexplored field of docking experiments. As a result, a more advanced and very promising predictive engine is already under development.
