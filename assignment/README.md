# Assignment 5

This time, create your own GitHub repository locally with `git init assignment4` or some other folder name to your liking. In this repository: 

 1. Create a `conda` environment with `python`, `numpy`, `scipy`, `pandas`, `ipython`, `jupyterlab`, and `matplotlib`. Name the environment to your liking.
 2. Export this `conda` environment with the `--from-history` option into a file named `environment.yml`. 
 3. Activate this environment and work on the assignment in this environment.
 4. You can either `git clone` the `wcm_datasci_basics_04python` repository to your laptop, and copy (`scp`, `cp`, or `rsync` all works) the `pypet` script to your assignment repository. Or copy the raw python code from GitHub and paste into a new file you create in your assignment repo with (`touch`). 

Make sure you are using the appropriate `conda` environment when working on a Python project (this assignment). And `deactivate` your Python environment once you are done using Python. This assignment will be graded through binder. So you must have a working environment file pushed to GitHub in your repository for me to check your work. After pushing your assignment to GitHub, add `axiezai@gmail.com` as a collaborator to your private repository. 

## Part 1

Using the same tamagotchi pet script we created during class, do the following (I expect you to make appropriate commits based on your added functionality, points will be taken off for a repository with only 1 commit history entry). No need to put any answers here in the markdown file, I will review your final script that you push to Github.:

 1. Add a happiness meter that decreases over time at half the rate of hunger. 

 2. Add a pet action, when you pet your tamagotchi pet, the happiness will increase. How happy your pet gets is up to you. 

 3. Modify the feed action, so that when you feed you pet, both hunger and happiness will change (less hungry, more happy). But your pet should receive less happiness from feeding than petting. 

 4. Make sure you pet's new happiness data is saved and can be loaded with the same `.csv` file.


## Part 2

Complete the exercises in the jupyter notebook `p-values.ipynb`. Remember you can start a jupyter work session with `jupyter lab` or `jupyter notebook` in your terminal. To use your assignment's `conda` environment, make sure you add it to `jupyter` with `python -m ipykernel install --user --name={your env name}`.
