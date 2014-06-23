==========
Chapter 02
==========

-----------------
Introducing GAUDI
-----------------

|

1. What is GAUDI
================
Chemobiological researches are often challenged with molecular design tasks that combine several types of compounds, proceeding from both bio to artificial environments, including organic and inorganic structures. These hybrid systems are, by far, much less studied than their individual parts, which results in an obvious lack of software tools that can help confront the challenges they propose. A fresh approach that can satisfy this increasing demand is needed, and that is the blank GAUDI will try to fill.

**GAUDI** stands for **G**\ enetic **A**\ lgorithms for **U**\ niversal **D**\ esign **I**\ nference. It is a simple but robust attempt to provide the researchers with a powerful multi-objective exploration of the huge search space that chemobiological system design problems usually present.

Given a customizable list of objectives, GAUDI will optimize the given compounds to simultaneously satisfy all the requested criteria. Questions like *"Can I build a appropriately substituted 6-to-12-carbons-long ligand that features this prosthetic group, while keeping the terminal ends covalently attached to these two atoms, minimizing all possible clashes and finding the best set of rotamers for the binding pocket residues?"* have now an answer. 

1.1. Genetic exploration with multi-objective capabilities
----------------------------------------------------------
As the first two characters in the name suggest, GAUDI uses a genetic algorithm (GA) to explore the search space. Unlike programs like GOLD, which use a single-objective approach, GAUDI relies on the robust NSGA-II algorithm to provide full multi-objective exploration. All the objectives are optimized simultaneously, generating what is known as a *non-dominated Pareto Front*. This means that, by definition, there is not a single, top-score, undoubtedly-winner solution. 

Instead, a set of different possible structures is proposed, leaving the final decision up to the researcher. One may think that this further complicates the problem, rather than helping solve it, but there's no reason to believe this is a complex task. In fact, the scientist is given full control over all the possible solutions and, thanks to the accompanying GaudiView GUI tool, he or she can visually inspect the whole set through a straight-forward process. It simply consists of filtering the solutions with an individual cut-off for each objective and then sorting the results by any desired field.

1.2. Molecular design guided by evolutionary pressure
-----------------------------------------------------
GAUDI features a simple but powerful ligand design tool that allows to widen the chemical exploration possibilities. Instead of providing a list of already built ligands, the user can input a list of alphabetically-sorted directories that contain individual molecular building blocks. GAUDI will then parse the supplied files and build the resulting ligands on the fly, as requested by the genetic algorithm selection operators.

The builder does not impose any restraints on the building blocks, as long as they are formatted as standard mol2 files. By default, the blocks will be joined by the atoms that present the least and greatest serial number, respectively, but the user may specify any other atoms in an additional \*.attr file. 

This approach is versatile enough to explore the solution landscape in terms of spatial requirements (*"How many atoms would I need to build a ligand that can bridge these two subunits"*) and chemical substitutions (*"Which groups should this 10C ligand feature so it can form an hydrogen bond with this aspartic acid?"*). Thanks to the integrated parser, a short list of SMILES strings will suffice to launch an initial essay.

1.3. Biochemical and steric optimization of the search space
------------------------------------------------------------
GAUDI makes full use of Dunbrack's and Dynameomics rotameric libraries to optimize the conformational space of the protein. Just select the desired residues by their serial position and write them down in the GAUDI input file. The user may also activate a mutation flag so that the algorithm randomly swaps the selected residue with any other natural amino-acid. This feature enables us to explore the conformational space in both steric and biochemical dimensions. 

Of course, a lot of the mutations may not make sense for a particular position. Why would someone want to change an important GLU for a VAL residue? That's why the input supports specifying a subset of the natural amino-acids list, thus improving the sensitivity of the mutational search. If the new *mutamer* results in favouring some crucial interactions, its fitness will be better, and so will be their chances to *survive*.

The ligand flexibility is also configurable. Instead of letting choose between *rigid* and *flexible*, the input file requires a maximum torsion angle that will determine the global flexibility. If is set to zero, it will behave as a rigid compound; setting it up to 360 will have the opposite effect. Any integer in between is acceptable, so it is possible to choose a reasonable torsion that will provide just the right amount of flexibility without losing the initial conformational information. 

1.4. Forcefield-less energetic terms
------------------------------------
Artificial systems often take advantage of organometallic groups to produce new chemical reactions in a very precise chemospecific context. Because of the presence of those unparametrized metal ions, the commonly applied forcefields are rendered useless. Since quantum mechanics are still a long way of being used in screening essays, energetic terms are inevitably left out of scope.

To help palliate this issue, GAUDI proposes two workarounds. The first one exploits the multi-objective capabilities and provides several modules that try to resemble the forcefield interactions. As of now, GAUDI supports hydrogen bonds discovery, steric clashes  and hydrophobic patches detection, and solvent accessible and excluded surfaces calculations (via MSMS). These four forces are usually enough to guide the exploration in energetic terms. 

With this simple approach, metals can be happily part of both the protein and the ligand, overcoming one of the main limitations found in most of docking programs. In fact, nothing prevents us from using a metal ion as a ligand and optimize the surrounding rotamers to find a suitable coordination geometry, as it will be discussed in chapter 4.

2. Decisions made during the implementation
===========================================

|

3. Programmatic details
=======================

*Potentially, this can end up being an annex*