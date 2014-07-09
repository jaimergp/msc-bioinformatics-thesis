.. role:: cite

.. role:: citein

.. raw:: latex

    \providecommand*\DUrolecite[1]{\citep{#1}}
    \providecommand*\DUrolecitein[1]{\citet{#1}}

============
 Chapter 01
============

------------
Introduction 
------------
primera edition 


The so-called small molecule universe (those whose molecular weight is less than 500 Da) is thought to comprise more than :math:`10^{60}` compounds . According to :citein:`Bohacek1996`, only an infinitesimal part of it it's actually explored: one part in :math:`10^{50}`. A recent study by :citein:`Virshup2013` represents that in a visually-appealing work. As depicted in figure 1, the resulting map is mostly white, which means that, despite having synthesized more than 100 million compounds, scientists have been focusing on the same regions over and over. This suggests that if new molecular design approaches are established, new regions would be visited and *brought to life*. A whole new world of possible solutions is simply waiting for us. *not very sure about this last sentence*

.. figure:: fig/uncharted_space.png 
	:align: center
	:height: 200 px

	Beratan and coworkers have mapped the small-molecule universe by means of a new computer algorithm called ACSESS (Algorithm for Chemical Space Exploration with Stochastic Search) that divides the chemical space in a six-by-six grid. The results show that most of it it's currently unexplored. Taken from :citein:`Virshup2013`

Thus, it's not surprising that computer-assisted molecular design is becoming an increasingly demanding area :cite:`Hoffer2013,Tang2014,Hoksza2014` where scientists are lacking specific software implementations. This is especially true in hybrid disciplines, such as chemobiological systems, where already existent solutions cannot face the high dimensionality it presents: all three chemical, biological and conformational axes need to be explored at once. If the chemical space was already immense, the chemobiological one is simply unfathomable. 

The conformational axis is usually considered first. Standard docking problems only comprise the spatial accommodation of a small ligand inside a protein pocket, a process in which 90% of the leading forces have to do with non-covalent interactions. However, molecular design is closely linked to covalent bonding and metal coordination, a marginally considered aspect in most cases. Indeed, one of the most promising fields in chemobiological design is the creation of artificial enzymes by combining well-known biological scaffolds with established industrial catalyst systems, which usually include the aforementioned metallic centres. This aspect makes the problem slightly more complicated since the metal moieties tend to be lowly parametrized in standard forcefields, thus leaving energy calculations out of scope.

Besides spatial allocation, molecular design has a higher dimensionality, which comprises additional axes like chemical modifications or biological mutations. When it comes to design a brand-new ligand that fulfil a set of given criteria, the researcher must visit both the chemical and biological space; i.e., take into account the possibility of having different chemical groups in crucial points of the molecule in order to, for example, guarantee a certain type of interaction with a central residue that is needed for the reaction to take place. Also, in biological contexts, chemical modification tends to mean *directed mutagenesis of an aminoacid*, which can have more structural consequences than a simple group variation. This dual aspect further enlarges the search space that molecular design has to deal with.

All of these issues result in a huge space in which an accurate representation of the problem cannot be afforded, so an alternative strategy must be applied. Our objective is to provide a program that can deal with all these problems by delivering an easy-to-deploy interface that can respond to commonly asked questions in the molecular design world.

.. figure:: fig/3Dmap.png
	:align: center
	:height: 200 px

	The chemobiological space can be studied in terms of three main axes: conformational, chemical and biological variations. Standard docking essays only move in the conformational axis and they already require a simplified expression of binding energies to help face their search space. Adding biological and chemical axes only enlarges that space, so a 


2. Current molecular design in silico strategies
================================================
Over the years, several exploration strategies have been applied with more or less success. While the chemobiological search space can be huge, it is also discrete, meaning that each candidate solution can be uniquely characterized by its structure and atomic components. With this in mind, some attempts have gone for exhaustive enumeration, in which a part of the search space is explored sequentially. Though it may seem inefficient, it has produced relevant results, as proved by :citein:`Fink2007`.

Exhaustive enumeration can work well if the constraints are limiting enough to reduce the search space to a feasible portion, but with bigger problems it is no longer the case. One alternative is to classify the enumerated elements in branches so, if the elements of one branch are detected as fruitless, they can be removed at once by pruning that branch. These algorithms are called *Branch and Bound* (BB) and have been implemented successfully in several fragment-based drug designs :cite:`Hajduk2007`.

However, the applicability of BB is limited and in some cases stochastic techniques are very much preferred, such as Monte Carlo-like algorithms (MC) :cite:`Das2008`, or even evolutionary approaches (EA) --- particularly, genetic algorithms (GA). This former group of strategies are extensively used in docking programs, like GOLD :cite:`Jones1997` or AutoDock :cite:`Trott2010`. Evolutionary algorithms are a common choice because they deal with several candidate solutions at once, which is also the case in these multi-objective optimization problems. This common partnership will be further detailed in chapter 3.

A recent advance proposes a new paradigm that focus on inverse relationships. Instead of enumerating a series of ligands and testing their fitness to the problem, inverse molecular design rely on optimizing molecular property functionals with respect to a limited number of chosen variables :cite:`Huggins2009`.

3. Current design approaches have some limitations
==================================================
Though the number of available software is not little by any means, very few of the programs can be actually used to deal with chemobiological problems. They either fail at it due to being too ambitious in their calculations accuracy, or limited by design to compute covalent interactions. For example, one could use Baker's Rosetta modelling tool to find a suitable ligand in a protein scaffold :cite:`Combs2013`, but that would need at least some scripting knowledge and a powerful computation cluster to start getting the first results. On the other hand, if the problem is approached using a docking protocol, the researcher would soon find that most of the programs do not support metal ions at all or, if they do, he or she would face awful complications :cite:`Ortega-Carrasco2014`. These are some of the main motivations behind this dissertation.

Covalent docking is still a Chimera
-----------------------------------
Of all the available docking programs, only a few support covalent docking essays. GOLD does provide an option to anchor the ligand to one of the protein atoms, and so does AutoDock, but that's it. If a researcher wanted to try several anchoring points in a branched ligand, he or she would find that it is currently impossible. Let alone looking for possible H bonds or hydrophobic patches for a given set of atoms. 

Though alternative methods are available, they are not versatile enough to meet our requirements, or rely on modifications on existent programs that tend to be overly complicated :cite:`Katritch2007`. A promising new option called CovalentDock was released past year as a modification of the popular AutoDock. This novel program implements a new layer in AutoGrid to help screen the possible acceptors and donors in the protein and the ligand, which results in improved accuracy :cite:`Ouyang2013`. However, it only allows a single covalent bond and is clearly biased towards drug screening, resulting in a limited option for strict molecular design.

Metallic moieties and docking essays
------------------------------------
GOLD or Glide are docking programs that support metal moieties in the protein but they were not designed to handle metal ions in the ligand itself. Though some attempts have been successful at extending this limitation with a series of tricks, such as substituting the metal elements with dummy atoms, these *hacks* force to consider the first coordination sphere of the metal as a rigid shell :cite:`Ortega-Carrasco2014`.

FlexX is another docking program that includes a knowledge-based approach to handle ligands with metallic centres and is able to predict coordination geometries and use that information as part of the docking process :cite:`Seebeck2008`. However, one of the challenges that artificial enzymes present is using exotic transition metals as an instrumental part of the reactivity. Since this kind of elements rarely appear on biological systems, we cannot conclude the effectiveness of FlexX until a thorough assessment is performed. 
