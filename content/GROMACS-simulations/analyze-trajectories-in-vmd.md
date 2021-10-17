---
title: "Analyze Trajectories in VMD"
date: 2021-10-17T13:45:18-04:00
draft: false
weight: 10
---

Useful information can be found on the following websites:

[Analyze your trajectory](https://smog-server.org/SB_analysis.html)  


- Start `VMD`, open the `Tk console` and load the `.gro` file of the trajectory to analyze.

```zsh
cd /Users/davidoppenheimer/Dropbox\ \(UFL\)/GROMACS/gromacs-prot-ligand-tutorial
mol new md_0_10.gro
```

- Then load the trajectory file into the previously loaded molecule.
    - In the `VMD Main` window, select the molecule.
    - Go to *File* → *Load Data into Molecule* and choose the `.xtc` trajectory to load from the file browser. Or use the `Tk console`.

```zsh
mol addfile {/Users/davidoppenheimer/Dropbox (UFL)/GROMACS/gromacs-prot-ligand-tutorial/md_0_10_fit.xtc} type {xtc} first 0 last -1 step 1 waitfor 1 0
```

- Create a representation of the water molecule, and then hide it.

```zsh
mol addrep 0 # add representation to molecule 0; this is rep 1
# modstyle rep_number molecule_number rep_style
mol modstyle 1 0 lines # rep 1, molecule 0
# modselect rep_number molecule_number select_method: Change the current selection for the given representation in the specified molecule.
mol modselect 1 0 water
# showrep molecule_number rep_number [on | off]
mol showrep 0 1 off
mol addrep 0
mol modselect 2 0 protein
mol modselect 3 0 resname JZ4
mol modstyle 3 0 vdw
mol showrep 0 0 off # turn off the "all" representation
```

Note done yet! More to come.



