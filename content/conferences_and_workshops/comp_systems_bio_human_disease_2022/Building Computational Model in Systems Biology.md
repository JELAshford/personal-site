---
title: Building Computational Model in Systems Biology
tags:
  - compsysbio_2022
speaker: Dr. Anna Niarakis
---

![Screenshot 2022-12-05 at 09.09.39](Screenshot%202022-12-05%20at%2009.09.39.png)
Organisms as complex systems figure - Ritchie et. al. 10.1038/nrg3868

Systems biology: 
- How do the individual parts interact to yield system behaviour? 
- Biology has focussed on figuring out the pieces
**But what happens when you fit them together?** 
https://stremble.com/systems-biology


What is a model? 
- An **abstraction** of the real thing
- Helps us study and understand the **system** of interest and the **mechanisms** under which it operates

Networks
- Operate to organise information, at multiple scales
- Connects interacting concepts and can reveal key factors

The key goal of systems biology is:
- "Full Scale Integration": construct networks at different cellular levels to investigate cellular machinery
- BUT there is not satisfactory method to construct an integrated cellular network. 
- Interpret large scale data sets and extract true information to understand biological systems. 

>**The Systems Biology Paradigm**
>*Palsson BO, Systems Biology, Cambridge University Press 2006*
>"Components -> Networks -> Computational Models -> Phenotypes"
>e.g.
>Databases -> Knowledge Base -> *in silico* modelling -> Validation, Discovery, and Use


Organising biological information in the context of networks is a powerful concept: networks can be seen as graphs. 
- (https://doi.org/10.1016/j.cell.2012.05.044)
- Digital Twin: Virtual equivalents of physical objects.

DTs require advances in three critical areas; 
1. Develop sufficiently powerful and detailed simulation models of human immune system
2. Acquire then appropriate real-world data to train and validate the simulation models
3. Personalise and customise the simulation models to individual patients and for particular purposes.

Human body has 37.2 trillion cells across ~200 different cell types: we NEED abstraction. 

Intra-cellular networks: 
- Transciprtional regulation networks
- Protein structure networks
- Metabolic networks
- Protein-protein interaction (PPI) networks
- Cell signalling networks

Constructing a netowrk: 
1. Bottom Up (from scratch)
	- Literature based
	- Curation
	- Databases
	- Preeviosu Knowledge
	- From local to global
2. Top Down (data driven)
	- Data dependent
	- Algorithms
	- Inference 
	- Reverse engineering

----
**---TOP DOWN NETWORK CONSTRUCTION---**

- Network inference: "process of revealing the network structure of a biological system by reasoning backwards from data" He et. al.
- Why? Most/All functions in cells are performed through interactions which are hard to measure - but adbundances are easier to measure
- (https://doi.org/10.1016/j.artmed.2018.10.006)
![Screenshot 2022-12-05 at 09.29.57](Screenshot%202022-12-05%20at%2009.29.57.png)


Choosing an appraoch: 
1. Simplicity/Interpretability
2. Scalability
3. Assumptions
4. Sensitivity vs. Specificity
5. Ability to model feedback loops and combinatorial regulations
6. Availability of data
7. Pitfalls of different network types
8. Integrating prior knowledge improves results

Experimental design / Input Data considerations: 
1. Network ingerence is a largely undetermined proble (i.e. has multiple solutions)
2. To help deconvolute uncertainties, the best network build in known biological features


---
**---Bottom Up Network Construction---**
Make sure your sources are up to date, be careful with KEGG
- Causal Biological Network Database
- IMEx Consortium: International Molecular Exchange Consortium



[Systems biology graphical notation](https://sbgn.github.io/) (SBGN): describing mechanisms in a sytematic fashion
- Process decsriptions ([Prof. Hiroaki Kitano](https://scholar.google.com/citations?user=027fc-oAAAAJ))
- Activity flows
- Entity relationships

Process Descriptinos: Cancer Signalling Atlas, [Disease Maps project](http://disease-maps.org), REACTOME. See slides for links.

Molecular Interaction Maps
- Represents biological processes that ar eboth human and macine readable
- High quality source of knowledge - template for data visualization
- Can be analysed with topological methods

MIRIAM: Minimal Informatino Required In the Annotation of Models
MINERVA: Molecular Interactino NEtwoRk VisuAlization

Newt Pathway Viewer and Editor


-----
Adding a dynamical layer:
1. Why build models? *Biotechnol Prog 14:8-20 (1998)*
	1. Organize disparate information
	2. To relate components and interactions logically 
	3. Discover newe strategies
	4. Make important corrections to the conventinoal wisdom
2. Computational Models
	1. Use to represent actual quntitative/qulitative systems
	2. Have na inherent execution scheme atached to the model 
3. Central Dogma of COmputatinoal Systems Biology
	Components -> Networks -> Computational Models -> Phenotypes
4. Model Building
	1. Generate model based on literature
	2. Compile a calibratino dataset
	3. Mdoel fit to the experimental data
	4. *In silico* simulation to make predictions
	5. Iterate

 > "In theory, there is no difference between theory and practice. But in practice, there is" - Benjamin Brewster

 >
 [![File:Molecular simulation process.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/9/9b/Molecular_simulation_process.svg/733px-Molecular_simulation_process.svg.png?20220515052028)](https://upload.wikimedia.org/wikipedia/commons/9/9b/Molecular_simulation_process.svg)


Time granulaity in various modelling approaches:
 ![Screenshot 2022-12-05 at 10.10.06](Screenshot%202022-12-05%20at%2010.10.06.png)


SBML: Inter-operable format for reaction/process models (https://doi.org/10.1515/jib-2017-0081).