---
title: Introduction to Boolean Modelling
tags:
  - compsysbio_2022
speaker: Dr. Anna Niarakis
---

Discrete dynamical modelling allows us to describe qualitative/quantitative properties of the network **and** predict the behviours of the network under different conditions. 

Cell function involves multiple layers/levels in the cell - so we can't parameterise the model independently of it's local context. 
- It's (almost) impossible to associate a single cause with each consequence, or a single consequence with each cause. 
- Information flow is **not** unidirectional - "cellular actors only exist in interaction"

### Boolean Algebra
States = {0, 1}
Functions = {
	AND: "Conjunction", 
	OR: "Disjunction", 
	NOT:, "Negation", 
	...
}

----
Kauffman:
 - Idealised gene networks, interactions with K-random neighbours. 
 - Focussed on asymptotoic behaviour w/ deteminisitc and synchronous models. 
  - Studied attractors: stable states, or simple cycles.

René Thomas: 
- Detailsed logical description of mechanisms
- Asynchronous
-------


How does this work? 
- State(t) = [1, 0, 0, 1, ...] (a bool for each gene)
- Updates are govered by boolean functions (AND, OR, etc.)
- State(t+1) = bool_functions(State(t))
- Automata!

Updates:
- Can now use categorical (multilevel) variables and study feedback loops
- Loops classified by number of negative (inhibitory) interactions in the loops:
	- If even, the loop is "Positive"
	- else, "Negative"
	- 
Thomas found "Positive" feedback loops are a necessary condition for the existence of multiple steady states. Thus:
- **Cell Differentiation** is based on positive feedback loops
- **Homeostasis** is based on negative feedback loops

When to use logical modelling?
- There is no quantitative information
- The details of some interactions are unknown
- The biological question is qualitative. 

Discrete Models: Boolean Networks
- Widely used to model gene regulation
- Assumes Switch-like behaviour of gene regulation, resembles logic circuit behaviour
- Conceptually easy framework
- Extend naturally to dynamic modelling
- Binarisation reduces noise but loses information.
- Need to justify thresholding continuous data to boolean representation.
- Logical models are (technically) parameter free

A Boolean Network is a graph G = G(V, F)
Nodes V = {x1, ..., xn}, xi = {0, 1}
Functions F = {f1,..., fm}
Apply fn to xn

Sigmoid functions can be naturally binaries without great loss of information

## Example

G = G(V, F) = G(
	V = {x1, x2, x3}, 
	F = {
		x2 AND x3, 
		x3, 
		x2
	}
)
```python
from itertools import product

def step(s):
	return [s[1] and s[2], s[0], s[1]]
	
for state in product([1, 0], repeat=3):
	print(f"{s}     {step(s)}")
# (1, 1, 1)    [1, 1, 1]
# (1, 1, 0)    [0, 1, 1]
# (1, 0, 1)    [0, 1, 0]
# (1, 0, 0)    [0, 1, 0]
# (0, 1, 1)    [1, 0, 1]
# (0, 1, 0)    [0, 0, 1]
# (0, 0, 1)    [0, 0, 0]
# (0, 0, 0)    [0, 0, 0]
```
*It's a cellular automata without a "kernel" but rules over arbitrary distances...maybe that has it's own name. Finite Automata?*


Stable State: Terminal nodes in the state transition graph
Cyclic attractor: Terminal strongly connected components 
Transient Cycle(s): Non-terminal strongly connected components
Basins of Attraction: Subsets of transient states