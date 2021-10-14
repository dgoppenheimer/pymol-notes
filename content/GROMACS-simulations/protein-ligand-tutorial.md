---
title: "Protein-Ligand Complex Tutorial"
date: 2021-10-14T15:53:20-04:00
draft: false
---

This tutorial is from [this website](http://www.mdtutorials.com/gmx/complex/index.html).

- Download [3HTB](https://www.rcsb.org/structure/3HTB) from the [RCSB website](https://www.rcsb.org/).

- View the structure in VMD.

```zsh
cd ../gromacs-work/gromacs-prot-ligand-tutorial
mol new 3htb.pdb
mol modstyle 0 0 licorice
```

A few of the amino acids have 2 conformers that are present in the structure.

- Strip out the crystal waters, PO<sub>4</sub>, and BME.

```zsh
awk '!(/HETATM/ && /HOH/ || /HETATM/ && /BME/ || /HETATM/ && /PO4/)' 3htb.pdb > 3htb-clean.pdb
# && is AND, and  || is OR
# ! shows the lines that do not have the pattern
```

The tutorial-supplied file is `3htb_clean.pdb`. For fun, run:

```zsh
git diff --no-index --word-diff 3htb-clean.pdb 3htb_clean.pdb
```

Because I used the more specific `awk` command instead of multiple `grep` commands, I have a few lines in the `3htb-clean.pdb` file that are missing from the `3htb_clean.pdb` file. Also the `3htb_clean.pdb` file is missing all the `CONECT` entries. I'll remove them from `3htb-clean.pdb`:

```zsh
awk '!/CONECT/' 3htb-clean.pdb > 3htb-tmp.pdb
mv -i 3htb-tmp.pdb 3htb-clean.pdb
```

Nice!

- Put the `JZ4` coordinates into its own file.

```zsh
grep JZ4 3HTB-clean.pdb > jz4.pdb
awk '!(/HETATM/ && /JZ4/)' 3HTB-clean.pdb > 3htb-tmp.pdb
mv -i 3htb-tmp.pdb 3htb-clean.pdb
```

- Check which version of `python` you are using with `python --version`. 
- Download the `CHARMM36` force field file from the [MacKerell lab website](http://mackerell.umaryland.edu/charmm_ff.shtml#gromacs).
- Download the `cgenff_charmm2gmx.py` script from the same website.
- Unarchive the force field tarball:

```zsh
tar -zxvf charmm36-feb2021.ff.tgz
rm charmm36-feb2021.ff.tgz
```

- Write the topology for the T4 lysozyme with `pdb2gmx`:

```zsh
gmx pdb2gmx -f 3htb-clean.pdb -o 3htb_processed.gro
# choose the CHARMM36 force field (option 1)
# choose the default water model (TIP 3-point, recommended, by default uses CHARMM TIP3 with LJ on H)
```

## Step Two: Prepare the Ligand Topology

### Add Hydrogen Atoms to JZ4

- Add hydrogens using Avogadro.
- Save the file as a `.mol2` file.
- Open the file in a text editor. Under the `@<TRIPOS>MOLECULE` heading, change `*****` to `JZ4`.
- Fix the residue names and numbers such that they are all the same. Change:

```mol2
@<TRIPOS>ATOM
      1 C4         24.2940  -24.1240   -0.0710 C.3   167  JZ4167     -0.0650
      2 C7         21.5530  -27.2140   -4.1120 C.ar  167  JZ4167     -0.0613
      3 C8         22.0680  -26.7470   -5.3310 C.ar  167  JZ4167     -0.0583
      4 C9         22.6710  -25.5120   -5.4480 C.ar  167  JZ4167     -0.0199
      5 C10        22.7690  -24.7300   -4.2950 C.ar  167  JZ4167      0.1200
      6 C11        21.6930  -26.4590   -2.9540 C.ar  167  JZ4167     -0.0551
      7 C12        22.2940  -25.1870   -3.0750 C.ar  167  JZ4167     -0.0060
      8 C13        22.4630  -24.4140   -1.8080 C.3   167  JZ4167     -0.0245
      9 C14        23.9250  -24.7040   -1.3940 C.3   167  JZ4167     -0.0518
     10 OAB        23.4120  -23.5360   -4.3420 O.3   167  JZ4167     -0.5065
     11 H          25.3133  -24.3619    0.1509 H       1  UNL1        0.0230
     12 H          23.6591  -24.5327    0.6872 H       1  UNL1        0.0230
     13 H          24.1744  -23.0611   -0.1016 H       1  UNL1        0.0230
     14 H          21.0673  -28.1238   -4.0754 H       1  UNL1        0.0618
     15 H          21.9931  -27.3472   -6.1672 H       1  UNL1        0.0619
     16 H          23.0361  -25.1783   -6.3537 H       1  UNL1        0.0654
     17 H          21.3701  -26.8143   -2.0405 H       1  UNL1        0.0621
     18 H          21.7794  -24.7551   -1.0588 H       1  UNL1        0.0314
     19 H          22.2659  -23.3694   -1.9301 H       1  UNL1        0.0314
     20 H          24.5755  -24.2929   -2.1375 H       1  UNL1        0.0266
     21 H          24.0241  -25.7662   -1.3110 H       1  UNL1        0.0266
     22 H          23.7394  -23.2120   -5.1580 H       1  UNL1        0.2921
```

to

```mol2
@<TRIPOS>ATOM
      1 C4         24.2940  -24.1240   -0.0710 C.3     1  JZ4        -0.0650
      2 C7         21.5530  -27.2140   -4.1120 C.ar    1  JZ4        -0.0613
      3 C8         22.0680  -26.7470   -5.3310 C.ar    1  JZ4        -0.0583
      4 C9         22.6710  -25.5120   -5.4480 C.ar    1  JZ4        -0.0199
      5 C10        22.7690  -24.7300   -4.2950 C.ar    1  JZ4         0.1200
      6 C11        21.6930  -26.4590   -2.9540 C.ar    1  JZ4        -0.0551
      7 C12        22.2940  -25.1870   -3.0750 C.ar    1  JZ4        -0.0060
      8 C13        22.4630  -24.4140   -1.8080 C.3     1  JZ4        -0.0245
      9 C14        23.9250  -24.7040   -1.3940 C.3     1  JZ4        -0.0518
     10 OAB        23.4120  -23.5360   -4.3420 O.3     1  JZ4        -0.5065
     11 H          25.3133  -24.3619    0.1509 H       1  JZ4         0.0230
     12 H          23.6591  -24.5327    0.6872 H       1  JZ4         0.0230
     13 H          24.1744  -23.0611   -0.1016 H       1  JZ4         0.0230
     14 H          21.0673  -28.1238   -4.0754 H       1  JZ4         0.0618
     15 H          21.9931  -27.3472   -6.1672 H       1  JZ4         0.0619
     16 H          23.0361  -25.1783   -6.3537 H       1  JZ4         0.0654
     17 H          21.3701  -26.8143   -2.0405 H       1  JZ4         0.0621
     18 H          21.7794  -24.7551   -1.0588 H       1  JZ4         0.0314
     19 H          22.2659  -23.3694   -1.9301 H       1  JZ4         0.0314
     20 H          24.5755  -24.2929   -2.1375 H       1  JZ4         0.0266
     21 H          24.0241  -25.7662   -1.3110 H       1  JZ4         0.0266
     22 H          23.7394  -23.2120   -5.1580 H       1  JZ4         0.2921
```

- Fix the bond order using the `sort_mol2_bonds.pl` script.

```zsh
perl sort_mol2_bonds.pl jz4.mol2 jz4_fix.mol2
```

- Use the `jz4_fix.mol2` file for the next step.

### Generate the JZ4 Topology with CGenFF

- Log in to your account on the [CGenFF server](https://cgenff.umaryland.edu/).
- Click the `Choose File` button and choose the `jz4_fix.mol2` file. Then click the `Upload File` button.
- Compare the file with the one from the tutorial:

```zsh
git diff --no-index --word-diff jz4_fix.str jz4.str
```

The tutorial file has atoms labeled `C1, C2, etc.` whereas the file I generated has atoms labeled `C4, C5, etc.` but none labeled `C1 etc.`. This is due to how `CGenFF` processed the file, because the `jz4_fix.mol2` file I generated was the same as the one downloaded from the tutorial site.

All of my penalty scores were less than 10, so the file should be good to go.

- Use the `cgenff_charmm2gmx_py3_nx2.py` script to convert the topology from `CHARMM` to `GROMACS` format.

```zsh
python cgenff_charmm2gmx_py3_nx2.py JZ4 jz4_fix.mol2 jz4_fix.str charmm36-feb2021.ff

# I got this error:
#   File "cgenff_charmm2gmx_py3_nx2.py", line 53, in <module>
#    import numpy as np
# ModuleNotFoundError: No module named 'numpy'
```

Try to install `numpy`.

```zsh
python3 -m pip install numpy
```

Looks like it installed okay. Try script again:

```zsh
python cgenff_charmm2gmx_py3_nx2.py JZ4 jz4_fix.mol2 jz4_fix.str charmm36-feb2021.ff

# got this error:
# ModuleNotFoundError: No module named 'networkx'

# try to fix:
python3 -m pip install networkx

# try again:
python cgenff_charmm2gmx_py3_nx2.py JZ4 jz4_fix.mol2 jz4_fix.str charmm36-feb2021.ff

# Got this error:
#   File "cgenff_charmm2gmx_py3_nx2.py", line 991, in <module>
#     if(float(nx.__version__) < 2.0):
# ValueError: could not convert string to float: '2.6.3'

# try to use the other script:

python cgenff_charmm2gmx_py3_nx1.py JZ4 jz4_fix.mol2 jz4_fix.str charmm36-feb2021.ff
```

- Try the following:

```zsh
python3 -m pip uninstall networkx
python3 -m pip install networkx==2.3
python cgenff_charmm2gmx_py3_nx2.py JZ4 jz4_fix.mol2 jz4_fix.str charmm36-feb2021.ff
```

I got the error:

```zsh
NOTE 3: Please be sure to use the same version of CGenFF in your simulations that was used during parameter generation:
--Version of CGenFF detected in  jz4_fix.str : 4.5
--Version of CGenFF detected in  charmm36-feb2021.ff/forcefield.doc : 4.4

WARNING: CGenFF versions are not equivalent!
```

From the web:

>Don't worry about this difference. The web server needs to be updated, but not in any way that affects the end user.

&#8212;Justin A. Lemkul, Ph.D.

### Build the Complex

- Convert `jz4_ini.pdb` to `.gro` format with `editconf`:

```zsh
gmx editconf -f jz4_ini.pdb -o jz4.gro
```

- Copy `3htb_processed.gro` to a new file to be manipulated, for instance, call it `complex.gro`.

```zsh
cp 3htb_processed.gro complex.gro
```

- Copy the coordinate section of `jz4.gro` and paste it into `complex.gro`, below the last line of the protein atoms, and before the box vectors, like so:

```zsh
  163ASN   HD22 2611   0.944  -0.584  -0.525
  163ASN      C 2612   0.621  -0.740  -0.126
  163ASN    OT1 2613   0.624  -0.616  -0.140
  163ASN    OT2 2614   0.683  -0.703  -0.011
    1JZ4     C4    1   2.429  -2.412  -0.007
    1JZ4     C7    2   2.155  -2.721  -0.411
    1JZ4     C8    3   2.207  -2.675  -0.533
    1JZ4     C9    4   2.267  -2.551  -0.545
    1JZ4    C10    5   2.277  -2.473  -0.430
    1JZ4    C11    6   2.169  -2.646  -0.295
    1JZ4    C12    7   2.229  -2.519  -0.308
    1JZ4    C13    8   2.246  -2.441  -0.181
    1JZ4    C14    9   2.392  -2.470  -0.139
    1JZ4    OAB   10   2.341  -2.354  -0.434
    1JZ4     H1   11   2.531  -2.436   0.015
    1JZ4     H2   12   2.366  -2.453   0.069
    1JZ4     H3   13   2.417  -2.306  -0.010
    1JZ4     H4   14   2.107  -2.812  -0.407
    1JZ4     H5   15   2.199  -2.735  -0.617
    1JZ4     H6   16   2.304  -2.518  -0.635
    1JZ4     H7   17   2.137  -2.681  -0.204
    1JZ4     H8   18   2.178  -2.476  -0.106
    1JZ4     H9   19   2.227  -2.337  -0.193
    1JZ4    H10   20   2.458  -2.429  -0.214
    1JZ4    H11   21   2.402  -2.577  -0.131
    1JZ4    H12   22   2.374  -2.321  -0.516
   5.99500   5.19182   9.66100   0.00000   0.00000  -2.99750   0.00000   0.00000   0.00000
```

- **IMPORTANT!** Since we have added 22 more atoms into the `.gro` file, increment the second line of `complex.gro` to reflect this change. There should be 2636 atoms in the coordinate file now.

### Build the Topology

- Insert a line that says `#include "jz4.itp"` into `topol.top` after the position restraint file is included, like so:

```zsh
; Include Position restraint file
#ifdef POSRES
#include "posre.itp"
#endif

; Include water topology
#include "./charmm36-feb2021.ff/tip3p.itp"
```

Becomes ...

```zsh
; Include Position restraint file
#ifdef POSRES
#include "posre.itp"
#endif

; Include ligand topology
#include "jz4.itp"

; Include water topology
#include "./charmm36-feb2021.ff/tip3p.itp"
```

- Insert an `#include` statement to add the `jz4.prm` parameters at the **TOP** of `topol.top`, like so:

```zsh
; Include forcefield parameters
#include "./charmm36-feb2021.ff/forcefield.itp"

[ moleculetype ]
; Name            nrexcl
Protein_chain_A     3
```

Becomes

```zsh
; Include forcefield parameters
#include "./charmm36-feb2021.ff/forcefield.itp"

; Include ligand parameters
#include "jz4.prm"

[ moleculetype ]
; Name            nrexcl
Protein_chain_A     3
```

It is important to place this statement **before** any `[ moleculetype ]` entry. It must also appear **AFTER** the #`include` statement for the parent force field.

- Add the JZ4 molecule to `[ molecules ]` directive, like so:

```zsh
[ molecules ]
; Compound        #mols
Protein_chain_A     1
JZ4                 1
```

## Step Three: Defining the Unit Cell & Adding Solvent

- Define the unit cell and fill it with water.

```zsh
gmx editconf -f complex.gro -o newbox.gro -bt dodecahedron -d 1.0
gmx solvate -cp newbox.gro -cs spc216.gro -p topol.top -o solv.gro
```

## Step Four: Adding Ions

- Use `grompp` to assemble a `.tpr` file, using any `.mdp` file. I downloaded the `ions.mdp` file from [this website](http://www.mdtutorials.com/gmx/complex/Files/ions.mdp).

```zsh
gmx grompp -f ions.mdp -c solv.gro -p topol.top -o ions.tpr
```

- Pass the `.tpr` file to `genion`:

```zsh
gmx genion -s ions.tpr -o solv_ions.gro -p topol.top -pname NA -nname CL -neutral
# when prompted, choose group 15:
# Group    15 (            SOL) has 30882 elements
```

The names of the ions specified with `-pname` and `-nname` are always the elemental symbol in all capital letters. Refer to `ions.itp` for proper nomenclature if you encounter difficulties.

- Open the `topol.top` file. Your `[ molecules ]` directive should now look like:

```zsh
[ molecules ]
; Compound        #mols
Protein_chain_A     1
JZ4                 1
SOL         10288
CL               6
```

## Step Five: Energy Minimization

- Create the binary input using `grompp` using the `em.mdp` input parameter file from [this website](http://www.mdtutorials.com/gmx/complex/Files/em.mdp):

```zsh
gmx grompp -f em.mdp -c solv_ions.gro -p topol.top -o em.tpr
```

- Invoke `mdrun` to carry out the energy minimization (EM):

```zsh
gmx mdrun -v -deffnm em
```

For the tutorial author, the system converged relatively quickly:

```zsh
Steepest Descents converged to Fmax < 1000 in 143 steps
Potential Energy  = -4.9014547e+05
Maximum force     =  8.7411469e+02 on atom 27
Norm of force     =  5.6676244e+01
```

My system also converged relatively quickly:

```zsh
Steepest Descents converged to Fmax < 1000 in 156 steps
Potential Energy  = -4.9085631e+05
Maximum force     =  8.8324829e+02 on atom 2045
Norm of force     =  5.6558131e+01
```

## Step Six: Equilibration

### Restraining the Ligand

- Generate a position restraint topology for the ligand. First, create an index group for JZ4 that contains only its non-hydrogen atoms:

```zsh
gmx make_ndx -f jz4.gro -o index_jz4.ndx
...
 > 0 & ! a H*
 > q
```

- Then, execute the `genrestr` module and select this newly created index group (which will be group 3 in the `index_jz4.ndx` file):

```zsh
gmx genrestr -f jz4.gro -n index_jz4.ndx -o posre_jz4.itp -fc 1000 1000 1000
```

- Include this information in our topology. Here we want to restrain the ligand whenever the protein is also restrained. Add the following lines to your topology file (`topol.top`) in the location indicated (**Location matters!**):

```zsh
; Include Position restraint file
#ifdef POSRES
#include "posre.itp"
#endif

; Include ligand topology
#include "jz4.itp"

; Ligand position restraints
#ifdef POSRES
#include "posre_jz4.itp"
#endif

; Include water topology
#include "./charmm36-feb2021.ff/tip3p.itp"
```

### Thermostats

Important: **Do not couple every single species in your system separately.**

Group JZ4 with the protein for the purposes of temperature coupling. In the same way, the few Cl- ions we inserted are considered part of the solvent. We need a special index group that merges the protein and JZ4. We accomplish this with the `make_ndx` module, supplying any coordinate file of the complete system. Here, we use `em.gro`, the output (minimized) structure of our system:

```zsh
gmx make_ndx -f em.gro -o index.ndx
…

  0 System              : 33506 atoms
  1 Protein             :  2614 atoms
  2 Protein-H           :  1301 atoms
  3 C-alpha             :   163 atoms
  4 Backbone            :   489 atoms
  5 MainChain           :   651 atoms
  6 MainChain+Cb        :   803 atoms
  7 MainChain+H         :   813 atoms
  8 SideChain           :  1801 atoms
  9 SideChain-H         :   650 atoms
 10 Prot-Masses         :  2614 atoms
 11 non-Protein         : 30892 atoms
 12 Other               :    22 atoms
 13 JZ4                 :    22 atoms
 14 CL                  :     6 atoms
 15 Water               : 30864 atoms
 16 SOL                 : 30864 atoms
 17 non-Water           :  2642 atoms
 18 Ion                 :     6 atoms
 19 JZ4                 :    22 atoms
 20 CL                  :     6 atoms
 21 Water_and_ions      : 30870 atoms
```

- Merge the "Protein" and "JZ4" groups with the following, where `>` indicates the `make_ndx` prompt:

```zsh
> 1 | 13

Copied index group 1 'Protein'
Copied index group 13 'JZ4'
Merged two groups with OR: 2614 22 -> 2636

 22 Protein_JZ4         :  2636 atoms

> q
```

We can now set `tc-grps = Protein_JZ4 Water_and_ions` to achieve our desired "Protein Non-Protein" effect.

- Proceed with NVT equilibration using the `nvt.mdp` file from [this website](http://www.mdtutorials.com/gmx/complex/Files/nvt.mdp).

```zsh
gmx grompp -f nvt.mdp -c em.gro -r em.gro -p topol.top -n index.ndx -o nvt.tpr
gmx mdrun -deffnm nvt
```

Laptop fan started running almost immediately. I used an ice pack to cool the computer.  
Started run at 2:02PM, finished at 2:12PM (634 s).

## Step Seven: Equilibration, Part 2

- Once the NVT simulation is complete, proceed to NPT using the `npt.mdp` file from [this website](http://www.mdtutorials.com/gmx/complex/Files/npt.mdp):

```zsh
gmx grompp -f npt.mdp -c nvt.gro -t nvt.cpt -r nvt.gro -p topol.top -n index.ndx -o npt.tpr
gmx mdrun -deffnm npt
```

Run started at 2:25PM, finished at 2:38PM (690 s).

## Step Eight: Production MD

- Download the `md.mdp` file from [this website](http://www.mdtutorials.com/gmx/complex/Files/md.mdp).
- Run a 10 ns molecular dynamics (MD) simulation:

```zsh
gmx grompp -f md.mdp -c npt.gro -t npt.cpt -p topol.top -n index.ndx -o md_0_10.tpr
gmx mdrun -deffnm md_0_10
```

The 100 ps runs took \\(\approx\\) 10 min. For the production MD run of 10 ns, expect:

\\[
\mathsf{10\,ns\times\cfrac{1000\,ps}{ns}\times\cfrac{10\,min}{100\,ps}\times\cfrac{1\,hr}{60\,min}=16.7\,hr}
\\]

Note: to print the math, I used [MathJax](http://docs.mathjax.org/en/latest/index.html). For inline math, use `\\(<math here>\\)` and for more complex math use:

````
\\[
<complex math here>
\\]
````
