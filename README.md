# v.net.models
Node-based network model generator for GRASS GIS 7.x (can also be used via the QGIS Processing plug-in: see instructions below).

This program takes a vector points layer as input and produces a new vector lines layer that represents the links of the reconstructed network.
Several connectivity models have been implemented.

# Included files

This program is distributed as a set of files:

* v.net.models - This is the actual script for GRASS that does all the work.
* v.net.models.txt - Parameters description file for the QGIS Processing plug-in.
* v.net.models.bat - A start-up script that is only required on Windows.

# Installation

This software is a Bourne Shell script that has been tested and verified to run on Linux, Windows and macOS operating systems.

Tested with:

GRASS GIS 7.8.3 & QGIS 3.10.12

This script requires a working installation of GRASS 7 (confirmed to work with GRASS), the Bourne Shell (alternatively: BASH or another sh-compatible Shell) and a set of support tools (GNU awk, expr and grep are strictly required). Linux and macOS users will already have the latter on their systems. Windows users must install an additional software package (MSYS2).

Installation details vary depending on operating system.

## Linux

Linux-based operating systems have all required shell scripting tools.

To use v.net.models in GRASS GIS:
1. Copy the file "v.net.models" into the "scripts" subdirectory of your GRASS installation (use "sudo" if required).
2. Set the executable bit ("+x") for "v.net.models" so that it can be run (use "chmod").

(Alternatively: copy "v.net.models" to wherever you like and call it from within a running GRASS CLI session: "sh <path_to_script>/v.net.models")

For use via the QGIS Processing plug-in:
1. Make sure that the QGIS Processing plug-in and its GRASS provider are enabled.
2. Copy the file "v.net.models" to the scripts "subdirectory" of the GRASS installation used by QGIS.
3. Set the executable bit ("+x") for "v.net.models" so that it can be run (use "chmod").
4. Copy the parameters description file "v.net.models.txt" into the subdirectory "plugins/processing/algs/grass7" of your QGIS installation (the exact location of this subdirectory depends on your Linux distribution; on Arch Linux it is "/usr/share/qgis/python/plugins/processing/algs/grass7").
5. Restart QGIS.
6. You should now find "v.net.models" available as an "Algorithm" in the QGIS Processing Toolbox.

## macOS

On macOS operating systems, all required shell scripting tools should be in place. There is no graphical installer for "v.net.models": You need to use the Terminal and Finder to get it installed. The steps you have to perform are very similar to those on Linux-based systems.

To use v.net.models in GRASS GIS:
1. Browse the GRASS App folder using Finder (or "cd" into the "GRASS.Application" folder using the command line) and locate the subfolder "scripts".
2. Copy the file "v.net.models" into the "Contents/Resources/scripts" subdirectory of your GRASS Application bundle (use "sudo" if required).
3. Set the executable bit ("+x") for "v.net.models" so that it can be run (use "chmod").

(Alternatively: copy "v.net.models" to wherever you like and call it from within a running GRASS CLI session: "bash <path_to_script>/v.net.models")

For use via the QGIS Processing plug-in:
1. Make sure that the QGIS Processing plug-in and its GRASS provider are enabled.
2. Browse the QGIS App folder using Finder (or "cd" into the "QGIS.Application" folder using the command line) and locate the subfolder "apps/grass/scripts".
3. Copy the file "v.net.models" into the QGIS App subfolder "apps/grass/scripts".
3. Set the executable bit ("+x") for "v.net.models" so that it can be run (use "chmod").
4. Copy the parameters description file "v.net.models.txt" into the subdirectory "plugins/processing/algs/grass7" of your QGIS App bundle.
5. Restart QGIS.
6. You should now find "v.net.models" available as an "Algorithm" in the QGIS Processing Toolbox.

## Windows

Make sure to set the Windows file manager to show "Extensions for known file types". Otherwise you might have trouble telling apart the different files that share the base name "v.net.models"!

Use under Windows requires installation of a Bourne Shell (or compatible interpreter) and associated GNU command line tools. A great solution for this is to install the MSYS2 distribution which comes with everything you need to run "v.net.models" (and more). Of course, if you already have MSYS2 installed, then you can skip this step (but remember to check and sdjust "v.net.models.bat" if necessary: see point 4, below):

1. Go to go to https://www.msys2.org/ and download the installer package for MSYS2.
2. Run the installer. Accepting the default installation path (C:\msys64) is highly recommended.
3. If you went with the default installation settings: Skip straight to the section on "use v.net.models in GRASS GIS" or "use via the QGIS Processing plug-in"!
4. If (and only if) you have *not* installed into the default "C:\msys64" folder, then you now need to edit edit "v.net.models.bat" and modify its first two lines: 

set GRASS_SH=C:\msys64\usr\bin\sh.exe
set MSYSPATH=/c/msys64/usr/bin

The first line contains the full path to the Bourne Shell executable in your MSYS installation folder. Adjust it as needed to reflect your actual setup.
The second line contains the path in your MSYS installation where all of the MSYS tools (awk.exe, grep.exe, etc.) are stored. Also adjust as needed, but note that this path must be written in POSIX conformant format! The Windows (or rather: CP/M & MS-DOS) drive letter "C:" is replaced by "/c" (or whatever drive has your MSYS installation). Folders are separated by forward slashes "/", not backslashes. No separator slash must be added after the final "bin" folder.

To use v.net.models in GRASS GIS:
1. Make sure that you have installed MSYS2, as explained above.
2. Copy the file "v.net.models" into the "scripts" subdirectory of your GRASS installation (you might need an admin password).
3. Copy the file "v.net.models.bat" into the "bin" subdirectory of your GRASS installation.

For use via the QGIS Processing plug-in:
1. Make sure that you have installed MSYS2, as explained above.
2. Copy "v.net.models.txt" to QGIS subfolder "apps\qgis-ltr\python\plugins\processing\algs\grass7\description"
3. Copy "v.net.models.bat" to QGIS subfolder "apps\grass\grass78\bin"
4. Copy "v.net.models" (no file extension!) to QGIS subfolder "apps\grass\grass78\scripts"
5. Quit and restart QGIS using the "QGIS Desktop with GRASS 7"(!) launcher.

Windows notes: 
* GRASS modules might only be available in the QGIS Toolbox if QGIS was started via the "QGIS Desktop with GRASS 7" launcher.
* The current GRASS plug-in for QGIS is not very good at monitoring continuous status messages that come from GRASS modules. There is too much buffering going on (this is a general problem with text output on Windows consoles), which means that status output by CPU and/or I/O expensive operations will be delayed until it is basically useless. Be patient: Even if no progress is visible in the QGIS status monitor, v.net.models will eventually complete.




