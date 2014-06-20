GAUDI: Genetic Algorithms for Unified Docking Inference
=======================================================

*By Jaime Rodríguez-Guerra*

## 1. Introduction
- Molecular design and docking: finding the missing middle ground
- Current docking approaches have their limitations
    - Metal calculations are only available if the are located in the protein; ie, they can't be part of the ligand. This is an intrinsic limitation of the usual energetic approach, which is based on force fields optimized for proteins and small organic compounds
    - Covalent docking is a rare feature, generally lowly parametrized
- Current molecular design strategies and software


## 2. The GAUDI suite
- The concept behind GAUDI
    - How we coined such an awesome name
    - Based on a genetic algorithm based that features multi-objective optimization: NSGA-II
    - The force-field-less approach avoids metal limitations: HYDE derived functions
        - ... but we can't ignore the fact that force fields are necessary in some cases -> custom frcmods [lur]
    - A modular design that allows customized objectives
    - Explosively explored geometrical space: conformational, biological and chemical strategies
- Decisions made during the implementation
    - The philosophy
    - Why Chimera
    - Why Deap
    - Single objective versus multiple objective optimization
        - A single solution and the Pareto Front concept: dominance 
        - Solving multi-objective optimization problems: the importance of the human input, especially when dealing with variables whose importance is not known before hand 
    - Operators, genes and objectives
        - Crossover and mutation implementations in each objective
        - Formal definition of the chosen genes
- Programmatic details
    - Input files: YAML syntax
    - The loader: base.py (via Chimera)
    - GaudiView: resolving the Pareto Front with visual feedback

## 3. Case study I: Hemocyanin
- Background
    - Bridging organometallic chemistry and biomolecules
        - Enzymes and organometallics
        - Metallodrugs, artificial enzymes...
    - Streptavidin technology
        - Based on Victor Ph.D: different approaches for artificial metalloenzymes
- Solving the problem
    - The objective and the challenges
    - Strategy
        - Build linkers: custom script using SMILES web-service
        - Minimize organometallic group -> frcmod and stuff
        - Autobuild ligands so that torsions, H bonds, clashes, contacts and distances objectives are satisfied


## 4. Case study II: Aluminium and b-amyloid
- The concept
    - Metals play a vital role in several fields of biology
    - Ethiological implications in Alzheimer, Parkinson and other neurological diseases
- Bioinformatics state of the art *maybe this would fit better in the introduction?*
- Solving the problem
    - Input data: minimized alpha-C from GRID (that approach doesn't consider rotamers)
    - Strategy:
        - Find the aluminium center
        - Select residues that contain oxygen within n angstroms 
        - Apply rotamers and shift aluminium position
        - Optimize the distances and angles (oxygens-aluminium) and minimize any possible clashes
        - Report distances, abs(sin(dihedrals)), number of oxygens within coordination threshold
        - Launch GaudiView and analyze results
    - Proposed solutions