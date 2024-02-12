---
title: Model Checking in the BMA
tags:
  - compsysbio_2022
speaker: Ben Hall
---

Biological systems are frequently characterised by robustness, however we also experience and work with rare events

As systems become more complex, the space for rare events increases

Model checking addresses all of state space, allowing us to guarentee that some things can happen (rare events) and something never happen (robustness).


Model Checking in BMA: 
- Stability
	- Homeostasis or equilibrium
	- Bifurcations, oscillations
- Linear Temporal Logic
	- "Simulation Search"


Qualitative network (QN) fundamentals
- Each variable has a granularity and a target function; 
- Stability Proof:
	- "Does the model have a single fi point attractor" or multiple fix points or a cycle
	- Common to compute entire state transition graph, but BMA computes stability of individual nodes. 
- Define a variable (mkConstInt and how it updates (e.g. mkImplies(mkEq(..., ...), mkEq(..., ...)))


Z3 = Theorem Prover (/Constraint Solver)
- Very fast
- Allows you to work with logic, not code


LTL allows you to search for simulations:
 - "Does this gene ever transiently become active? "
 - "if this mutation is present, do cells ever become senescent?"
 - Query and simulation length as inputs (bounded model checking)
 - Returns example simulations

Pathfinder for sequential gene activation?!