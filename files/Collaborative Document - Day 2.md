![](https://i.imgur.com/iywjz8s.png)


# Collaborative Document - Day 2

2023-10-11 Introduction to Geospatial Raster and Vector Data with Python

Welcome to The Workshop Collaborative Document.

This Document is synchronized as you type, so that everyone viewing this page sees the same text. This allows you to collaborate seamlessly on documents.

----------------------------------------------------------------------------

This is the Document for today: [link](https://tinyurl.com/2023-10-11-geospatial-python)

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

Results from DAY1:
 - [rhodes_sentinel-2.json](https://raw.githubusercontent.com/esciencecenter-digital-skills/2023-10-10-ds-geospatial/main/files/rhodes_sentinel-2.json)
 - [rhodes.gpkg](https://raw.githubusercontent.com/esciencecenter-digital-skills/2023-10-10-ds-geospatial/main/files/rhodes.gpkg)
 - [assets.gpkg](https://raw.githubusercontent.com/esciencecenter-digital-skills/2023-10-10-ds-geospatial/main/files/assets.gpkg)
 - [red.tif](https://raw.githubusercontent.com/esciencecenter-digital-skills/2023-10-10-ds-geospatial/main/files/red.tif)
 - [dem_rhodes_match.tif](https://raw.githubusercontent.com/esciencecenter-digital-skills/2023-10-10-ds-geospatial/main/files/dem_rhodes_match.tif)
 - [burned.tif](https://raw.githubusercontent.com/esciencecenter-digital-skills/2023-10-10-ds-geospatial/main/files/burned.tif)

## 👩‍🏫👩‍🙋💻🎓 Instructors & Helpers

Francesco Nattino, Ou Ku, Pranav Chandramouli, Maurice de Kleijn


## 👩‍💻👩‍💼👨‍🔬🧑‍🔬🧑‍🚀🧙‍♂️🔧 Roll Call


## 🗓️ Agenda

 
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
| 15:40 | Calculating Zonal Statistics on Rasters |
| 16:15 | Post-workshop Survey                    |
| 16:30 | **Drinks**                              | 

## 🏢 Location logistics
* Coffee and toilets are in the hallway, just outside of the classroom.
* If you leave the building, 
  be sure to be accompanied by someone from the escience center to let you back in through the groundfloor door
* For access to this floor you might need to ring the doorbell so someone can let you in
* In case of an emergency, you can exit our floor using the main staircase.
  Or follow green light signs at the ceiling to the emergency staircase.
* **Wifi**: Eduroam should work. Otherwise use the 'matrixbuilding' network, password should be printed out and available somewhere in the room.

## 🎓 Certificate of attendance
If you attend the full workshop you can request a certificate of attendance by emailing to training@esciencecenter.nl .

## 🔧 Exercises

## 🧠 Collaborative Notes

We strat with looking back on what we did.

![](https://codimd.carpentries.org/uploads/upload_2a687b52c3540b093a5ed1cbcb1d6f26.png)

Yesterday we downloaded that satellite images and did some basic image reading and exploring. Futhermore, we generated an assets layer based on vector data. For this we used vector data from GADM and Open streetmap. 

Today we are going to focus at parrallel computing and zonal statistics. 

First we need to activate the environment again to run our jupyter notebook

```bash
conda activate geospatial 
```

Next you type 

``` bash
jupyter-lab
```

We continue where we were yesterday. 

THIS IS A COPY PASTE OF YESTERDAY - START

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
THIS IS A COPY PASTE OF YESTERDAY - END

From this point we continue.

Next, we are going to look at how to align two raster files. 

# Match two raster files

For this we will use the Digital Elevation Model (DEM) = rasterized version of the world. Every pixel in the raster represents the height. [wiki](https://en.wikipedia.org/wiki/Digital_elevation_model)

Let us open the file. 

```python
dem = rioxarray.open_rasterio('./data/dem/rhodes.tif')
dem
```

First check we need to do is to check the CRS.

``` python
dem.rio.crs
```
Now we are clipping the data. Using it to resample the data to the clipped image we processed before. First we check the crs.

``` python
visual_clip.rio.crs
```

Next we reproject them

``` python
dem.rio.reproject_match?
``` 

Check 
- size of the pixels
- resampling method that is used

``` python
dem_matched = dem.rio.reproject_match(visual_clip)
```

Check the results. You will find that the size of the images has changed.

Now we are going to save this file locally.

``` python
dem_matched.rio.to_raster('dem_rhodes_match.tif', driver = 'COG')
```
It uses the projection of the visual clip to the target raster.


# Episode 5 Raster calculations

Now we are going to process the satellite images to distinguish burned vs not burned areas.

[more info here](https://custom-scripts.sentinel-hub.com/sentinel-2/burned_area_ms/)

We want to get something like this:


```python
import pystac
```

Get our data from the metadatafile we created yesterday

``` python
items = pystac.ItemCollection.from_file('rhodes_sentinel-2.json')
```

``` python
item = items[0]
```
Get the red band and near infrared band
``` python
red_href = item.assets["red"].get_absolute_href()
nir_href = item.assets["nir"].get_absolute_href()
```

We want to work with raster data

```python
import rioxarray
```
```python
red = rioaxarray.open_raterio(red_href, masked=True, overview_level=1)

nir = rioaxarray.open_raterio(nir_href, masked=True, overview_level=1)
```

We are interesting in a specific area, thus clip for Rhodos. For this we use the polygon we created and stored yesseeet

``` python
import geopandas
```
``` python 
rhodes = geopandas.read_file('rhodes.gpkg')
rhodes_reprojected = rhodes.to_crs(red.rio.crs)
```
Now we have the data loaded and reprojected

Let us get our bounding box

``` python
bbox = rhodes_reprojected.total_bounds
````
show it
``` python
bbox

````

Now we are going to clip the NIR and REd bands 

``` python
red_clip = red.rio.clip_box(*bbox)

nir_clip = red.nir.clip_box(*bbox)

```

Now let us have a look at the Red band data
``` python
red_clip.plot(robust=True)
```

Lets have a look at the NIR

``` python
nir_clip.plot(robust=True)
```

You will aready see some scorched areas.

We now have our data ready (reprojected, clipped etc.)


# Raster math

We are going to do some raster math.

Let us check if the shapes are aligned.

``` python
print(red_clip.shape, nir_clip.shape)
```

NDVI is calculated as

NDVI = (nir - red)/ (nir + red)

Now let us code this.

```python
ndvi = (nir_clip - red_clip) / (nir_clip + red_clip)
```

To show the output as a map
``` python
ndvi.plot()
```

To show it as a histogram
``` python
ndvi.plot.hist()
```

To optimize the histogram you can add min and max

``` python
ndvi.plot(vmin=0, vmax=1)
```

### Exercise: NDWI and custom index to detect burned areas

Calculate the other two indices required to compute the burned area classification mask, specifically

* The [normalized difference water index (NDWI)](https://en.wikipedia.org/wiki/Normalized_difference_water_index), derived from the **green** and **NIR** bands (with band ID "green" and "nir", respectively)

$NDWI = \frac{green - NIR}{green + NIR}$

* A custom index derived from the  1600 nm and the 2200 nm **short-wave infrared (SWIR)** bands ( "swir16" and "swir22", respectively):

$INDEX = \frac{SWIR_{16} - SWIR_{22}}{SWIR_{16} + SWIR_{22}}$

SOLUTION:
We are creating a function, since we are going to do the same over and over again


``` python
def get_band_and_clip(item, band_name, bbox, ov_level=1):
    href = item.assets[band_name].get_absolute_href()
    band = rioxarray.open_rasterio(href, masked=True, overview_level=ov_level)
    return band.rio.clip_box(*bbox)


```


```python
green_clip = get_band_and_clip(item, 'green', bbox)
swir16_clip = get_band_and_clip(item, 'swir16', bbox, ov_level=0) 
swir22_clip = get_band_and_clip(item, 'swir22', bbox, ov_level=0) 
```

Now do the math!

``` python
ndwi = (green_clip - nir_clip)/(green_clip + nir_clip)
index = (swir16_clip - swir22_clip)/(swir16_clip + swir22_clip)
```

Let us look at the result by plotting it.

```python
ndwi.plot(robust=True)
```

Now we are going to look at the burned, but first we check the resolution.

```python
ndvi.rio.resolution(), ndwi.rio.resolution(), index.rio.resolution()
```

We need to reproject the data in order to acutally do the math

```python
index_match = index.rio.reproject_match(ndvi)
swir16_clip_match = swir16_clip.rio.reproject_match(ndvi)
```

To confirm it works check the resolution
``` python
index_match.rio.resolution()
```

Burned areas are considered.

burned = 
ndvi <= 0.3 
ndwi <= 0.1
index_match + nir_clip <= 0.1
blue_clip <= 0.1 
swir16_clip_match >= 0.1

we got these settings [here](https://custom-scripts.sentinel-hub.com/sentinel-2/burned_area_ms/)

We did not have the blue band yet.

```python
blue_clip = get_band_and_clip(item, 'blue', bbox)
``` 

Now let us implement the formula

```python
burned = (
    (ndvi <= 0.3) & 
    (ndwi <= 0.1) &
    ((index_match + nir_clip) <= 0.1) &
    (blue_clip <= 0.1) & 
    (swir16_clip_match >= 0.1)
)

```

The reflectance is not taken into account. We should check. It needs to be divided to 10.000 to actually work with it. Therefore you should divide it by 100000

```python

burned = (
    (ndvi <= 0.3) & 
    (ndwi <= 0.1) &
    ((index_match + nir_clip/10000) <= 0.1) &
    ((blue_clip/10000) <= 0.1) & 
    ((swir16_clip_match/10000) >= 0.1)
)
```

Let us have a look at burned

``` python
burned
```

The burned result has 3 indeces. Let us use a squeeze it.

``` python
burned = burned.squeeze()
```

Let us see what happened

``` python
burned.shape
```

```python
visual_href = item.assets['visual'].get_absolute_href()
visual = rioxarray.open_rasterio(visual_href, overview_level=1)
visual_clip = visual.rio.clip_box(*bbox)
```

Let us compare the two.

```python
visual_clip.plot.imshow()
```

Now we are going to set the red channel as maximum and set the rest to 0. This creates a clear distinction of burned areas.

We want to set the Red band to be equal to 255.

We are using a negation of the mask. We want to keep the value for the non burned area and make the burned areas red.
```python
visual_clip[0] = visual_clip[0].where(~burned, 255)
visual_clip[1:3] =`visual_clip[1:3].where(~burned, 0)
```

Let us have a look!
``` python
visual_clip.plot.imshow()
```
Let us save this burned mask to a file.

``` python
burned.rio.to_raster('burned.tif', dtype='int8')
```


# Episode 6

TIME 11.15

![](https://codimd.carpentries.org/uploads/upload_c74dc2d37b5e5090656c8f0ef50322c4.png)

Working with rasters there are multiple operations.

Different Operations
**Local** 	 Operations performed on a cell by cell basis
**Focal**    Operations performed using a moving group of cells
**Zonal**    Operations performed using zones (groups of cells having the same value)
**Global**   Operations performed using the whole grid

In the previous chapter we have been doing Local Raster statistics. We calculated with the cells (for example):

![](https://codimd.carpentries.org/uploads/upload_0601dd3d0bb6c9d90ffa881d609816b1.png)


Now we are going to do look at the cells around it. We are calculating the mean. This is considerd focal statistics.

![](https://codimd.carpentries.org/uploads/upload_3780b5b7bb9e3cbc9f4c958504da9b16.png)

Technically we use focal statistics to do parallel raster calculations

![](https://codimd.carpentries.org/uploads/upload_01a78fd6ee4c0b134d7e83fd577b9f61.png)

We are going to split our array into different chunks. Then we group the different chuck to different parts of our memory. This will increase your performance. 

Chuck size and shape will have a huge impact on the performance. 

The python library we use for this is [DASK](https://www.dask.org/).

- Python library for parallel and distributed computing.
- Different data structures and abstractions – we'll use Dask Arrays.
- Well integrated with (rio)xarray.

Until now we have been loading our data as NumPy arrays. Now we use Dask arrays on top of it.
![](https://codimd.carpentries.org/uploads/upload_c2038977793091b538ec6c311b69e181.png)

Exploiting the fact we have multiple cores in our local machines.


**Parallel or serial..?**

Be careful in deciding to do the parallelization. It is not always the best way to go. Some points the think of:

- Performance gain highly depends on:
  - Operations/algorithms
  - Size of the dataset
  - "Parameters", e.g. size (and shape) of the chunks
- Parallelization brings overhead – serial calculations can be faster!
- Always start from serial calculations, time profile 

DASK needs to orchestrate all the small operations. 

TIME 11.35 BREAK
START 11.48

We are going to open the DEM, smooth it to remove errors, next we are going to calculate the slope.

We are first going to do this serial. After which we are going to do it in parallel. After that we are going to compare the two of them.

We are opening a new notebook. Before that it makes sense to shut down other kernels in jupyter lab

Let us import the library

``` python
import rioxarrey
```

Get the data we previously created

``` python
dem = rioxarray.open_rasterio('dem_rhodes_match.tif', masked=True)

```

Now we squeeze it again
``` python
dem = dem.squeeze()
```

``` python
dem.load()
```
Note that by default data is loaded using Numpy arrays as underlying data structure. We can visualize the raster:

``` python
dem.plot (vmin=0)
```
We can verify if there are negative values in the dataset.

``` python
dem.min()
```
We want to exclude the negative values. 

To explore the data we can make selections as well. For this you can use the sel function
``` python
dem_y = dem.sel(y=4000000, method='nearest') #or 4_000_000 which is purely visual to make it easier to read for humans
```
Let us have a look at it
``` python
dem_y.plot()
```

Now we want to compute the slope (focal)

xarray spatial has already some good spatial function. Like slope calculations. Let us import it.

``` python
import xrspatial 
```
Let us use the slope function

``` python
slope = xrspatial.slope(dem)
```
Let us have a look at it again

``` python
slope.plot.imshow(figsize = (40,30))
```

You see that the slope has a couple of not very realistic slopes (or at least debatable/ very noisy ). Therefore we need to smoothen the original DEM. We are doing this with medianfilter

We are now constructiing a rolling window that processes it over your dataset. We set the window to 9 x 9 meter

We also want to time the step.
In jupyter you can do it with jupyter magics. These can be used through the %% characters. 
%%time created the time for the cell
%time for the line
Magic needs to be be in the top of the cell
``` python
%%time
dem_smooth = dem.rolling(x=9, y=9, center=True).median()
```
Let us now do the slope calculation on the smoothend DEM
This should be 5-7 seconds (depening on your machine as well)

``` python
slope_smooth = xrspatial.slope(dem_smooth)
```

To plot out the smoothed out slope you can use, (Use figsize argument to play around with the size of the image)
``` python
slope_smooth.plot(figsize=(20,20))
```
Now we need to convert this slope units from degrees to percentages 
Let us convert them. From degrees to radians to percentages.

For this we need numpy functions, thus import it
``` python
import numpy as np
```
Now let us do the conversion

``` python
slope_smooth_perc = np.tan(np.radians(slope_smooth))
```
Let us save it again

``` python
slope_smooth_perc.rio.to_raster('slope.tif', driver='COG')
```

Let us do the first step of the parallel approach

# Parallel
First we want to open the raster file as a chucked version. To add it as chucked dask array. We use the red.tif we created yesterday.

We are breaking it into chucks of 4000 x 4000.
``` python
red = rioxarray.open_rasterio('red.tif', chunks = (1, 4000, 4000))

```
The representation of the array changed. You can see this by visualizing the variable


``` python
red
```
TIME LUNCH 12.30 to 13.31

### Exercise: Chunk sizes matter

We have already seen how COGs are regular GeoTIFF files with a special internal structure. Another feature of COGs is that data is organized in “blocks” that can be accessed remotely via independent HTTP requests, enabling partial file readings. This is useful if you want to access only a portion of your raster file, but it also allows for efficient parallel reading. You can check the blocksize employed in a COG file with the following code snippet:

```python
import rasterio
with rasterio.open('/path/or/URL/to/file.tif') as r:
    if r.is_tiled:
        print(f"Chunk size: {r.block_shapes}")
```

In order to optimally access COGs it is best to align the blocksize of the file with the chunks employed when loading the file. Open the red-band asset (the “red” asset) of a Sentinel-2 scene as a chunked `DataArray` object using a suitable chunk size. Which elements do you think should be considered when choosing the chunk size?

What are good practices in choosing the size of the chucks?

``` python
import rasterio
with rasterio.open('red.tif') as r:
    if r.is_tiled:
        print(f"Chunk size: {r.block_shapes}")
```
If you are familiar with gdal tools you need to do it as command line. In Jupyter notebook y9ou can do that with !

``` bash
!gdalinfo red.tif
```

You want to have them as a size that is not too small and big enough for the parallelization. 

Rule of thumb according to Francesco :-) Configure the size of the chunks 100mb. In this case with this setting you get 500 kb. Which is too small

``` python
red = rioxarray.open_rasterio('red.tif', chunks=(1, 512, 512))

```

``` python
red = rioxarray.open_rasterio('red.tif', chunks=(1, 6144, 6144))
```
Results in 72 mb. This is generally a good size for the chucks.

DASK can also help you. For this you can use the following auto function.
You can use it, but you should not trust it.

``` python
red = rioxarray.open_rasterio('red.tif', chunks='auto')
```

Let us use this approach on our workflow. 

We are going to use this on the DEM and to calculate the slope. To compare it with what we did serial before.

``` python
dem_dask = rioxarray.open_rasterio('dem_rhodes_match.tif', masked=True, chunks=(1, 512, 512), lock=False)
```
The locking is to make sure it is not synchronized. 

Let us have a look at the data.

``` python
dem_dask
```
Now we want to move on. Let us see how fast this is compare to approach presented above.

Load the data and start 4 threads to let is work on. Threads are more ligthweight. We choose 4, we would recommend that you align this with the number of cores you have in your local machine (in our case 4). You define that by adding num_workers.

``` python
dem_dask = dem_dask.persist(scheduler='threads', num_workers=4)
```
Now let us squeeze it again.

``` python
dem_dask = dem_dask.squeeze()
```

Now let us do the smoothening and see how it behaves


``` python
%%time
dem_smooth_dask = dem_dask.rolling(x=9, y=9, center=True).median()
```
The wall time will be very short. Too short to have actually done something.
Dask is a bit lazy, so we actually need to really ask dask to run them. 

To get a idea of what is going on. You can look at a visualisation of the series of tasks that dask is building up. For this you can use
``` python
import dask
dask.visualize(dem_smooth_dask)
```
You will see 9 starting points (which relate to the chunks you defined above). (this will create a .png as well) Next, we will execute it again. 

``` python
%%time
dem_smooth_dask = dem_smooth_dask.persist(scheduler='threads', num_workers=4)
```
Should be about 2-3 seconds (depending on your machine as well) Twice as fast compared to the first approach.

Now let us calculate the slope on it
``` python
xrspatial.slope(dem_smooth_dask)
```
This is again a lazy operation, we therefore need to assign it to a variable


``` python
slope_dask = xrspatial.slope(dem_smooth_dask)

```
Next, we need to convert it to percentages again
``` python
slope_percentage_dask = np.tan(np.radians(slope_dask))

```
Now write this as a raster file.
We need to add a lock to the raster to allow the parallelization in multiple threads.

``` python
from threading import Lock
slope_percentage_dask.rio.to_raster('slope_dask.tif', driver='COG', lock=Lock())
```
TIME 14.38 BREAK

TIME 14.52

# Episode 6 Zonal Statistics

We will do zonal statistics 


![](https://codimd.carpentries.org/uploads/upload_6ff8cc805ca170a7e302741108e1f292.png)


We start a new Jupyter Notebook

Let us load the data from the previous episodes
``` python

import rioxarray
burned = rioxarray.open_rasterio('burned.tif')
burned
```

The vector data

``` python
import geopandas as gpd
assets = gpd.read_file('assets.gpkg')
assets
```

Now that we have our data. We check the CRS.


```python
burned.rio.crs
````

```python
assets.crs
```

There is a diffences,  so we need to align the CRSs

```python
assets = assets.to_crs(burned.rio.crs)
````

Now we are going to rasterize our vector dataset (i.e. assets)

``` python
geom = assets[['geometry', 'code']].values.tolist()

```
Now let us check this

``` python
geom
```
We now has va list with the polygon and multipolygon

Let us have a look at the burned layer


``` python
burned.shape
```
You will see an dimension we need to get rid of. We use Squeeze for this.


``` python
burned_squeeze = burned.squeeze()
```
We want to go from our vector data to a raster dataset.

``` python
# Rasterize the vector data.
# transform represents the projection from pixel space to the projected coordinate space. 
# By default, the pixels that are not contained within a polygon in our shapefile will be filled with 0.
from rasterio import features
assets_rasterized = features.rasterize(geom, out_shape=burned_squeeze.shape, transform=burned.rio.transform())
```
Now let us take a look at the rasterized dataset

``` python
assets_rasterized
```
It does not look very fancy, since it is a numpy array (and not xarray, thus not spatial data)

``` python
type(assets_rasterized)
```

So let us make it into spatial data

``` python
numpy.ndarray
```
# Create an Xarray.DataArray from Results

We are going to create an empty xarray and append the data to it
``` python
assets_rasterized_xarr = burned_squeeze.copy()
assets_rasterized_xarr

```

``` python
assets_rasterized_xarr.data = assets_rasterized
```

You will see that the data is appended. Let have a look at the dataset.

``` python
assets_rasterized_xarr.plot()
```

So now we are ready for the zonal statistics!


We want to knwo for the different zones how much of these are effected by the burned areas. 

![](https://codimd.carpentries.org/uploads/upload_3bf57a0c2382b5314a86c1c079d65e9f.png)

We are using this library for this.

``` python
from xrspatial import zonal_stats
```
Now let us configure the zonal stats


``` python
stats = zonal_stats(assets_rasterized_xarr, burned_squeeze)
```

Now look at the stats

``` python
stats
```

### Exercise: Zonal stats over slope zones

Now let's look into the burned areas in relation to slope classes like [Zhai et al., 2023](https://www.mdpi.com/1999-4907/14/4/807) have done. To reproduce their analysis, we will:
1. Reclassify the slope classes into 5 categories  0%–5%, 5%–10%, 10%–15%, 15%–25%, and above 25%. Then,
2. Perform the zonal statistics on the above categories.

Hints:
1. Load slope data from 'slope_dask.tif'.
2. The big chellenge will be how to compute the slope zones. Consider:
    - Convert slope data to zones using [`numpy.digitize`](https://numpy.org/doc/stable/reference/generated/numpy.digitize.html).
    - Use [`xarray.apply_ufunc`](https://docs.xarray.dev/en/stable/generated/xarray.apply_ufunc.html) to apply `numpy.digitize` to all the elements in an Xarray. 
3. Note that the slope data and burn index data are in different resolution.




**From the article**:
Consistent with Oliveira et al. (2014) [37], our research found that lands with slopes greater than 25% were significantly less fire-prone than those with lower slopes. Previous research explained that steeper areas are less accessible, and as a result, human beings are less likely to ignite wildfires. In addition, there is less vegetation and accumulated fuel on steeper slopes, so the vegetation structure is less flammable [64,65]. However, Mermoz et al. (2005) [64] and Carmo et al. (2011) [33] found that wildfires are more likely to occur on steep slopes, and explained that land cover types on steep slopes were most likely to burn [24,66].


**Solution** 

# Step 1 Load slope_dask.tif

``` python
slope = rioxarray.open_rasterio('slope_dask.tif', masked=True).squeeze()

```
# Step 2 Bin the slopes
``` python
# Defines the bins for pixel values
# Zone 0 will be values <=0.; Zone 5 will be values >0.25; Zone 6 will be NaN values(>1000)
slope_bins = (0., 0.05, 0.1, 0.15, 0.25, 1000)
```
 
``` python
# Method 1:
# def rasterize_slopes(data):
#     func=lambda x: np.digitize(x, slope_bins, right=False)
#     return xr.apply_ufunc(func, data)
#
# binned_sloped = rasterize_slopes(slope)

# Method 2
slope_zones = xr.apply_ufunc(
    np.digitize,
    slope,
    slope_bins
)
```

# Step 3 Check the size


``` python
slope_zones.shape
```

``` python
burned_squeeze.shape
```

To reproject the data 
``` python
slope_zones = slope_zones.rio.reproject_match(burned_squeeze)
```

# Step 4 call the zonal stats!

``` python
stats_slopes = zonal_stats(slope_zones, burned_squeeze)

```

``` python
stats_slopes
```


## 📚 Resources

- [Burned area index](https://custom-scripts.sentinel-hub.com/sentinel-2/burned_area_ms/)
- [Sentinel-2 units (digital numbers to reflectance)](https://docs.sentinel-hub.com/api/latest/data/sentinel-2-l2a/#units)

- [**Post-workshop survey**](https://www.surveymonkey.com/r/HYDZCG6)