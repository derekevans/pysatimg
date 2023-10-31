
# stac-extract
A python package for extracting raster data for an area of interest from the SpatioTemporal Asset Catalogs (STAC).

Note: This is a work in progress. 

## Usage

```python
import pysatimg
import geopandas as gpd
from datetime import date
from shapely.geometry import box

# Create the area of interest (AOI) which can be a gpd.GeoSeries or gpd.GeoDataFrame
# This can also be loaded from a file (.shp, .gpkg, etc.) using gpd.read_file
polygon = box(306040, 4431910, 306830, 4432720)
aoi = gpd.GeoSeries(polygon, crs="EPSG:32616")

# Generate multiband rasters in the AOI with blue, green, red and scl bands for each available date in the date range  
extractor = pysatimg.Extractor(
    source_name='sentinel-2-l2a', 
    aoi=aoi, 
    pixel_size=(10, -10), 
    out_dir='/path/to/output/rasters', 
    start_date=date(2023,7,4), 
    end_date=date(2023, 7,7), 
    assets=['blue', 'green', 'red', 'scl'],
    n_threads=2
)
extractor.extract()
```

From the command line:
```sh
pysatimg_extract.py --aoi '/path/to/input/aoi.shp' --source sentinel-2-l2a --pixel_x 10 --pixel_y -10 --start_date 2023-07-04 --end_date 2023-07-07 -a 'red' -a 'green' -a 'blue' --n_threads 8 --out_dir '/path/to/output/rasters'
```
### Sources
List all available sources:
```python
from pysatimg.utils.sources import pprint_sources

pprint_sources()
```

From the command line:
```sh

```
## Development

To install the development environment for this package, you first need to have Docker Desktop installed.  Once installed:

```sh
git clone https://github.com/derekevans/pysatimg
cd pysatimg
make docker/build
make docker/attach
make install
```

### Jupyter Lab
To install Jupyter Lab and start the Jupyter server:

```sh
cd pysatimg
make docker/attach
make jupyter/install
make jupyter/start/dev
```

Once started, you can access Jupyter Lab on http://localhost:8889/lab

## TODO:
1. Add docstrings and function annotations
2. Write tests
3. Suppress GDAL output b/c it effects progress bar
4. Add additional sources