# QGIS workshop for LA 4940 (Taylor), focusing on trees and toxics in 

Workshop 2024-09-11 by Keith Jenkins, GIS Librarian at Mann Library, Cornell University. \
This document is online at: <https://kgjenkins.github.io/la-4940-taylor/>

For help after this workshop, contact me at kgj2@cornell.edu  
Or set up a Zoom appointment at <https://guides.library.cornell.edu/gis/help>

All the data and documentation for this workshop can be downloaded from: \
<https://github.com/kgjenkins/la-4940-taylor/archive/main.zip>


## Some GIS terminology and concepts

**GIS** stands for Geographic Information System (or Science)

"GIS data" refers to data with a spatial component -- it can be mapped!

**QGIS** (<https://qgis.org/>) is one of several popular GIS programs for mapping and spatial analysis.  It is free, open-source desktop software that runs on Windows, Mac, and Linux.  QGIS is created by developers around the world, supported by municipal and national governments, corporations, user groups, and individual users.  Discussions about new features, bug fixes, and future directions happen on [e-mail lists](https://qgis.org/en/site/getinvolved/mailinglists.html) and the [project's GitHub organization](https://github.com/qgis).

Questions about how to use QGIS are typically asked and answered on [GIS StackExchange](https://gis.stackexchange.com/).  If you search the web for QGIS questions, there is a good chance you'll end up finding the answer already there, so always be sure to search before asking a new question.

**Vector** = points, lines, polygons
Common file formats are GeoPackage (.gpkg) and Shapefile (.shp, .dbf, .prj, etc.)

**Raster** = pixels (images, but also data)
Preferred file format is GeoTIFF (.tif), but .jpg and other formats can also be used.

Vector data does not usually contain any information about how to style the display of the data.

Styles are added in a map project.  The same data could be styled in different ways in different maps, or even in the same map!

The QGIS project file .qgz contains your styles and pointers to the data, but does not include the data itself.  I recommend keeping everything within a containing folder (there can be subfolders) that you can zip up or save to a device or the cloud.  Something like this:

* myproject/
  * data/
    * boundaries/
      * counties.gpkg
      * states.gpkg
    * buildings.gpkg
    * streets.shp
  * notes-about-sources.txt
  * mymap.qgz

**CRS** = Coordinate Reference System

Ideally, CRS details are quietly handled behind the scenes, so we barely even need to think about it.

But in the real world, you'll eventually run into CRS puzzles and problems, and CRS becomes even more important when doing distance- or area-based analysis.  Here are some of the CRS you might see in New Jersey:

* EPSG:4326 = WGS 84 -- "common longitude/latitude"
* EPSG:4269 = NAD83 -- longitude/latitude, similar to WGS 84, but based on a 1983 model of the Earth that is only defined for North America
* EPSG:3857 = WGS 84 / Pseudo-Mercator -- also called "Web Mercator", uses false "meters" (only true at equator)
* EPSG:3424 = NAD83 / New Jersey (ftUS) -- one of many state coordinate systems measured in feet
* EPSG:26718 = NAD27 / UTM zone 18N -- meter-based system based on a 1927 model of the Earth only defined for North America
* EPSG:26918 = NAD83 / UTM zone 18N -- meter-based system based on a 1983 model of the Earth only defined for North America
* EPSG:32618 = WGS 84 / UTM zone 18N -- meter-based system based on a 1984 model of the Earth used by GPS satellites


## Boundaries

We have the county boundaries for all of New Jersey in a shapefile format, which is probably the most common geospatial data format found on the Internet.  But the shapefile format dates from the early 1990s, which is why there are multiple component files (.shp, .dbf, .shx, .prj, and sometimes others).  To add a shapefile to QGIS, we just need to select the .shp file and QGIS will take care of the rest.  Shapefiles also have other quirks, mostly notably a 10-character limit for attribute names.  Learn more at <http://switchfromshapefile.org/>

Load the boundaries in QGIS:
* Look in the "data/boundaries" folder and drag the "NJ_Counties_3857.shp" file onto the QGIS window

Change the layer style:
* Open the Layer Styling panel – colorful paintbrush icon at top left of the Layers panel
* Change the color (click the back arrow at top to exit the color selector and return to the other styling options)
* Click "Simple fill" to change other properties, like stroke color and width

Get information about a polygon:
* Click "Identify Features" tool, select the layer, then click a polygon

View information about all the polygons as a table:
* Right-click layer in Layers pane > Open Attribute Table
* Click on a column header to sort by that column

The table and map are linked, so select a row from the table (by clicking the row number) to see it highlighted in yellow on the map.  There is a toolbar button to "Deselect features from all layers".

Add labels by clicking the label tab (yellow "abc") in the styling panel
* Single Labels, value = "COUNTY"
* 9 tabs of options!
* 1st tab (text) - font, size, color
* 8th tab (placement) - horizontal mode tends to center the labels in the polygons

To find out what CRS (coordinate reference system) is being used:
* Right-click > Properties...  Information tab

We have the municipal boundaries for New Jersey in a GeoPackage data format.  GeoPackage is a modern GIS format that was designed to improve upon the many limitations of shapefiles.  One benefit is that it is a single file.

* Look in the "data/boundaries" folder and drag the "NJ_Municipalities..." .gpkg file onto the QGIS window.

There are many ways of using the attributes of the layer to highlight specific features.  Here's one way:
* In the Layers panel, right-click the NJ_municipalities layer > Filter
* Enter the following filter expression: `"NAME"='Fair Lawn Borough'`
* Click "OK"

Now you should just see Fair Lawn on the map, without the clutter of all the other boroughs.


## Save your project!

Save "mymap.qgz" to the folder that also contains your data -- for example, within the "" folder. That way, you can zip up or move the containing folder around while keeping your map and data intact. The .qgz project file contains your map styles and pointers to your data files, but not the data itself.


## Streets

OpenStreetMap.org is a great source of streets data, and is widely considered to be the most complete and accurate street map in the world.  But New Jersey,  like many states, has very good official streets data that it makes available to the public, so we'll use a GeoPackage of streets for Fair Lawn and the surrounding area that was clipped from a statewide dataset.

* Look in the "streets" folder, and drag the "streets_clipped.gpkg" file onto QGIS
* If it ends up underneath the boundaries layer, drag the boundaries lower in the list of layers.

Explore the data a bit (identify tool, attribute table).  Try labeling the lines.


## Basemaps via QuickMapServices

Basemaps are web-based map images designed by professional cartographers who have already done the hard work of aggregating different data layers (streets, rivers, buildings, etc.) and customizing styles and labels to work at different zoom levels.  Basemaps are usually global in scope, although there are some that only focus on certain regions.  They can be used to add context to your map, or just to help confirm that your data is correctly aligned.  The QuickMapServices plugin makes this easy, but you may need to install it first.  (It should already be installed on the Mann Library computers.)

* Plugins menu > Manage and Install Plugins...
* Scroll down, or search all plugins for "quickmap" (you don’t need to type the whole name)
* Select the QuickMapServices plugin
* Click the "Install plugin" button (it installs in seconds)
* Click "Close"

When you first install QuickMapServices, you'll want to get the full set of basemap definitions.

* Web menu > QuickMapServices > Settings
* "More services" tab > "Get contributed pack"

To add the Google Hybrid basemap, which combines aerial photos with placename labels:

* Web menu > QuickMapServices > Google > Google Hybrid

**You may need to turn off your boundaries layer to see the basemap.**

Most basemaps are in a different projection (CRS) called Pseudo- or Web-Mercator, EPSG:3857.  QGIS is reprojecting it to match the CRS of the first dataset we loaded, which can cause some pixelation, making the labels hard to read.  If you zoom out, you'll notice that the US looks stretched out.

To avoid the pixelization and the stretched shapes, we can set our map to use the basemap CRS:
* Right-click the Google layer > Set CRS > Set Project CRS from Layer
* Right-click the Google layer > Zoom to Native Resolution (100%)

Zoom in to explore how well your roads data aligns with the imagery.  It may be helpful to lighten the Google layer a bit:
* In the layer styling panel for the Google layer, click the transparency tab (2nd tab on the left)
* Set the opacity to about 50%

For the rest of this workshop, it may help to use a plainer basemap:
* Web menu > QuickMapServices > OSM > OSM Standard
* Uncheck the Google Hybrid layer to hide it
* Adjust the other layer styles as needed

Note that basemaps are just images.  You'll be able to include them when exporting your map as an image, but not when exporting data as a CAD format.


## Exporting your map to an image file

To export the current map view to an image file:
* Project menu > Import/Export > Export Map to Image...
* Update the extent or leave as is
* Increase the resolution (try 300dpi)
* Save as a .png file


## Exporting your map to a PDF file

Exporting to PDF is useful if you are going to print your map, or if you want to do further work in a program like Inkscape or Adobe Illustrator.  The PDF will keep separate layers for each data layer.  Vector layers will remain vectors, and raster layers will remain raster.

* Project menu > Import/Export > Export Map to PDF...
* Check the settings, especially the resolution if your map includes raster layers, including basemaps

We won't have time to cover this in the workshop, but it's also possible to set up more complex page layouts, with grid lines, north arrow, scale bar, and additional text and images.  This is done using "Print layouts", found under the Project menu.  Once created, print layouts can also be exported to image files or PDFs.


## Exporting data layers to a DXF file (for CAD software)

Architects often want to export GIS layers for use in a CAD program like Rhino or AutoCAD.  Since DXF is a vector format, only vector layers can be exported -- sorry, no basemaps!  When exporting to CAD, be sure to set an appropriate CRS for the exported data, which means a CRS based on meters or feet -- not degrees longitude/latitude.
* Project menu > Import/Export > Export Project to DXF...
* Click the "..." button to specify where to save the .dxf file
* Be sure to select an appropriate CRS!  QGIS will reproject all your layers to the chosen CRS.  Some good CRS options for central New Jersey include:
  * EPSG:2261 = NAD83 / New Jersey Central (ftUS) -- if you want to work in US feet
  * EPSG:32618 = WGS 84 / UTM zone 18N -- if you want to work in meters

Do **NOT** export to DXF using any of the following CRS, or you will see distortions and incorrect measurements:
* EPSG:4326 = WGS 84 -- "common longitude/latitude"
* EPSG:4269 = NAD83 -- longitude/latitude, similar to WGS 84, but only defined for North America
* EPSG:3857 = WGS 84 / Pseudo-Mercator -- also called "Web Mercator", uses false "meters" (only true at equator)

If you have data that extends beyond your area of interest, and don't want to export it all to DXF, try checking the "Export features intersecting the current map extent".  (You may need to cancel if you need to adjust the map extent, and then open the export dialog again after zooming into the area you want to export.)


## Georeferencing a scanned map

Historical maps can be great sources of information that pre-dates the computer era.  However, most scanned maps images found online are just images, with no information about the coordiates that the map covers.  To view these maps in QGIS with other layers, we will need to do a process called "georeferencing".

There are 2 maps in the "historical-maps" folder that we can try to georeference.  The "Georeferencer" tool can be found in the "Layer" menu.  (In older versions of QGIS, it was in the "Raster" menu, but now it can also be used to georeference vector datasets such as DXF files, so it was moved to the Layer menu.)

We'll work through the process together in the workshop, but for future reference, here are a couple of good video demos:

Georeferencing a map in QGIS:
<https://youtu.be/XV62QEk0Cxg>

Digitizing points, lines, and polygons from a georeferenced map:
<https://youtu.be/m12ZXpGBoDc?t=786>


## Elevation raster data

Various scales of elevation data are available for different parts of the world.  For much of the globe, the best available is 30-meter SRTM data, collected as part of the Shuttle Radar Topography Mission in 2000.  Most of the United States is covered by the National Elevation Dataset (NED) with 10-meter pixels (NED 1/3 arcsec) and 30-meter pixels (NED 1 arcsec).  Some places have even higher resolution data (like 1m or 2m pixels) available from a local lidar survey, but these are usually only recommended for relatively small areas, due to the large size of the data.  We'll use a GeoTIFF file that was clipped from the NED 1/3 arcsec for this workshop.

* Open the `elevation` folder in your Windows file explorer or Mac finder
* Drag the "NED13_clipped.tif" file onto your project

By default, raster data is displayed with a gray gradient, with higher values brighter.  We can change the style to bring out more details in the data.

* In the Layer Styling panel, change from "Singleband gray" to "Singleband pseudocolor"
* Try changing the color ramp to "Turbo" (click the small down-arrow to the right of the color bar)

It will be easier to adjust a small number of colors.  If you have more than 5-6 colors listed, try setting the Mode to "Quantile" and adjust the number of Classes to 6.

* Double-click a color to change it


## Value Tool plugin

Although we could use the "Identify" tool to click pixels and see their values, the "Value Tool" plugin will allow you to instantly show the values from raster layers without even having to click.

* Plugins > Manage and Install Plugins...
* Search all plugins for "value tool" and install it

The Value Tool will appear on your toolbar as a green circle with white "i".

* Click the Value Tool button to open the panel
* Check the box to enable the tool
* Move your cursor across the map to view the raster values

Try checking the elevations for various parts of the map.  Note that the elevations are in meters (which could be verified by reading the metadata file.)


## Elevation hillshade

Even though we can see the differences between the highest and lowest parts of the map, much the area within Seneca county looks rather flat and devoid of detail.  Let's use a hillshade to made it look more 3-dimensional.

* In the Layer Styling panel, change "Singleband pseudocolor" to "Hillshade"

Because the horizontal units are in degrees, and the heights are in meters, we need to adjust the Z Factor, which is usually the ratio between the vertical units (meters) and horizontal units (also meters).

* Set Z Factor to 0.00003 (trying increasing or decreasing it slightly to adjust the intensity of the shading)

* Try turning the dial to adjust the angle of the "sun"

Also notice that if you zoom in too far, you see individual pixel edges.  To avoid this effect:
* Set the resampling "Zoomed in" and "Zoomed out" to "Bilinear" which will smooth out the pixels


## Generating contour lines

There is a "contours" display style for rasters, which is just a quick and simple visual effect:
* In the Layer Styling panel, change "Hillshade" to "Contours"
* Set the Contour Interval to 5 (meters)

This will let you preview what the contour lines will look like.  If we want to export 3D contour lines for use in a program like Rhino or AutoCAD, we will need to use a processing tool to create a vector contour layer.

* Open the processing toolbox (gear icon, or in the Processing menu)
* Search for "contour" and open the "Contour" tool under GDAL > Raster extraction
* Set the following parameters:
  * Input layer = "NED13_clipped"
  * Interval between contour lines = 5 (this value, which is meters in this case, should usually not be much less than the pixel size)
  * Check the box to "Produce 3D vector" (if you don't see it, expand the Advanced Parameters section)
* Keep the other options and click "Run"

To export the 3D contours to DXF, right-click the layer > Export > Save features as ... AutoCAD DXF or else export your whole project to DXF.  Be sure to set an appropriate CRS as described in the "Exporting data layers to a DXF file" section above.

For a smaller file size, you could first run the "Clip raster by extent" tool to create a temporary layer that covers a smaller area, then run the contour tool on that.

To generate contours that cover more than one elevation raster tile, you could first use the "Build virtual raster" tool to create a temporary layer that effectively treats multiple files as a single layer.


## Citing your Data Sources

The data for this workshop was derived from the following sources:

* Boundaries:
    * County: <https://njogis-newjersey.opendata.arcgis.com/datasets/ed7887264b054f4e82a4afb23a9214a4_0/explore>
    * Municipality: <https://njogis-newjersey.opendata.arcgis.com/datasets/newjersey::municipal-boundaries-of-nj-hosted-3857/explore>
* Streets: <https://njogis-newjersey.opendata.arcgis.com/datasets/newjersey::road-centerlines-of-nj-hosted-3424/explore?layer=0>
* Elevation: <https://cugir.library.cornell.edu/catalog/cugir-009000>
* Historical map of Radburn (1929): <https://digital.library.cornell.edu/catalog/ss:574386>

If you are using a basemap, you'll want to provide attribution to the basemap provider.  For some of the basemaps, the layer properties may have these details and possibly a link to the terms of use.  For example:

* Positron > right-click > Properties > QGIS Server tab
  > Map tiles by CartoDB, under CC BY 3.0. Data by OpenStreetMap, under ODbL.
* OSM Standard > right-click > Properties > QGIS Server tab
  > © OpenStreetMap contributors, CC-BY-SA


