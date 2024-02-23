---
title: CoLoMoTo - Accessible and Reproducible Analyses
tags:
  - compsysbio_2022
speaker: Pedro T. Monteiro
---

Structural -> Dynamical View
Componenets of the network need to be quantified
- Values change onver time (created / consumed) - must describe system evolution

#### Established Qualitative Frameworks for Gene Refulatory Networks: 
- Piecewise-linear differential equations (PLDEs)
- Logical formalisms (boolean networks)
- Petri nets
Restrictions:
- Lack of quantitative data
- Discrete time
Implicit assumptions: 
- Ignore intermediate products
- Ignore machinery


#### Regulatory Graph

(G, E)
G = {g1, g2, ..., gn} nodes, 
E = {e_12, e_23, ...} edges
± E_ij indicates allow/inhibit
K = {k1, k2, ..., kn} boolean functions for each G
State space S = all possible combinations of G
Inputs are components that don't have regulators (nothing comes into them)

**State Transition Graph (STG)** - represent all states S (nodes) and possible moves between them (arc). 
G_i(t+1) = K_g_i(g_1(t),...,g_n(t))

**Attractors** - correspond to biologically relevant asymptotic behaviours
- Stable state: all gene levels are maintained
- Complex attractor: long lasting oscillatory behaviour. 

Possible interesting properties
- What are the attractors of the system? Is there multi-stability? 
- Are these attractors reachable from initial conditions? 
- Are these attractors maintained under input variations? 
- What is the likelihood of reaching an attractor from a given portion of the state space? 


Challenges:
1. Number of possible states (S) explodes combinatorially: (num_components^num_possible_values)
2. Update rules:
	Level of "synchrony" is define in the context of the state - do we update all or one bit of the state at a time
	1. "Async" - update triggered only along a single bit change. As we don't know rates, have to randomly choose a possible future if multiple bits could update: non-deterministic
	2. "Sync" - next state is product of all component updates, multiple bits can change at the same time: deterministic.
3. Huge state has many dynamics, some can be restricted: strong connected component (SCC) graphs collapse all nodes that are linked (like attractors) into a singe node.
4. Explosion of number of transitions: 
	1. Modes like "Generalised Asynchronous" generate all possible transitions combinations between synchronous and async.
	2. "Buffer Networks" provide less competition between alternative transitions. 
	3. "Most Permissive" try to collapse generalised async options, but comes with uncertainty.
	4. Can step from Generalised to permissive by making fewer kinetic assumptions. Lack of kinetic knowledge makes many alternative trajectories possible. 

Methods: Identifying Attractors
- Stable States = Constrain Solving; find f(x) = x
- Complex Attractors = Symbolic Exploration; find C = set of x; where f(x) is always in C.
- Trap Spaces (subspaces of S where there's at least one attractor) = Constraint Solving; find hypercube h, where h = set of x, where f(x) in h

Methods: Stable State Identification WITHOUT explicit state exploration
- Express logical functions os OMDDs (ordered multivalued decision diagrams) - state 1 = take right branch, state 0 = take left branch
- Convert each OMDD as a stability graph, where you examine all possible outcomes given the state it depends on and evaluate it remains stable (outcome 1 if it is!)
- Substitute the stability graphs into each other, and then read out all states that lead to positive stability (outcome 1)

Methods: Reachability without generating all states
- Developed since the 70s, for hardware/software checking in boolean circuits. 
- Used in many areas of systems biology
- Temporal Logic: 
	- CRL (Computation tree logic) - Branching Time
	- LTL (Linear Temporal Logic) - Linear-time
	- CRTL (Computation Tree Regular Logic)
- Quantifying reachability: 
	- Stochastic simulations from {Avatar, MaBoSS} -> Probability of being in states
- Static reachability analysis from "Prime Implicant Graphs"

Gene Interaction network simulation ([GINsim](http://www.ginsim.org))
- Computer tools for modelling and simulation of genetic regulatory networks implementing the logical formalism.

[CoLoMoTo](http://colomoto.org): **Co**nsortium for **Lo**gical **Mo**del and **To**ols
- Install = Conda and Docker
- Analysis = Python/Jupyter
- Share = Jupyter and SBMLqual exchange formats