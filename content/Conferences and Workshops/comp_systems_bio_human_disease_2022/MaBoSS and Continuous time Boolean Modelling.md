---
title: MaBoSS and Continuous time Boolean Modelling
tags:
  - compsysbio_2022
speaker: Vincent Noël
---


Quantitative Modelling
- Values can be any quantity
- Continuous time
- Difficult to write
- Difficult to scale
Qualitative Modelling
- Fix set
- Step time
- Easier to scale

**Markov Boolean Stochastic Simulator**
- Boolean
- Stochastic
- Physical Time
- Handle different time-scale processes (transcription, phsophorylation,...)
- Fills the gap bewteen ODEs and Boolean Modelling

How does it work? 
- Simulate transition from a bolean network state to another using a markov chian
- Transition rate p(S->S') = {R_up(S) if S_i = 0, R_down(S), if S_i= 1}.
	- R_up is activation, R_down = inactivation
	- Kinetics-like which allows multi-scale timesclaes
- Transitions are stochastic and async; multiple trajectories are possibles and we take the means -> probabilities per state over time. 


Model defied with:
- BND file: 
	- Defines activation and inactivation raeets
	- Allows complex cases: multiple rates for multiple states
	- Possibility to use parameters defined in the simulation settings
	- Now allows interoperable formats: SBMLqual, BNet. 
- CFG file
	- Iniital state of nodes (fixed, stochastic)
	- Internal/Output variables
	- Parameters (for BND)
	- Settings (for model running; time, granularity, trajectories, ...)

>Because of fixed time run, final state is not steady state

Can model the influence of a microenvironment on the behavior of the cell - e.g.
- activating microenvironment on Cohen's transitions majority of cells to Invasive
- Can then activate damage and see most cells go to apoptosis (but not all!)
- If you force Notch activity and block p53 (Notch+++, p53--), get complete Invasive phenotype compared to heterogeneous WT. 


MaBoSS ecosystem: 
MaBosSS ->
- EnsembleMaBoss: ensembles of non-interacting cells; use Answer-Set-Programming with BoNesis
- UPMaBoSS: ensembels of interacting cells; add nodes for division and death, allow inter-cell communications, track population sizes. 
- PhysiBoSS: agent base models with PhysiCell; add models as the "brain" of each agent; augment models with I/O nodes. 