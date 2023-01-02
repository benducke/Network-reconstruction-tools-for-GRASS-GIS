# v.net.models
This is a model-based network (physical, transport, roads) generator and research tool for GRASS GIS 7.x (can also be used via the QGIS Processing plug-in: see instructions below). It provides functionality for reconstructing (hypothetical) network links from a set of given nodes, using one of several reconstruction models. The output is primarily intended to serve as a baseline for comparison with more advanced reconstructions (computed with other tools). A greater degree of realism can be achieved if the user supplies a sufficiently detailed cost surface.

Summary: This program takes a vector points layer as input and produces a new vector lines layer that represents the links of the reconstructed network.
Several connectivity models have been implemented.

# Included files

This program is distributed as a set of files:

* v.net.models - This is the actual script for GRASS that does all the work.
* v.net.models.txt - Parameters description file for the QGIS Processing plug-in.
* v.net.models.bat - A start-up script that is only required on Windows.

# Installation

*Note:* These instructions are only valid for the files in the current Release package! Files in the code repository are under active development and installing them will most likely not result in an installation that works as expected (or at all).

This software is a Bourne Shell script that has been tested and verified to run on Linux, Windows and macOS operating systems.

Tested with:

GRASS GIS 7.8.3 & QGIS 3.10.12

At a minimum, running this script requires a working installation of GRASS 7, the Bourne Shell (alternatively: BASH or another sh-compatible Shell) and a set of support tools (GNU awk, expr and grep are strictly required). Linux and macOS users will already have the latter on their systems. Windows users must install an additional software package (MSYS2). For a more user-friendly interface, install QGIS, as well.

Installation details vary depending on operating system.

## Linux

Linux-based operating systems have all required shell scripting tools.

To use v.net.models in GRASS GIS:
1. Copy the file "v.net.models" into the "scripts" subdirectory of your GRASS installation (use "sudo" if required).
(Alternatively: copy "v.net.models" to wherever you like and call it from within a running GRASS CLI session: "sh <path_to_script>/v.net.models")
2. Set the executable bit ("+x") for "v.net.models" so that it can be run (use "chmod").
3. Copy the manual page "description.html" to the "docs" subdirectory of your GRASS installation, and rename it to "v.net.models.html".
4. Also copy all ".png" files (they are the illustrations for the manual page) into "docs".

For use via the QGIS Processing plug-in:
1. Make sure that the QGIS Processing plug-in and its GRASS provider are enabled.
2. Copy the file "v.net.models" to the scripts "subdirectory" of the GRASS installation used by QGIS.
3. Set the executable bit ("+x") for "v.net.models" so that it can be run (use "chmod").
4. Copy the manual page "description.html" to the "docs" subdirectory of your GRASS installation (the one used by QGIS), and rename it to "v.net.models.html".
5. Also copy all ".png" files (they are the illustrations for the manual page) into "docs".
6. Copy the parameters description file "v.net.models.txt" into the subdirectory "plugins/processing/algs/grass7/description" of your QGIS installation (the exact location of this subdirectory depends on your Linux distribution; on Arch Linux it is "/usr/share/qgis/python/plugins/processing/algs/grass7/description").
7. Restart QGIS.
8. You should now find "v.net.models" available as an "Algorithm" in the QGIS Processing Toolbox.

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

## Running v.net.models via QGIS Processing
This works well in general, with a few quirks:
* Some options (such as "cats=" and "layer=") do not apply to the QGIS Processing environment and will not be available there.
* It is not possible (tested with QGIS 3.10.x) to restrict attribute field choices to only fields of type integer. This means that it will be possible to select a double type (aka floating point) field for the "key=" option, which will lead to an error message ("key=" must be set to a field of type integer).
* The included HTML manual page applies to use from within GRASS GIS (but most of the information also applies to running v.net.models from within QGIS).

## Use on Windows OS
* GRASS modules might only be available in the QGIS Toolbox if QGIS was started via the "QGIS Desktop with GRASS 7" launcher (This seems to be a problem specific to QGIS 3.10.x).
* The current GRASS plug-in for QGIS is not very good at monitoring continuous status messages that come from GRASS modules. There is too much buffering going on (this is a general problem with text output on Windows consoles), which means that status output by CPU and/or I/O expensive operations will be delayed until it is basically useless. Be patient: Even if no progress is visible in the QGIS status monitor, v.net.models will eventually complete.

# Acknowledgments
Work on this software was supported by a grant from the National Science Centre of Poland, project number NCN 2015/17/D/HS3/00249 (“Connected Worlds of the European Late Bronze Age").
