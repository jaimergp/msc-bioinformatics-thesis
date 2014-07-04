.. role:: cite

.. role:: citein

.. raw:: latex

    \providecommand*\DUrolecite[1]{\citep{#1}}
    \providecommand*\DUrolecitein[1]{\citet{#1}}

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

**GAUDI** stands for **G**\ enetic **A**\ lgorithms for **U**\ niversal **D**\ esign **I**\ nference. It consists of a novel platform that aims to satisfy a increasingly demanding area in molecular design: artificial chemobiological systems. It does so by providing the researchers with a powerful multi-objective optimization engine to explore the huge search space that chemobiological design problems usually present.

Given a customizable list of objectives, GAUDI will optimize the given compounds to simultaneously satisfy all the requested criteria. Questions like *"Can I build a appropriately substituted 6-to-12-carbons-long ligand that features this prosthetic group, while keeping the terminal ends covalently attached to these two atoms, minimizing all possible clashes and finding the best set of rotamers for the binding pocket residues?"* have now an answer. 

2. The methodology: Genetic exploration with multi-objective capabilities
=========================================================================
Most real-life optimization problems comprise several (usually conflicting) objectives, but they tend to focus on just one of them, describing the other ones as restraints. For example, given a cone, one would want to minimize both their total surface and the lateral surface. A first approach would try to fix the total surface and then find the smallest lateral surface, or the other way around. At any case, both strategies wouldn't consider the wide variety of intermediate solutions :cite:`Coello2001`.

Molecular design problems belong to this kind of problems. Most of the existent approaches consist of an energy minimization guided by a set of user-provided restraints. However, this strategy may be leaving a lot of the possible solutions out of scope. Why would we want to renounce to any chances of finding the right molecular construction that will solve our problem?

*!# Place cone analogy picture here*

Having multiple objectives implies that the concept of a single "optimum" solution is no longer valid. Instead, multi-objective optimization algorithms usually propose a set of good *trade-offs* between the studied variables :cite:`Coello2001`. This idea is known as *Pareto optimality*, as enunciated by Wilfredo Pareto in his studies of income distribution.

Given a set of candidate solutions, a Pareto improvement is a change that can make at least one solution better off, without worsening the situation of the other candidate solutions. When no further Pareto improvements can be applied on the set, that set is said to be *Pareto optimal*. The so-called *Pareto front* is the set of all the Pareto optimal solutions, and, in principle, all of them are good answers to the problem. With n dimensions or objectives, the Pareto front can be depicted as an hypersurface that hosts the optimal solutions of the hypervolume.

Finding the true Pareto front can be difficult, but it can be approximated by a rich set of of non-dominated solutions. A solution ``a`` is set to dominate solution ``b`` if it solves at least one of the objectives better than ``b``, without losing to ``b`` in any of the remaining objectives. In words, ``a`` dominates ``b`` if it makes for a better answer to the problem.

As using Pareto optimality criteria usually means working with multiple solutions, it makes sense to use exploration algorithms that can deal with several candidate solutions at once. One of the most common choices are genetic algorithms (GA). GAs are part of evolutionary algorithms, which, as their name states, are heavily inspired by Darwin's evolution theory. In fact, they take a lot of the nomenclature from it. For example, a candidate solution is called *individual*, whose terms, variables or parameters are named *genes* or *chromosomes*. The base idea is to expose the candidate solutions to an evolutionary simulation, in which the fitness of the individuals is determined by an evaluation function that runs the optimization process. 

A simple GA starts by generating a random set of *individuals*, the so-called initial *population*. Then, that population is exposed to a series of evolutionary operators, such as gene mutation, chromosome recombination or, in some approaches, migration. As a result, a new set of individuals is produced by the parent population. Some of them will be *fitter* than their preceding counterparts, some of them not. That's why all of them are tested by the evaluation function, which will return their *fitness* in the form of a score. Only the fittest individuals will be allowed to continue in the solutions pool or, in biological terms, *selected* to take part in the next *generation*.

After a few generations, the population will have evolved towards a reasonable set of solutions that approximate the Pareto optimal front. As the number of objectives increases, it becomes harder to accurately reconstruct the true Pareto front. Furthermore, it can consist of hundreds of solutions. To determine which one the researcher is really looking for, a scalarization technique must be applied --- after all, only a section of the hypersurface might be necessary. Of all the possible approaches, weighted sums are one of the most common due to their simplicity, but also encompasses a few limitations, such as knowing the importance and precedence of the different objectives beforehand :cite:`Hwang1979`. As GAUDI mixes energetic, geometric and chemobiological criteria together, this cannot be anticipated easily; instead, GAUDI returns the whole Pareto front, leaving the decision up to the researcher's own criterion and the visual advice provided by Chimera and GaudiView, GAUDI's accompanying GUI tool.

3. Main features
================

GAUDI relies on two main projects to achieve its functionality: UCSF Chimera :cite:`Chimera` and DEAP :cite:`Deap`, both written in Python. On top of these two main pillars, a custom framework has been implemented to help guide molecular design essays. Further technical details of this implementation are discussed in Appendix A,

3.1 Molecular design guided by evolutionary pressure
----------------------------------------------------
GAUDI features a simple but powerful ligand design tool that allows to widen the chemical exploration possibilities. Instead of providing a list of already built ligands, the user can input a list of alphabetically-sorted directories that contain individual molecular building blocks. GAUDI will then parse the supplied files and build the resulting ligands on the fly, as requested by the genetic algorithm selection operators.

The builder does not impose any restraints on the building blocks, as long as they are formatted as standard mol2 files. By default, the blocks will be joined by the atoms that present the least and greatest serial number, respectively, but the user may specify any other atoms in an additional ``\*.attr`` file. 

This approach is versatile enough to explore the solution landscape in terms of spatial requirements (*"How many atoms would I need to build a ligand that can bridge these two subunits"*) and chemical substitutions (*"Which groups should this 10C ligand feature so it can form an hydrogen bond with this aspartic acid?"*). Thanks to the integrated parser, a short list of SMILES strings will suffice to launch an initial essay.

3.2 Biochemical and steric optimization of the search space
-----------------------------------------------------------
GAUDI makes full use of Dunbrack's and Dynameomics rotameric libraries to optimize the conformational space of the protein. Just select the desired residues by their serial position and write them down in the GAUDI input file. The user may also activate a mutation flag so that the algorithm randomly swaps the selected residue with any other natural amino-acid. This feature enables us to explore the conformational space in both steric and biochemical dimensions. 

Of course, a lot of the mutations may not make sense for a particular position. Why would someone want to change an important GLU for a VAL residue? That's why the input supports specifying a subset of the natural amino-acids list, thus improving the sensitivity of the mutational search. If the new *mutamer* results in favouring some crucial interactions, its fitness will be better, and so will be their chances to *survive*.

The ligand flexibility is also configurable. Instead of letting choose between *rigid* and *flexible*, the input file requires a maximum torsion angle that will determine the global flexibility. If is set to zero, it will behave as a rigid compound; setting it up to 360 will have the opposite effect. Any integer in between is acceptable, so it is possible to choose a reasonable torsion that will provide just the right amount of flexibility without losing the initial conformational information. 

3.3 Forcefield-less energetic terms
-----------------------------------
Artificial systems often take advantage of organometallic groups to produce new chemical reactions in a very precise chemospecific context. Because of the presence of those unparametrized metal ions, the commonly applied forcefields are rendered useless. Since quantum mechanics are still a long way of being used in screening essays, energetic terms are inevitably left out of scope.

To help palliate this issue, GAUDI proposes two workarounds. The first one exploits the multi-objective capabilities and provides several modules that try to resemble the forcefield interactions. As of now, GAUDI supports hydrogen bonds discovery, steric clashes  and hydrophobic patches detection, and solvent accessible and excluded surface areas calculations. These four forces are usually enough to guide the exploration in energetic terms. 

With this simple approach, metals can be happily part of both the protein and the ligand, overcoming one of the main limitations found in most of docking programs. In fact, nothing prevents us from using a metal ion as a ligand and optimize the surrounding rotamers to find a suitable coordination geometry, as it will be discussed in chapter 5.


3.4 GaudiView: A GUI explorer for Pareto fronts
-----------------------------------------------
As previously stated, multi-objective optimization processes often generate more than one possible solution to the problem. To help find the most suitable ones, GAUDI includes a visual tool to help explore the Pareto Front: GaudiView. GaudiView has been tailored as a native Chimera extension that lazy-loads the whole range of solutions, no matter the size. Some complex experiment can produce thousands of candidate individuals, so the program must be both efficient and effective. Lazy-loading avoids long initial wait times, since the file is only loaded into memory when it's actually requested. This is the same approach that GAUDI internally uses to build the ligands library on the fly.

Furthermore, the GUI tool includes multiple sorting and filtering utilities to help discern the adequate portion of the Pareto hypersurface. In some complex cases, the set of solutions may not include what the researcher would call a *perfect solution*, but he or she may be able to identify a *pretty good one* if some trade-offs are applied. Instead of performing another run, GaudiView allows to parse the whole Pareto front and retrieve the most promising results effortlessly.

Last but not least, a number of goodies have been included to add special visual support in some specific cases. GaudiView provides effective integration channels with some powerful built-in tools of UCSF Chimera, such as the Metal Geometry utility or the MMTK minimizer. This premium features open the doors to a vertical integrative platform where the researcher would be able to obtain reasonably sound solutions by simply writing a list of objectives.

.. raw:: latex

    \newpage

    \bibliographystyle{newapa}

    \bibliography{bibliography}