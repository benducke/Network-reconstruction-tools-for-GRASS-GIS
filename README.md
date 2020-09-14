# v.net.models
Node-based network model generator for GRASS GIS 7.x

Takes a vector points layer as input, produces a new vector lines layer that represents the links of the reconstructed network.
Several connectivity models have been implemented to drive the reconstructions.

Linux users: Simply make this script executable and run it from within a GRASS session (tested with GRASS 7.8).
Windows users: You need to create a DOS ".BAT" file so that Windows can run this. Take a look at the other modules of your GRASS installation to see how it's done.

If you want to run this under QGIS (GRASS Toolbox), look into the "grass" subfolder of your complete QGIS installation. You will many other GRASS modules in there. Copy "v.net.modules" into the same folder and make it executable (DOS user: ".BAT" file ... see above). Then restart QGIS.
