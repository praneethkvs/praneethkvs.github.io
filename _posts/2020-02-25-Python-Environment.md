---
layout: post
title: Create Anaconda Environment
---

When juggling with multiple projects and workstreams each of which has a disparate set of dependencies, it is a good idea to
isolate your work environments. This not only keeps everything clean and organized, it also makes switching between different projects easier and in general is a good programming practice.

To Create an isolated python environent in anaconda.  

```shell
conda info --envs #Lists all available environments

#Create Environment
#The ‘-y’ flag essentially tells the command line to say ‘yes’ to all of the prompts that follow
conda create --name your_env_name python=3.7 -y 
```
The create command automatically loads all available packages, if you want only a subset you could specify them in the command line as arguments as follows:   
`conda create --name your_env_name python=3.7 scipy=0.15.0 astroid babel`

If you want all packages that are in the base anaconda, you could simply create a clone of the base environment as follows:  
`conda create --name your_env_name --clone base`



Or you could also create an environment using a YAML File, see:   
https://towardsdatascience.com/getting-started-with-python-environments-using-conda-32e9f2779307

```
#Activate environment
conda activate your_env_name 

#deactivate environment
conda deactivate 

#Remove Environment --all flag is necessary to remove all packages associated
conda remove --name your_env_name --all 
```

If you get a permission error when trying to remove an environment especially if you are on macOS 10.14 Mojave, run the following in your terminal and then try to remove the environment.
You are basically deleting all the .app files from the bin folder in the environment.
```shell
cd ~/anaconda3/envs/your_env_name/bin
ls *.app
rm -rf *.app
conda update -n your_env_name --all --yes

```



If you want to Run Jupyter Notebooks using the `jupyter notebook` command.   
You might have to install the following before you can get it to work.  

```shell
conda install jupyter
conda install nb_conda
conda install ipykernel
```
