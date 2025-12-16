# **Urban form and multi-dimensional performance**

This repository contains the code used in the study:

**Optimizing urban form through social, economic, and environmental performance: A three-dimensional production possibility frontier approach in 36 Chinese cities**

The project develops a three-dimensional Production Possibility Frontier (PPF) to benchmark urban performance and investigates how urban form drives performance gaps using machine learning and explainable AI methods.



## Project overview

Cities face inherent trade-offs between social vitality, economic output, and environmental quality. This project:

- Constructs a 3D PPF to define the empirical upper bound of jointly attainable urban performance

- Quantifies performance gaps as distances to the frontier

- Uses XGBoost to model the relationship between urban form and performance

- Applies SHAP to identify nonlinear and context-dependent effects of urban form

- Identifies heterogeneous urban form pathways toward high performance through SHAP-based clustering

The analysis is conducted at a 500 m × 500 m grid scale for 36 Chinese cities.



## Datasets

| Name                                  | Period             | Spatial resolution | Source                                         |
| ------------------------------------- | ------------------ | ------------------ | ---------------------------------------------- |
| Night-time light                      | 2023               | ~500 m             | Extended NPP-VIIRS-like NTL Data [1]           |
| Location-based service                | 6–11 November 2023 | 100 m              | Baidu                                          |
| Urban heat island intensity (daytime) | July 2020          | 1 km               | Global urban heat island intensity dataset [2] |
| Building footprint                    | 2019, 2020         | Vector             | Baidu Maps, 3D-GloBFP [3]                      |
| Street network                        | 2023               | Vector             | OpenStreetMap                                  |
| Fractional vegetation cover           | 2022               | 250 m              | China Academy of Urban Planning and Design     |
| Impervious surface                    | 2022               | 30 m               | China Academy of Urban Planning and Design     |
| Digital Elevation Model (DEM)         | 2020               | 30 m               | China Academy of Urban Planning and Design     |
| Points of Interest (POIs)             | 2022               | Vector             | Amap                                           |

Note: Due to licensing restrictions, some raw datasets (e.g., Baidu LBS data) cannot be publicly shared. Processed indicators and example data structures may be provided upon reasonable request.

References:

[1] Chen, Z. et al. An extended time series (2000–2018) of global NPP-VIIRS-like nighttime light data from a cross-sensor calibration. Earth Syst. Sci. Data 13, 889–906 (2021).

[2] Yang, Q. et al. A global urban heat island intensity dataset: Generation, comparison, and analysis. Remote Sens. Environ. 312, 114343 (2024).

[3] Che, Y. et al. 3D-GloBFP: the first global three-dimensional building footprint dataset. Earth Syst. Sci. Data 16, 5357–5374 (2024).



## Method summary

### Performance indicators

- **Social**: Local social vitality derived from OD flows (based on Baidu location-based service data)
- **Economic**: Night-time light intensity (NPP–VIIRS-like data)
- **Environmental**: Daytime urban heat island intensity (July 2020)

### Urban Form Indicators

- 40 indicators capturing:
  - Building morphology
  - Street network structure
  - Land use intensity
  - Vegetation and surface permeability

### Modeling and Interpretation

- **XGBoost** selected based on model comparison
- **SHAP** used for:
  - Global feature importance
  - Nonlinear dependence analysis
  - Instance-level explanations
- **K-means clustering (k = 4)** applied to SHAP vectors to identify heterogenous pathway toward high performance



