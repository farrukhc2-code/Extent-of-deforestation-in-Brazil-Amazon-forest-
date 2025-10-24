# NDVI-based remote sensing analysis of deforestation in the Brazilian Amazon (2014–2022) using Landsat 8/9 imagery and supervised classification techniques.Extent-of-deforestation-in-Brazil-Amazon-forest

📘 Overview

This project analyzes the extent and causes of deforestation in a section of the Brazilian Amazon rainforest between 2014 and 2022, using satellite imagery (LANDSAT 8 and 9).
By generating NDVI (Normalized Difference Vegetation Index) composites and supervised classification, the project quantifies vegetation loss and identifies human-driven land-use changes such as grazing, poor farming practices, and over-cultivation.

🎯 Objectives

Quantify the extent of vegetation loss from 2014 to 2022.

Determine the primary causes of deforestation (human vs. natural).

Apply remote sensing techniques to assess land cover change.

Visualize temporal NDVI trends using satellite imagery analysis.

🗺️ Study Area

Location: Southern Amazon Rainforest, Brazil

Coordinates:

NW Corner: 07° 34’ 14”S, 061° 48’ 29”W

SE Corner: 08° 08’ 02”S, 061° 13’ 21”W

Characteristics:

Sparsely populated farmland region

Road-centered “sawtooth” deforestation pattern

🧩 Methodology

Data Collection:

Satellite Imagery: LANDSAT 8 & LANDSAT 9 (2014–2022)

Selected images during dry season with <10% cloud cover.

Preprocessing:

Cloud masking applied to minimize NDVI distortion.

Bands selected for vegetation analysis (NIR & Red).

NDVI Calculation:

Computed yearly NDVI composites to assess vegetation health.

NDVI = (NIR - Red) / (NIR + Red)

Supervised Classification:

Classified areas into vegetation, bare soil, and water.

Used training samples to differentiate land cover types.

Change Detection:

Compared NDVI and classification results between 2014, 2018, and 2022.

Quantified total area of vegetation loss.

🧰 Tools & Technologies
Tool / Library	Purpose
Google Earth Engine (GEE)	Satellite data access and NDVI computation
ArcGIS Pro / QGIS	Spatial visualization and land cover classification
Python (rasterio, numpy, matplotlib)	Image analysis and plotting
Landsat 8 & 9 Data (USGS)	Multispectral imagery
Excel / Pandas	Quantitative change analysis

```python
import rasterio
import numpy as np
import matplotlib.pyplot as plt

# Load Red and NIR bands for 2014
red_2014 = rasterio.open("LANDSAT8_2014_B4.tif").read(1).astype(float)
nir_2014 = rasterio.open("LANDSAT8_2014_B5.tif").read(1).astype(float)

# Load Red and NIR bands for 2022
red_2022 = rasterio.open("LANDSAT9_2022_B4.tif").read(1).astype(float)
nir_2022 = rasterio.open("LANDSAT9_2022_B5.tif").read(1).astype(float)

# Compute NDVI
ndvi_2014 = (nir_2014 - red_2014) / (nir_2014 + red_2014)
ndvi_2022 = (nir_2022 - red_2022) / (nir_2022 + red_2022)

# Compute NDVI change
ndvi_change = ndvi_2022 - ndvi_2014

# Plot NDVI maps
fig, axs = plt.subplots(1, 3, figsize=(15, 5))
axs[0].imshow(ndvi_2014, cmap='YlGn')
axs[0].set_title("NDVI 2014")

axs[1].imshow(ndvi_2022, cmap='YlGn')
axs[1].set_title("NDVI 2022")

axs[2].imshow(ndvi_change, cmap='RdYlGn')
axs[2].set_title("NDVI Change (2014–2022)")
plt.show()

```
Another method used was using google earth engine code
```javascript
// Load Landsat 8 and 9 imagery
var area = ee.Geometry.Rectangle([-61.81, -8.08, -61.22, -7.57]);
var landsat2014 = ee.ImageCollection('LANDSAT/LC08/C01/T1_SR')
  .filterBounds(area)
  .filterDate('2014-06-01', '2014-09-30')
  .median();

var landsat2022 = ee.ImageCollection('LANDSAT/LC09/C02/T1_L2')
  .filterBounds(area)
  .filterDate('2022-06-01', '2022-09-30')
  .median();

// Compute NDVI
var ndvi2014 = landsat2014.normalizedDifference(['B5', 'B4']).rename('NDVI');
var ndvi2022 = landsat2022.normalizedDifference(['B5', 'B4']).rename('NDVI');

// Calculate NDVI change
var ndviChange = ndvi2022.subtract(ndvi2014);

// Display
Map.centerObject(area, 10);
Map.addLayer(ndvi2014, {min:0, max:1, palette:['white','green']}, 'NDVI 2014');
Map.addLayer(ndvi2022, {min:0, max:1, palette:['white','green']}, 'NDVI 2022');
Map.addLayer(ndviChange, {min:-0.5, max:0.5, palette:['red','white','green']}, 'NDVI Change');



```
📊 Results

Vegetation cover decreased significantly between 2014 and 2022.

The rate of deforestation accelerated after 2018, correlating with agricultural expansion.

NDVI difference maps show a shift from dense vegetation (green) to bare soil (white/yellow).

Supervised classification confirmed major loss in forested land cover.

🌱 Conclusion

The Amazon rainforest continues to experience measurable degradation due to human activities.

Vegetation loss is accelerating, and may not be easily reversible without major conservation efforts.

Satellite imagery and NDVI analysis provide powerful tools for monitoring environmental change over time.
