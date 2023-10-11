![](https://i.imgur.com/iywjz8s.png)


# Collaborative Document - Day 1

2023-10-10 Introduction to Geospatial Raster and Vector Data with Python

Welcome to The Workshop Collaborative Document.

This Document is synchronized as you type, so that everyone viewing this page sees the same text. This allows you to collaborate seamlessly on documents.

----------------------------------------------------------------------------

This is the Document for today: [link](https://tinyurl.com/2023-10-10-geospatial-python)

Collaborative Document day 1: [link](https://tinyurl.com/2023-10-10-geospatial-python)

Collaborative Document day 2: [link](https://tinyurl.com/2023-10-11-geospatial-python)


## 👮Code of Conduct

Participants are expected to follow these guidelines:
* Use welcoming and inclusive language.
* Be respectful of different viewpoints and experiences.
* Gracefully accept constructive criticism.
* Focus on what is best for the community.
* Show courtesy and respect towards other community members.
 
## ⚖️ License

All content is publicly available under the Creative Commons Attribution License: [creativecommons.org/licenses/by/4.0/](https://creativecommons.org/licenses/by/4.0/).

## 🙋Getting help

To ask a question, just raise your hand.

If you need help from a helper, place a pink post-it note on your laptop lid. A helper will come to assist you as soon as possible.

## 🖥 Links

Workshop website: [link](https://esciencecenter-digital-skills.github.io/2023-10-10-ds-geospatial/)

🛠 Setup: [link](https://carpentries-incubator.github.io/geospatial-python/index.html)

🔧 Download files: [link](https://figshare.com/ndownloader/files/42603754)

Search results: [link](https://raw.githubusercontent.com/esciencecenter-digital-skills/2023-10-10-ds-geospatial/main/files/rhodes_sentinel-2.json)

## 👩‍🏫👩‍🙋💻🎓 Instructors & Helpers

Francesco Nattino, Ou Ku, Pranav Chandramouli, Maurice de Kleijn


## 👩‍💻👩‍💼👨‍🔬🧑‍🔬🧑‍🚀🧙‍♂️🔧 Roll Call

## 🗓️ Agenda
**Day 1**
 
| Time  | Topic                                   |
| ----- | --------------------------------------- |
| 09:30 | Welcome and icebreaker, setup check     |
| 09:45 | Introduction to raster, vector, and CRS |
| 10:30 | **Coffee break**                        |
| 10:40 | Access satellite imagery using Python   |
| 11:30 | **Coffee break**                        |
| 11:40 | Read and visualize raster data          |
| 12:30 | **Lunch break**                         |
| 13:30 | Vector data in Python                   |
| 14:30 | **Coffee Break**                        |
| 14:40 | Align Raster and vector data            | 
| 16:15 | Wrap-up                                 |
| 16:30 | END                                     |
 
**Day 2**

| Time  | Topic                                   |
| ----- | --------------------------------------- |
| 09:30 | Welcome and icebreaker, setup check     |
| 09:45 | Raster Calculations in Python           |
| 10:30 | **Coffee break**                        |
| 10:40 | Parallel raster computations using Dask |
| 11:30 | **Coffee break**                        |
| 11:40 | Parallel raster computations using Dask |
| 12:30 | **Lunch break**                         |
| 13:30 | Calculating Zonal Statistics on Rasters |
| 14:30 | **Coffee Break**                        |
| 14:40 | Calculating Zonal Statistics on Rasters |
| 16:15 | Post-workshop Survey                    |
| 16:30 | **Drinks**                              |

## 🏢 Location logistics
* Coffee and toilets are in the hallway, just outside of the classroom.
* If you leave the building, be sure to be accompanied by someone from the escience center to let you back in through the groundfloor door
* For access to this floor you might need to ring the doorbell so someone can let you in
* In case of an emergency, you can exit our floor using the main staircase.
  Or follow green light signs at the ceiling to the emergency staircase.
* **Wifi**: Eduroam should work. Otherwise use the 'matrixbuilding' network, password should be printed out and available somewhere in the room.

## 🎓 Certificate of attendance
If you attend the full workshop you can request a certificate of attendance by emailing to training@esciencecenter.nl .

## 🔧 Exercises

## 🧠 Collaborative Notes

The first thing we did is downloading the data that we are going to use and put in your data folder and make sure to unzip it.
[link](https://figshare.com/ndownloader/files/42603754)

Now we are opening a new jupyter notebook. In your geospatial python folder through a terminal (can be vscode or an anaconda command prompt).
Now type *jupyter -lab* and an empty jupyter notebook will start in your browser.

Type: 
``` python
import rioxarray
``` 
in your notebook to check if everything is  working.

# EPISODE 1: ACCESS DATA
Access to the satellite image of our study area.

First we explored the website that has a nice interface to obtain a satellite image. 
[copernicus interface](https://dataspace.copernicus.eu/)

For a one time getting data action this is fine. However, if you want to do this multiple times or have a pipeline that you can rerun/ modify your selection you can use the API. 

In our workshop we are going to work with this API to obtain and analyse satellite images by creating a pipeline.

STAC stands for SpatialTemporal Assest Catalogs. It an initiative to have a common language to describe geospatial information. This is generic in order to allign working with API's. 

[More info about STAC](https://stacspec.org/en)

Today we will access data from one of these portals using the API STAC language. 

[Earth Search by Element 84](https://stacindex.org/catalogs/earth-search#/)

Explore the webinterface. All in the STAC metadata can be accessed through the API. 

> TIME 10:28 <
> BREAK <
> TIME 10:40 <

Now we are going to open jupyter lab. 
Create a jupyter notebook

Rename the notebook Let us call it *01-data-access.ipynb* .

Type: 
``` python
import rioxarray
``` 
in your notebook to check if everything is  working.


We are now going to interact with the api.

``` python
api_url = 'https://earth-search.aws.element84.com/v1'
````

``` python
from pystac_client import Client
client = Client.open(api_url)
```
If you want to see the object > type in client to look at it:

``` python
client
```

Wildfire was in July, so we want to look at images around that period.

``` python
for collection in client.get_collections():
    print(collection)
    
```
We want to look at the sentinel 2 data set. To do so, we will create a collection_id variable with the sentinel-2 dataset.

``` python
collection_id = 'sentinel-2-l2a'
```
Now we are going to look at our area. In our case this is the center point of Rhodos.

```python
from shapely.geometry import Point
point = Point(27.95 , 36.20) # lon, lat
```

Run the cell and you will see your point. 
Note: the rendering of the point might vary across versions  of jupyter.
```python
point
```

Now we are going to set up our first search. To get help in python, you can use shift + tab. In particular, for jupyter you can add '?' at the end of the command to get help.

```python
# help: shift+tab, in jupyter add "?"
search = client.search(
    collections=[collection_id],
    intersects=point, # press tab for autocompletion
)
```

Let us see how many items match our search. 

```python
search.matched
```
(should be 522 which are the tiles of various datasets) These are all the datalayers available in the sentinel-2 dataset.


**EXERCISE 1:** searching satellite scenes with a time filter

Add a time filter to the search in order to select the only scenes recorded between 1 July and 31 August. You can find the input argument and the required syntax in the documentation of client.search (which you can access from Python or [online](https://pystac-client.readthedocs.io/en/stable/api.html#pystac_client.Client.search). How many scenes do now match our search?

**SOLUTION:**


```python
search = client.search(
    collections=[collection_id],
    intersects=point,
    datetime='2023-07-01/2023-08-31',
)
```

Now we run again the search.matched to see the number of scenes match our search.

```python
search.matched()
```
(should be 12 in our case)

```python
items = search.item_collection()
```

look at the items to explore the data / scenes you retrieved.
``` python
items
```

``` python
len(items)
```

Do a loop through the items to get a list of the items.

``` python
for item in items:
    print(item)
````

Will create a list of items. We can also index these items. On e item gives a snapshot of a certain time.

``` python
item = items[-1]
```

To get the geometry of your scene
``` python
item.geometrey
```
Will result in a polygon of the scene's area

To get the date of your scene
``` python
item.datetime
```

To get the properties of your scene
``` python
item.properties
```

**EXERCISE 2:** searching satellite scenes using metadata filters

Let's add a filter on the cloud cover to select the only scenes with less than 1% cloud coverage. How many scenes do now match our search?

Hint: generic metadata filters can be implemented via the query input argument of client.search, which requires the following syntax (see [docs](https://pystac-client.readthedocs.io/en/stable/usage.html#query-extension))
    

TIME 11.31
BREAK
TIME 11.46

**SOLUTION :** 
We are going to select scenes with less than 1 % cloud coverage.
               
```python
search = client.search(
    collections=[collection_id],
    intersects=point,
    datetime='2023-07-01/2023-08-31',
    query=['eo:cloud_cover<1']
)
```
' # Just to get rid of format issue
        
``` python
search.matched()
```

``` python
items = search.item_colleciton()
len(items)
```

Now we are going to save our search results in a file so we can reuse it lateron.

``` python
items.save_object('rhodes_sentinel-2.json')
```

We downloaded the metadata (not the data itself).

## Access the data

![](https://codimd.carpentries.org/uploads/upload_254ecfb6f9db3235bb1b4db9701aa0d1.png)

Let us take a look at one item from the list of items we have identified,        
``` python
item = items[0]
assets = item.assets
```

Loop through list of assets in the item,
```python
for key, asset in assets.items():
    print(key, ':', asset.title)
```
        
To see the actual data and see the thumbnail of it        
```python        
assets['thumbnail'].href
    
```
In the thumbnail you can already see the scorched area.
        
To render the image in **markdown** within jupyter:

```markdown
![](URL)
```
        
Now we are going to look at the real data. Let us take the Red band.
        
``` python
assets['red'].href
```
You will retrieve a URL to the real dataset. 

Now let us assign this to a variable
        
``` python
red_href = assets['red'].href
```

TIME 12.06
        
We are going to work with the rioxarray library to work with the multidimensional array data. It come from Climate research
        

To import this library:
``` python
import rioxarray
```
To access a function that reads the file of the red band

``` python
red = rioxarray.open_rasterio(red_href)
```

To explore the data
        
``` python
red
``` 

Now we want to store the red band locally on our machine
```
red.rio.to_raster('red.tif', driver='COG')
```

So we just opened the raster and saved it locally. (this can take some time, since it is ~60 mb )

# EPISODE 2: RASTER DATA IN PYTHON
                             
Now we are creating a new notebook to explore the raster objects.
                             
Raster_data_python.ipynb
                             
We are first going to import rioxarray
                             
``` python
import rioxarray
```
To open the file we saved in the previous episode,
``` python                             
red = rioxarray.open_rasterio('red.tif')
```
To explore the data
``` python
red.values
```
and
                             
``` python
red
```
You will see the values of your raster. 

Now we are going to open a smaller resolution of our dataset.

                             
                             
``` python                             
# overview level: access 3 zoom factor (is 8 times coarser resolution)
red = rioxarray.open_rasterio('red.tif', overview_level=2)
```                             

Let us visualize our raster
``` python
red.plot()
```                             
You will get a basic quick visualisation. You can configure your visualisation as well.

Robust true creates a clearer visualisation. You can also set the ranges using vmin and vmax (red.plot(vmin=0, vmax= 2000))

``` python
red.plot(robust=True)
```                         

to read the CRS.

``` python
red.rio.crs
``` 

Will give you the coordinate reference system.

If you want to see more about your CRS (the ESPG code for instance)
``` python
import pyproj
epsg = red.rio.crs.to_epsg()
crs = pyproj.CRS(epsg)
crs
```
This will give you all the information about the CRS. 

AFTER BREAK
13.30

A common issue with rasterdata. Most of the time rasters are squared. The data itself often is not.

How to deal with missing data/ missing values

In many programming languages this is defined as *Nan*

Let us open again our raster datafile:
``` python
red_nodata = rioaxarray.open_rasterio('red.tif, overview_level=2, masked=true)
```

Let us read our data:

```python
red_nodata
```
We switched from integers to floats. NaN cannot be encoded as integer, therefore the datatyps is converted from integer to float.

``` python
red_nodata.values
```
Are floats.

``` python
red.values
```
Are integers.

With the masked raster you will see it will be floats.

Let us have a look at multiband rasters. Images which are coded as RedGreenBlue (RGB) values. (truecolor images)

Go back to our search file (above 'rhodes_sentinel-2.json' around line 333)
``` python
import pystac
items = pystac.ItemCollection.from_file('rhodes_sentinel-2.json')

```

In case you did not save it, please download it here:
Search results: [rhodes_sentinel_file](https://raw.githubusercontent.com/esciencecenter-digital-skills/2023-10-10-ds-geospatial/main/files/rhodes_sentinel-2.json)

``` python
visual_href = items[0].assets['visual'].href
```

Now we are going to open the three bands
``` python
visual = rioxarray.open_rasterio(visual_href, overview_level=2)
```

To look in to the shape of the data,
```python 
visual.shape
```

Visualize the image
```python 
visual.plot()
```
You will get a histogram. xarray tries to estimate what your data would look like. Its first guess is a histogram. We however want to have a visualisation of the raster as an image. To do so:

```python
visual.plot.imshow()
```

Now you will see the image.


Note, if you masked this you will get an error, since it is looking for integers, whereas for the masked version the data is transformed to floats. 
                             
``` python
visual = rioxarray.open_rasterio(visual_href, overview_level=2 , masked=true)
```
This will make the visual.plot.imshow fail.

To plot, we need to normalize the data as follows,

```python
(visual / 255).plot.imshow()
```
However, the visual band is not intended for manipulation and thus does not encode the NAN values accurately. So, your image might have lots of white areas.


TIME 13.51
# EPISODE 3: VECTOR DATA

![](https://codimd.carpentries.org/uploads/upload_a4e2ff16bdcfc3988d44b397f84a1b8c.png)


For this episode we are going to use the following datasets.

1. Database of Global Administrative Areas GADM. Download link to obtain a boundary of our study area.
[GADM](https://gadm.org/index.html)

2.OpenStreetMap (OSM) data downloaded from geofabrik. Download link to retrieve the vital infrastructure and built-up areas of our study area.

[Open Street Map data](https://www.openstreetmap.org/#map=14/51.9890/5.9132).
[Download vector data](http://www.geofabrik.de/data/download.html)

We start a new notebook. 
Episode 3

Geopandas is a mature package that allows you to work with vector data as a dataframe with spatial attributes.(which allows you to perform all kind of analysis)

```python
import geopandas as gpd
```

Let us read the GADM dataset (we are using a geopackage file)

```python
gdf_greece = gpd.read_file('./data/gadm/greece.gpkg')
```

Now let us print it.

```python
gdf_greece
```

We looked into the data and found out that the island Rhodos can be isolated from the field "NAME_3"

Let us explore the data. Let us see which values are in the geodataframe

```python

gdf_greece['NAME_3'].unique()

```
The `unique()` function is used to identify all the unique values and to print the results.

Now let us select Rhodos:

```python

gdf_rhodes = gdf_greece.loc[gdf_greece['NAME_3']=='Rhodos']

```
Now let us have a look at the data through a beautiful plot!

```python
gdf_rhodes.plot()
```

Let us have a look at the geometry column

```python
type(gdf_rhodes['geomtery'])
```

Now that we have the polygon of the island Rhodos (from GADM), we are going to use it to make sub selections from our other data (Open Street Maps, we want to select roads and buildings).

We have 4 files:

Buildings, landuse, roads and transport

This is what we want to create:
![](https://codimd.carpentries.org/uploads/upload_cc57c435b310e026e9dfed0158a596dc.png)

We are going to make subselections from the datasets. 

# Infrastructure
Let us look at the roads.

```python
gdf_roads = gdf.read_file('./data/osm/roads.gpkg')
```

Let us have a look at the data

```python
gdf_roads.plot()
```

You will see also roads that are not on Rhodos. 
Therefore we are going to mask it with the polygon we got from the GADM dataset.

```python
gdf_roads = gpd.read_file('./data/osm/roads.gpkg', mask=gdf_rhodes)
```
Now let us have a look at the data again. You will now see a subselection of the roads, thus only the roads on Rhodos.

```python
gdf_roads.plot()
```

We see a lot of "roads" So let us have a look on what it contains.

```python
gdf_roads['fclass'].unique()
```

You will see that it also contains footpaths. We are however interested in main infrastructure. Which are primary, secondaty and tertiary labelled roads.

Let us make a list of attributes.

```python
infra_labels = ['primary', 'secondary', 'tertiary']

infra_roads = gdf_roads[gdf_roads['fclass']'.isin(infra_labels)]
```

Let us have a look a the data again.

```python
infra_roads.plot()
```
*note to teachers; rhodos and roads sound the same!*

BREAK 14.29 - 14.42

Now we are going to create a buffer around our "roads". However the CRS is not projected. We are therefore going to project our data in order to add a 100 meter buffer. We are therefore going to project the data, add a buffer and convert it back.

Step 1 we are making a variable that is project according to CRS 2100
``` python
infra_roads_meters = infra_roads.to_crs(2100)
```

Now check the crs of the projected layer
``` python
infra_roads_meters.crs
```

Now the data is projected we can create a buffer around it. 

```python
infra_roads_meters_buffer = infra_roads_meters.buffer(100)
```

You can plot the buffer using,
```python
infra_roads_meters_buffer.plot()
```
This may not be very meaningful given the size of the map being plotted.

When you look at your data now, you will see polygons.
```python
infra_roads_meters_buffer
```

We can reproject this buffered roads back to our original coordinate reference system,
```python
infra_roads_buffer = infra_roads_meters_buffer.to_crs(4326)
```
Let have a look at the data (buffer around the roads)
```python
infra_roads_buffer.plot()
```
Since we are going to do the same for our other datasets it makes sense to make our lives easier. To do so we are going to create a function which we can call later on to rerun a series of processing steps.

```python
def buffer_crs(gdf, size, meter_crs=2100, target_crs=4326):
    return gdf.to_crs(meter_crs).buffer(size).to_crs(target_crs)

```

Now let us test this function.

We had an object, now we want to create a buffer around it.

```python
infra_roads_buffer = buffer_crs(infra_roads, 100)
```
You will see it does the same, however now in a function. We can call this function over and over again.

Lets now do this for 2000 meters and plot it out,
```python
infra_roads_buffer_2000 = buffer_crs(infra_roads, 2000)
infra_roads_buffer_2000.plot()
```

## Built-up areas

Now are going to do the same thing for the built-up area.

``` python
gdf_landuse = gpd.read_file('./data/osm/landuse.gpkg', mask=gdf_rhodes)
```
Let us plot it!
```python
gdf_landuse.plot()
```
We want to create a geodataframe that only contains the built up area.

Let us explore the dataset. Let us make a thematic map.
```python
gdf_landuse.plot(column='fclass', legend=True)
```

While exploring the dataset we decidided to focus at specific land use types.
We put them in a list
```python
builtup_labels = ['commercial', 'industrial', 'residential' ]
```
Now we are creating a subset that only contains polygons with the listed attributes for fclass

```python
builtup_landuse = gdf_landuse[gdf_landuse['fclass'].isin(builtup_labels)]
builtup_landuse.plot(column='fclass', legend=True)
```

Let us do the same for buildings:

``` python
builtup_buildings = 
gpd.read_file('./data/osm/buildings.gpkg', mask = gdf_rhodes)
```

Let us have a look at the buildings

```python
builtup_buildings.plot()
```

### Exercise 3: Make 10m buffer for landuse and buildings

Now we have two components of built-up area: `builtup_landuse` and `builtup_buildings`. Can you grow a 10 meter buffer for both datasets? (Hint: use the function we have created for infrastructures.)

SOLUTION:

```python
builtup_landuse_buffer = buffer_crs(builtup_landuse, 10)
builtup_buildings_buffer = buffer_crs(builtup_buildings, 10)
```

Now let us see what we have!

```python
builtup_landuse_buffer.plot()
builtup_buildings_buffer.plot()
```
Next, we want to merge everything into one geodataframe. We are actually going to use pandas for this.

So let us load pandas in our notebook

```python
import pandas as pd
```

Now we can call the concat function to merge the geo dataframes

```python
builtup_buffer = pd.concat([builtup_buildings_buffer, builtup_landuse_buffer])
```

To see the result of the concatenation,
```python
builtup_buffer
```

Now we are going to add a for loop into our workflow.

```python
builtup_buffer = None
list_gdf = [builtup_landuse, builtup_buildings]

for gdf in list_gdf:
    # if it is none
    if builtup_buffer is None:
        builtup_buffer = buffer_crs(gdf,10)
    else:
        gdf_buffer = buffer_crs(gdf,10)
        builtup_buffer = pd.concat([builtup_buffer, gdf_buffer])
```

Run it and check the builtup buffer we created.

```python
builtup_buffer
```

TIME 15.34

### Exercise 4: Merge three buffers

There is still one component missing for the builtup area, which is the "transportation" dataset. It is stored in './data/osm/transport.gpkg'.

In this exercise, please:

- Load the transportation areas on Rhodes Islands.
- Grow a 10m buffer around the transportation datasets
- Merge the three buffers (10m buffers of landuse, buildings and tranportation) into one dataset.

(Hint: re-use the code!)

SOLUTION
```python
#First read in the file
builtup_tranport = gpd.read_file('./data/osm/transport.gpkg', mask=gdf_rhodes)

builtup_buffer = None
# we add builtup_transport
list_gdf = [builtup_landuse, builtup_buildings, builtup_tranport]

for gdf in list_gdf:
    if builtup_buffer is None:
        builtup_buffer = buffer_crs(gdf, 10)
    else:
        gdf_buffer = buffer_crs(gdf, 10)
        builtup_buffer = pd.concat([builtup_buffer, gdf_buffer])

```

Now we do our last step. 

Since a lot of the data is overlapping we are going to merge the buffers using union.

![](https://codimd.carpentries.org/uploads/upload_80b52f98efb2bd09a009867a869d1af9.png)


```python
infra_roads_buffer.unary_union
```

to check the data

```python
type(infra_roads_buffer.unary_union)
```
That provides a shapely file. However that looses the geospatial aspect.

``` python
gs_infrastructure = gpd.GeoSeries(infra_roads.buffer.unary_union, crs=infra_roads)buffer.crs)

gs_builtup = gpd.GeoSeries(builtup_buffer.unary_union, crs=builtup_buffer.crs)

```

Now let us explore our data

```python
gs_builtup
gs_builtup.plot()


gs_builtup.explode()
```

Last step is to glue the two datasets we created together. For this we are using panda's concat again.

```python
gs_assets = pd.concat([gs_infrastrcture, gs_builtup]).reset_index(drop=True)
```

As a last step we are assigning a zone identifier to the polygons

```python
data = {'code':[1, 2], 
        'type': ['infrastructure', 'builtup' ],
        'geometry': gs_assets}
```

Now we create a new geodataframe with the newly created data

```python
gdf_assets = gpd.GeoDataFrame(data)
```

Now let us save the processed file as a geopackage, which we can use tomorrow!

```python
gdf_assets.to_file('assets.gpkg')
```

To summarize, we had 4 layers for which we made a subselection based on the attributes, next we created buffers and merged them into a new dataframe.

TIME 15.59

# EPISODE 4: Align the data 

```python
import pystac
pystac.ItemColleciton.from_file("rhodes_sentinel-2.json")
```
We are going to look at the last image in this metadatafile.

```python
item = items[0]
visual_href = item.assets["visual"].href
```

Now let us load the actual data from the metadata file. We this we are uding rioxarray again.

```python 
import rioxarray
visual = rioxarray.open_rasterio(visual_href, overview_level=1)
visual
```

Now we are going to load the data with geopandas again.

```python
import geopandas as gpd
gdf_rhodes = gpd.read_file('assets.gpkg')
```

First trick is to crop the data based on the boudning box.

Let us have a look at the data:

```python
visual.plot.imshow()
```

By calling total_bounds we will find the min max of our dat (extent)


```python
assets.total_bounds

assets = assets.to_crs(visual.rio.crs)

```

Let us check the total bounds again.
```python
assets.total_bounds #minx,miny, maxx and maxy
```

Now we can do the cropping.

```python
visual.rio.clip_box? 
#to see what it does.
```

```python
visual_clipbox = visual.clip_box(*assets.total_bounds)
visual_clipbox.plot.imshow()#to see the subset of the data
```

So now we are going to crop this with our vector data

We are going to work on the visual clip.

```python
visual_clip = visual_clipbox.rio.clip(assets["geometry"])
```
Now let us have a look at it.

```python
visual_clip.plot.imshow()
``` 
Time 16.17


## 📚 Resources
[STAC Specifications](https://stacspec.org/)
[Earth Search by Element 84](https://stacindex.org/catalogs/earth-search#/)
[Shapely User Guide](https://shapely.readthedocs.io/en/stable/manual.html)
[Cloud optimized geotiffs](https://www.cogeo.org/)
[geofabrik](https://download.geofabrik.de/)
[GADM](https://gadm.org/download_country.html)
[Coordinate Reference System](https://epsg.io/)

[Via Appia project](https://revisited-via-appia.nl/)




