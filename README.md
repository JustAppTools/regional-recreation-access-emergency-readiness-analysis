# Regional Recreation Access & Emergency Readiness Analysis

## Project Overview
This project uses public GIS datasets to identify recreation or visitor sites in a regional National Recreation Area study area that may be more difficult to support during emergencies. The analysis combines proximity to medical facilities, Fire/EMS stations, major roads, and terrain context into a simple portfolio-ready screening map.

## Research Question
Which visitor and recreation sites in the study area may have higher access-related emergency-readiness concern based on straight-line proximity to emergency services and major roads?

## Intended Use
This project is intended as a preliminary GIS screening demonstration for identifying recreation sites with potential access-related emergency-readiness concerns. It is designed for GIS portfolio communication, not operational response planning.

## Data Sources
- National Park Service boundary FeatureServer for Lake Roosevelt National Recreation Area.
- National Park Service public POI FeatureServer for visitor and recreation locations.
- HIFLD public critical facilities layers for medical facilities and Fire/EMS stations.
- U.S. Census TIGER/Line 2024 roads for surrounding counties.
- Esri Light Gray Canvas and World Hillshade services for cartographic context.

## Methods
The project is organized as an ArcGIS Pro project with raw data, processed data, outputs, maps, and documentation folders. Source layers were copied or referenced through ArcGIS Pro-supported workflows and processed into `data_processed/Lake_Roosevelt_Emergency_Readiness.gdb`.

Analysis layers were projected to `NAD_1983_UTM_Zone_11N` before distance calculations. Lake Roosevelt NRA was buffered by 30 miles to define the surrounding service/access context. Emergency-service and road layers were clipped or selected to this analysis area.

Visitor/recreation sites were selected from NPS POIs for recreation-facing categories. Categories present in the final layer are: Beach (2), Boat Launch (22), Campground (36), Information (1), Interpretive Exhibit (1), Picnic Area (2), Ranger Station (1), Visitor Center (2). The `Site_Name` field uses the NPS map label where available, otherwise the source POI name.

ArcGIS Pro nearest-neighbor analysis calculated planar straight-line distances from each visitor/recreation site to the nearest medical facility, nearest Fire/EMS station, and nearest major road. Distances were stored in meters and converted to miles in `Hosp_Mi`, `FireEMS_Mi`, and `MajorRoad_Mi`.

## Emergency Readiness Concern Score
Each distance input was ranked from 1 to 3 using tertile breaks across the visitor-site dataset:

- 1 = lower concern for that input
- 2 = moderate concern for that input
- 3 = higher concern for that input

Higher distance to medical facilities, Fire/EMS stations, or major roads receives a higher input score. The additive `Emergency_Readiness_Concern_Score` sums the three input scores. Final classes are:

- Low: score 3-4
- Medium: score 5-6
- High: score 7-9

Terrain was not used as a numeric DEM-derived slope input. Terrain context is represented cartographically through hillshade and described as contextual interpretation rather than a measured scoring variable.

## Results Summary
The processed visitor/recreation layer contains 67 sites. Supporting clipped layers include 159 medical facilities, 164 Fire/EMS stations, and 203 major-road features.

Concern class counts:

- Low: 12
- Medium: 32
- High: 23

## Top High-Concern Sites
The table below lists the five highest-concern sites, ordered by concern score and then by summed input distances to break ties.

| Site | Class | Nearest medical facility (mi) | Nearest Fire/EMS (mi) | Nearest major road (mi) | Score |
|---|---:|---:|---:|---:|---:|
| Jones Bay Boat Launch | High | 9.77 | 9.62 | 4.98 | 9 |
| Jones Bay Campground | High | 9.76 | 9.64 | 4.95 | 9 |
| Penix Campground | High | 9.82 | 9.61 | 4.76 | 9 |
| Hanson Harbor Boat Launch | High | 11.41 | 8.39 | 2.97 | 9 |
| Goldsmith Campground | High | 11.60 | 8.03 | 2.23 | 9 |

## Limitations
This is an independent portfolio GIS demonstration and is not an official National Park Service product. It should not be interpreted as official emergency planning guidance, a prediction model, or an operational emergency response plan.

The model uses straight-line distance, not drive time, travel impedance, dispatch routing, or real response time. It does not account for seasonal road closures, road condition, weather, communications coverage, staffing, dispatch boundaries, mutual aid agreements, boat access, visitor volume, incident history, or facility capability. HIFLD medical facilities are a public substitute for hospital access and may include broader medical-facility types. Terrain is represented through hillshade/context unless DEM-derived slope is added in a future version.

## Skills Demonstrated
- ArcGIS Pro project organization
- File geodatabase management
- Public GIS data acquisition
- Coordinate systems and projections
- Vector data cleaning
- Nearest-neighbor analysis
- Buffering and clipping
- Attribute calculations
- Cartographic design
- Layout creation and map export
- Reproducible GIS workflow using ArcGIS Pro and ArcPy

## Files Included
- `Lake_Roosevelt_Emergency_Readiness.aprx`
- `data_processed/Lake_Roosevelt_Emergency_Readiness.gdb`
- `outputs/Lake_Roosevelt_Emergency_Readiness_Map_v10.pdf`
- `outputs/Lake_Roosevelt_Emergency_Readiness_Map_v10.png`
- `data_raw/data_source_references.txt`
- `README.md`

## Version Notes
Version 10 preserves the v9 layout and analysis while fixing two final layout issues: the north arrow is moved to the upper-left of the main map with centered alignment, and the At a glance box is extended so all four result rows sit inside the same white background.
