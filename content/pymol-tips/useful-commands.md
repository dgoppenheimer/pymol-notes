---
title: "Useful Commands"
date: 2021-11-10T09:52:31-05:00
draft: false
lastmodifierdisplayname: DGO
---

I created this document to store the commands I use often in PyMOL, but can't remember them when I need them.

### Downloading the biological assembly from RCSB

The default of the `fetch` command is to download the asymmetric unit from RCSB. If your protein of interest is a dimer or multimer then you need to download the biological assembly.

See the [PyMOL wiki entry for `fetch`](https://pymolwiki.org/index.php/Fetch), examples 3 and 4. Also see the entry for [`assembly`](https://pymolwiki.org/index.php/Assembly).

Example code for downloading the biological assembly of [6tof](https://www.rcsb.org/structure/6TOF):

```py
fetch 6tof, type=pdb1
spit_states 6tof
```

Or

```py
set assembly, 1
fetch 6tof, assembly1, async=0
split_states assembly1
```

An alternative is to use symmetry mates. Here is the [PyMOL wiki page for `symexp`](https://pymolwiki.org/index.php/Symexp) showing how to do that. An example:

```py
# Expand the ''object'' around its ''selection'' by cutoff Angstroms and
# prefix the new objects withs ''prefix''.
symexp prefix, object, selection, cutoff [, segi]
```

Or

```py
fetch 6tof
symexp mate, 6tof, 6tof, 1
```

To use `symexp`, you need to know which of the symmetry mates to keep and which to discard. In other words, you need to know what the biological assembly looks like, which is usually shown on the RCSB page for the protein of interest.

### Reinitialize

They do not mention this on the [PyMOL wiki reinitialize page](https://pymolwiki.org/index.php/Reinitialize), but you can run this command using `reinit` or even `rein`.


