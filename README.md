
# 🌦️ Pakistan Climate Bivariate Mapping Toolkit

![Bivariate Map](bivaerate_map1.png)

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
```r
library(tidyverse); library(sf); library(terra); library(biscale)
pakistan <- st_read("Pakistan_with_Kashmir.shp")
# Process rasters, create bivariate classes, plot using ggplot2 and cowplot
ggsave("pakistan_climate_map.png", final_map, width = 10, height = 8, dpi = 300)
```

---

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

## 📁 Repository Structure

```
├── data/               # Downloaded and processed datasets  
├── scripts/            # R and GEE scripts  
├── outputs/            # Final maps and visualizations  
├── Dockerfile / renv   # Reproducible environment  
└── README.md           # Project documentation  
```

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
