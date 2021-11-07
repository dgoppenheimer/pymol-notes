---
title: "MD Notes"
date: 2021-10-29T11:36:52-04:00
draft: false
lastmodifierdisplayname: DGO
---

## Common software

### GROMACS

### CHARMM

[CHARMM Tutorials](https://www.charmm.org/archive/charmm/documentation/tutorials/)  

### AMBER

### NAMD

[ABINIT](https://www.abinit.org/) for free energy calculations  
[DL_POLY](https://www.scd.stfc.ac.uk/Pages/DL_POLY.aspx)  

[Making it rain: Cloud-based molecular simulations for everyone](https://pablo-arantes.github.io/making-it-rain/)  


## Force fields

### Force fields for small molecules

There is a recent paper that covers this topic by Lin and MacKerell [^1], [Force fields for small molecules](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6733265/).

OPLS-All-Atom (OPLS-AA)  
OPLS3  
CHARMM General force field (CGenFF) see [ParamChem](https://cgenff.umaryland.edu/)  
General AMBER Force Field (GAFF)  
Merck Molecular Force Field (MMFF)  
GROMOS  
Drude Polarizable Force Field for Small Organic Molecules  
Drude General Force Field (DGenFF)  

>Parameters for drug-like small molecules have not been reported with the AMBER polarizable force field or CHARMM FQ models.

### QM calculations

To validate the parameters:

Force Field Toolkit (ffTK)  
General Automatic Atomic Model Parametrization (GAAMP) utility  

Also see the [MacKerrell lab website](http://mackerell.umaryland.edu/)  

[kaggle notebook on kinase inhibitors](https://www.kaggle.com/xiaotawkaggle/inhibitors)  


[Tutorial - Quantum Chemistry with Gaussian using GaussView](https://answers.uillinois.edu/scs/page.php?id=103608)

[Molecular Dynamics Method 2](https://www.ks.uiuc.edu/Training/SumSchool/materials/lectures/6-2-Intro-Protein-Structure-Dynamics/4-SS03_MDMethods.pdf)


Explore using treeClust (machine learning) to uncover associations between residues that have high RMSD => allostery








[^1]: Lin, F.Y., and MacKerell, A.D. (2019). Force Fields for Small Molecules. Methods Mol. Biol. 2022: 21–54. doi: [10.1007/978-1-4939-9608-7_2](https://dx.doi.org/10.1007%2F978-1-4939-9608-7_2)
