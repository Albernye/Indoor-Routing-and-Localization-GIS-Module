# Indoor GIS & Spatial Routing Module

This repository contains the **Geographic Information System (GIS) layer** of an Indoor Routing & Localization System developed during my engineering internship.

Its purpose is to:
- Model an indoor environment as a spatial graph
- Georeference building floor plans
- Store indoor spatial data in a spatial database
- Enable indoor routing using a graph-based approach

This repository focuses exclusively on **spatial data preparation, GIS processing, and indoor routing**, independently from localization algorithms or mobile sensing.

---

## Objectives

- Georeference indoor floor plans
- Digitize walkable areas and transitions
- Build an indoor routing graph
- Store spatial data in a PostGIS-compatible database
- Compute indoor paths using pgRouting
- Provide GIS assets reusable by localization or web modules

---

## Project Structure

gis_project/
├── data/ # Raw and processed GIS data
│ ├── OBuilding_Floor2.png.points
│ └── plan etage/
│ ├── Map_georeferenced.tif
│ ├── OBuilding_Floor2_modified.tif
│ ├── OBuilding_Floor2.png
│ ├── *.aux.xml
│ └── *.points
│
├── db/ # Spatial database
│ ├── init.sql # Database initialization
│ └── export/ # SQL or spatial exports
│
├── pgrouting/ # Indoor routing logic
│ ├── indoor_graph.sql # Graph construction
│ ├── indoor_route.sql # Routing queries
│ ├── pgrouting_test.sql # Tests and experiments
│ ├── indoor_graph.qmd # Documentation / analysis
│ └── indoor_route.qmd
│
├── qgis_project/ # QGIS project files
│ ├── practive_project.qgz
│ ├── UOC_v2.qgz
│ └── ver2.qgz
│
├── notes/ # Technical notes
│ └── todo.md
│
└── README.md


---

## GIS Workflow

### 1. Floor Plan Preparation
- Import building floor plans (PNG / TIFF)
- Clean and adjust geometry if needed
- Convert plans into GIS-compatible raster formats

### 2. Georeferencing
- Use ground control points (`.points`)
- Generate georeferenced rasters (`.tif`)
- Store projection metadata (`.aux.xml`)

### 3. Digitization
- Digitize walkable areas, corridors, doors, and transitions
- Convert raster plans into vector layers
- Assign topology and connectivity attributes

### 4. Indoor Graph Construction
- Convert vector layers into graph edges and nodes
- Assign traversal costs (distance, constraints)
- Store graph structure in a spatial database

### 5. Indoor Routing
- Compute shortest paths between indoor locations
- Support room-to-room routing
- Enable integration with localization systems

---

## Spatial Database

The project uses a **PostGIS-compatible database** with **pgRouting**.

### Database Initialization

```bash
psql -d indoor_gis -f db/init.sql
```
### Indoor Routing (pgRouting)

Routing logic is implemented in the `pgrouting/` directory.

### Key files

- `indoor_graph.sql` → graph creation  
- `indoor_route.sql` → routing queries  
- `pgrouting_test.sql` → validation and testing  

### Example usage

```sql
SELECT * FROM pgr_dijkstra(
    'SELECT id, source, target, cost FROM indoor_edges',
    start_node,
    end_node
);
```

### QGIS Projects

The qgis_project/ directory contains:

- Complete GIS projects

- Georeferenced floor plans

- Digitized indoor layers

- Graph visualization and validation

These projects can be opened directly to:

- Inspect geometry

- Edit indoor features

- Validate routing paths

### Integration

This GIS module is designed to be:

- Used by localization algorithms (QR, PDR, Wi-Fi)

- Queried by a backend service (API)

- Visualized through a web or mobile interface

It acts as the spatial backbone of the full indoor navigation system.
