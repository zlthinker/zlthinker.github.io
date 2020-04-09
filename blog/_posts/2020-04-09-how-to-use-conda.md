---
title: How to use Conda
updated: 2020-04-09 14:50
---

* TOC
{:toc}

# Introduction

Conda is a useful cross-platform environmental package manager.


# Commands

* List all the environments
```
conda info -e / conda env list
```

* Create an environment
```
conda create -n <environment_name> 
conda create -n <environment_name> --no-default-packages    # no default packages will be pre-installed
conda create -n <environment_name> python=3.6   # create an environment with a specific version of python
```

* Create a environment from a define yml file or a requirement list
```
conda create -n <environment_name> -f <path_to_yml_file / path_to_requirement_list>
```

* Create a environment by copying from an existing enviroment
```
conda create -n <new_environment_name> --clone <environment_name>
```

* Get into/off an environment
```
conda activate <environment_name> / conda deactivate
```

* Remove an environment
```
conda env remove --name <environment_name> / conda remove --name <environment_name> --all
```

* List the installed packages of an environment
```
conda list -e
```

* Export a conda environment to a yml file
```
conda env export > environment.yml
```

