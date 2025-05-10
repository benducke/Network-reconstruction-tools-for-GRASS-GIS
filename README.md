
# Network reconstruction tools for GRASS GIS (and QGIS)

This project provides a set of additional commands (modules) for [GRASS GIS](https://grass.osgeo.org/) that can be used to compute the links of a network, given  the network's nodes and a choice of connectivity criteria (aka _network reconstruction_). The bulk of the work is done by [v.net.models](https://github.com/benducke/Network-reconstruction-tools-for-GRASS-GIS/tree/master/v.net.models), a flexible, model-based network generator. The remaining modules serve mainly to preprocess input data or to explore and compare reconstruction results.

The currently included modules are:

* v.net.modules (main command: reconstruct network links based on nodes)
* v.net.stats (analysis: provide descriptive statistics for output of v.net.modules)
* v.sort (helper: enforce storage order of input nodes)
* v.points.thin (helper: systematically reduce number of input nodes)

All modules have been designed to run under recent versions of GRASS GIS (they are basically convenient wrappers around numerous low-level standard GRASS GIS commands). In addition, interface description files are provided for use under [QGIS](https://qgis.org).

For best performance, it is recommended to run this software on a Linux-based operating system. Operation under macOS and Windows is also possible but might be subject to some limitations in performance and/or functionality.

Please read the below instructions carefully, including the section on "Notes and caveats".

# Installation

*Note:* These instructions are valid for the files in the most current [Release](https://github.com/benducke/v.net.models/releases) package! Files in the code repository are under active development and installing them will likely not result in an installation that works as expected (or at all).

These instructions have been written for **GRASS GIS 7/8 and QGIS 3.40 LTR**.

It is assumed that you have successfully installed GRASS GIS and optionally QGIS (if you wish to run GRASS commands from within QGIS).

The modules provided here are scripts that have been developed to run in a Bash shell (minimum version 3) or in a traditional Bourne Shell (with severe compromises regarding speed and efficency). Linux-based distributions and macOS should provide all required essentials out-of-the-box. On Windows, installation of the free [MSYS2](https://www.msys2.org/) package is a necessary prerequisite.

Installation details vary depending on operating system (see below).

## Linux

Modern Linux-based operating systems ship with all the tools that are required (in addition to a working installation of GRASS GIS) to run the GRASS modules provided here.
You just need to copy the files provided by this project into the correct directories of your file system.

Note: Use "sudo" or other means of acquiring root user permission to add or modify files in protected directories.

To use these modules in GRASS GIS:
1. Determine the directory where GRASS GIS is installed on your Linux-based OS (often this will be "/usr/local/grass" or "/opt/grass").
2. Copy the "v.net.*" set of script files into the "scripts" subdirectory of your GRASS installation.
3. Set the executable bit ("+x") for all of the above files (use "chmod").
4. Copy the manual page "description.html" for each module to the "docs" subdirectory of your GRASS installation, changing "description" to the name of the module (e.g. for v.net.models the correct file name would be "v.net.models.html").
5. Also copy all ".png" files (they are the illustrations for the manual page) into the "docs" subdirectory of your GRASS installation.

For use via the QGIS Processing plug-in:
1. QGIS will automatically pick up your system's GRASS GIS installation, so you first need to perform the installation steps described above, for the installation of GRASS GIS.
2. Next, determine the QGIS base installation directory for your Linux-based OS. Frequently, this will be "/usr/share/qgis".
3. Within the QGIS base installation directory, open "python/plugins/grassprovider/description/algorithms.json" with a text editor of your choice to insert interface descriptions for the new modules. If you insert these at the end of the file, then the final lines of "algorithms.json" should look like this:
```
  {
    "name": "v.net.models",
    "display_name": "v.net.models",
    "command": "v.net.models",
    "short_description": "v.net.models - Performs model-based reconstruction of network links from input points",
    "group": "Network reconstruction",
    "group_id": "network_reconstruction",
    "ext_path": "v_net_models",
    "hardcoded_strings": [],
    "parameters": [
      "QgsProcessingParameterFeatureSource|input|Network nodes (points)|0|None|False",
      "*QgsProcessingParameterString|where|WHERE conditions of SQL statement without 'where' keyword|None|False|True",
      "QgsProcessingParameterFeatureSource|initial|Initial links produced by previous run (lines)|1|None|True",
      "QgsProcessingParameterRasterLayer|costmap|Cost surface|None|True",
      "QgsProcessingParameterField|key|Integer key field in input nodes' attribute table|None|input|0|False|False",
      "QgsProcessingParameterField|label|Label/name field (default: same as 'key')|None|input|-1|False|True",
      "QgsProcessingParameterEnum|model|Network connectivity model|attsim;complete;delaunay;nn;xtent|False|1|False",
      "QgsProcessingParameterField|attributes|Attribute field(s) for similarity test(s) (model 'attsim')|None|input|-1|True|True",
      "QgsProcessingParameterNumber|neighbors|Number of nearest neighbors to connect (model 'nn')|QgsProcessingParameterNumber.Integer|None|True|2|None",
      "QgsProcessingParameterNumber|maxdist|Absolute cost/distance threshold (several models)|QgsProcessingParameterNumber.Double|None|True|0.0|None",
      "QgsProcessingParameterField|size|Numeric attribute field with node 'size' (models 'xtent', 'nn')|None|input|-1|False|True",
      "QgsProcessingParameterNumber|a|Exponential size weight 'a' (model 'xtent')|QgsProcessingParameterNumber.Double|None|True|0.0|None",
      "QgsProcessingParameterNumber|k|Linear distance weight 'k' (model 'xtent')|QgsProcessingParameterNumber.Double|None|True|0.0|None",
      "QgsProcessingParameterEnum|avg|Distance averaging function (model 'xtent')|mean;median|False|0|False",
      "QgsProcessingParameterNumber|costsyn|Cost synergy effect of re-using network links (requires 'costmap')|QgsProcessingParameterNumber.Double|0.0|True|0.0|1.0",
      "QgsProcessingParameterNumber|costerr|Costmap error margin (+/-) in percent (requires 'costmap')|QgsProcessingParameterNumber.Double|0.0|True|0.0|100.0",
      "QgsProcessingParameterNumber|costres|Costmap resampling factor (requires 'costmap' and 'costerr')|QgsProcessingParameterNumber.Integer|1|True|1|10",
      "*QgsProcessingParameterNumber|costmem|Max. memory for cost raster ops in MB (passed to 'r.cost')|QgsProcessingParameterNumber.Integer|300|True|300|300000",
      "*QgsProcessingParameterNumber|threshold|Threshold distance for topology tests|QgsProcessingParameterNumber.Double|0.000001|True|0.0|100000000",
      "*QgsProcessingParameterNumber|threads|Number of concurrent processing threads (1=no concurrency)|QgsProcessingParameterNumber.Integer|1|True|1|1024",
      "*QgsProcessingParameterNumber|sqlbuflen|Number of SQL statements to buffer (performance)|QgsProcessingParameterNumber.Integer|20|True|1|1000",
      "QgsProcessingParameterVectorDestination|links|Reconstructed network links (lines)",
      "QgsProcessingParameterVectorDestination|nodes|Attributed network nodes (points)",
      "*QgsProcessingParameterBoolean|-k|'Knight's move' for more accurate (but slower) cost computations|False",
      "QgsProcessingParameterBoolean|-m|Measure least-cost paths in meters instead of costs|False",
      "*QgsProcessingParameterBoolean|-r|Reduce input nodes to those in GRASS region (set below)|False"
    ]
  },
  {
    "name": "v.net.stats",
    "display_name": "v.net.stats",
    "command": "v.net.stats",
    "short_description": "v.net.stats - Produces statistics for output generated by v.net.models",
    "group": "Network reconstruction",
    "group_id": "network_reconstruction",
    "ext_path": "v_net_stats",
    "hardcoded_strings": [],
    "parameters": [
      "QgsProcessingParameterFeatureSource|links|Network links (as generated by v.net.models)|1|None|False",
      "QgsProcessingParameterFeatureSource|nodes|Network nodes (as generated by v.net.models)|0|None|False",
      "QgsProcessingParameterField|key|Name of INTEGER key field in input NODES' attribute table|None|nodes|0|False|False",
      "QgsProcessingParameterVectorDestination|nstats|Output nodes (points) with added statistical attributes",
      "QgsProcessingParameterFileDestination|report|Output text file for statistical report|None|None|True|True",
      "QgsProcessingParameterString|title|Title string for statistical report|None|False|True",
      "QgsProcessingParameterBoolean|-a|Append statistical report to existing text file|False",
      "*QgsProcessingParameterBoolean|-g|Print statistics in shell script style (console output only)|False",
      "*QgsProcessingParameterBoolean|-s|Skip input data consistency checks|False"
    ]
  },
  {
    "name": "v.points.thin",
    "display_name": "v.points.thin",
    "command": "v.points.thin",
    "short_description": "v.points.thin - Sorts storage order of features on values of a chosen attribute",
    "group": "Network reconstruction",
    "group_id": "network_reconstruction",
    "ext_path": "v_points_thin",
    "hardcoded_strings": [],
    "parameters": [
      "QgsProcessingParameterFeatureSource|input|Input points|0|None|False",
      "*QgsProcessingParameterString|where|WHERE conditions of SQL statement without 'where' keyword|None|False|True",
      "QgsProcessingParameterNumber|dist|Absolute thinning distance threshold (must be >= 0.0)|QgsProcessingParameterNumber.Double|None|False|1.0|None",
      "QgsProcessingParameterEnum|order|Order of thinning|pkeyasc;pkeydesc;random|False|0|False",
      "QgsProcessingParameterVectorDestination|output|Thinned points",
      "QgsProcessingParameterBoolean|-i|Invert effect of 'where=' option (if given)|False"
    ]
  },
  {
    "name": "v.sort",
    "display_name": "v.sort",
    "command": "v.sort",
    "short_description": "v.sort - Sorts storage order of features on values of a chosen attribute",
    "group": "Network reconstruction",
    "group_id": "network_reconstruction",
    "ext_path": "v_sort",
    "hardcoded_strings": [],
    "parameters": [
      "QgsProcessingParameterFeatureSource|input|Vector layer to sort|0|None|False",
      "QgsProcessingParameterVectorDestination|output|Sorted output vector layer",
      "QgsProcessingParameterField|by|Attribute field by which to sort|None|input|0|False|False",
      "QgsProcessingParameterEnum|order|Sort order|asc;desc|False|0|False",
      "QgsProcessingParameterString|delimiter|Delimiter for text field parsing (use only in case of errors)|None|False|True",
      "QgsProcessingParameterBoolean|-e|Empty field values allowed|False",
      "QgsProcessingParameterBoolean|-i|Ignore leading blanks in field values|False",
      "QgsProcessingParameterBoolean|-n|Numeric sorting (forced)|False",
      "QgsProcessingParameterBoolean|-p|Print known and supported DBMS (then quit)|False",
      "QgsProcessingParameterBoolean|-u|Unique field values required|False"
    ]
  }
]
```
   
4. Make sure that the QGIS Processing plug-in and its GRASS provider are enabled in QGIS.
5. You should now find the additional modules in group "GRASS/Network reconstruction" in the QGIS Processing Toolbox (you might have to restart QGIS first).

For use via the QGIS GRASS 8 plug-in:
1. Perform installation into GRASS GIS and determination of QGIS base installation directory as described above, for the "QGIS Processing plug-in".
2. Now copy the ".qgm" and ".svg" files for each module into the "grass/modules" subdirectory of the QGIS base installation directory.
3. Open "grass/modules/default.qgc" with a text editor of your choice to insert a new "section" element for the additional modules. If you insert this at the end of the file, then the final lines of "default.qgc" should look like this:
  ```
  <section label="Help">
    <grass name="g.manual"/>
  </section>
   
  <section label="Network reconstruction add-on">
    <section label="Preprocess input nodes">
      <grass name="v.sort"/>
      <grass name="v.points.thin"/>
    </section>
    <section label="Reconstruct network links">
      <grass name="v.net.models"/>
      <grass name="v.net.models.simple"/>
    </section>
      <section label="Explore reconstruction results">
        <grass name="v.net.stats"/>
        <grass name="v.net.stats.simple"/>
      </section>
    </section>
  </section>
  
  </modules>
  </qgisgrass>
  ```
4. Make sure that the GRASS 8 plug-in is enabled in QGIS.
5. You should now find the additional "v.net.*" available under "Network reconstruction add-on" on the GRASS 8 plug-in pane (you might have to restart QGIS first).

## macOS

Similar to the Linux case, macOS operating systems ship with all the tools that are required (in addition to a working installation of GRASS GIS) to run the GRASS modules provided here.
There is no graphical installer provided by this project: You need to use the Terminal and Finder to get the GRASS modules installed.
The steps you have to perform are very similar to those on Linux-based systems.

The main difference is that both GRASS and QGIS come as App packages on macOS.

To use these modules in GRASS GIS:
1. Browse the GRASS App package using Finder ("Show Package Contents").
2. Open the folder "Contents/Resources" within the GRASS App package and proceed by copying this project's files to the same GRASS subfolders specified in the instructions for Linux (above).
3. Set the executable bit ("+x") for all of the above files (use "chmod" in Terminal: no other way to do this in macOS).
4. Copy the manual page "description.html" for each module to the "docs" subfolder of your GRASS installation, changing "description" to the name of the module (e.g. for v.net.models the correct file name would be "v.net.models.html").
5. Also copy all ".png" files (they are the illustrations for the manual page) into the "docs" folder of your GRASS installation.

For use via the QGIS Processing plug-in:
1. On macOS, QGIS comes with a bundled version of GRASS 7. Browse the QGIS App package using Finder ("Show Package Contents").
2. Open the folder "Contents/Resources/grass78" and copy this project's files to the same GRASS subfolders specified in the instructions for Linux (above).
3. Proceed by locating and editing "algorithms.json", as described in the instructions for Linux (above).
4. Make sure that the QGIS Processing plug-in and its GRASS provider are enabled in QGIS.
5. You should now find the additional modules in group "GRASS/Network reconstruction" in the QGIS Processing Toolbox (you might have to restart QGIS first).

For use via the QGIS GRASS 7 plug-in:
1. Perform installation into bundled GRASS GIS as described above, for the "QGIS Processing plug-in".
2. Copy the ".qgm" and ".svg" files for each module into the "Contents/Resources/grass/modules" subfolder of the QGIS App package.
3. Open "Contents/Resources/grass/modules/default.qgc" with a text editor of your choice to insert a new "section" element for the additional modules, as detailed in the instructions for Linux (above).
4. Make sure that the GRASS 7 plug-in is enabled in QGIS.
5. You should now find the additional "v.net.*" available under "Network reconstruction add-on" on the GRASS 7 plug-in pane (you might have to restart QGIS first).

## Windows

Make sure to set the Windows file manager to show "Extensions for known file types". Otherwise you might have trouble telling apart the different files that share the same base name (such as "v.net.models")!

Use under Windows requires installation of Bash and associated GNU command line tools. A great solution for this is to install the MSYS2 distribution which comes with everything required:

1. Go to go to https://www.msys2.org/ and download the installer package for MSYS2.
2. Run the installer. Accepting the default installation path (C:\msys64) is highly recommended.
3. If you went with the default installation settings then the MSYS2 installation is done.
4. Otherwise (if you have *not* installed into the default "C:\msys64" folder), you now need to edit the ".bat" file for each module (e.g. "v.net.models.bat") and modify the first two lines: 

> set GRASS_SH=C:\msys64\usr\bin\sh.exe  
> set MSYSPATH=/c/msys64/usr/bin

The first line contains the full path to the shell executable in your MSYS installation folder. Adjust it as needed to reflect your actual setup.
The second line contains the path in your MSYS installation where all of the MSYS tools (awk.exe, grep.exe, etc.) are stored. Also adjust as needed, but note that this path must be written in POSIX conformant format! The Windows (or rather: CP/M & MS-DOS) drive letter "C:" is replaced by "/c" (or whatever drive has your MSYS2 installation). Folders are separated by forward slashes "/", not backslashes. Do not add a slash after the final "bin" folder.

To use these modules in GRASS GIS:

1. Determine the directory where GRASS GIS is installed on your system.
2. Copy the "v.net.*" set of script files into the "scripts" subdirectory of your GRASS installation.
3. Copy the "*.bat" start-up files for all modules into the "bin" subdirectory of your GRASS installation.
4. Copy the manual page "description.html" for each module to the "docs" subfolder of your GRASS installation, changing "description" to the name of the module (e.g. for v.net.models the correct file name would be "v.net.models.html").
5. Also copy all ".png" files (they are the illustrations for the manual page) into the "docs" subdirectory of your GRASS installation.

For use via the QGIS Processing plug-in:
2. Copy "v.net.models.txt" into QGIS subfolder "apps\qgis-ltr\python\plugins\processing\algs\grass7\description"
3. Copy "v.net.models.bat" into QGIS subfolder "apps\grass\grass78\bin"
4. Copy "v.net.models" (no file extension!) into QGIS subfolder "apps\grass\grass78\scripts"
5. Copy the manual page "description.html" into QGIS subfolder "apps\grass\grass78\docs", and rename it to "v.net.models.html".
6. Also copy all ".png" files (they are the illustrations for the manual page) into "apps\grass\grass78\docs".
7. Quit and restart QGIS using the "QGIS Desktop with GRASS 7"(!) launcher.

For use via the QGIS GRASS 8 plug-in:
1. Perform installation into bundled GRASS GIS as described above, for the "QGIS Processing plug-in".
2. Copy the ".qgm" and ".svg" files for each module into the "Contents/Resources/grass/modules" subfolder of the QGIS App package.
3. Open "Contents/Resources/grass/modules/default.qgc" with a text editor of your choice to insert a new "section" element for the additional modules, as detailed in the instructions for Linux (above).
4. Make sure that the GRASS 7 plug-in is enabled in QGIS.
5. You should now find the additional "v.net.*" available under "Network reconstruction add-on" on the GRASS 7 plug-in pane (you might have to restart QGIS first).

# Notes and caveats

## Changing the shell interpreter
This software has been developed for Bash. If you want to run it in a different shell (for whatever reason), then you need to change "bash" in the very first line to the name of your preferred shell. A warning will be issued and a slower compatibility mode used for shells other than Bash.

## Running v.net.models via QGIS Processing
This works well in general, with a few quirks:
* Progress display (this affects all GRASS modules) does not work properly: The status will remain at "0" while a task is processing and instantly jump to "100" percent when it completes -- not very useful, unfortunately.
* Some options (such as "cats=" and "layer=") do not apply to the QGIS Processing environment and will not be available there.
* It is not possible (tested with QGIS 3.10.x) to restrict attribute field choices to only fields of type integer. This means that it will be possible to select a double type (aka floating point) field for the "key=" option, which will lead to an error message ("key=" must be set to a field of type integer).
* The included HTML manual page applies to use from within GRASS GIS (but most of the information also applies to running v.net.models from within QGIS).

## macOS Notes
In general, macOS ships has been shipping with an outdated POSIX environment.
This means that tools such as 'grep' or 'sort' are more primitive than what you get on a modern Linux-based OS or with MSYS2 for Windows.
Also, the latest version of Bash (the default shell used by these scripts) shipped with macOS is version 3. 
THe GRASS modules provided here have been designed with theses limitations in mind and will fall back to a behaviour compatible with macOS where needed.
In some cases, this can lead to performance degradation.
Please consult each module's HTML manual page for details.
If you want newer, more powerful versions of these tools, install them via [the homebrew project](https://brew.sh).
Explaining how to use brew.sh and how to change your environment to use updated POSIX tools is out of the scope of this README.

In addition, QGIS on macOS ships with GRASS version 7.8 (whereas GRASS 8.x is the current version).
The GRASS modules provided here have been designed to work with GRASS 7.8 as well as more recent versions of GRASS GIS.

Operation under macOS has not been tested intensively.
Please report any problems to this project by creating an Issue (ticket).

## Windows OS Notes
* GRASS modules might only be available in the QGIS Toolbox if QGIS was started via the "QGIS Desktop with GRASS 7" launcher (This seems to be a problem specific to QGIS 3.10.x).
* The current GRASS plug-in for QGIS is not very good at monitoring continuous status messages that come from GRASS modules. There is too much buffering going on (this is a general problem with text output on Windows consoles), which means that status output by CPU and/or I/O expensive operations will be delayed until it is basically useless. Be patient: Even if no progress is visible in the QGIS status monitor, v.net.models will eventually complete.

# Acknowledgments
Work on this software was supported by a grant from the National Science Centre of Poland, project number NCN 2015/17/D/HS3/00249 (“Connected Worlds of the European Late Bronze Age").
