
# 🌦️ Pakistan Climate Bivariate Mapping 


[![Bivariate Map]](https://github.com/adnanabbasshah/Bivariate-Map-/blob/main/bivaerate%20map1.png?raw=true)


## A Complete Workflow for Analyzing and Visualizing Temperature-Precipitation Relationships Across Pakistan (1990–2023)

**Author**: [Adnan Abbas Shah](https://github.com/adnanabbasshah)  
**LinkedIn**: [linkedin.com/in/adnan-abbas-shah](https://www.linkedin.com/in/adnan-abbas-shah/)  
**License**: MIT  
**DOI**: _To be updated_  

---

## 📌 Table of Contents

- [✨ Features](#-features)  
- [📁 Repository Structure](#-repository-structure)  
- [💾 Installation](#-installation)  
- [🚀 Usage](#-usage)  
- [🔄 Workflow](#-workflow)  
- [📊 Results](#-results)  
- [🤝 Contributing](#-contributing)  
- [🧾 License](#-license)  
- [📚 Citation](#-citation)  

---

## ✨ Features

- ✅ **Automated Data Pipeline**: From raw ERA5 data to publication-ready maps  
- ✅ **Bivariate Visualization**: Customizable temperature-precipitation mapping  
- ✅ **Reproducible Research**: Containerized with Docker and `renv`  
- ✅ **Multi-Scale Analysis**: From national to district-level insights  

---

## 🚀 Step-by-Step Tutorial: Creating a Bivariate Climate Map of Pakistan using GEE and R

Climate change is reshaping Pakistan’s environment in profound ways. This tutorial demonstrates how to visualize the temperature and precipitation relationship using Google Earth Engine and R (1990–2023).

### 🔹 Step 1: Data Extraction from Google Earth Engine
```javascript
// ERA5 Temperature & Precipitation Extraction from GEE
var startDate = ee.Date('1990-01-01');
var endDate = ee.Date('2023-12-01');
var era5 = ee.ImageCollection("ECMWF/ERA5/DAILY")
    .filterDate(startDate, endDate)
    .filterBounds(pakistan);
// Temperature (Kelvin to Celsius)
var dailyMean = era5.map(function(img) {
    var tavg = img.select('minimum_2m_air_temperature')
        .add(img.select('maximum_2m_air_temperature'))
        .divide(2)
        .subtract(273.15)
        .rename('daily_mean_temp');
    return tavg.copyProperties(img, ['system:time_start']);
});
// Precipitation (meters to mm)
var dailyPrecip = era5.select('total_precipitation')
    .map(function(img) {
        return img.multiply(1000).rename('precip_mm_day');
    });
Export.image.toDrive({...});
```

### 🔹 Step 2 to 7: R Processing and Visualization (Summary)

With our data downloaded, we'll use R for processing and visualization:
```r
# Load required packages
library(tidyverse)
library(sf) # For spatial data handling
library(terra) # Raster processing
library(biscale) # Bivariate mapping
library(cowplot) # Plot arrangement
library(extrafont) # For font management
# Import Pakistan boundary shapefile
pakistan <- st_read("Pakistan_with_Kashmir.shp")
```
## Step 3: Processing Temperature Data
```r
# Load and resample temperature raster
temp_original <- rast("mean_temp_1990_2023.tif")
target_res <- rast(ext(temp_original), resolution = 0.025) # ~2.5km resolution
temp_resampled <- resample(temp_original, target_res)
# Crop to Pakistan boundaries
temp_cropped <- crop(temp_resampled, pakistan, mask = TRUE)
```
## Step 4: Processing Precipitation Data
```r
# Repeat for precipitation data
ppt_original <- rast("mean_precip_1990_2023.tif")
ppt_resampled <- resample(ppt_original, target_res)
ppt_cropped <- crop(ppt_resampled, pakistan, mask = TRUE)
```
## Step 5: Creating Bivariate Classes
```r
# Combine into a data frame
climate_data <- c(temp_cropped, ppt_cropped) |>
 as.data.frame(xy = TRUE) |>
 na.omit()
# Create bivariate classes
climate_data <- bi_class(climate_data,
 x = mean_temp_1990_2023, 
 y = mean_precip_1990_2023,
 style = "quantile", 
 dim = 4) # 4x4 classes
```
## Step 6: Building the Map
```r
# Base map
main_map <- ggplot() +
 geom_raster(data = climate_data, 
 aes(x = x, y = y, fill = bi_class)) +
 bi_scale_fill(pal = "BlueOr", dim = 4) +
 geom_sf(data = pakistan, fill = NA, color = "black", linewidth = 0.7) +
 labs(title = "Pakistan: Temperature and Precipitation Patterns (1990–2023)",
 caption = "Source: ERA5 Climate Data | Author: Your Name") +
 theme_void() +
 theme(plot.title = element_text(hjust = 0.5, face = "bold"))
# Create legend
bivar_legend <- bi_legend(pal = "BlueOr",
 dim = 4,
 xlab = "Temperature (°C)",
 ylab = "Precipitation (mm)")
# Combine map and legend
final_map <- ggdraw() +
 draw_plot(main_map, 0, 0, 1, 1) +
 draw_plot(bivar_legend, 0.65, 0.1, 0.3, 0.3)
```
## Step 7: Exporting the Final Visualization
```r
ggsave("pakistan_climate_map.png", final_map, 
 width = 10, height = 8, dpi = 300)

<img width="637" alt="bivaerate map1" src="https://github.com/user-attachments/assets/3f4cba6a-2d7c-4dbf-a6bd-615b134c7d0f" />

## 📊 Key Findings from the Visualization

1. **Extreme Arid Zones**: Hot & dry Balochistan regions  
2. **Agricultural Shifts**: Variable rainfall in Punjab  
3. **Northern High Precipitation**: Himalayan zone shows climate sensitivity  

---

## 🧠 Applications for Decision Makers

- Water resource planning  
- Agricultural resilience strategy  
- Urban climate adaptation  
- Ecosystem monitoring  

---


## 💾 Installation

```bash
git clone https://github.com/adnanabbasshah/pakistan-bivariate-climate-map.git
cd pakistan-bivariate-climate-map
Rscript install_packages.R  # or use renv::restore()
```

---

## 🔄 Workflow

1. Data extraction from GEE  
2. Raster processing in R  
3. Bivariate classification using `biscale`  
4. Map generation with `ggplot2`  

---

## 🤝 Contributing

Feel free to fork this repository, make changes, and submit pull requests!

---

## 🧾 License

Licensed under the MIT License.

---

## 📚 Citation

Please cite this toolkit as:

**Shah, A. A. (2024)**. *Pakistan Climate Bivariate Mapping Toolkit*. GitHub Repository: [https://github.com/adnanabbasshah](https://github.com/adnanabbasshah)

---

📍 _Let’s visualize climate together. What patterns do you see in your region?_ 🌍
