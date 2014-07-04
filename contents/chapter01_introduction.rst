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

Computer-assisted molecular design is an increasingly demanding area where scientists are lacking specific software implementations. This is especially true in hybrid disciplines, such as artificial chemobiological systems, where already existent solutions cannot face the challenges that these systems present because the search space is simply unfathomable. In this kind of situations, an accurate representation of the problem cannot be afforded, so an alternative strategy comes in handy.

.. figure:: fig/funnel.ps 
	:align: center

	#! TODO - *Methodological funnel. GAUDI is sitting on the top of the funnel, because it has to deal with a HUGE HUGE search space that comprises more dimensions than a simple docking essay (in which 90% of the leading forces have to do with non-covalent interactions), so some precision must be sacrificed in exchange of speed.*
	.. !# gnuplot code
	.. j(u,v) = a*u*cos(v)
	.. k(u,v) = a*u*sin(v)
	.. l(u,v) = log(u)
	.. set parametric
	.. set term postscript
	.. set output "output.ps"
	.. unset key, border, xtics, ytics, ztics
	.. splot [0.15:1] j(u,v),k(u,v),l(u,v) 


2. Current molecular design strategies
======================================

|

2.1. Available software and strategies in use
---------------------------------------------

|

3. Current docking approaches have their limitations
====================================================

|

3.1. Calculations involving metals
----------------------------------

- This a result of force-field based approaches.


3.2. Covalent docking is not very developed
-------------------------------------------

|