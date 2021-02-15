# Introduction to Python, Jupyter, Conda
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/Brainhack-NY/py-basics-tutorial/HEAD)

This is an introduction to using Conda and Jupyter for scientific research projects. The contents are based on materials from Neurohackademy and lessons put together by Tal Tarkoni.

## Conda
The setup instructions had you install a program called `miniconda`, and we've given you a few `conda` commands to configure and install a few Python packages. 

### What is Conda?
From [conda's documentation:](https://docs.conda.io/projects/conda/en/latest/index.html):
>Conda is an open-source package management system and environment management system that runs on Windows, macOS, and Linux. Conda quickly installs, runs, and updates packages and their dependencies. Conda easily creates, saves, loads, and switches between environments on your local computer. It was created for Python programs but it can package and distribute software for any language.

>Conda as a package manager helps you find and install packages. If you need a package that requires a different version of Python, you do not need to switch to a different environment manager because conda is also an environment manager. With just a few commands, you can set up a totally separate environment to run that different version of Python, while continuing to run your usual version of Python in your normal environment.

`miniconda` is simply a minimalistic version of the much "heavier" `anaconda` package manager. We are using its commandline program named `conda` to manage our Python computing environment. If you've never used Conda before and simply followed our set-up instructions, you should only have the default `base` environment available, which contains files, packages, and their dependencies in an isolated environment that does not interact with your computer's native environment.

### Pathing Review:
Our "computing environment" is managed by a set to paths. We already use `pwd` frequently to check where we are working at in our terminal. When we use a program like `ls` or `cd`, the shell searches in a set of pre-determined paths for the programs we called. We can check what these paths are with `which`. `Conda` is an environment manager that helps us manage these paths, the most important one being `$PYTHONPATH`.

### From our set-up instructions:
```
conda config --set auto_activate_base false
conda config --append conda-forge
conda config --set channel_priority strict
conda install -y flake8 python ipython jupyter jupyterlab matplotlib numpy pandas scipy seaborn
```
We appended a channel named `conda-forge`, which hosts a lot of the libraries we will install. After you ran these commands, your `base` environment will be able to interactively run Python programs, open Jupyter notebooks, and perform data science operations.

Environment managers such as `conda`, `brew`, and `apt` can conflict with each other. So we recommend you to disable the `base` environment by default, and only activate a conda environment when you need to use its contents (python). 

### Managing environments with `conda`:
Being able to manage isolated environments means we can create project specific envrionments and share them! We can view all of our current environments with 
```
$ conda env list
```
Now let's imagine we need to initiate a project where we visualize and preprocess diffusion-weighted MR images with [`dipy`](https://dipy.org/documentation/1.3.0./installation/). We don't want this to interfere with our nice `base` envrionment, and we want to share our work with our colleagues after we are done. We can create a new isolated environment with
```
$ conda create --name dwi_py
```
Here we used `conda` to `create` an environment named "`dwi_py`". Conda will ask if you want to proceed with default path settings, simply enter `y` for "Yes". Then we can list the contents of our new environment with `conda list`, which should show that it's currently empty. Next we will tell `conda` to install the packages we need for our new project:
```
$ conda activate dwi_py
$ conda install -c conda-forge python=3.6 dipy pip
```
We first activated our new environment avoid interfering with our native computer environment, then we told `conda` to install a specific version of Python (3.6), `dipy` for handling diffusion imaging, and the package installer for python named `pip`. Conda will automatically find all the libraries we requested, AND their dependencies.

According to [dipy's documentation](https://dipy.org/documentation/1.3.0./installation/), we need a Python specific package named `fury` to visualize some methods:
```
$ pip install --user fury
```
Now we have everything we need to proceed with our project! Imagine we've created some `dipy` analysis in Python, and our colleague wishes to try it on another machine in the lab with newly acquired data. Conda allows us to share everything we just installled in a `environment.yml` file. To export our `dwi_py` environment information:
```
$ conda env export >> environment.yml
```
This command tells `conda` that we want to `export` our currently active `env`, and `>>` is a shell notation that appends command outputs to another file. We can take a look at this environment file with any text editor we like. We see that not only did `conda` export the libraries we told it to install, but also specific dependencies and their commit hashes. This strictness could cause problems when sharing environments across platforms, so we can tell `conda` to only export packages we explicitly asked for in the history of this environment:
```
$ conda env export --from-history > environment.yml
```
Note: `>` overwrites while `>>` appends. Now we should see a more simple output:
```
(dwi_py) ➜  py-basics-tutorial git:(main) ✗ cat environment.yml
name: dwi_py
channels:
  - conda-forge
  - defaults
dependencies:
  - python=3.6
  - dipy
  - pip:
    - fury      # needed to add this extra line
prefix: /Users/xxie/miniconda3/envs/dwi_py
```
We may want to delete the `prefix` line before sharing so the path to the environmeent becomes flexible. Now we can share this environment file via GitHub! Let's pretend we are our colleague, and pretend we currently do not have the environment on our machine by deleting what we just created:
```
$ conda deactivate
$ conda remove --name dwi_py --all
```
We instructed our colleague to clone the shared `environment.yml` file, and recreate the environment with:
```
$ conda env create -f environment.yml
```
Now our colleague can activate the exact same environment on her machine to run Python code!

Lastly, you can find the complete guide to conda environment management on their [documentation website](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#).

### Binder - environments improve reproducibility
Binder uses the `repo2docker` tool to build containerized versions of your environment, and hosts an interactive Jupyter session online for people to access via browsers.
 - See overview at: https://jupyter.org/binder
 - For example: https://jupyter.org/try
 - 2007 Nobel Prize in Physics: https://blog.jupyter.org/congratulations-to-the-ligo-and-virgo-collaborations-from-project-jupyter-5923247be019


### For this tutorial:
Additional setups are required in your `base` conda environment to run interactive widgets. In your terminal, with the `base` environment active:

```
$ conda activate      # activate the base env
(base)$ conda install -c conda-forge nodejs=11.9.0
(base)$ pip install --user nibabel
(base)$ jupyter labextension install @jupyter-widgets/jupyterlab-manager@2.0
```
We also need to make sure our current conda environment is visible to Jupyter:
```
(base)$ python -m ipykernel install --user --name=base
```
Of course, you can replace "base" with the name of any other environment you create in the future. We will take a look at Jupyter next.
