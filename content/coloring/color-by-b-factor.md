---
title: "Color by B-factor"
date: 2022-02-08
draft: false
lastmodifierdisplayname: DGO
---

B-factors are measures of how much thermal motion an atom exhibited during the determination of a protein structure by X-ray crystallography. It is useful to color a protein based on the b-factors of its backbone atoms to see the segments of the protein that are most dynamic (or where the uncertainty in atom positions is the highest).

The best way to color by b-factor in PyMOL is to use the `color_b.py` script. The script can be downloaded from the [PyMOL Script Repository](http://pldserver1.biochem.queensu.ca/~rlc/work/pymol/).

- Download the [`color_b.py` script](http://pldserver1.biochem.queensu.ca/~rlc/work/pymol/color_b.py).
- Put the script into your PyMOL scripts directory or your current working directory.
- Type `run color_b.py` in the PyMOL command line.

### Usage

- Make a selection.

```py
select ca, name ca # select alpha carbons and name the selection ca
```

- Color the selection.

```py
color_b selection='ca',mode=ramp,gradient=bwr,nbins=30,sat=.5, value=1
```

- `color_b`: invokes the command
- `selection='ca'`: indicates the named selection that will be colored
- `mode=ramp`: chooses ramp for gradient mode

From the `help` file:

>The division of B-value ranges can be in either of two modes: 'hist' or 'ramp'. 'hist' is like a histogram (equal-sized B-value increments leading to unequal numbers of atoms in each bin). 'ramp' as a ramp of B-value ranges with the ranges chosen to provide an equal number of atoms in each group.

- `gradient=bwr`: chooses one of the many preset gradients

>The gradients can be:
>```py
>'bgr': blue -> green   -> red
>'rgb': red  -> green   -> blue
>'bwr': blue -> white   -> red
>'rwb': red  -> white   -> blue
>'bmr': blue -> magenta -> red
>'rmb': red  -> magenta -> blue
>'rw' : red -> white
>'wr' : white -> red
>'ry' : red -> yellow
>'yr' : yellow -> red
>'gw' : green -> white
>'wg' : white -> green
>'bw' : blue -> white
>'wb' : white -> blue
>'gy' : green -> yellow
>'yg' : yellow -> green
>'gray' : black -> white
>'reversegray' : white -> black
>'user' : user defined in this script
>```

- `nbins=30`: Sets the number of bins for the color gradient. Higher numbers are wider gradients.
- `sat=.5, value=1`:

>You can also specify the saturation and value (i.e. the "s" and "v" in the "HSV" color scheme) to be used for the gradient. The defaults are 1.0 for both "sat" and "value".

- Type `help(color_b)` to get these and additional usage guidelines.
