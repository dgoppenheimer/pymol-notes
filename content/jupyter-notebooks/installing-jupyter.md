---
title: "Installing Jupyter"
date: 2021-10-29T16:17:12-04:00
draft: false
lastmodifierdisplayname: DGO
---

I've decided to give [JupyterLab](https://jupyter.org/index.html) a try. Follow the installation instructions on the [Jupyter website](https://jupyter.org/install).

## Install `conda`

First I need to install `conda`.

- Download the [Miniconda installer](https://docs.conda.io/en/latest/miniconda.html).
- Check the installer hash:

```zsh
shasum -a 256 ~/Downloads/Miniconda3-latest-MacOSX-x86_64.sh
```

An easier way to compare the hashes:

```zsh
cd ~/Downloads
echo "8fa371ae97218c3c005cd5f04b1f40156d1506a9bd1d5c078f89d563fd416816  Miniconda3-latest-MacOSX-x86_64.pkg" | shasum -a 256 -c

Miniconda3-latest-MacOSX-x86_64.pkg: OK
```

- Double-click on the installer and follow the prompts.
- Restart iTerm2.
- Test installation with `conda list`.

Success!

## Install JupyterLab

- Use `conda` to install `jupyterlab`.

```zsh
conda install -c conda-forge jupyterlab
```

- Restart iTerm2.
- Launch JupyterLab with `jupyter-lab`.

Success!

- Use Use `Control-C` to stop the JupyterLab server.
