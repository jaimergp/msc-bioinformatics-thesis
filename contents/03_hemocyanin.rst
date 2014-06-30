.. role:: cite

.. role:: citein

.. raw:: latex

    \providecommand*\DUrolecite[1]{\citep{#1}}
    \providecommand*\DUrolecitein[1]{\citet{#1}}

============
 Chapter 03
============

------------------------
Case Study I: Hemocyanin
------------------------

|

1. Enzymes and organometallics
==============================
Life can be seen as a beautifully orchestrated succession of chemical reactions. Most of those reactions could not take place outside cells or, if they did, they would take an unaffordable amount of time. So, how do cells manage to do that? The answer lies in enzymes.

Enzymes are cell-tailored catalysts that provide the necessary environment for biological reactions to take place. The enzymes we know are the product of millions of years of evolution. During that period of time, nature has been sculpting their structure and functionality, so that they become better and better at doing their job. This results in a very specific chemospatial context that greatly lowers the energetic barrier of the transition state, allowing the reaction to happen at mild conditions in a very short period of time. 

To fulfil their role, most enzymes do not transform the substrate on their own. Instead, they are helped by a variable number of small diverse molecules called cofactors. One of the more interesting types of cofactors are transition metal compounds.

Transition metals exhibit unique chemical properties due to their versatile electronic configuration which allows them to present a wide range of oxidation states. Subsequently, they are able to coordinate to other atoms featuring a number of different geometries, which confers enough stability without renouncing to its distinguishing reactivity.

Artificial metalloenzymes seek to combine the specificity of biocatalysts with the reaction possibilities of organometallic groups in a single system. De novo design is still far from being doable, but meanwhile some other less ambitious approaches have been reported successful, such as DNA-based catalysts :cite:`Roelfes2005` and several types of protein-based designs. In this former group, three main strategies can be detailed, based on the type of ligand anchoring: dative, covalent or supramolecular :cite:`Steinreiber2008`. 

.. figure:: /home/jr/x/thesis/contents/fig/artificial_types.png
    :align: center
    :height: 200 px

    Dative anchoring (A) features xxxxx. Covalent anchoring has been used in XXXX. Supramolecular anchoring relies on covalently 
    
1.1 Oxygen transport in invertebrates: hemocyanins
--------------------------------------------------
Hemocyanins are a family of oxygen-carrier proteins in some invertebrate animals, such as snails, crabs, lobsters or spiders. The transport is achieved thanks to two copper atoms that bind an oxygen molecule. Their oxygen-carrying capabilities often result in comparisons with hemoglobins, though this former family uses iron for the binding. 

Hemoglobins are better carriers than hemocyanins in blood conditions, but hemocyanins outperform hemoglobins at low temperatures and oxygen pressure. This is the reason why they can be regarded as better candidates to inspire artificial oxygenases. Some studies have also pointed out that it has some antitumoral effects on murine models :cite:`Moltedo2006`. 

.. figure:: /home/jr/x/thesis/contents/fig/1nol.png
    :align: center
    :height: 200 px

    Hemocyanin features a di-copper centre that reversibly binds a single molecule of oxygen. This depiction shows the oxygenated form of *Limulus polyphemus*' hemocyanin (PDB: 1NOL, :citein:`Hazes1993`).

1.2 Streptavidin-biotin technology
----------------------------------
Biotin --- also known as vitamin B7 or H --- is the natural ligand of (strept)avidin, and they both make for a well-known couple in biology due to manifesting the strongest non-covalent interaction in nature; the dissociation constant gets up to :math:`K_d \approx 10^{-14}M` :cite:`Green1975`. The complex is also greatly resistant to extreme pH and temperature levels, organic solvents, proteolytic enzymes and other adverse conditions. All of these features make the system an excellent candidate for supramolecular anchoring techniques and biotechnological developments. So far, this has been one of the most successful strategies in the field. 

.. figure:: /home/jr/x/thesis/contents/fig/3pk2_pov.png
    :align: center
    :height: 200 px


    Ward et al. are actively working on streptavidin-biotin based systems to implement diverse metallogroups. One of their results is an artificial hydrogenase (PDB: 3PK2, :citein:`Ward2011`), in which a catalyst (violet dye) is covalently linked to a biotin (grey dye) inside the streptavidin pocket (brown surface). This way, the biotin acts as an anchor that helps place the catalyst inside the streptavidin scaffold. 

2. The challenge: an artificial hemocyanin
==========================================
One of the new experiments Ward's group is working in is an artificial hemocyanin, built upon the streptavidin-biotin system. Several wet-lab attempts have been tried, but none of them have succeeded. In order to shed light on the problem, a GAUDI simulation was run based on the following experiment requirements.

    1. The hemocyanin core shall be placed around the interface of the two hemocyanin subunits.
    2. It shall be covalently linked to the two biotins that reside in each of the afore-mentioned subunits.
    3. Subsequently, two linkers of unknown length have to be used to connect the biotins with the hemocyanin core.

The problem was implemented in GAUDI following what we have called an *anchor & seek* strategy. This approach consists of a covalent bond restraint on one end of the dynamically constructed ligand and one covalent-suitable distance objective on the other end. The built-in genetic algorithm will then optimize the linkers length and their torsion angles to help the chain reach the other biotin while minimizing the clashes and maximizing hydrogen bond forming and hydrophobic interactions.

The dynamical builder was fed with this overall structure: ``linker - hemocyanin core - linker``. The so-called ``linker`` block could be represented by any of the following compounds: ethane, propane, butane, pentane, hexane, heptane and octane. 

The ``hemocyanin core`` block was built from scratch following a draft provided by Ward (see figure **X**) and then minimized with a standard quantum-mechanics protocol. The resulting structure was then processed and converted into a standard GAUDI-compatible mol2 file. This block was intentionally left rigid and the algorithm did not alter any of its torsion angles. 

.. figure:: fig/coreblock.png
    :align: center
    :height: 200 px

    From draft to QM-minimized structure, step-by-step.

.. raw:: latex

    \newpage

    \bibliographystyle{newapa}

    \bibliography{bibliography}