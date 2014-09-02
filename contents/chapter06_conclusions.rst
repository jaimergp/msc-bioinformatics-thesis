.. role:: cite

.. role:: citein

.. raw:: latex

    \providecommand*\DUrolecite[1]{\citep{#1}}
    \providecommand*\DUrolecitein[1]{\citet{#1}}

===========
Conclusions
===========

The results of my Masters in Science has been named after the famous Catalonian architect, Antoni Gaudí. **GAUDI\ :sub:`ASM`** stands for **G**\ enetic **A**\ lgorithms for **U**\ niversal **D**\ esign **I**\ nference and **A**\ tomic **S**\ cale **M**\ odelling. As a result, I can proudly present a novel platform that aims to satisfy increasingly demanding areas of molecular design. It will do so by providing the researchers with a powerful multi-objective optimization engine to explore the huge search space that chemobiological design problems usually present.

GAUDI\ :sub:`ASM` has proved to be a valid proof of concept. Simplistic energy calculation terms do generate good results if combined with the appropriate restraints, which is one of the most powerful strengths of this software. It will help both experimentalists and theoreticians with a rapid sketching tool that will produce feasible in results in a reasonable amount of time. After all, it has already shed light on two experiments where no existent tools could hardly help with.

Further work
============
After only six months of work, GAUDI\ :sub:`ASM` cannot be considered a finished product. We are already making big steps towards a stable 1.0 version, which will include some large improvements. This is a non-exhaustive list of such milestones.

- Further development of the spatial exploration engine: improve free docking results and benchmarking, global protein flexibility
- More objectives, like a transition metal coordination geometry predictive engine
- QM/MM minimization interface, implementing easy refinement tools directly into GaudiView as the starting point of an vertically-integrative platform
- Custom \*.frcmod inputs as energy terms
- Performance improvements: thread parallelization, code polish