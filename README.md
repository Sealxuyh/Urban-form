# **Sustainable urban performance admits multiple structural equilibria: Evidence from efficiency frontiers across Chinese cities**

This repository contains the code used in the study:

**Sustainable urban performance admits multiple structural equilibria: Evidence from efficiency frontiers across Chinese cities**

This study develops a three-dimensional (3D) Production Possibility Frontier (PPF) to benchmark sustainable urban performance using high-resolution data from nearly 200,000 urban blocks (500 m × 500 m grid cells) across 36 Chinese cities, and examines how urban form is associated with performance gaps through machine learning and explainable AI methods.

![img](figures/fig_cluster.png)



## Project overview

The pursuit of cities that simultaneously deliver social vitality, economic productivity and environmental quality has become a central challenge for urban governance worldwide. Yet interventions designed to advance one objective routinely compromise others, pointing to underlying constraints that remain poorly characterized. This study:

- Constructs a 3D PPF to define the empirical upper bound of jointly attainable urban performance

- Quantifies performance gaps as distances to the frontier

- Uses XGBoost to model the relationship between urban form and performance

- Applies SHAP to identify nonlinear and context-dependent effects of urban form

- Identifies multiple structural equilibria, with high performance achieved through dense, highly connected urban cores as well as through selectively articulated developments that integrate building complexity with environmental features

We reframe urban sustainability as a frontier problem. Rather than treating social vitality, economic productivity, and environmental quality as independent objectives to be maximized in isolation, we show that these dimensions are jointly constrained within a bounded performance space whose shape is conditioned by urban form. 



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

References:

[1] Chen, Z. et al. An extended time series (2000–2018) of global NPP-VIIRS-like nighttime light data from a cross-sensor calibration. Earth Syst. Sci. Data 13, 889–906 (2021).

[2] Yang, Q. et al. A global urban heat island intensity dataset: Generation, comparison, and analysis. Remote Sens. Environ. 312, 114343 (2024).

[3] Che, Y. et al. 3D-GloBFP: the first global three-dimensional building footprint dataset. Earth Syst. Sci. Data 16, 5357–5374 (2024).



## Method summary

### Multidimensional performance indicators

- **Social**: Local social vitality derived from OD flows (based on Baidu location-based service data)
- **Economic**: Night-time light intensity (NPP–VIIRS-like data)
- **Environmental**: Daytime urban heat island intensity (July 2020)

### Urban form indicators

- 40 indicators capturing:
  - Building morphology
  - Street network structure
  - Land use intensity
  - Vegetation and surface permeability

| Urban form indicator              | Abbreviation         | Description                                                  |
| --------------------------------- | -------------------- | ------------------------------------------------------------ |
| Building Count                    | buildingCount        | Number of buildings within the block                         |
| Total Building Height             | totalHeight          | Sum of building heights within the block                     |
| Average Building Height           | avgHeight            | Mean building height within the block                        |
| Maximum Building Height           | maxHeight            | Height of the tallest building within the block              |
| Minimum Building Height           | minHeight            | Height of the shortest building within the block             |
| Height Range                      | heightRange          | Difference between maximum and minimum building heights      |
| Highest Building Index            | highestBuildingIndex | Height of the tallest building divided by total building height |
| Building Height Density           | heightDensity        | Total building height divided by block area                  |
| Building Height Otherness         | heightOtherness      | Coefficient of variation of building heights                 |
| Total Corner Count                | cornerCountTotal     | Total number of building corner points within the block      |
| Average Corner Count              | cornerCountAvg       | Average number of corner points per building                 |
| Total Base Perimeter              | basePerimeterTotal   | Sum of base perimeters of all building footprints            |
| Average Base Perimeter            | basePerimeterAvg     | Mean base perimeter of building footprints                   |
| Maximum Base Perimeter            | basePerimeterMax     | Maximum base perimeter among buildings                       |
| Minimum Base Perimeter            | basePerimeterMin     | Minimum base perimeter among buildings                       |
| Plan Shape Complexity             | shapeComplexity      | Mean shape compactness ratio calculated as area divided by perimeter |
| Building Compactness              | compactness          | Mean isoperimetric quotient measuring building circularity   |
| Total Footprint Area              | footprintAreaTotal   | Total area covered by building footprints                    |
| Total Floor Area                  | totalArea            | Total gross floor area of buildings                          |
| Area Variance                     | areaVariance         | Variance of building footprint areas                         |
| Average Building Area             | avgBuildingArea      | Mean building footprint area                                 |
| Parcel Area                       | parcelArea           | Total parcel area within the block                           |
| Largest Patch Index               | largestPatchIndex    | Area of the largest building patch divided by block area     |
| Three Dimensional Shape Index     | shape3DIndex         | Ratio of total building envelope area to the surface area of a sphere with equivalent volume |
| Sky View Factor                   | SVF                  | Proportion of visible sky area within the block, ranging from 0 to 1 |
| Building Volume Evenness Index    | evennessIndex        | Deviation of individual building volumes from the mean volume |
| Floor Area Ratio                  | FAR                  | Ratio of total gross floor area to block area                |
| Building Coverage Ratio           | coverageRatio        | Ratio of total building footprint area to block area         |
| Fractional Vegetation Cover       | FVC                  | Proportion of vegetated area within the block                |
| Permeable Surface Ratio           | permeableRatio       | Proportion of permeable surface within the block             |
| POI Density                       | poiDensity           | Number of points of interest divided by block area           |
| POI Diversity                     | poiDiversity         | Diversity of points of interest measured using the Shannon entropy index |
| Street Height to Width Ratio      | streetRatio          | Ratio of average building height to street width             |
| Road Network Density              | roadDensity          | Total road length divided by block area                      |
| Intersection Density              | intersectionDensity  | Number of road intersections divided by block area           |
| Building Proximity                | buildingProximity    | Average nearest neighbor distance between buildings          |
| Minimum Distance to Buildings     | buildingMinDist      | Minimum distance from a building to its nearest neighboring building |
| Maximum Distance to Buildings     | buildingMaxDist      | Maximum distance from a building to its farthest neighboring building |
| Average Distance to Buildings     | buildingDistAvg      | Mean distance between buildings within the block             |
| Variance of Distance to Buildings | buildingDistVar      | Variance of distances between buildings within the block     |

### Modeling and interpretation

- **XGBoost** selected based on model comparison
- **SHAP** used for:
  - Global feature importance
  - Nonlinear dependence analysis
  - Instance-level explanations
- **K-means clustering** applied to SHAP vectors to identify multiple structural equilibria



## Data availability

Due to licensing restrictions, certain raw datasets, such as Baidu location based service data, cannot be made publicly available. Processed indicators and example data structures used in this study may be provided upon reasonable request. Requests should be directed to Yuhan Xu [yxu899@gatech.edu].

