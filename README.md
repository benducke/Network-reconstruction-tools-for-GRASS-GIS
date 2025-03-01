# Network reconstruction tools for GRASS GIS (and QGIS)

This project provides a set of additional commands (modules) for [GRASS GIS](https://grass.osgeo.org/) that can be used to compute the links of a network, given  the network's nodes and a choice of connectivity criteria (aka _network reconstruction_). The bulk of the work is done by [v.net.models](https://github.com/benducke/Network-reconstruction-tools-for-GRASS-GIS/tree/master/v.net.models), a flexible, model-based network generator. The remaining modules serve mainly to preprocess input data or to explore and compare reconstruction results.

The currently included modules are:

* v.net.modules (main command: reconstruct network links based on nodes)
* v.net.stats (analysis: provide descriptive statistics for output of v.net.modules)
* v.sort (helper: enforce storage order of input nodes)
* v.points.thin (helper: systematically reduce number of input nodes)

All modules have been designed to run under **versions 7.8 and 8.4.x of GRASS GIS** (they are basically convenient wrappers around numerous low-level standard GRASS GIS commands). In addition, interface description files are provided for use under [QGIS](https://qgis.org) (**tested with QGIS 3.34 LTR**).

For best performance, it is recommended to run this software on a Linux-based operating system. Operation under macOS and Windows is also possible but might be subject to some limitations in performance and/or functionality.

# Installation

*Note:* These instructions are valid for the files in the most current [Release](https://github.com/benducke/v.net.models/releases) package! Files in the code repository are under active development and installing them will likely not result in an installation that works as expected (or at all).

The modules provided here are scripts that have been developed to run in a Bash shell (minimum version 3) or in a traditional Bourne Shell (with severe compromises regarding speed and efficency). Linux-based distributions and macOS should provide all required essentials out-of-the-box. On Windows, installation of the free [MSYS2](https://www.msys2.org/) package is a necessary prerequisite.

Installation details vary depending on operating system (see below).

## Linux

Linux-based operating systems have all required shell scripting tools. You just need to copy the files provided by this project into the correct directories of your file system.

To use these modules in GRASS GIS:
1. Determine the directory where GRASS GIS is installed on your Linux-based OS (often this will be "/usr/local/grass" or "/opt/grass").
2. Copy the "v.net.*" set of script files into the "scripts" subdirectory of your GRASS installation  (use "sudo" if required).
3. Set the executable bit ("+x") for all of the above files (use "chmod").
4. Copy the manual page "description.html" for each module to the "docs" subdirectory of your GRASS installation, renaming it to match each module's name with the extension ".html" (e.g. for v.net.models the target name would be "v.net.models.html").
5. Also copy all ".png" files (they are the illustrations for the manual page) into the "docs" folder of your GRASS installation.

For use via the QGIS Processing plug-in:
1. QGIS will automatically pick up your system's GRASS GIS installation, so you first need to perform the installation steps described above, for the installation of GRASS GIS.
2. Make sure that the QGIS Processing plug-in and its GRASS provider are enabled in QGIS.
3. You should now find the additional "v.net.*" available as new "Algorithm" entries in the QGIS Processing Toolbox (you might have to restart QGIS first).

For use via the QGIS GRASS 8 plug-in:
1. QGIS will automatically pick up your system's GRASS GIS installation, so you first need to perform the installation steps described above, for the installation of GRASS GIS.
2. In addition, 
3. Make sure that the QGIS Processing plug-in and its GRASS provider are enabled in QGIS.

## macOS

On macOS operating systems, all required shell scripting tools should be in place. There is no graphical installer for "v.net.models": You need to use the Terminal and Finder to get it installed. The steps you have to perform are very similar to those on Linux-based systems.

To use v.net.models in GRASS GIS:
1. Browse the GRASS App folder using Finder (or "cd" into the "GRASS.Application" folder using the command line) and locate the subfolder "scripts".
(Alternatively: copy "v.net.models" to wherever you like and call it from within a running GRASS CLI session: "bash <path_to_script>/v.net.models")
2. Copy the file "v.net.models" into the "Contents/Resources/scripts" subfolder of your GRASS Application bundle (use "sudo" if required).
3. Set the executable bit ("+x") for "v.net.models" so that it can be run (use "chmod").
4. Copy the manual page "description.html" to the "Contents/Resources/docs" subfolder of your GRASS Application bundle, and rename it to "v.net.models.html".
5. Also copy all ".png" files (they are the illustrations for the manual page) into "Contents/Resources/docs".

For use via the QGIS Processing plug-in:
1. Make sure that the QGIS Processing plug-in and its GRASS provider are enabled.
2. Browse the QGIS App folder using Finder (or "cd" into the "QGIS.Application" folder using the command line) and locate the subfolder "apps/grass/scripts".
3. Copy the file "v.net.models" into the QGIS App subfolder "apps/grass/scripts".
4. Set the executable bit ("+x") for "v.net.models" so that it can be run (use "chmod").
5. Copy the manual page "description.html" into the QGIS App subfolder "apps/grass/docs", and rename it to "v.net.models.html".
6. Also copy all ".png" files (they are the illustrations for the manual page) into "apps/grass/docs".
7. Copy the parameters description file "v.net.models.txt" into the subdirectory "plugins/processing/algs/grass7/description" of your QGIS App bundle.
8. Restart QGIS.
9. You should now find "v.net.models" available as an "Algorithm" in the QGIS Processing Toolbox.

## Windows

Make sure to set the Windows file manager to show "Extensions for known file types". Otherwise you might have trouble telling apart the different files that share the base name "v.net.models"!

Use under Windows requires installation of a Bourne Shell (or compatible interpreter) and associated GNU command line tools. A great solution for this is to install the MSYS2 distribution which comes with everything you need to run "v.net.models" (and more). Of course, if you already have MSYS2 installed, then you can skip this step (but remember to check and sdjust "v.net.models.bat" if necessary: see point 4, below):

1. Go to go to https://www.msys2.org/ and download the installer package for MSYS2.
2. Run the installer. Accepting the default installation path (C:\msys64) is highly recommended.
3. If you went with the default installation settings: Skip straight to the section on "use v.net.models in GRASS GIS" or "use via the QGIS Processing plug-in"!
4. If (and only if) you have *not* installed into the default "C:\msys64" folder, then you now need to edit edit "v.net.models.bat" and modify its first two lines: 

> set GRASS_SH=C:\msys64\usr\bin\sh.exe  
> set MSYSPATH=/c/msys64/usr/bin

The first line contains the full path to the Bourne Shell executable in your MSYS installation folder. Adjust it as needed to reflect your actual setup.
The second line contains the path in your MSYS installation where all of the MSYS tools (awk.exe, grep.exe, etc.) are stored. Also adjust as needed, but note that this path must be written in POSIX conformant format! The Windows (or rather: CP/M & MS-DOS) drive letter "C:" is replaced by "/c" (or whatever drive has your MSYS installation). Folders are separated by forward slashes "/", not backslashes. No separator slash must be added after the final "bin" folder.

To use v.net.models in GRASS GIS:
1. Make sure that you have installed MSYS2, as explained above.
2. Copy the file "v.net.models" into the "scripts" subdirectory of your GRASS installation (you might need an admin password).
3. Copy the file "v.net.models.bat" into the "bin" subdirectory of your GRASS installation.
5. Copy the manual page "description.html" into the "docs" subdirectory of your GRASS installation, and rename it to "v.net.models.html".
6. Also copy all ".png" files (they are the illustrations for the manual page) into "docs".

For use via the QGIS Processing plug-in:
1. Make sure that you have installed MSYS2, as explained above.
2. Copy "v.net.models.txt" into QGIS subfolder "apps\qgis-ltr\python\plugins\processing\algs\grass7\description"
3. Copy "v.net.models.bat" into QGIS subfolder "apps\grass\grass78\bin"
4. Copy "v.net.models" (no file extension!) into QGIS subfolder "apps\grass\grass78\scripts"
5. Copy the manual page "description.html" into QGIS subfolder "apps\grass\grass78\docs", and rename it to "v.net.models.html".
6. Also copy all ".png" files (they are the illustrations for the manual page) into "apps\grass\grass78\docs".
7. Quit and restart QGIS using the "QGIS Desktop with GRASS 7"(!) launcher.

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
The latest version of Bash (the default shell used by these scripts) shipped with macOS is version 3. This should still work fine. If you want a newer version of Bash, install one via [the homebrew project](https://brew.sh).

## Windows OS Notes
* GRASS modules might only be available in the QGIS Toolbox if QGIS was started via the "QGIS Desktop with GRASS 7" launcher (This seems to be a problem specific to QGIS 3.10.x).
* The current GRASS plug-in for QGIS is not very good at monitoring continuous status messages that come from GRASS modules. There is too much buffering going on (this is a general problem with text output on Windows consoles), which means that status output by CPU and/or I/O expensive operations will be delayed until it is basically useless. Be patient: Even if no progress is visible in the QGIS status monitor, v.net.models will eventually complete.

# Acknowledgments
Work on this software was supported by a grant from the National Science Centre of Poland, project number NCN 2015/17/D/HS3/00249 (“Connected Worlds of the European Late Bronze Age").
