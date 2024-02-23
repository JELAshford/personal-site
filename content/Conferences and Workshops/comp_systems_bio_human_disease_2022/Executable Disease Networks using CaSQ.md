---
title: Executable Disease Networks using CaSQ
tags:
  - compsysbio_2022
speaker:
  - Sylvain Soliman
---
(https://lifeware.inria.fr/~soliman/)


Curate Literature -> Make Knowledge Map -> ...now what?!
Want to compare them with experiments!
>[CaSQ](https://gitlab.inria.fr/soliman/casq/) turns maps into models

SMBL format is a *reusable* and *comprehensive* Process Description language

No explicit time in Boolean models - only *logical* time dictated by the steps taken on other states. 

CaSQ can: 
- Handle even very large detailed mechanisitic maps
- Keep and use all infomration in the annotations
- Build a fully executable/dynamic logical model. 
CaSQ does not: 
- Extract a relevant submap
- only convert PD (process description) to AF (activity flow)
- build a numerical (ODE-based) dynamical model
- build *the best* logical model
- handle model **analysis**


The AF graphs can lose information: these two graphs would be the same (A->C, B->C), but CaSQ can differentiate them as AND or OR cases.
A \\                         A -[]-\
   --[]-- C                        C
B /                         B -[]-/


What does CaSQ do:
1. Map simplification
	Practically, the reduction is around 30-50%
	1. Receptor Delection: A+B -> AB. If A and B don't take part in any other reaction, A is marked as a **receptor** and reaction is recast as a *heterodimer association. B -> AB
	2. Complex Merge: (A+B)+C -> AB. If A and B don't take part in any other reaction, also recast to *heterodimer association*. C -> AB
	3. Inactive Species Removal: A -> A* (such as being phosphorylated). A only appears in this raction with a single product, both reactant and product have the same name. Just left with: A*.
	4. Transport Simplification: C1[B->A-]-C2[-A]. If A (reactant) does not take any other active part, reaction annotatd as transport between two compartments, same name in both compartments. C1[B-]-C2[->A]
2. Logical Model Computation
	>A target is on when **any** of the reactions producing it is on. A reaction is on if **all reactants are on**, all **inhibitors are off**, and one of **the catalysts is on if there are any**
	R POSITIVE t
	C POSITIVE t
	I NEGATIVE t
3. Model Simplification / Cleanup
	- If maps are not connected, keep only some CCs (connected components).
	- If names are ambiguous, rename species if necessary.
	- Generate auxiliary files if requested (SIF, CSV, BMA)


Conserving Information: 
- All vertices of the model correspond to one (species) vertex of the original map -> keeps the original layout for readability. 
- Annotation abou the nature of a species (e.g. receptor) are used in the transformation. 
- Bibliographic information (MIRIAM: Minimal Information Required in the Annotation of Models) is propogated when nodes are removed, and merged when nodes are kept. 

**Standards are crucial for interoperability**
- SBML IN -> SBML out
- Keep rich annotations


> "The Map is not the territory" - Alfred Korzybski

> "All models are wrong, but some are useful"  - George Box 