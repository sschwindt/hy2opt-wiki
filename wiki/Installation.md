Installation
============

***

- [Install *Hy2Opt*](#git_install)
- [Update *Hy2Opt*](#update)
- [Launch *Hy2Opt*](#launch)
- [Prepare file structure](#structure)
- [Program environment setup and batchfile modification](#env)
- [Requirements and dependencies](#req)
- [Logfiles](#logs)

***

<a name="started"></a>

*Hy2Opt* is based on *Python3*. The [requirements](#req) section provides more details and installation hints for external packages.
*Hy2Opt* is designed as an external application rather than a python package, but its modules can be imported as packages for external use (see the *Alternative Run* sections in the module Wikis).<br/>

***

## Installation with *Git Bash/GUI*<a name="git_install"></a>

We recommend downloading and updating *Hy2Opt* using [*Git Bash*](https://git-scm.com/downloads) (or Git GUI). Alternatively, *Hy2Opt* can be downloaded as *zip* file. However, updates are tricky when *Hy2Opt* is downloaded as *zip* file.

**Git Bash**<br/>
*GitHub* provides detailed descriptions and standard procedures to work with their repositories ([read more](https://help.github.com/en/articles/cloning-a-repository)). The following "recipe" guides through the first time installation (cloning) of *Hy2Opt:

1. Open *Git Bash*
2. Type `cd "D:/Target/Directory/"` to change to the target installation directory (recommended: `cd "D:/Python/hy2opt/"`). If the directory does not exist, it can be created in the system explorer (right-click in empty space > `New >` > `Folder`).
3. Clone the repository: `git clone https://github.com/sschwindt/hy2opt`

Done. Close *Git Bash* and start working with *Hy2Opt*.

**Git GUI**<br/>
For using *Git GUI*, open *Git GUI* and choose `Clone Existing Repository`. Enter the *Source Location*: `https://github.com/sschwindt/hy2opt`, and the *Target Directory*: `D:/Python/hy2opt` (or any other directory, but ensure that the directory does not exist). Click `Clone`. Done.

***

## Update with *Git Bash/GUI* (re- pull)<a name="update"></a>

Currently, *Git Bash* (or Git GUI) are the only options to update local copies of *Hy2Opt*. **Before updating *Hy2Opt*, we recommend to save any files and *Conditions* produced with *Hy2Opt* externally (create a save copy).**

**Git Bash**<br/>
Open *Git Bash* and do the following:

1. Go to the local *Hy2Opt* installation directory: `cd "D:/Python/hy2opt"` (or wherever *Hy2Opt* was cloned).
2. Check the current status: `git status`
3. Pull changes: `git pull`

Please note that merge errors may occur when changes were made in the local copy of *Hy2Opt*. In this case, *Git Bash* will guide through the manual merge process.

**Git GUI**<br/>
Open *Git Bash* and open `D:/Python/hy2opt` from the *Open Recent Repository* list (or wherever *Hy2Opt* is installed). If not listed, click on `Open Existing Repository` and select the *Hy2Opt* installation directory. Then:

1. Click on `Rescan`.
2. Click on the `Remote` drop-down menu > `Fetch from` > `origin`.
3. Click on the `Merge` drop-down menu > `Local Merge`.

Please note that merge errors may occur when changes were made in the local copy of *Hy2Opt*. In this case, *Git Bash* may be required to go through the manual merge process.

***

## Launch *Hy2Opt*<a name="launch"></a>

To start using *Hy2Opt*:

1. Right-click on `DRIVE:\path\to\...\hy2opt\Start_Hy2Opt.bat` and click on `Edit`.
   - In the `Start_Hy2Opt.bat` batchfile, ensure that the Python path is correctly defined according to the installed version of *ArcGIS*: `call `**EDIT>>`"%PROGRAMFILES%\Python\"`<<**`"%cd%\master.py"`.
   - Save and close `Start_Hy2Opt.bat`.
2. Double-click on `DRIVE:\path\to\...\hy2opt\Start_Hy2Opt.bat` to start *Hy2Opt*.
3. The Welcome tab opens.
4. Select the tab of the model that you want to use.

*Hy2Opt* starts in a GUI that is stored in `master.py`.

# Package structure, requirements and logfiles

***

## File structure<a name="structure"></a>

The main directory (`/hy2opt/`) contains the program launcher named `Start_Hy2Opt.bat` and the *Python3* file `master.py`. The *Hy2Opt* modules are located in sub-folders. The master folder (`/hy2opt/`) includes the following files and directories:

- **pyorigin**
  
      Contains adapted third-party Python packages and own packages
  
  - `graphics`
    
        Contains illustrations and figures.
  
  - `dialogues`    
    
        Textfiles for user dialogues.
    
    - `tf_geofile_creation.txt` contains instructions for creating *Tuflow* input geofiles.
  
  - `tf_tree`
    
        Model tree template for *Tuflow*

- **pypool** contains global *Python* functions, classes and configuration files.

- **templates4users** user templates for model input.

- **tuflow** contains routines for creating, optimizing, and running *Tuflow* models.

- **pyhi** contains the welcome tab.

- **master.py** is the main *Python* file to start *Hy2Opt*.

- **tabs.py** contains the template for model tabs.

- **Start_Hy2Opt.bat** launches `master.py`.

## Program environment setup and batchfile modification<a name="env"></a>

***

*Hy2Opt* runs with *Python3* . Before launching the *Hy2Opt* package for the first time, the `Start_Hy2Opt.bat` batchfile may require adaptions for the system environment. On **Windows** set the batch file environment as follows:

1. Right-click on `Start_Hy2Opt.bat` and choose *Edit* (*with Texteditor*) or *Open with \...* and choose a *Texteditor* software.

2. Check, and if necessary, replace the path to the good python interpreter:<br/>
   
       *Default:* `"%PROGRAMFILES%\Python\"`<br/>
       *Note:* Future versions of *Hy2Opt*'s will require `gdal` and `rasterio`. Therefore, developers recommend to create a new conda environment and to install these packages to that new environment:
   
   - ...add...

3. Save `Start_Hy2Opt.bat` and close *Texteditor*.

After editing the batch files, launch *Hy2Opt* by double-clicking on `Start_Hy2Opt.bat`.

## Requirements and dependencies<a name="req"></a>

***

### Python packages

The execution of *Hy2Opt* requires the following packages: 

- `glob`
- `logging`
- `os`
- `osgeo` (module: `osgeo.ogr`)
- `shutil`
- `subprocess` (not mandatory, also works without this package)
- `tkinter`

In addition, *Hy2Opt* uses [Damien Farrell](http://dmnfarrell.github.io/)'s [tkintertable](https://github.com/dmnfarrell/tkintertable) package (GNU General Public License v3 *GPLv3*) as built-in pypool function.

Any folder beginning with a "." for example `.cache` must not be modified or assessed by any other program, in particular during the execution of package methods. At the end of execution, the applied modules have created their output folders, which are indicated in the command prompt.

### Other software and dependencies

***

A workbook editing software such as *Excel* or [*LibreOffice*][libreoffice] is required for modifications of user-defined databases (`.xlsx` files).

For the visualization of geodata (`.shp` and `.tif` files), GIS software is required, for example: [*QGIS* ](https://www.qgis.org/en/site/forusers/download.html) or [SAGA](http://www.saga-gis.org/en/index.html) (*Please note: There are many more GIS softwares out there.*).

## Logfiles<a name="logs"></a>

***

Logfiles `logfile.log` are created in the module directories during every run task. These files contain time-stamped terminal messages of program activities, warnings and error messages. Thus, logfiles enable the user to review process duration and to trace back problems. The handling of potential errors and warning messages are listed in the [Troubleshooting Wiki](Troubleshooting)  with descriptions of problem sources (cause) and solutions (remedy).

[libreoffice]: https://www.libreoffice.org/
