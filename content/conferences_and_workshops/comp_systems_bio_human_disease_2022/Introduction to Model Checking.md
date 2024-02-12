---
title: Introduction to Model Checking
tags:
  - compsysbio_2022
speaker: Ben Hall
---

#### Links between Systems Biology and Computer Science
Systems Biology: 
- Systemic POV: focussed on interation between biological entities to userstand properties of the whole system
- Formal Tools are needed to describe bio networks
Computer Science:
- Has techniques for modelling:
	- Concurrent software systmes, the composition of communicating processes: with langauges for describing them and analysis technqiues calibrated over these language formalisms.

CompSci                                 SysBio
Communicating Processes <-> Interacting Agents


Software Verification
- Verification: formal verification tecniques establish guarantees of software correctness
- Testing: Shows bugs, but cannot show their absense (E. Dijkstra)
- Model Checking: fully automated approach for software verification

---
#### Model Checking - Intuition and Main Idea

Model Checking:
- Allows for desired behaviour of a system to be verified on the basis of a suitable model of the sysmte
- Completely automatic and offers counterexamples in case a model fails to satisfy a property
- Tools and scalability have improved considerably over the last decade

Checks all states of system (phi) against properties (M). 
If M |= phi, model checker generates a counterexample and lets you replay the scenario

Possible outcome results: 
- Property is valid
- Property is violated
- Model is too large to handle (State-space explosion problem)

System Models = Accurate and unambiguous description of model behaviours
Transition System (TS) = (S, Act, T, I, AP, L)
- S = States
- Act = Actions
- T = Transition Relations (SxActxS)
- I = Initial States (in S)
- AP = set of atomic processes
- L = (2^AP) Labelling function
Within this we can express conditions, variables, and constraints.

Property Specification: Temporal Logic
- Extends propositional logic with temporal operations to refer to behaviour over time: 
	- Functional correctness
	- Reachability
	- Safety
	- Liveness 
	- Fairness (can, under certain conditions, an event occur repeatedly)
	- Real-time properties
- Propositional logic = properties of states
- Temporal logic = properties on transitions

Linear Temporal Logic (LTL)
- Time is linear and deterministic
- Atomic propositions are valid formulae
- If "p" and "q" are valid formulae, then so are: 
	1. Always P
	2. Eventually p
	3. Next p 
	4. p until q
- New formulae can be obtained by applying boolean operators on these (not, and, or implies, and equivalence)
- All properties in LTL refer to only one execution
- A formula (phi) holds in a sate s if all possible paths satisfy it. 
- There are other temporal operators, and can specify dynamic properties. 

