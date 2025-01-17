Slippy map tile generator for QGIS
Version 0.3
Copyright 2018 Alexander Hajnal All rights reserved

This software is released under the terms of the Version 3 of the GNU Affero 
General Public License.  See the LICENSE file for details.  Contact me via 
email to negotiate licensing if you want to use this software under different a 
license.

Email:   SLIPPYsoftware@alephnull.net
         (remove the type of map to get the real address)
Website: http://alephnull.net/software/gis/tile_writer.shtml
Github:  https://github.com/Alex-Kent/tile_writer


About
--------------------------------------------------------------------------------
This program is a slippy map tile generator for use with QGIS. All rendering is
done by QGIS so what you see on your screen is what you'll get in your output
tiles. Functionally it is quite similar to the QTiles plugin. Here's a
comparison of the two:

Advantages of this program:
 o Renders the map a section at time.
   This allows even very large datasets to be rendered.
 o Renders each zoom level separately.
   This means that scale-dependant feature visibility is honored.
 o Does not exhibit the rendering issues that QTiles has at tile edges:
   o Does not truncate point icons
   o Labels don't shift
   o Patterned lines display correctly
 o Fast: 20 minutes to render a complex 60km x 70km topographic map with 
   roads, water, POIs, etc. from levels 5 through 15 on a 3.5GHz i7.
 o Resistant to the QGIS bug that causes raster layers to sometimes not be 
   displayed.
 o Copious progress status display
 o Simple Python script that can be easily modified to suit your needs.

Disadvantages of this program:
 o No graphical interface.
 o Cannot be made to stop early without killing QGIS.
 o Rendering complex maps causes the QGIS GUI to temporarily freeze during 
   some parts of the rendering process.
 o Lacks some of the additional options that QTiles offers.
 o Doesn't always clip the rendering to the specified area of interest.
   At lower zoom levels tiles will be rendered that are not within the area 
   of interest.

Comments are welcome, you can email SLIPPYsoftware@alephnull.net
(remove the type of map to get the real address).


Version history
--------------------------------------------------------------------------------
v0.3    adapted for QGIS 3 and Python 3 by JB
v0.2.2  Clarified license, Corrected documentation, Added Github repository 
        (no code changes)
v0.2.1  Documentation updates, added scale presets file (no code changes)
v0.2    Now supports both TMS and Google naming conventions
v0.1    Initial release


Files
--------------------------------------------------------------------------------
readme.txt         This file
tile_writer.py     The main script
globalmercator.py  For latitude/longitude to tile coordinate conversions
                   Taken from GDAL2Tiles
scales.xml         Scale presets for QGIS matching the tile zoom levels
LICENSE            License for this software


Usage
--------------------------------------------------------------------------------
Prelimary steps:
 o Create your map in QGIS
   o If you are using scale-dependant feature visibility see the section 
     "Zoom levels" below

Running the script:
 o Place tile_writer.py and globalmercator.py into the same directory
   (As packaged, this has already been done)
 o In QGIS, open the Python console (Plugins->Python Console)
 o Click on the "Show editor" icon in the Python Console (middle icon)
 o Load the tile_writer.py script using the "Open file" icon 
   (top icon on the right)
 o Define the map's area of interest:
   o Create a new polygon vector layer
   o Make it editable
   o Add a polygon covering the area you want rendered
   o Save it to disk; the filename you chose should be put into the 
     area_of_interest variable in the script.
   o Hide the layer
 o Adjust the settings to suit your needs (see below).
 o Note that all paths and filenames are relative to QGIS's current working 
   directory.
   o If you run QGIS from the commandline then the directory you started it 
     from is the working directory.
   o If you run QGIS from a graphical interface under Unix/Linux then the 
     working directory is probably your home directory.
   o If you run QGIS under Windows drop me an email and let me know what 
     QGIS's working directory is :^)
 o Run the script using the "Run script" icon (the bottom icon)
 o The script's progress will appear in the console
 o An "All done" message will be printed when the script has finished

Note that QGIS may appear to have locked up while generating the regional tiles.
If this happens simply be patient; complex maps may take several minutes to 
draw.

The script will avoid regenerating images whenever possible.  This means that 
if you want to regenerate a map you should either specify a different 
output_path or first delete all of the tile directories (numbered) as well as 
the region tiles (e.g. "10_300_651_s16_b2.png").


Configuration variables
--------------------------------------------------------------------------------
These are found near the top of tile_writer.py and should be changed as 
appropriate before running the script.

start_z
  Minimum zoom level 
  Default value: 10

end_z
  Maximum zoom level 
  Default value: 15

step
  How many map tiles to place in each regional tile
  (the actual number of map tiles per regional tile is step * step) 
  Higher number speed up rendering (assuming enough RAM is available).
  Lower numbers decrease memory usage but increase rendering time.
  Default value: 16

border
  Number of extra map tiles to render along each edge of a regional tile
  By rendering extra, unused border tiles we can avoid shifting labels, 
  truncated images, and line-drawing inconsistancies at tile boundaries. 
  If this value is set to 0 you will encounter rendering issues at tile 
  boundaries.
  Default value: 2

output_path
  Directory to write the regional images and level subdirectories to 
  Default value: '.'

area_of_interest
  Path of shapefile defining the area of interest
  The extent of the shapefile is used to limit rendering to a particular area. 
  Note that no clipping is done so at lower zoom levels tiles from outside the 
  area of interest will be generated. 
  Default value: 'border.shp'

tile_format
  Filename convention to follow
  Can be set to either 'tms' or 'google'
  Default value: 'tms'


Zoom levels
--------------------------------------------------------------------------------
If you are using scale-dependant feature visibility you should use the scales 
listed below when deciding visibility.
These scales can be found in the scales.xml file and installed in QGIS using:
Settings->Options->Map Tools->Predefined Scales->Import from file (folder icon)

    Level Scale
    ----- ---------------------
    1     1 : 295,829,355.45
    2     1 : 147,914,677.73
    3     1 : 73,957,338.86
    4     1 : 36,978,669.43
    5     1 : 18,489,334.72
    6     1 : 9,244,667.36
    7     1 : 4,622,333.68
    8     1 : 2,311,166.84
    9     1 : 1,155,583.42
    10    1 : 577,791.71
    11    1 : 288,895.85
    12    1 : 144,447.93
    13    1 : 72,223.96
    14    1 : 36,111.98
    15    1 : 18,055.99
    16    1 : 9,028.00
    17    1 : 4,514.00
    18    1 : 2,257.00
    19    1 : 1,128.50
    20    1 : 564.25
    21    1 : 282.12
    22    1 : 141.06
    23    1 : 70.53


Warranty
--------------------------------------------------------------------------------
I make no warranty or representation, either express or implied, with respect
the behavior of this script, its quality, performance, accuracy,
merchantability, or fitness for a particular purpose. This script is provided
'as is', and you, by making use thereof, are assuming the entire risk. That
said, I hope you this script useful. Have fun!
