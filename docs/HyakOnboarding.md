# Hyak storage

`$HOME`  is located at `/mmfs1/home/USERID/` and has the most limited storage, at 10 GB.  
The "research lab" directory is located at `/mmfs1/gscratch/ssmc/`.  Sub-directories of this root-directory are the best places for installing miniconda3 environments and storing data. 
SSMC has a disk quota of 92 TB and has used (as of this file creation) 60 TB.  This storage is communal.  

Use command `hyakstorage` to get more information on `home` and `"research lab"` storage

Installing conda environments to `$HOME` doesn't work due to disk limits (see, e.g. [Hyak Miniconda3 instructions](https://hyak.uw.edu/docs/tools/python/)), so the installation has to go to `/mmfs1/gscratch/ssmc/USRS/PSI/` subdirectories.  The way that I was able to get this work (and I tried a few ways!) was to add a line to `.bashrc` that defines `$HOME` as the subdirectory of `/mmfs1/gscratch/ssmc/USRS/PSI/` in which `.conda` directory will reside.  For example:
```
HOME="/mmfs1/gscratch/ssmc/USRS/PSI/Rachael"
```
The install instructions below only worked when this line was added to my `/mmfs1/home/USERID/.bashrc` (i.e. `/mmfs1/home/rdmseas/.bashrc`).  What this means is that `~/` will point to `/mmfs1/gscratch/ssmc/USRS/PSI/Rachael`.  As such, I also added an alias to allow me to easily navigate to my login (and former $HOME) directory `/mmfs1/home/rdmseas/`.
```
alias cdh='cd /mmfs1/home/rdmseas'
```

# Install Miniconda3 in lab workspace
We install miniconda in the lab workspace because there isn't enough disk space to accomodate miniconda environments in `$HOME`.  See [Hyak Miniconda3 instructions](https://hyak.uw.edu/docs/tools/python/) for more details.  

Navigate to lab workspace
```
$ cd /mmfs1/gscratch/ssmc/PSI/
```
create a personal folder and change directory to this directory
```
$ mkdir your-personal-folder-name
$ cd your-personal-folder-name
```
Download miniconda3 in this directory
```
$ wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```
The output will look something like: 
```
--2022-05-19 10:14:24--  https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
Resolving repo.anaconda.com (repo.anaconda.com)... 104.16.131.3, 104.16.130.3, 2606:4700::6810:8303, ...
Connecting to repo.anaconda.com (repo.anaconda.com)|104.16.131.3|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 76607678 (73M) [application/x-sh]
Saving to: ‘Miniconda3-latest-Linux-x86_64.sh’

Miniconda3-latest-L 100%[===================>]  73.06M   232MB/s    in 0.3s    

2022-05-19 10:14:25 (232 MB/s) - ‘Miniconda3-latest-Linux-x86_64.sh’ saved [76607678/76607678]
```
Install miniconda3 to this directory
```
$ bash Miniconda3-latest-Linux-x86_64.sh -b -p ./miniconda3
```
My output looked like:
```
PREFIX=/mmfs1/gscratch/ssmc/USRS/PSI/Rachael/miniconda3
Unpacking payload ...
Collecting package metadata (current_repodata.json): done                       
Solving environment: done

## Package Plan ##

  environment location: /mmfs1/gscratch/ssmc/USRS/PSI/Rachael/miniconda3

  added / updated specs:
    - _libgcc_mutex==0.1=main
    - _openmp_mutex==4.5=1_gnu
    - brotlipy==0.7.0=py39h27cfd23_1003
    - ca-certificates==2022.3.29=h06a4308_1
    - certifi==2021.10.8=py39h06a4308_2
    - cffi==1.15.0=py39hd667e15_1
    - charset-normalizer==2.0.4=pyhd3eb1b0_0
    - colorama==0.4.4=pyhd3eb1b0_0
    - conda-content-trust==0.1.1=pyhd3eb1b0_0
    - conda-package-handling==1.8.1=py39h7f8727e_0
    - conda==4.12.0=py39h06a4308_0
    - cryptography==36.0.0=py39h9ce1e76_0
    - idna==3.3=pyhd3eb1b0_0
    - ld_impl_linux-64==2.35.1=h7274673_9
    - libffi==3.3=he6710b0_2
    - libgcc-ng==9.3.0=h5101ec6_17
    - libgomp==9.3.0=h5101ec6_17
    - libstdcxx-ng==9.3.0=hd4cf53a_17
    - ncurses==6.3=h7f8727e_2
    - openssl==1.1.1n=h7f8727e_0
    - pip==21.2.4=py39h06a4308_0
    - pycosat==0.6.3=py39h27cfd23_0
    - pycparser==2.21=pyhd3eb1b0_0
    - pyopenssl==22.0.0=pyhd3eb1b0_0
    - pysocks==1.7.1=py39h06a4308_0
    - python==3.9.12=h12debd9_0
    - readline==8.1.2=h7f8727e_1
    - requests==2.27.1=pyhd3eb1b0_0
    - ruamel_yaml==0.15.100=py39h27cfd23_0
    - setuptools==61.2.0=py39h06a4308_0
    - six==1.16.0=pyhd3eb1b0_1
    - sqlite==3.38.2=hc218d9a_0
    - tk==8.6.11=h1ccaba5_0
    - tqdm==4.63.0=pyhd3eb1b0_0
    - tzdata==2022a=hda174b7_0
    - urllib3==1.26.8=pyhd3eb1b0_0
    - wheel==0.37.1=pyhd3eb1b0_0
    - xz==5.2.5=h7b6447c_0
    - yaml==0.2.5=h7b6447c_0
    - zlib==1.2.12=h7f8727e_1


The following NEW packages will be INSTALLED:

  _libgcc_mutex      pkgs/main/linux-64::_libgcc_mutex-0.1-main
  _openmp_mutex      pkgs/main/linux-64::_openmp_mutex-4.5-1_gnu
  brotlipy           pkgs/main/linux-64::brotlipy-0.7.0-py39h27cfd23_1003
  ca-certificates    pkgs/main/linux-64::ca-certificates-2022.3.29-h06a4308_1
  certifi            pkgs/main/linux-64::certifi-2021.10.8-py39h06a4308_2
  cffi               pkgs/main/linux-64::cffi-1.15.0-py39hd667e15_1
  charset-normalizer pkgs/main/noarch::charset-normalizer-2.0.4-pyhd3eb1b0_0
  colorama           pkgs/main/noarch::colorama-0.4.4-pyhd3eb1b0_0
  conda              pkgs/main/linux-64::conda-4.12.0-py39h06a4308_0
  conda-content-tru~ pkgs/main/noarch::conda-content-trust-0.1.1-pyhd3eb1b0_0
  conda-package-han~ pkgs/main/linux-64::conda-package-handling-1.8.1-py39h7f8727e_0
  cryptography       pkgs/main/linux-64::cryptography-36.0.0-py39h9ce1e76_0
  idna               pkgs/main/noarch::idna-3.3-pyhd3eb1b0_0
  ld_impl_linux-64   pkgs/main/linux-64::ld_impl_linux-64-2.35.1-h7274673_9
  libffi             pkgs/main/linux-64::libffi-3.3-he6710b0_2
  libgcc-ng          pkgs/main/linux-64::libgcc-ng-9.3.0-h5101ec6_17
  libgomp            pkgs/main/linux-64::libgomp-9.3.0-h5101ec6_17
  libstdcxx-ng       pkgs/main/linux-64::libstdcxx-ng-9.3.0-hd4cf53a_17
  ncurses            pkgs/main/linux-64::ncurses-6.3-h7f8727e_2
  openssl            pkgs/main/linux-64::openssl-1.1.1n-h7f8727e_0
  pip                pkgs/main/linux-64::pip-21.2.4-py39h06a4308_0
  pycosat            pkgs/main/linux-64::pycosat-0.6.3-py39h27cfd23_0
  pycparser          pkgs/main/noarch::pycparser-2.21-pyhd3eb1b0_0
  pyopenssl          pkgs/main/noarch::pyopenssl-22.0.0-pyhd3eb1b0_0
  pysocks            pkgs/main/linux-64::pysocks-1.7.1-py39h06a4308_0
  python             pkgs/main/linux-64::python-3.9.12-h12debd9_0
  readline           pkgs/main/linux-64::readline-8.1.2-h7f8727e_1
  requests           pkgs/main/noarch::requests-2.27.1-pyhd3eb1b0_0
  ruamel_yaml        pkgs/main/linux-64::ruamel_yaml-0.15.100-py39h27cfd23_0
  setuptools         pkgs/main/linux-64::setuptools-61.2.0-py39h06a4308_0
  six                pkgs/main/noarch::six-1.16.0-pyhd3eb1b0_1
  sqlite             pkgs/main/linux-64::sqlite-3.38.2-hc218d9a_0
  tk                 pkgs/main/linux-64::tk-8.6.11-h1ccaba5_0
  tqdm               pkgs/main/noarch::tqdm-4.63.0-pyhd3eb1b0_0
  tzdata             pkgs/main/noarch::tzdata-2022a-hda174b7_0
  urllib3            pkgs/main/noarch::urllib3-1.26.8-pyhd3eb1b0_0
  wheel              pkgs/main/noarch::wheel-0.37.1-pyhd3eb1b0_0
  xz                 pkgs/main/linux-64::xz-5.2.5-h7b6447c_0
  yaml               pkgs/main/linux-64::yaml-0.2.5-h7b6447c_0
  zlib               pkgs/main/linux-64::zlib-1.2.12-h7f8727e_1


Preparing transaction: done
Executing transaction: done
installation finished.
```
Initialize `bash` shell

```
$ module load foster/python/miniconda/3.8
$ conda init bash
no change     /sw/contrib/foster-src/python/miniconda/3.8/condabin/conda
no change     /sw/contrib/foster-src/python/miniconda/3.8/bin/conda
no change     /sw/contrib/foster-src/python/miniconda/3.8/bin/conda-env
no change     /sw/contrib/foster-src/python/miniconda/3.8/bin/activate
no change     /sw/contrib/foster-src/python/miniconda/3.8/bin/deactivate
no change     /sw/contrib/foster-src/python/miniconda/3.8/etc/profile.d/conda.sh
no change     /sw/contrib/foster-src/python/miniconda/3.8/etc/fish/conf.d/conda.fish
no change     /sw/contrib/foster-src/python/miniconda/3.8/shell/condabin/Conda.psm1
no change     /sw/contrib/foster-src/python/miniconda/3.8/shell/condabin/conda-hook.ps1
no change     /sw/contrib/foster-src/python/miniconda/3.8/lib/python3.8/site-packages/xontrib/conda.xsh
no change     /sw/contrib/foster-src/python/miniconda/3.8/etc/profile.d/conda.csh
modified      /mmfs1/home/rdmseas/.bashrc

==> For changes to take effect, close and re-open your current shell. <==
```
Log out and log back in. Now `(base)` shows up at the base of terminal, indicated that miniconda3 is active.

Next: keep miniconda3 deactivated until called
```
conda config --set auto_activate_base false
```
use `conda activate` before each use and unload it with `conda deactivate`

# create a miniconda environment using a Yaml file
Yaml files are great because they use a data-oriented language structure that allows for useful, easy-for-humans-to-read structure that is also machine readable. 

Create a `envs` folder to contain environment.yaml files
```
$ mkdir envs
```
Add Yaml file to `envs` directory.  Mine is called `klone-jupyter.yml` and includes packages that I anticipate needing to use for my project.  The nice thing about using a Yaml file for creating an environment is that there is a clear record of the packages used and the file can be modified down the road, as needed.  This is what I have inside `klone-jupyter.yml`.
```
$ more klone-jupyter.yml 
# virtualenv environment description for a useful jupyter
# environment on Klone
#
# Create a virtualenv containing these packages with:
#
#    module load foster/python/miniconda/3.8
#    conda env create -f /gscratch/ssmc/USRS/PSI/Rachael/envs/klone-jupyter.yml 
#
# To activate this environment use:
#    conda activate klone-jupyter
# 
# Deactivate with:
#    conda deactivate
#
# Delete environment using:
#    conda env remove -klone_jupyter
#

name: klone_jupyter

channels:
 - conda-forge
 - defaults

dependencies:
 - pyaml
 - cmocean
 - jupyterlab
 - matplotlib
 - scipy
 - cartopy
 - netCDF4
 - xarray
 - geopandas
```

From within `envs` folder, we can install this environment as follows
```
$ module load foster/python/miniconda/3.8
$ conda env create -f ./klone-jupyter.yml
```
It took a while (~10 minutes) to get through `Solving environment:` mode.  Afterwards, the environment packages and versions were listed out.  The list looked like:
```
$ conda env create -f ./klone-jupyter.yml
Collecting package metadata (repodata.json): done
Solving environment: done


==> WARNING: A newer version of conda exists. <==
  current version: 4.10.3
  latest version: 4.12.0

Please update conda by running

    $ conda update -n base -c defaults conda



Downloading and Extracting Packages
freetype-2.10.4      | 890 KB    | ##################################### | 100% 
tornado-6.1          | 657 KB    | ##################################### | 100% 

...[and many more!]...

readline-8.1         | 295 KB    | ################################################ | 100% 
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
#
# To activate this environment, use
#
#     $ conda activate klone_jupyter
#
# To deactivate an active environment, use
#
#     $ conda deactivate
```

# Initiate a remote Jupyterlab session
Jupyterlab provides an ability to plot up model results and develop methods visualizing model output.  Opening a visual session requires that we set up our local system with an `SSH` connection to `HYAK`.  Setting up an `SSH` connection is a bit tedious, though.  Here are steps to make it more functional.  

From the wiki: "Mox nodes have 28, 32 or 40 cores. Ask the experienced members of your Hyak group about the number of cores for  the nodes in your group."  Need information on Hyak. 
### Create an alias to initiate an interactive node
Create a shell script in your root directory (I chose `/mmfs1/home/rdmseas`) that specifies account and partition.  Use `hyakalloc` to see which account and partition apply to your personal account.  Mine are `ssmc` and `compute`.  The `$1` for `time` indicates that the first variable passed in will be the requested time allocation.  Here, I just select 1 node and 1 CPU with ~5GB memory (in multiples of 1280). 
```
#!/bin/bash

# activate a compute node
salloc --time=$1 --ntasks=1 --cpus-per-task=1 --mem-per-cpu=5120M --account=ssmc -p compute
```
I then use an alias to call this shell script.  The alias is in my `.bashrc` file.  I have three aliases with the three most common compute window sessions that I like to use. 
```
alias allocate130='bash /mmfs1/home/rdmseas/./startallocation.sh 1:30:00'
alias allocate1='bash /mmfs1/home/rdmseas/./startallocation.sh 1:00:00'
alias allocate2='bash /mmfs1/home/rdmseas/./startallocation.sh 2:00:00'
```
Once this is setup (and `source .bashrc` is run or terminal re-started) then an interactive node for 1-hour can be started with this alias
```
(base) [rdmseas@klone1 rdmseas]$ allocate1
salloc: Granted job allocation 4618953
salloc: Waiting for resource configuration
salloc: Nodes n3012 are ready for job
bash-4.4$ 
```
Now we are ready to start a JupyterLab session without overloading the login node and making others mad.  :-) 

### Create an alias to initiate a JupyterLab session
Create a shell script to initiate a JupyterLab session.  Mine is called `klone_jupyter.sh` and looks like:
```
#!/bin/bash

# load modules and jupyter environment
module load cesg/python/3.8.10
# UPDATE PATH BELOW WITH YOUR USER PATH
source /mmfs1/gscratch/ssmc/USRS/PSI/USERID/miniconda3/etc/profile.d/conda.sh
conda activate klone_jupyter

# activate remote login
jupyter lab --no-browser --ip $(hostname -f)
```
NOTE: the `klone_jupyter` environment name used here matched the name used in the environment YAML files
```
name: klone_jupyter
```
Change it to match whichever environment name you have.  

Create an alias to call this shell script.  This is mine:
```
alias startjupyter='bash /mmfs1/home/rdmseas/./klone_jupyter.sh'
```
Use the alias shortcut to initiate the JupyterLab session, e.g.
```
$ startjupyter
```
The output will look something like: 
```
(klone_jupyter) bash-4.4$ jupyter lab --no-browser --ip $(hostname -f)
[I 2022-05-20 15:21:45.624 ServerApp] jupyterlab | extension was successfully linked.
[I 2022-05-20 15:21:45.635 ServerApp] nbclassic | extension was successfully linked.
[I 2022-05-20 15:21:45.653 ServerApp] Writing Jupyter server cookie secret to /mmfs1/gscratch/ssmc/USRS/PSI/Rachael/.local/share/jupyter/runtime/jupyter_cookie_secret
[I 2022-05-20 15:21:48.001 ServerApp] notebook_shim | extension was successfully linked.
[I 2022-05-20 15:21:48.163 ServerApp] notebook_shim | extension was successfully loaded.
[I 2022-05-20 15:21:48.165 LabApp] JupyterLab extension loaded from /mmfs1/gscratch/ssmc/USRS/PSI/Rachael/.conda/envs/klone_jupyter/lib/python3.10/site-packages/jupyterlab
[I 2022-05-20 15:21:48.165 LabApp] JupyterLab application directory is /mmfs1/gscratch/ssmc/USRS/PSI/Rachael/.conda/envs/klone_jupyter/share/jupyter/lab
[I 2022-05-20 15:21:48.171 ServerApp] jupyterlab | extension was successfully loaded.
[I 2022-05-20 15:21:48.203 ServerApp] nbclassic | extension was successfully loaded.
[I 2022-05-20 15:21:48.204 ServerApp] Serving notebooks from local directory: /mmfs1/home/rdmseas
[I 2022-05-20 15:21:48.204 ServerApp] Jupyter Server 1.17.0 is running at:
[I 2022-05-20 15:21:48.204 ServerApp] http://n3012.hyak.local:8888/lab?token=03324ebd47d67438d610da6044be0c1b10147525a40b4767
[I 2022-05-20 15:21:48.204 ServerApp]  or http://127.0.0.1:8888/lab?token=03324ebd47d67438d610da6044be0c1b10147525a40b4767
[I 2022-05-20 15:21:48.204 ServerApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 2022-05-20 15:21:48.215 ServerApp] 
    
    To access the server, open this file in a browser:
        file:///mmfs1/gscratch/ssmc/USRS/PSI/Rachael/.local/share/jupyter/runtime/jpserver-62970-open.html
    Or copy and paste one of these URLs:
        http://n3012.hyak.local:8888/lab?token=03324ebd47d67438d610da6044be0c1b10147525a40b4767
     or http://127.0.0.1:8888/lab?token=03324ebd47d67438d610da6044be0c1b10147525a40b4767
```
The most important bits from the above is the computer and port address `n3012.hyak.local:8888` and the key `03324ebd47d67438d610da6044be0c1b10147525a40b4767`.  We used these in a `ssh` call from a local computer as follows (using my USERID "rdmseas").
```
ssh -N -L localhost:8800:n3012.hyak.local:8888 rdmseas@klone.hyak.uw.edu
```
In this call, I'm connecting my local system port `8800` to HYAK's port `8888` using `localhost:8800:n3012.hyak.local:8888`, followed by my login.  The system will prompt for password and two-factor login authentication. Once the password and authentication is provided, open a new browser window and use `localhost:8800` as the web address.  Et voila!  Compute away.  

# In summary: Creating a JupyterLab session on Klone
Login to Klone and type the following three shortcuts at the command line to initialize an interactive node and to start a JupyterLab session.  For example, for a 2-hour coding session, type:
```
$ allocate2
$ cda
$ startjupyter
```
Where `cda` is my `alias-to-directory-where-notebooks-are-located`, i.e., in my `.bashrc` I've specified
```
alias cda='cd /mmfs1/gscratch/ssmc/USRS/PSI/Rachael/Projects'
```
Something that I've noticed on `Klone` is that my `.bashrc` isn't automatically initialized when I start an interactive node.  In this case, the setup requires four command lines:
```
$ allocate2
$ source .bashrc
$ cda
$ startjupyter
```
Happy computing! 

# `.bashrc` setup
Here is an overview of useful bits in my `.bashrc` file
```
# ~~~ SYSTEM CONFIG ~~~
# Point $HOME to lab directory for conda environment installations
HOME="/mmfs1/gscratch/ssmc/USRS/PSI/Rachael"

# ~~~ ALIASES ~~~
alias cdw='cd /mmfs1/gscratch/ssmc/USRS/PSI/Rachael'
alias cda='cd /mmfs1/gscratch/ssmc/USRS/PSI/Rachael/Projects'
alias cdh='cd /mmfs1/home/rdmseas'
alias sukscripts='cd /mmfs1/gscratch/ssmc/USRS/PSI/Sukyong/script'
alias ssmoutputs='cd /mmfs1/gscratch/ssmc/USRS/PSI/Adi/BS_WQM/2014_SSM4_WQ_exist_orig/hotstart/o
utputs'

# interactive node
alias allocate130='bash /mmfs1/home/rdmseas/./startallocation.sh 1:30:00'
alias allocate1='bash /mmfs1/home/rdmseas/./startallocation.sh 1:00:00'
alias allocate2='bash /mmfs1/home/rdmseas/./startallocation.sh 2:00:00'

# jupyter
alias startjupyter='bash /mmfs1/home/rdmseas/./klone_jupyter.sh'

# utils
alias du="du -h"
alias ls="ls --color=auto -F"
alias grep="grep --color=auto"
alias df="df -h"
alias la="ls -a"
alias rm="rm -i"
alias ll="ls -al"

```