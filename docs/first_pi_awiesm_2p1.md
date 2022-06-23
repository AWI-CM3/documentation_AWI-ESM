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

```{hint}
Some content will be in tabs. You should select which computer you want to work on.
```

````{admonition} Notation
In this documentation, you will sometimes see nested yaml entries in "dot" notation. Thiat means that `a.b: foo` would look like this in `yaml`:
```yaml
a:
  b: foo
```
````

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

``````{tab-set}
`````{tab-item} DKRZ Levante
```{note}
These instructions are specific to DKRZ's Levante System!
```
````{card}
Terminal
^^^
```
$ export PROJECT_BASE=/work/ab0246/a270077/SciComp/Model_Support/AWIESM/Tutorials/First_PI
$ mkdir -p $PROJECT_BASE
$ cd $PROJECT_BASE
```
````
`````
`````{tab-item} AWI Ollie
```{note}
These instructions are specific to AWI's Ollie System!
```
````{card}
Terminal
^^^
```
$ export PROJECT_BASE=/work/ollie/pgierz/SciComp/Model_Support/AWIESM/Tutorials/First_PI
$ mkdir -p $PROJECT_BASE
$ cd $PROJECT_BASE
```
````
`````
``````

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
`````{tab-item} AWI Ollie
```{note}
These instructions are specific to AWI's Ollie System!
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
# Common climate tools: cdo
module load cdo
# Set up a project structure for Python where the esm-tools will be
# installed:
layout python
# export the PROJECT_BASE variable for easy use to get back to the top of the
# experiment folder
export PROJECT_BASE=/work/ollie/pgierz/SciComp/Model_Support/AWIESM/Tutorials/First_PI
```
````
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
things come from. One way to do this is to mirror the web address of the
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
`````
`````{tab-item} AWI Ollie
````{card}
Terminal
^^^
```shell
$ which -a esm_tools
# /work/ollie/pgierz/SciComp/Model_Support/AWIESM/Tutorials/First_PI/.direnv/python-3.7.7/bin/esm_tools
$ which -a esm_master
# /work/ollie/pgierz/SciComp/Model_Support/AWIESM/Tutorials/First_PI/.direnv/python-3.7.7/bin/esm_master
$ which -a esm_runscripts
# /work/ollie/pgierz/SciComp/Model_Support/AWIESM/Tutorials/First_PI/.direnv/python-3.7.7/bin/esm_runscripts
```
````
`````
``````

```{note}
The output you have for `which -a` may be longer if you have installed the
`esm-tools` in more than one location visible to your ${PATH}!
```

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
Unless you have configured for password-less access, you will be asked to enter
your password a few times! Please make sure to check which username and
password is being asked for! For `awiesm-2.1` git *should* request the
`gitlab.awi.de` credentials
```

After downloading the source code, the model will compile. This can take a
while (between 15 and 20 minutes).

## Customizing a Run Configuration

There are several run configurations available for you to use which are already
pre-shipped with the `esm-tools`. In your
`${PROJECT_BASE}/software/github.com/esm-tools/esm_tools` directory you will
find a folder labeled `runscripts`. There are basic examples for most models
that are supported in the `esm-tools` framework. The example run script we will
take comes from a subfolder `CI` (continuous integration), where the
`esm-tools` team stores run configurations which are benchmarked and tested
automatically.

To start, generate a folder to store your run configuration files:

````{card}
Terminal
^^^
```
$ mkdir -p ${PROJECT_BASE}/run_configs
```
````

Next, copy the template file to your `run_configs` folder.

``````{tab-set}
`````{tab-item} DKRZ Levante
````{card}
Terminal
^^^
```shell
$ cp ${PROJECT_BASE}/software/github.com/esm-tools/esm_tools/runscripts/CI/awiesm/levante/pi.yaml ${PROJECT_BASE}/run_configs/my_first_pi.yaml 
```
````
Here we only need to change out two things for the runscript to work. 
````{card}
File: `${PROJECT_BASE}/run_configs/my_first_pi.yaml`
^^^
```diff
 general:
 ...
     compute_time: "00:20:00"
     initial_date: "1850-01-01T00:00:00"       # Initial exp. date
     final_date: "1850-04-01T00:00:00"         # Final date of the experiment
-    project_base: "/work/ab0246/a270077/SciComp/Model_Support/awiesm_porting/"
+    project_base: !ENV ${PROJECT_BASE}
     base_dir: "${general.project_base}/experiments"
     nmonth: 1
     nyear: 0
 ...
     clean_old_rundirs_except: 2
     clean_old_rundirs_keep_every: 25
     version: "2.1"
+    account: "ab0246"
     model_dir: "${general.project_base}/model_codes/awiesm-2.1"
 ...

 jsbach:
+   dynamic_vegeations: True

 fesom:
     restart_rate: 1
     restart_unit: "m"
     restart_first: 1
     namelist_dir: "${general.project_base}/model_codes/awiesm-2.1/fesom-2.1/config"
+    mesh_dir: "/work/ab0246/pool/FESOM2/MESHES/mesh_CORE2_finaltopo_mean/"
```
````
```{note}
The above difference is truncated!
```

The first difference, in `general.project_base`, tells the `esm-tools` where it
will store experiments. Here we use the special `!ENV` tag to extract a shell
environmental variable, in this case `${PROJECT_BASE}`. In our case, the
`${PROJECT_BASE}` variable is set up for us with `direnv`, as shown above in
the Preliminaries section.

The second difference is unique to Levante, and sets up the job's `slurm`
accounting, which informs the batch system where to deduct node-hours from
while running your simulation. You will need to add an entry for
`general.account: ab0246`. You can substitute the computing account for
another Levante resource allocation if you want. That translates to the
following in the submitted run script which `slurm` gets:
```
#SBATCH --account=ab0246
```
Next, we explicitly turn on dynamic vegetation. Note that the key is indeed
plural: `jsbach.dynamic_vegetations`. This is still a legacy spelling and will
be renamed more sensibly in the future.

Finally, as the pool on Levante is not yet standardized, we define the group
directory for AWI Paleoclimate Dynamics as the mesh pool.
`````



`````{tab-item} AWI Ollie
````{card}
Terminal
^^^
```shell
$ cp ${PROJECT_BASE}/software/github.com/esm-tools/esm_tools/runscripts/CI/awiesm/ollie/pi.yaml ${PROJECT_BASE}/run_configs/my_first_pi.yaml 
```
````
There are no changes needed to the template runscript for Ollie; all user
specific settings are already present in the environment thanks to `direnv`.
`````
``````



## Submitting your first job

At this point, you are ready to submit your first `AWI-ESM 2.1` simulation! We
will do this in two steps. First, we will ensure that the "check mode" runs
through correctly. This ascertains that all files needed for the simulation exist,
and will generate a preliminary folder structure for you to examine before
submitting the actual run to the supercomputer:

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

If the check mode works correctly, you should see:

``````{tab-set}
`````{tab-item} DKRZ Levante
````{card}
Terminal
^^^
```
First year, checking if we need to compile...

================================================================================
STARTING SIMULATION JOB!
Experiment ID = tutorial_pi_001
Setup = awiesm
This setup consists of:
- echam
- fesom
- oasis3mct
- jsbach
- hdmodel
Experiment is installed in:
       /work/ab0246/a270077/SciComp/Model_Support/AWIESM/Tutorials/First_PI/experiments/tutorial_pi_001
================================================================================

- NOTE: removing the variable: OpbndPath from the namelist: namelist.config
- NOTE: removing the variable: TideForcingPath from the namelist: namelist.config
- NOTE: removing the variable: ForcingDataPath from the namelist: namelist.config
- NOTE: removing the variable: K_gm from the namelist: namelist.oce
- NOTE: removing the variable: gethd from the namelist: namelist.jsbach
- NOTE: removing the variable: puthd from the namelist: namelist.jsbach
tasks: 256
nproc: 288
tasks: 544
resubmit tasks: 544
Actually not submitting anything, this job preparation was launched in 'check' mode (-c).
```
````
`````
`````{tab-item} AWI Ollie
````{card}
Terminal
^^^
```
Copying standard yamls from:  /work/ollie/pgierz/SciComp/Model_Support/AWIESM/Tutorials/First_PI/software/github.com/esm-tools/esm_tools/configs/
Copying standard namelists from:  /work/ollie/pgierz/SciComp/Model_Support/AWIESM/Tutorials/First_PI/software/github.com/esm-tools/esm_tools/namelists/
/work/ollie/pgierz/SciComp/Model_Support/AWIESM/Tutorials/First_PI/run_configs/my_first_pi.yaml
First year, checking if we need to compile...

================================================================================
STARTING SIMULATION JOB!
Experiment ID = tutorial_pi_001
Setup = awiesm
This setup consists of:
- echam
- fesom
- oasis3mct
- jsbach
- hdmodel
Experiment is installed in:
       /work/ollie/pgierz/SciComp/Model_Support/AWIESM/Tutorials/First_PI/experiments/tutorial_pi_001
================================================================================

- NOTE: removing the variable: OpbndPath from the namelist: namelist.config
- NOTE: removing the variable: TideForcingPath from the namelist: namelist.config
- NOTE: removing the variable: ForcingDataPath from the namelist: namelist.config
- NOTE: removing the variable: K_gm from the namelist: namelist.oce
- NOTE: removing the variable: gethd from the namelist: namelist.jsbach
- NOTE: removing the variable: puthd from the namelist: namelist.jsbach
tasks: 576
nproc: 288
tasks: 864
resubmit tasks: 864
Actually not submitting anything, this job preparation was launched in 'check' mode (-c).
```
````
`````
``````

Finally, you can remove the `-c` flag and submit your simulation.
