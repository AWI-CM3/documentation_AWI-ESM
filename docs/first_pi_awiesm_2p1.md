# Your First Pre-Industrial Run with `AWI-ESM 2.1`
```{article-info}
:avatar: https://secure.gravatar.com/avatar/709ea66dc102e6bc4547032f85ff6c95 
:avatar-link: mailto:paul.gierz@awi.de 
:avatar-outline: muted
:author: Paul Gierz 
:date: June 2022
:read-time: "30 to 45 min read and hands-on"
:class-container: sd-p-2 sd-outline-muted sd-rounded-1

```

This guide takes you through the process of creating your first Pre-Industrial simulation with `AWI-ESM 2.1`

```{note}
Some content will be in tabs. You should select which computer you want to work on.
```

```{admonition} Upcoming Video
:class: seealso
**A video tutorial will be available soon!**

Please be aware that the video tutorial will be specific for each HPC system.
You may need to select different computer tabs in the text below to follow
along correctly. 
```

These instructions will guide you through setting up a simple Pre-Industrial
simulation. It is assumed you have already gone
through the [`direnv` tutorial](./direnv.md). 

## Preliminaries
To begin, we are going to define a working directory for our project,
`$PROJECT_BASE`, create the folder, and go there.

````{card}
Terminal
^^^
```
$ export PROJECT_BASE=/work/ab0246/a270077/SciComp/Model_Support/AWIESM/Tutorials/First_PI
$ mkdir -p $PROJECT_BASE
$ cd $PROJECT_BASE
```
````

```{hint}
You should replace the `PROJECT_BASE` with your own location!
```

Once in this folder, we are going to create a `direnv` based folder. To do
that, open up a new `.envrc` file, and add the following.

``````{tab-set}
`````{tab-item} DKRZ Levante
```{note}
These instructions are specific to DKRZ's Levante System!
```
````{card}
**New** File: `.envrc`
^^^
```shell
# Load a modern version of Python for using the esm-tools on Levante
module load python3
# Ensure that the git version control software is available when entering
# this folder
module load git
# Common climate tools: ncview, ncdump, cdo
module load netcdf-c
module load ncview
module load cdo
# Set up a project structure for Python where the esm-tools will be
# installed:
layout python
# export the PROJECT_BASE variable for easy use to get back to the top of the
# experiment folder
export PROJECT_BASE=/work/ab0246/a270077/SciComp/Model_Support/AWIESM/Tutorials/First_PI
```
````
`````
``````

Save and close this file, and if needed, run the `direnv allow` command if you
get a warning that the file is still blocked.

## Updating basic software in the `direnv` environment

In the step above, one of the last commands in the `.envrc` file was `layout
python`. 

````{card}
File: `.envrc`
^^^
```{code-block} shell
---
linenos: True
emphasize-lines: 11
---
# Some computer specific commands appear here
# such as:
# module load git
# or
# module load cdo
# or
# module load python
#
# Set up a project structure for Python where the esm-tools will be
# installed:
layout python
# export the PROJECT_BASE variable for easy use to get back to the top of the
# experiment folder
export PROJECT_BASE=...
```
````

This tells `direnv` to prepare a [virtual
environment](https://realpython.com/python-virtual-environments-a-primer/) for
use with Python programs that are installed **in this directory**. Once you
leave the directory, packages and versions will revert back to your defaults.

Regardless of the computer you are on, there are a few good commands to run
once the virtual environment has been prepared to ensure everything is working
correctly:

````{card}
Terminal
^^^
```
$ which -a python
$ pip install -U pip
$ pip install wheel
```
````
First, we check which `python` is being used. The one in your `${PROJECT_BASE}`
folder should be at the front of this list. Next, we update the `pip` program,
which is used to install Python packages, and finally we install the `wheel`
program, which allows for pre-compiled binaries of Python packages to be used,
if they are available. This saves some time during the install process.

Once these steps are finished, you can install the `esm-tools`.

## Installing the `esm-tools` inside of the `direnv` environment

First, we need to download the `esm-tools` package. While you can do this
anywhere inside of the `${PROJECT_BASE}` folder, it is nice to know where
things come from. One way to do this, is to mirror the web address of the
download command as closely as possible.

````{card}
Terminal
^^^
```
$ git clone https://github.com/esm-tools/esm_tools software/github.com/esm-tools/esm_tools
```
````

Now, you can install the package:
````{card}
Terminal
^^^
```
$ cd software/github.com/esm-tools/esm_tools
$ ./install.sh
```
````

If everything worked correctly, you should now have the following programs:
``````{tab-set}
`````{tab-item} DKRZ Levante
````{card}
Terminal
^^^
```shell
$ which -a esm_tools
# /work/ab0246/a270077/SciComp/Model_Support/AWIESM/Tutorials/First_PI/.direnv/python-3.9.9/bin/esm_tools
$ which -a esm_master
# /work/ab0246/a270077/SciComp/Model_Support/AWIESM/Tutorials/First_PI/.direnv/python-3.9.9/bin/esm_master
$ which -a esm_runscripts
# /work/ab0246/a270077/SciComp/Model_Support/AWIESM/Tutorials/First_PI/.direnv/python-3.9.9/bin/esm_runscripts
```
````
```{note}
The output you have for `which -a` may be longer if you have installed the
`esm-tools` in more than one location visible to your ${PATH}!
```
`````
``````

## Compiling the model

You are now ready to compile your model. Go back to the main project folder:
````{card}
Terminal
^^^
```shell
$ cd ${PROJECT_BASE}
```
````
Here, we will create a `model_codes` folder and enter it:
````{card}
Terminal
^^^
```shell
$ mkdir model_codes
$ cd model_codes
```
````
This is where we will keep all of our model source code for this project. You
can now use the `esm_master` command to list available models:

````{card}
Terminal
^^^
```shell
$ esm_master
```
````

To download and compile `awiesm-2.1`, you can do:
````{card}
Terminal
^^^
```shell
$ esm_master install-awiesm-2.1
```
````

```{warning}
Unless you have configered for password-less access, you will be asked to enter
your password a few times! Please make sure to check which username and
password is being asked for! For `awiesm-2.1` git *should* request the
`gitlab.awi.de` credentials
```

After downloading the source code, the model will compile. This can take a
while (between 15 and 20 minutes).

## Customizing a Run Configuration

## Submitting your first job

At this point, you are ready to submit your first `AWI-ESM 2.1` simulation on
Levante! We will do this in two steps:

````{card}
Terminal
^^^
```
$ cd ${PROJECT_BASE}/run_configs
$ esm_runscripts my_first_pi.yaml -e tutorial_pi_001 -c 
```
+++
Above, the first argument is your run configuration file, then, `-e` for
experiment ID followed by a string for the name of your experiment, and finally
`-c` to launch in "check" mode.
````


