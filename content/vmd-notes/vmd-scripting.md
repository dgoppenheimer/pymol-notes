---
title: "VMD Scripting"
date: 2021-10-17T17:02:04-04:00
draft: false
lastmodifierdisplayname: DGO
---

VMD scripting is handy for executing command that you do often--like preparing a molecule for a figure, or for carrying out certain analyses. When I use PyMOL, I have a set of scripts for coloring atoms and defining various representations so I don't have to repeat these by hand each time I need them. In addition, there are useful scripts available for analyzing protein structure and trajectories.

## Scripting Resources

Here are some web resources for scripting in VMD:

From the VMD wiki:  
[Scripting in VMD](https://www.ks.uiuc.edu/Training/Tutorials/vmd/tutorial-html/node4.html)  
[Advanced Script Writing](https://www.ks.uiuc.edu/Research/vmd/vmd-1.3/ug/node246.html) 

Other VMD scripting resources:  
[VMD Script Library](https://www.ks.uiuc.edu/Research/vmd/script_library/)  
A few useful short scripts: [VMD](http://titin.abrol.csun.edu/abrollab/protocols/temp_docs/VMD.html)  
[Resources](https://miaolab.ku.edu/resources.html)  

Other useful structural biology resources:  
[OpenStructure](https://www.openstructure.org/)  
[SpotOn: High Accuracy Identification of Protein-Protein Interface Hot-Spots](https://alcazar.science.uu.nl/cgi/services/SPOTON/spoton/)


From the [VMD wiki](https://www.ks.uiuc.edu/Research/vmd/current/ug/node97.html#SECTION00938300000000000000):


>**Finding contact residues**
>Suppose you want to view the atoms in A which are in contact with B. Use the within <distance> of <selection> selection command. For purposes of demonstration, let A be protein, B be nucleic, and define contact as an atom in A which is within 2 Å of an atom in B. Then the selection command is
>
>```tk
>protein within 2 of nucleic
>```
>
>If you want to see all the residues of A which have at least one atom in contact with B, use
>
>```tk
>same residue as (protein within 2 of nucleic)
>```

[mono2poly 1.0 script](http://www.ks.uiuc.edu/Research/vmd/script_library/scripts/mono2poly/)  
This script takes a monomer from an asymmetric unit of a crystal structure and makes the appropriate translations as found in the `.pdb` header to get the full polymer.

{{% notice tip %}}
The `mono2poly.tcl` script strips out ligands and other hetero atoms.
{{% /notice %}}


How to use:

```tk
source mono2poly.tcl
mol delete all
mol new 1c8e.pdb
set sel [atomselect top all]
set matrix [parsematrix 1c8e.pdb]
mono2poly -o outname $sel $matrix
```

An alternative to the `mono2poly.tcl` script to build the biological assembly from a monomer in the asymmetric unit is to:

1. download the biological assembly file from `RCSB`.
2. Go into the *Trajectory* tab and change the *Draw Multiple Frames:* value to `0.20` for a dimer, or `0.30` for a trimer, etc.


{{% notice info %}}
With the above alternative, I cannot select residues on each monomer--I  can only select residues on one monomer, and each monomer is labeled chain A.
{{% /notice %}}

Here is another script to create a biological assembly from a `.pdb` file:

[mergemultiframepdb 1.0](http://www.ks.uiuc.edu/Research/vmd/script_library/scripts/mergemultiframepdb/)

EXAMPLE USAGE:

```tcl
source mergemultiframepdb.tcl # the script is in the working directory
mol delete all
mol new 2cft.pdb # the multi-frame .pdb file
merge_multi_frame_structure top /tmp/ 2cft-poly

# Results will be written to /my/final/filename.psf and /my/final/filename.pdb
```

{{% notice info %}}
The above script fails if your protein has a ligand.
{{% /notice %}}


[check out this site](https://chryswoods.com/dynamics/visualisation/comparing.html)

Try [TopoTools](https://sites.google.com/site/akohlmey/software/topotools)

This is probably the best way: I could use grep or awk or just do it by hand:

[preparing a dimeric protein for NAMD](https://www.ks.uiuc.edu/Research/namd/mailing_list/namd-l.2005-2006/1573.html)

build section of main menu and use the pdb scan function. ( I can't find it)




