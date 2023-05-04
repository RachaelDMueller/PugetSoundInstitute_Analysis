# Table of contents
1. [Introduction](#intro)
1. [Setup](#setup)
    1. [System requirements](#requirements)
    2. [Creating a configuration file](#configuration)
2. [Tables](#tables)
    1. [Non-compliance by region (days, volume, and percent volume)](#noncompliantTable)
    2. [Dissolved Oxygen below 2, 5, and DO standard by region](#threshold)
3. [Graphics](#graphics)
    1. [Time series of volume noncompliant](#noncompliantTS)
        - [Individual time series](#noncompliantTS_individual)
        - [5-panel time series compiliations](#noncompliantTS_5panel)
    2. [Nutrient Loadings (WWTP and rivers)](#nutrientLoading)
4. [Animations](#movies)
    1. [Salinity, N03, and/or DOXG (map-style)](#moviesConc)
    2. [Hypoxic cells (DO < 2 mg/l, map-style)](#moviesHypoxia)
    3. [Percent volume of cell that is hypoxic (DO < 2 mg/l, map-style)](#moviesPercentHypoxic)
    4. [Dissolved oxygen noncompliant (scenario - reference < -0.2 or -0.25)](#moviesNonComplaint)
5. [Reference links](#references)

# Introduction <a name="intro"></a>
The goal of this file is to provide an overview of the setup and resources required to develop the tables, graphics, and animations provided to King County for the evaluation of nutrient loading impacts. It is currently in development.  Please email [Rachael Mueller](mailto:rdmseas@uw.edu) with comments, suggestions, and/or corrections.  

# Setup <a name="setup"></a>
## Requirements <a name="requirements"></a>
1. A miniconda environment in which to run scripts and notebooks.  My miniconda environment file (jupyter-klone.yaml) looks like:
```
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
2. An Apptainer "Container" in which to run FFMPEG.  See, FFMPEG section of [HyakOnboarding.md](https://github.com/RachaelDMueller/KingCounty-Rachael/blob/main/docs/HyakOnboarding.md#ffmpeg) in my personal Git folder (future updates will port over here). 

## Create run configuration file <a name="configuration"></a>
1. Define run and model output file locations using the `2_Scenarios Runs` tab in the OneDrive spreadsheet [Municiap model runs and scripting task list.xlsx](https://uwnetid.sharepoint.com/:x:/r/sites/og_uwt_psi/Shared%20Documents/Nutrient%20Science/9.%20Modeling/Municipal%20%20model%20runs%20and%20scripting%20task%20list.xlsx?d=w417abadac06143409d092a23a26727e6&csf=1&web=1&e=tgJY69)
2. Create [SSM_config_whidbey](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/etc/SSM_config_whidbey.ipynb) file with file paths and tag names for this set of model runs.

# Tables <a name="tables"></a>
## Create table of noncompliant (days, volume, and percent volume) <a name="noncompliantTable"></a>
The code for non-compliance uses a threshold value that can be passed in.  The default values for the `scenario - reference` difference is -0.25 mg/L, which is equivalent to the Department of Ecology (DOE) `rounding method` based on a -0.2 mg/L threshold.  See pp. 49 and 50 of Appendix F of [Optimization Report Appendix](https://www.ezview.wa.gov/Portals/_1962/Documents/PSNSRP/Appendices%20A-G%20for%20Tech%20Memo.pdf) for more details.  
1. Run [process_netcdf.py](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/py_scripts/process_netcdf.py) to generate minimum dissolved oxygen in water column and bottom level.
Change lines 27 and 231 of `process_netcdf.py` and `calc_DO_noncompliant`, respectively, to call the correct configuration file, `SSM_config_whidbey`, i.e.: 

```
with open('../etc/SSM_config_whidbey.yaml', 'r') as file:
        ssm = yaml.safe_load(file)
```
Be sure to update the number of `slurm-arrays` used in bash scripts to match the number of scenarios.  For whidbey, this is:
```
#SBATCH --array=0-9

```
## Create tables for calculating DO below 2, 5, and/or DO standard <a name="threshold"></a>
1. Change case to `whidbey` in `bash_scripts/calc_DO_below_threshold.sh`
2. I updated code to eliminate need to specify reading `SSM_config_whidbey.yaml` by hard-coding in the use of `case` 
```
with open('../etc/SSM_config_{case}.yaml', 'r') as file:
   ssm = yaml.safe_load(file)
   # get shapefile path    
   shp = ssm['paths']['shapefile']
```
3. I updated code to to use `run_tag` dictionary (ssm['run_information']['run_tag'][`whidbey`]) for column names to eliminate need to modify code by hand. (Same upgrade as in `calc_DO_noncompliant.py`)

The runtime for creating these spreadsheets (using a slurm array over three nodes, one for each spreadsheet) is: ~16 minutes

File Reference:
- [calc_DO_below_threshold.sh](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/bash_scripts/calc_DO_below_threshold.sh)
- [calc_DO_below_threshold.py](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/py_scripts/calc_DO_below_threshold.py)

# Graphics <a name="graphics"></a>
## Create time series graphics for volume noncompliant <a name="noncompliantTS"></a>
### Individual time series <a name="noncompliantTS_individual"></a>
Updated [calc_DO_noncompliant_timeseries.sh]():
```
#SBATCH --array=0-9

case="whidbey"

run_folders=(
"wqm_baseline"
"wqm_reference"
"3b"
"3e"
"3f"
"3g"
"3h"
"3i"
"3l"
"3m"
)
script_path="/mmfs1/gscratch/ssmc/USRS/PSI/Rachael/projects/KingCounty/SalishSeaModel-analysis/py_scripts/"
```
Updated python script to assign .yaml file name by `case`:
```
with open(f'../etc/SSM_config_{case}.yaml', 'r') as file:
        ssm = yaml.safe_load(file)
        # get shapefile path    
        shp = ssm['paths']['shapefile']

```
It takes ~6 minutes of computing time to create the spreadsheets.

Similarly updated code for `plot_noncompliant_timeseries`, except the shell script for this function call requires `run_tag`:
```
run_tag=(
"baseline"
"reference"
"3b"
"3e"
"3f"
"3g"
"3h"
"3i"
"3l"
"3m"
)
```
It takes ~0.1 minutes of computing time to create the graphics.

File Reference:
- [calc_DO_noncompliant_timeseries.sh](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/bash_scripts/calc_DO_noncompliant_timeseries.sh)
- [calc_DO_noncompliant_timeseries.py](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/py_scripts/calc_DO_noncompliant_timeseries.py)
- [plot_noncompliant_timeseries.sh](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/py_scripts/calc_DO_noncompliant_timeseries.py)
- [plot_noncompliant_timeseries.py](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/py_scripts/plot_noncompliant_timeseries.py)

### 5-panel time series <a name="noncompliantTS_5panel"></a>

File Reference:
- [calc_DO_noncompliant_timeseries.sh](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/bash_scripts/calc_DO_noncompliant_timeseries.sh)
- [calc_DO_noncompliant_timeseries.py](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/py_scripts/calc_DO_noncompliant_timeseries.py)
- [plot_5panel_noncompliant_timeseries.sh](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/bash_scripts/plot_5panel_noncompliant_timeseries.sh)
- [plot_5panel_noncompliant_timeseries.py](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/py_scripts/plot_5panel_noncompliant_timeseries.py)

## Nutrient Loading graphics <a name="nutrientLoading"></a>

These are still done in a Jupyter Notebook that requires quite a bit of manual editing. 
See [plot_nutrient_loading_whidbey.ipynb](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/notebooks/plot_nutrient_loading_whidbey.ipynb)

# Animations <a name="movies"></a>
All animations are created using the software `ffmpeg` through an Apptainer Container, on Hyak.  It wasn't obvious to me how to control the runtime, and I needed to do a bit of online searching and experimenting to figure out a solution.  What I found was that the `-r` flag didn't do anything.  The solution that worked for my setup was the `-framerate` flag, e.g.:
```
apptainer exec --bind ${graphics_dir} --bind ${output_dir} ~/ffmpeg.sif ffmpeg -start_number 6 -framerate 6 -i ${graphics_dir}${case}_${run_tags[${SLURM_ARRAY_TASK_ID}]}_${param}_${stat_type}_conc_${loc}_%d_whidbeyZoom.png -vcodec mpeg4 ${output_dir}${case}_${run_tags[${SLURM_ARRAY_TASK_ID}]}_${param}_${stat_type}_${loc}_whidbeyZoom.mp4
```
Here, I use `-framerate 6` to get a minute-long movie by incorporating 6 images per second from a pool of ~366 images.  Including `-vcodec mpeg4` was neeccessary for me to get the product to play on my macOS Monterey.  We ran into trouble playing the output on a PC and worked around this problem by saving to `.avi` before finding the problem was on the PC side.  In the process of troubleshooting, I also read that adding `-c:v libx264 -pix_fmt yuv420p` can make the `.mp4` more broadly accessible, but I haven't yet received confirmation that this is an important/neccessary specification.  

## Salinity, N03 and DO for the Whidbey run cases <a name="moviesConc"></a>
1. Create a sub-set of the model output that only includes information for the desired variable (e.g. `DOXG`).  A file will be created for all 3D values and can be created for surface-only and bottom-only values using the `surface_flag` and/or `bottom_flag` when calling [process_netcdf.py]().  Use [process_netcdf.sh]() for a shell script to call `process_netcdf.py` with the desired setup. The way I have the file structure setup, the files are saved to, e.g.: ```/mmfs1/gscratch/ssmc/USRS/PSI/Rachael/projects/KingCounty/data/SOG_NB/DOXG/2a_sog_river_0.5times/surface/daily_mean_DOXG_surface.nc```
<br /> A different choice that I'm considering is:<br /> ```/mmfs1/gscratch/ssmc/USRS/PSI/Rachael/projects/KingCounty/data/SOG_NB/DOXG/surface/2a_sog_river_0.5times/daily_mean_DOXG_surface.nc``` 

2. Create a set of graphics for creating movies, with daily graphics saved to, e.g.:
```
/mmfs1/gscratch/ssmc/USRS/PSI/Rachael/projects/KingCounty/graphics/SOG_NB/salinity/1e_med_sog
_wwtp_off/surface_for_movie/
```
The script takes ~15 minutes of computing time to run, with each case running in tandem on a separate node (via a slurm array), i.e., the script will take ~15 minutes to process regardless of the number of cases. 

## Hypoxia (DO < 2 mg/l) <a name="moviesHypoxia"></a>
1. [calc_DO_below_threshold.sh](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/bash_scripts/calc_DO_below_threshold.sh)
2. [plot_threshold_movie.sh](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/bash_scripts/plot_threshold_movie.sh)
3. [create_DO_threshold_movie.sh](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/bash_scripts/create_DO_threshold_movie.sh)

## Percent Hypoxic <a name="moviesPercentHypoxic"></a>
1. [process_netcdf_DOXG_whidbey.sh](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/bash_scripts/process_netcdf_DOXG_whidbey.sh)
2. [plot_percentVolumeHypoxic_movie.sh](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/bash_scripts/plot_percentVolumeHypoxic_movie.sh)
3. [create_percentHypoxic_movie.sh](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/bash_scripts/create_percentHypoxic_movie.sh)

## NonComplaint <a name="moviesNonComplaint"></a>
The code for non-compliance uses a threshold value that can be passed in.  The default values for the `scenario - reference` difference is -0.25 mg/L, which is equivalent to the Department of Ecology (DOE) `rounding method` based on a -0.2 mg/L threshold.  See pp. 49 and 50 of Appendix F of  [Optimization Report Appendix](https://www.ezview.wa.gov/Portals/_1962/Documents/PSNSRP/Appendices%20A-G%20for%20Tech%20Memo.pdf) for more details.
1. [calc_DO_noncompliant.sh](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/bash_scripts/calc_DO_noncompliant.sh)
2. [plot_noncompliant_movie.sh](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/bash_scripts/plot_noncompliant_movie.sh)
3. [create_DO_noncompliant_movie.sh](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/bash_scripts/create_DO_noncompliant_movie.sh)

## Concentration (DO, salinity, NO3) <a name="moviesConcentration"></a>
1. [process_netcdf_DOXG_whidbey.sh]()
    - Saves netcdf files to `/mmfs1/gscratch/ssmc/USRS/PSI/Rachael/projects/KingCounty/data/whidbey/DOXG/{SCENARIO_NAME}/{LOC}` where `{LOC}` is either wc (water column), bottom, or surface. 


# References <a name="references"></a>
1. [Municiap model runs and scripting task list.xlsx](https://uwnetid.sharepoint.com/:x:/r/sites/og_uwt_psi/Shared%20Documents/Nutrient%20Science/9.%20Modeling/Municipal%20%20model%20runs%20and%20scripting%20task%20list.xlsx?d=w417abadac06143409d092a23a26727e6&csf=1&web=1&e=tgJY69)
2. [Whidbey configuration file](https://github.com/UWModeling/SalishSeaModel-analysis/blob/main/etc/SSM_config_whidbey.ipynb)
3. [Whidbey_Figures&Tables.xlsx](https://uwnetid.sharepoint.com/:x:/r/sites/og_uwt_psi/_layouts/15/Doc.aspx?sourcedoc=%7B9011F04E-F423-4B45-A0EA-75338168A1B3%7D&file=Whidbey_Figures%26Tables.xlsx&action=default&mobileredirect=true)
4. [SOG_NB_Figures&Tables.xlsx](https://uwnetid.sharepoint.com/:x:/r/sites/og_uwt_psi/_layouts/15/Doc.aspx?sourcedoc=%7B3788B09C-126F-40BF-86AF-22DEC185E831%7D&file=SOG_NB_Figures%26Tables.xlsx&action=default&mobileredirect=true)
