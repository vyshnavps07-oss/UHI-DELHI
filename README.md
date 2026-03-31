# Urban Heat Island Analysis in Delhi (2014-2024)

This notebook performs an analysis of Urban Heat Island (UHI) effects and urban expansion in Delhi, India, comparing data from 2014 and 2024. It utilizes Google Earth Engine (GEE) with Landsat 8 satellite imagery to derive key environmental indices.

## Objectives

- To calculate and visualize Land Surface Temperature (LST), Normalized Difference Vegetation Index (NDVI), and Normalized Difference Built-up Index (NDBI) for Delhi for the years 2014 and 2024.
- To analyze the spatiotemporal change in LST and NDBI over the decade.
- To identify and quantify heat hotspots within the region.
- To explore the statistical relationships between temperature, greenery, and urban density.

## Data Sources

- **Landsat 8 Collection 2 Tier 1 Level 2**: Utilized for imagery, providing surface reflectance and surface temperature bands. Data was filtered for the summer months (April 1 to June 30) for both 2014 and 2024, with cloud cover less than 10%.
- **FAO/GAUL/2015/level1**: Used to define the administrative boundary of Delhi (ADM1_NAME = 'Delhi').

## Methodology

1.  **Initialization**: Earth Engine was initialized, and the Delhi administrative boundary was loaded and clipped.
2.  **Image Collection & Index Calculation (2014 & 2024)**:
    -   **LST**: Calculated from Landsat 8 thermal band (ST_B10) using a standard conversion formula to Celsius. The 90th percentile was used to capture peak summer heat while minimizing cloud effects.
    -   **NDVI**: Calculated as `(SR_B5 - SR_B4) / (SR_B5 + SR_B4)` using near-infrared (SR_B5) and red (SR_B4) bands.
    -   **NDBI**: Calculated as `(SR_B6 - SR_B5) / (SR_B6 + SR_B5)` using short-wave infrared (SR_B6) and near-infrared (SR_B5) bands. A median composite was used for NDBI to reduce noise.
3.  **Change Detection (2024 - 2014)**:
    -   **Temperature Change (`LST_Change`)**: LST 2024 minus LST 2014.
    -   **Urban Expansion (`Urban_Expansion`)**: NDBI 2024 minus NDBI 2014.
4.  **Heat Hotspot Identification**: The `LST_Change` layer was sampled to find the top 5 areas with the highest temperature increase.
5.  **Multi-Parameter Sampling & Data Frame Creation**: For the identified hotspots, a master DataFrame was created containing LST 2024, LST Change, NDVI 2024, and NDBI 2024 values, along with their coordinates.
6.  **Statistical Correlation**: A correlation matrix was generated to understand the relationships between temperature (current and change), greenery, and urban density.
7.  **Visualizations**: Interactive maps were generated using `geemap` to display individual layers and side-by-side comparisons (linked maps, split panel) for 2014 and 2024 LST.

## Key Findings & Results

### Decadal Change (2014-2024)

-   **Mean LST 2014**: 49.05 °C
-   **Mean LST 2024**: 50.03 °C
    *   This indicates an average increase of approximately **0.98 °C** across Delhi's summer LST over the decade.
-   **Mean NDBI 2014 (Urban Density)**: 0.0006
-   **Mean NDBI 2024 (Urban Density)**: -0.0014
    *   The slight decrease in mean NDBI might suggest a complex urban development pattern or limitations of the index sensitivity when averaged over a large, diverse region.

### Top 5 Heat Hotspots

The analysis identified specific locations within Delhi experiencing significant temperature increases. For instance, one hotspot registered a temperature rise of **15.72 °C** between 2014 and 2024 (at coordinates: 77.313550, 28.518321).

### Statistical Correlation (Based on Hotspot Data)

The correlation matrix for the top hotspots reveals insightful relationships:

| Variable           | Temp_2024_C | Temp_Rise_C | Green_Index | Urban_Index |
| :----------------- | :---------- | :---------- | :---------- | :---------- |
| **Temp_2024_C**    | 1.00        | 0.88        | -0.71       | 0.27        |
| **Temp_Rise_C**    | 0.88        | 1.00        | -0.76       | 0.11        |
| **Green_Index**    | -0.71       | -0.76       | 1.00        | -0.29       |
| **Urban_Index**    | 0.27        | 0.11        | -0.29       | 1.00        |

-   **Strong Positive Correlation between Current Temperature and Temperature Rise**: `Temp_2024_C` and `Temp_Rise_C` (0.88) indicates that areas that are hotter in 2024 also experienced a greater temperature rise.
-   **Strong Negative Correlation with Greenery**: `Temp_Rise_C` shows a strong negative correlation (-0.76) with `Green_Index` (NDVI), affirming that areas with decreasing greenery are significantly more prone to temperature increases.
-   **Weak Positive Correlation with Urban Index**: `Temp_Rise_C` has a weak positive correlation (0.11) with `Urban_Index` (NDBI). While the expected 'Urban Heat Island' effect suggests a stronger positive correlation, this might be due to the specific characteristics of the identified hotspots or the chosen index thresholds. Further investigation might be needed to understand this relationship comprehensively.

## Visualizations

The notebook provides several interactive maps:
-   **Individual Layer Maps**: Displaying LST, NDVI, NDBI for both years.
-   **Change Maps**: Visualizing LST and NDBI differences.
-   **Linked Maps / Split Panel Maps**: Offering a side-by-side visual comparison of 2014 and 2024 LST, allowing for direct observation of change across the region.
-   **Correlation Heatmap**: Visually representing the statistical relationships between the derived indices.

## Exported Data

A CSV file named `Delhi_Hotspot_Analysis_2014_2024.csv` has been exported to the local Colab storage. This file contains detailed information for the top 5 heat hotspots, including:
-   `Temp_2024_C`: Land Surface Temperature in 2024 (°C)
-   `Temp_Rise_C`: Temperature increase from 2014 to 2024 (°C)
-   `Green_Index`: NDVI in 2024
-   `Urban_Index`: NDBI in 2024
-   `longitude`, `latitude`: Geographic coordinates of the hotspot.
