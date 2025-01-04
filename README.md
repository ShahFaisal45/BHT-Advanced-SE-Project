# Electric Vehicle (EV) Charging Station Visualization Program

# **Final Result Please Click Here**
https://advancedsoftwareengieering.streamlit.app/


## Task 8: Documentation

### I. Program Structure
The program processes and visualizes data related to electric vehicle charging stations and population distribution in Berlin. The goal is to generate a Streamlit application to identify areas requiring additional EV charging stations.

**Workflow Steps:**
1. **Load Data:**
   - Berlin geodata by postal codes (PLZ).
   - EV charging station data.
   - Population/resident data.
2. **Data Preprocessing:**
   - Process and merge data to create meaningful relationships.
   - Count charging stations by postal code (PLZ).
3. **Visualization:**
   - Generate an interactive Streamlit app for visual exploration of data.

---

### II. Key Components

#### 1. Imports
- **`pandas`**: For data manipulation.
- **`core.methods`**: A custom module containing data processing methods.
- **`core.HelperTools`**: Utility functions, such as a timer.
- **`config.pdict`**: Configuration dictionary storing filenames and parameters.

#### 2. Functions
- **`@ht.timer`**: A decorator to measure execution time for the `main()` function.
- **`main()`**: The main driver function for data loading, preprocessing, and app generation.

#### 3. Dataset Requirements
- **Geodata**: Spatial data for Berlin postal codes.
- **Charging Stations Data**: Existing EV charging station locations.
- **Residents Data**: Population data by postal code.

#### 4. Error Handling
Each step includes `try-except` blocks to handle and report errors during execution.

---

### III. Code Walkthrough

#### 1. Load Geodata
Reads the geodata file (`file_geodat_plz`) using `pd.read_csv()`. Defines Berlin's postal code regions for mapping.

#### 2. Load Charging Stations Data
Reads the dataset (`file_lstations`) with `pd.read_csv()` containing charging station locations.

#### 3. Preprocess Charging Stations Data
Uses `m1.preprop_lstat()` to clean, transform, and integrate charging station data with geodata.

#### 4. Count Charging Stations per PLZ
Uses `m1.count_plz_occurrences()` to count the number of charging stations for each postal code.

#### 5. Load Residents Data
Reads the residents' dataset (`file_residents`) with `pd.read_csv()`, containing population distribution.

#### 6. Preprocess Residents Data
Uses `m1.preprop_resid()` to clean and align population data with geodata.

#### 7. Generate Streamlit App
Combines processed data to create an interactive Streamlit app with `m1.make_streamlit_electric_Charging_resid()`.

---

### IV. Key Methods and Their Roles

- **`m1.preprop_lstat`**: Cleans and integrates charging stations data with Berlin's geodata.
- **`m1.count_plz_occurrences`**: Aggregates charging stations per postal code.
- **`m1.preprop_resid`**: Cleans and aligns residents data with Berlin's postal codes.
- **`m1.make_streamlit_electric_Charging_resid`**: Generates a Streamlit app for visualization.
- **`ht.timer`**: Logs the execution time of the `main()` function.

---

### V. Configuration (`config.pdict`)
The `pdict` dictionary stores file paths and parameters:
- `file_geodat_plz`: Filename for Berlin's postal code geodata.
- `file_lstations`: Filename for charging stations data.
- `file_residents`: Filename for population data.

---

### VI. Output

#### 1. Command-Line Messages
- Status updates for data loading, preprocessing, and app generation.
- Error messages in case of failures.

#### 2. Streamlit App
- Visualizes postal code regions, charging station density, and population distribution.
- Provides insights into areas requiring additional EV charging stations.

---

## Documentation for Utility Functions and Framework

This section explains the utility functions for data processing, serialization, computations, and randomization. These tools manage and analyze datasets involving DataFrames, dictionaries, and lists.

### I. Overview of Modules Imported

#### Standard Libraries:
- `math`, `random`, `pickle`: For mathematical operations, random value generation, and serialization.
- `time` and `functools`: Timing and decorators.
- `collections`:
  - `Counter`: Counts occurrences in collections.
  - `OrderedDict`: Maintains order in dictionaries.

#### Third-Party Libraries:
- **`pandas`**: For tabular data in DataFrames.

---

# Geospatial Data Preprocessing and Visualization Functions

This repository provides functions for geospatial data processing, preprocessing, and visualization of datasets. The focus is on electric vehicle (EV) charging stations and residential data with postal codes (PLZ) in Germany. 

---

## **Table of Contents**
1. [Imports](#imports)
2. [Core Functions](#core-functions)
   - [sort_by_plz_add_geometry](#sort_by_plz_add_geometry)
   - [preprop_lstat](#preprop_lstat)
   - [count_plz_occurrences](#count_plz_occurrences)
   - [preprop_resid](#preprop_resid)
   - [make_streamlit_electric_Charging_resid](#make_streamlit_electric_charging_resid)
3. [Data Insights and Findings](#data-insights-and-findings)
   - [Residents' Distribution](#residents-distribution)
   - [EV Charging Stations Distribution](#ev-charging-stations-distribution)
   - [Gap Analysis and Recommendations](#gap-analysis-and-recommendations)
4. [Conclusion](#conclusion)

---

## **Imports**

The following libraries are utilized in this project:
- **`geopandas`**: Geospatial data handling.
- **`core.HelperTools`**: Utility tools (e.g., `@ht.timer` for runtime logging).
- **`folium`**: Interactive map creation.
- **`streamlit`**: For building interactive web applications.
- **`streamlit_folium`**: To embed Folium maps into Streamlit apps.
- **`branca.colormap`**: For custom color mapping in visualizations.

---

## **Core Functions**

### **sort_by_plz_add_geometry**
Prepares and merges dataframes by postal codes (PLZ) and adds geospatial geometry.

**Parameters:**
- `dfr`: Pandas DataFrame containing the primary dataset.
- `dfg`: GeoDataFrame containing geospatial data.
- `pdict`: Dictionary containing column mappings (e.g., `pdict["geocode"]` for merge keys).

**Returns:**
- GeoDataFrame with sorted data and valid geometries.

---

### **preprop_lstat**
Preprocesses EV charging station data and filters rows based on geographic and postal code constraints.

**Parameters:**
- `dfr`: DataFrame from `Ladesaeulenregister.csv`.
- `dfg`: GeoDataFrame.
- `pdict`: Column mapping dictionary.

**Returns:**
- GeoDataFrame of filtered and geospatially processed charging station data.

**Steps:**
1. Filter relevant columns (`PLZ`, `Bundesland`, `Breitengrad`, `Längengrad`, `KW`).
2. Convert latitude and longitude values to decimal format.
3. Filter rows for Berlin with postal codes between `10115` and `14200`.
4. Merge with geospatial data and ensure valid geometries.

---

### **count_plz_occurrences**
Counts the occurrences of charging stations per postal code (PLZ).

**Parameters:**
- `df_lstat2`: GeoDataFrame of charging station data.

**Returns:**
- DataFrame with:
  - `PLZ`: Postal codes.
  - `Number`: Count of charging stations.
  - `geometry`: Geometry of the region.

---

### **preprop_resid**
Preprocesses residential population data by postal codes and merges it with geospatial data.

**Parameters:**
- `dfr`: DataFrame from `plz_einwohner.csv`.
- `dfg`: GeoDataFrame.
- `pdict`: Column mapping dictionary.

**Returns:**
- GeoDataFrame of residents' data with valid geometries.

**Steps:**
1. Filter relevant columns (`PLZ`, `Einwohner`, `Breitengrad`, `Längengrad`).
2. Convert latitude and longitude to decimal format.
3. Filter rows for postal codes between `10000` and `14200`.
4. Merge with geospatial data.

---

### **make_streamlit_electric_Charging_resid**
Creates an interactive Streamlit app for visualizing heatmaps of:
- Charging stations.
- Resident population.

**Parameters:**
- `dfr1`: GeoDataFrame with charging station data.
- `dfr2`: GeoDataFrame with resident population data.

**Returns:**
- Streamlit app rendering heatmaps.

**Features:**
1. **Map Layers**:
   - Heatmap for charging stations (based on `Number`).
   - Heatmap for residents (based on `Einwohner`).
2. **Color Maps**:
   - Gradation from yellow to red for density visualization.
3. **Interactivity**:
   - Toggle layers using radio buttons.
4. **Integration**:
   - Embed Folium maps into Streamlit via `folium_static`.

---

## **Data Insights and Findings**

### **Residents' Distribution**
- The heatmap reflects population density across Berlin.
- Higher densities are observed in districts like Pankow and Mitte, often exceeding 30,000 residents in specific neighborhoods.
- Southern and western Berlin exhibit more suburban or sparsely populated areas.

**Key Insight**: Areas with higher population densities require robust public services, including EV charging stations.

---

### **EV Charging Stations Distribution**
- Charging stations are clustered in central Berlin, particularly around high-traffic neighborhoods like Mitte.
- Hubs host up to 95 charging stations, with secondary clusters in eastern Berlin.

**Key Insight**: While central areas are well-equipped, other regions show notable disparities in infrastructure.

---

### **Gap Analysis and Recommendations**
1. **Northern Berlin**: Increase stations in Pankow to match high population density.
2. **Southern Berlin**: Address underserved areas to encourage EV adoption.
3. **Western Berlin**: Provide equitable station distribution despite lower population levels.
4. **Strategic Placement**: Include transit routes and commercial zones for accessibility.

---

## **Conclusion**

Berlin’s EV charging infrastructure shows progress in central areas but lacks equitable coverage for densely populated and underserved regions. Addressing these gaps through targeted expansion will enhance Berlin’s EV ecosystem and ensure sustainable growth.

---






