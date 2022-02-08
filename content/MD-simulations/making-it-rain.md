---
title: "Making It Rain"
date: 2021-10-29T19:20:18-04:00
draft: false
lastmodifierdisplayname: DGO
---

Making it rain [^1] is a front end for running molecular dynamics simulations using cloud-based resources. It makes use of the [OpenMM toolkit](https://openmm.org/), [Google Colab framework](https://colab.research.google.com/), and [Jupyter notebooks](https://jupyter.org/index.html).

Before I start running MD simulations in Colab, I will start with some Colab-Jupyter tutorials outlined in the paper by Engelberger et al. (2021) [^2]. These tutorials are available from the [Cloud-based Tutorials on Structural Bioinformatics github repository](https://github.com/pb3lab/ibm3202).

Also see this blog: [New paper describes Google Colab notebooks to efficiently run molecular dynamics simulations of proteins](https://towardsdatascience.com/new-preprint-describes-google-colab-notebook-to-efficiently-run-molecular-dynamics-simulations-of-9b317f0e428c)

## PyRosetta notebooks

### PyRosetta Google Drive Setup

First follow these instructions:

- Visit the [RosettaCommons/PyRosetta.notebooks github page](https://github.com/RosettaCommons/PyRosetta.notebooks).
- Click on [Chapter 1: How to get started](https://nbviewer.org/github/RosettaCommons/PyRosetta.notebooks/blob/master/notebooks/01.00-How-to-Get-Started.ipynb).
- **Change to the Google work account**
- (Step 1) Get the PyRosetta License.
- Mount Google Drive.
- (Step 2) Copy the `01.00-How-to-Get-Started.ipynb` notebook to your google drive.
- Rename the copy to `01.00-How-to-Get-Started-dgo.ipynb`.
- (Step 3) Run the first two cells in the `01.00-How-to-Get-Started-dgo.ipynb` jupyter notebook.

```bash
PyRosetta installed at 'prefix'... Please click "Runtime → Restart runtime" before using it.
```

Success! 

- Run the code in cell 2.

Success!



### Install extensions

- (Step 4) Run the cell with the following code:

```bash
if not os.getenv("DEBUG"):
    !pip install attrs billiard biopython blosc dask dask-jobqueue distributed GitPython graphviz jupyter matplotlib numpy pandas py3Dmol scipy seaborn traitlets --user
```

I got the following error:

```bash
NameError: name 'os' is not defined
```

Add the following to the code at the top:

```bash
import os
```

Success!

Now I will try the [Cloud-based Tutorials on Structural Bioinformatics](https://github.com/pb3lab/ibm3202).

---

## p3lab (IBM3202)tutorials

### Lab.00 Software

- Go to the [Cloud-based Tutorials on Structural Bioinformatics github repository](https://github.com/pb3lab/ibm3202).
- Choose Lab.00 and Open in Colab.
- **change account to work account**
- Copy the notebook to Google Drive and rename it `lab00_software-dgo.ipynb`.
- Setting up PyRosetta: follow instructions.
- Update the minor version information in the code. I changed

```py
if sys.version_info.major != 3 or sys.version_info.minor != 6:
```

to

```py
if sys.version_info.major != 3 or sys.version_info.minor != 7:
```

I also needed to change the version downloaded to:

```py
http://graylab.jhu.edu/download/PyRosetta4/archive/release/PyRosetta4.MinSizeRel.python37.linux/PyRosetta4.MinSizeRel.python37.linux.release-300.tar.bz2   
```

Success! File is downloading.

{{% notice info %}}
I'm not sure why the instructions say to follow the PyRosetta installation from the PyRosetta notebooks, then install it again, here. It appeared to be already installed in a `prefix` directory from my first installation attempt.
{{% /notice %}}

I saw a fatal error early in the running of the script in the output cell. However, the streaming was truncated to the last 5000 lines, and I cannot find where the log files (if any) are located. The error had something to do with `.pth` files. The installation appeared to succeed--it ended with `PyRosetta setup took: 687.3s...`, which is the last line of the script.

I ran the checks for PyRosetta and the scripts ran without problems. There was one user warning:

```bash
Import of 'rosetta' as a top-level module is deprecated and may be removed in 2018, import via 'pyrosetta.rosetta'.
  This is separate from the ipykernel package so we can avoid doing imports until
```

It looks like the warning was truncated.  

[From this site](https://www.rosettacommons.org/node/11238)

>As a result changing
>
>```py
>from rosetta import *
>from pyrosetta import *
>from rosetta.protocols.rigid import *
>```
>to
>```py
>from pyrosetta.rosetta import *
>from pyrosetta import *
>from pyrosetta.rosetta.protocols.rigid import *
>```

In my case:

Consider changing 

```py
from pyrosetta import *
from rosetta.protocols.rosetta_scripts import *
from pyrosetta import (
```

to

```py
from pyrosetta import *
from pyrosetta.rosetta.protocols.rosetta_scripts import *
from pyrosetta import (
```

### Installation of GROMACS

- Follow the instructions in the notebook.

Had some problems.

```bash
GROMACS set up completed
. . . 
GROMACS extraction completed
. . . 
-- Downloading... done
-- extracting... done
. . .
Making all in generic-simd256
. . . 
Making all in .
. . .
Making all in tools
. . .
Making install in .
. . .
[  1%] Completed 'fftwBuild'
. . .
src/gromacs/CMakeFiles/libgromacs.dir/build.make:63: recipe for target 'src/gromacs/CMakeFiles/libgromacs.dir/nbnxm/cuda/libgromacs_generated_nbnxm_cuda.cu.o' failed
CMakeFiles/Makefile2:3038: recipe for target 'src/gromacs/CMakeFiles/libgromacs.dir/all' failed
Makefile:162: recipe for target 'all' failed
GROMACS building completed
src/gromacs/CMakeFiles/libgromacs.dir/build.make:63: recipe for target 'src/gromacs/CMakeFiles/libgromacs.dir/nbnxm/cuda/libgromacs_generated_nbnxm_cuda.cu.o' failed
CMakeFiles/Makefile2:3038: recipe for target 'src/gromacs/CMakeFiles/libgromacs.dir/all' failed
CMakeFiles/Makefile2:1091: recipe for target 'CMakeFiles/check.dir/rule' failed
Makefile:587: recipe for target 'check' failed
GROMACS testing completed
src/gromacs/CMakeFiles/libgromacs.dir/build.make:63: recipe for target 'src/gromacs/CMakeFiles/libgromacs.dir/nbnxm/cuda/libgromacs_generated_nbnxm_cuda.cu.o' failed
CMakeFiles/Makefile2:3038: recipe for target 'src/gromacs/CMakeFiles/libgromacs.dir/all' failed
Makefile:162: recipe for target 'all' failed
GROMACS installation completed. Please check if any errors occurred during installation
/content/gromacs-2020.3/build/src/external/build-fftw/fftwBuild-prefix/src/fftwBuild/configure: line 8325: /usr/bin/file: No such file or directory
nvcc fatal   : Unsupported gpu architecture 'compute_30'

CMake Error at libgromacs_generated_nbnxm_cuda.cu.o.Release.cmake:219 (message):
  Error generating
  /content/gromacs-2020.3/build/src/gromacs/CMakeFiles/libgromacs.dir/nbnxm/cuda/./libgromacs_generated_nbnxm_cuda.cu.o


make[2]: *** [src/gromacs/CMakeFiles/libgromacs.dir/nbnxm/cuda/libgromacs_generated_nbnxm_cuda.cu.o] Error 1
make[1]: *** [src/gromacs/CMakeFiles/libgromacs.dir/all] Error 2
make: *** [all] Error 2
nvcc fatal   : Unsupported gpu architecture 'compute_30'
CMake Error at libgromacs_generated_nbnxm_cuda.cu.o.Release.cmake:219 (message):
  Error generating
  /content/gromacs-2020.3/build/src/gromacs/CMakeFiles/libgromacs.dir/nbnxm/cuda/./libgromacs_generated_nbnxm_cuda.cu.o

make[3]: *** [src/gromacs/CMakeFiles/libgromacs.dir/nbnxm/cuda/libgromacs_generated_nbnxm_cuda.cu.o] Error 1
make[2]: *** [src/gromacs/CMakeFiles/libgromacs.dir/all] Error 2
make[1]: *** [CMakeFiles/check.dir/rule] Error 2
make: *** [check] Error 2
nvcc fatal   : Unsupported gpu architecture 'compute_30'
CMake Error at libgromacs_generated_nbnxm_cuda.cu.o.Release.cmake:219 (message):
  Error generating
  /content/gromacs-2020.3/build/src/gromacs/CMakeFiles/libgromacs.dir/nbnxm/cuda/./libgromacs_generated_nbnxm_cuda.cu.o


make[2]: *** [src/gromacs/CMakeFiles/libgromacs.dir/nbnxm/cuda/libgromacs_generated_nbnxm_cuda.cu.o] Error 1
make[1]: *** [src/gromacs/CMakeFiles/libgromacs.dir/all] Error 2
make: *** [all] Error 2
```

[From this Stackoverflow issue](https://stackoverflow.com/questions/64633341/unsupported-gpu-with-nvcc-fatal)

>use GROMACS 2020.4

Restart Runtime (factory reset).

- Reinstall GROMACS using the 2020.4 version.

```bash
2021-11-01 15:41:58 (16.1 MB/s) - ‘gromacs-2020.4.tar.gz’ saved [29149899/29149899]
```


---

Start over from top.

```bash
PyRosetta setup took: 5000.8s...
```

Change 

```py
import pandas as pd
from pyrosetta import *
from rosetta.protocols.rosetta_scripts import *
from pyrosetta import (
    init, pose_from_sequence, pose_from_file, Pose, MoveMap, create_score_function, get_fa_scorefxn,
    MonteCarlo, TrialMover, SwitchResidueTypeSetMover, PyJobDistributor,
)
```

to

```py
import pandas as pd
from pyrosetta import *
from pyrosetta.rosetta.protocols.rosetta_scripts import *
from pyrosetta import (
    init, pose_from_sequence, pose_from_file, Pose, MoveMap, create_score_function, get_fa_scorefxn,
    MonteCarlo, TrialMover, SwitchResidueTypeSetMover, PyJobDistributor,
)
```

## Installing GROMACS

Change to

```bash
!wget http://ftp.gromacs.org/pub/gromacs/gromacs-2020.4.tar.gz
```

in the following code, change `gromacs-2020.3` to `gromacs-2020.4`


{{% notice info %}}
It appears that code cells that use `%%bash` scripts do not show intermediate output. You only get output when the entire script is done.
{{% /notice %}}

Success!

```bash
GROMACS installation completed. Please check if any errors occurred during installation
```

The next cell executed successfully. Output of `gmx -h` was the gromacs help file. 

### Backup GROMACS to Google Drive

Success!

### Installation of SBM-enhanced GROMACS

Downloading SBM-enhanced GROMACS was successful.

Extracting and Installing it did not show output `%%bash` until the end.

```bash
SBM-enhanced GROMACS installation completed. Please check if any errors occurred during installation
```

Installation check: `gmx -h` returned the help file. Success!

Backing up: 

```bash
GROMACS successfully backed up!
```

Software notebook, `lab00_software-dgo.ipynb`,  successfully completed!

### Colab tips

From [this website](https://www.kdnuggets.com/2019/01/more-google-colab-environment-management-tips.html).

- Go to [colab](https://colab.research.google.com/)
- **Change to the Google work account**
- Click on the 3 dots in the upper right corner of your Chrome browswer.
- Go to *More Tools* -> *Create Shortcut...*
- Give the shortcut a name such as `Colab`.
- Click the box, *Open as window*.
- Click *Create*.

Great! Now you have an app on your computer that will launch Colab in a new window.

### Lab.01 warmup

- Open the Colab app.
- In the `GitHub` tab, open [this link](https://github.com/pb3lab/ibm3202).
- Open the tutorials/lab01_intro.ipynb notebook.
- Save a copy to Drive, and rename it with the *-dgo* suffix.

{{% notice info %}}
As soon as you *make a copy* of the notebook, the copy is opened in the browser, which defeats the purpose of using the Colab shortcut app.
{{% /notice %}}

- Continue with tutorial.




## Resources for data visualization

### Seaborn

From the [Seaborn](http://seaborn.pydata.org/) website:

>Seaborn is a Python data visualization library based on matplotlib. It provides a high-level interface for drawing attractive and informative statistical graphics.

There is a nice Seaborn tutorial presented as a jupyter notebook in Colab: [04.14-Visualization-With-Seaborn.ipynb](https://colab.research.google.com/github/jakevdp/PythonDataScienceHandbook/blob/master/notebooks/04.14-Visualization-With-Seaborn.ipynb).

### Pandas

[Pandas](https://pandas.pydata.org/)

An introduction to Pandas is provided by Google as a [colab notebook](https://colab.research.google.com/notebooks/mlcc/intro_to_pandas.ipynb) and a tutorial can be found at [Pandas DataFrame UltraQuick Tutorial.ipynb](https://colab.research.google.com/github/google/eng-edu/blob/master/ml/cc/exercises/pandas_dataframe_ultraquick_tutorial.ipynb).

There are tutorials available on the [Pandas documentation website](https://pandas.pydata.org/pandas-docs/stable/index.html).

## Data science tutorials

[Data Science 101: Build your first Machine Learning Model with Pandas, Scikit-Learn, and Google Colab](https://towardsdatascience.com/data-science-101-start-with-pandas-scikit-learn-and-google-colab-ecbb6a247cc9)




[simple tutorial for sharing jupyter notebooks on GitHub](https://reproducible-science-curriculum.github.io/sharing-RR-Jupyter/01-sharing-github/)  


1. git init
1. change master to main
2. git add README.md  
3. git add GeeksForGeeks.ipynb
4. git commit -m "notebook first commit" 
5. git remote add origin https://github.com/{Your repo}/GeeksForGeeks.git 
6. git push -u origin master 

## Organizing notebooks

 
[extensions for code review](https://www.drivendata.co/blog/nbautoexport-jupyter-code-review/)


### nbpages

From the [nbpages documentation](https://jckantor.github.io/nbpages/):

>[nbpages](https://github.com/jckantor/nbpages#id1) is a command line tool for publishing a collection of Jupyter notebooks to Github Pages. This project was inspired by the tools included with the [Python Data Science Handbook](https://github.com/jakevdp/PythonDataScienceHandbook) by [Jake Vanderplas](https://github.com/jakevdp).



### Intalling `nbextensions`

The jupyter notebook extensions are a useful set of tools that makes working with jupyter notebooks easier. See [10 Jupyter Notebook Extensions Making My Lyfe Easier](https://medium.com/@maxtingle/10-jupyter-notebook-extensions-making-my-lyfe-easier-f40139a334ce) for some of them.

- Try these commands (note: `conda` is not installed on `colab`):

```bash
!pip install jupyter_contrib_nbextensions
# it appears many of the packages are already installed
!pip install jupyter_nbextensions_configurator
# Requirement already satisfied:
!jupyter contrib nbextension install --user 
!jupyter nbextensions_configurator enable --user
```

### Installation of PyRosetta

- Get the PyRosetta License.
- Mount Google Drive.

Success!


### Avoiding colab timeouts

[How to save Google Colab Notebooks from runtime timeouts](https://medium.com/analytics-vidhya/how-to-save-google-colab-notebooks-from-runtime-timeouts-4aa133375a7e) has a `keep alive` script that you can get from the [sour4bh/stop-cursing-colab](https://github.com/sour4bh/stop-cursing-colab) repository. It uses `AutoHotKey`.




---

### Adding CSS to colab notebooks

From [Stackoverflow](https://stackoverflow.com/questions/58019749/how-to-import-css-file-into-google-colab-notebook-python3).

More [Stackoverflow answers](https://stackoverflow.com/questions/18024769/adding-custom-styled-paragraphs-in-markdown-cells), but these are from a few years ago.

[This might work](https://stackoverflow.com/questions/61957742/how-to-increase-font-size-of-google-colab-cell-output)

### Other online jupyter notebook environments

[Kaggle](https://www.kaggle.com/)  access free GPUs  


[CoCalc](https://cocalc.com/)

Offers real-time collaboration, version control, chat, and other features.  
Also has been used for teaching since 2013, so has useful teaching features.  
Pricing: $14 per student, and a per month/core/project, etc. subscriptions.  
Academic Research Group: $831/yr  
Hobbyist: $70/yr, but only 1GB/project



### Installing `conda` on `colab`

- Follow the instructions at [How to install / use Conda on Google Colab](https://inside-machinelearning.com/en/how-to-install-use-conda-on-google-colab/) (May 2021).
- Check for `conda`.

```bash
!conda --version
/bin/bash: conda: command not found
```

`Conda` is not installed.

- Install it with the `pip` command.

```bash
!pip install -q condacolab
import condacolab
condacolab.install()
```

I may not need this if I stick with `pip`.







[^1]: Arantes, P.R., Polêto, M.D., Pedebos, C., and Ligabue-Braun, R. (2021). Making it Rain: Cloud-Based Molecular Simulations for Everyone. J Chem Inf Model 61: 4852–4856. doi: [10.1021/acs.jcim.1c00998](https://doi.org/10.1021/acs.jcim.1c00998).

[^2]: Engelberger, F., Galaz-Davison, P., Bravo, G., Rivera, M., and Ramírez-Sarmiento, C.A. (2021). Developing and Implementing Cloud-Based Tutorials That Combine Bioinformatics Software, Interactive Coding, and Visualization Exercises for Distance Learning on Structural Bioinformatics. doi: [10.1021/acs.jchemed.1c00022](https://pubs.acs.org/doi/10.1021/acs.jchemed.1c00022)








---

------