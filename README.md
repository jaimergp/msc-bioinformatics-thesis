GAUDI Table of Contents
=======================

*By Jaime Rodríguez-Guerra*

## 1. Introduction

- Current docking approaches have their limitations
    - Metals are only available if the are located protein, but can't be part of the ligand. This is an intrinsic limitation of the usual energetic approach, which is based on force fields optimized for proteins and small organic compounds
    - Covalent docking is a rare feature, generally lowly parametrized
- Single objective versus multiple objective optimization
    - A single solution and the Pareto Front concept: dominance 
    - Solving multi-objective optimization problems

## 2. The GAUDI suite
- The concept behind GAUDI
    - How we coined such an awesome name
    - Based on a genetic algorithm based that features multi-objective optimization: NSGA-II
    - The force-field-less approach avoids metal limitations: HYDE derived functions
    - A modular design that allows customized objectives
    - Explosively explored geometrical space: conformational, biological and chemical strategies
- Decisions made during the implementation
    - The philosophy
    - Why Chimera
    - Why Deap 
    - Operators, genes and objectives
- Programmatic details
    - Input files: YAML syntax
    - The loader: base.py
    - GaudiView

## 3. Case study: Aluminium and b-amyloid
- The concept
    - Metals play a vital role in several fields of biology
    - Ethiological implications in Alzheimer and other diseases
- Bioinformatics state of the art *maybe this would fit better in the introduction?*
- Solving the problem
    - Input data: minimized alpha-C, but without considering rotamers
    - Strategy:
        - Select residues that contain oxygen
        - Apply rotamers and shift aluminium position
        - Optimize the distances, angles and clashes
    - Possible solutions

## 4. Hemocyanin
- Background
    - Bridging organometallic chemistry and biomolecules
        - Metalodrugs, artificial enzymes...
    - Streptavidin technology
        - Based on Victor Ph.D: different approaches for artificial metalloenzymes
- Solving the problem
    - The objective and the challenges
    - Strategy
        - Build linkers: custom script
        - Minimize organometallic group -> frcmod and stuff
        - Autobuild ligands so that torsions, H bonds, clashes, contacts and distances objectives are satisfied
