---
title: "PyMOL Shortcuts"
date: 2021-11-07T19:49:28-05:00
draft: false
lastmodifierdisplayname: DGO
---

This page describes the installation and use of the [pymolshortucs](https://github.com/MooersLab/pymolshortcuts) built by B.H.M. Mooers and described in Mooers (2020) [^1].

## Install the date time module

The `pymolshortcuts.py` script has a shortcut to timestamp filenames produced by PyMOL. To use this, the date time module needs to be installed. Because I have `MacPorts` on my system, I'll used the suggested `MacPorts` installation instructions.

In a terminal window, type the following command:

```zsh
sudo -H /opt/local/bin/python3.7 -m pip install --upgrade datetime
```

Upgrade `pip` too.

```zsh
sudo -H /opt/local/bin/python3.7 -m pip install --upgrade pip
```

## Download the script file

- Download and unpack the `pymolshortcuts-master.zip` file. 
- Move the `pymolshortcuts-master` folder into the ~/pymol-work/1-scripts` directory.

## Edit the `.pymolrc`

- Add `run ~/pymol-work/1-scripts/pymolshortcuts-master/pymolshortcuts.py` to the bottom of the file.
- Save and close the file.

## Test the script

- Open PyMOL.
- Type the following into the command prompt:

```py
Fetch 3OAC
AO
```

Success! The molecule has ambient occlusion lighting.

## Install the shortcuts in VS Code

Follow the instructions on this site: [PyMOL Script Writing with Code Templates](https://mooerslab.github.io/pymolsnips/#VisualStudioCode).

- Open VS Code and install the `bioSyntax` extension.
- Download and unpack the `pymolsnips-master.zip` file from the [pymolsnips github repository](https://github.com/MooersLab/pymolsnips).
- Move into the `vscpymolsnips` folder and run the following command:

```zsh
mv pml.json ~/Library/Application\ Support/Code/User/snippets
```

- Restart VS Code.
- Create a new file and save it as `test.pml`.
- Type `ao` and the autocomplete should trigger to insert the `ao` code.
- Hit `enter` to insert the code.

Success! I can insert PyMOL code into documents I create/edit in VS Code.

[^1]: Mooers, B.H.M. (2020). Shortcuts for faster image creation in PyMOL. Protein Sci 29: 268–276.






