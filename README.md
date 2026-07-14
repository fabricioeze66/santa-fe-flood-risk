# Flood Risk and Health Infrastructure Exposure in Santa Fe, Argentina

![Poster](poster/poster-1.png)

GIS analysis of flood exposure across the province of Santa Fe (Argentina), combining hydrographic proximity analysis with digital elevation modeling to identify the departments where population and health infrastructure face real flood risk.

**Author:** Fabricio Vera — Geography, Universidad Nacional del Litoral (FHUC-UNL)
**Course:** Geographic Information Systems II — July 2026

## Objective

**General:** assess, through spatial analysis, the exposure of population and health infrastructure to flood risk in Santa Fe province.

**Specific:**
- Delimit potentially floodable areas from the hydrographic network and terrain elevation.
- Identify the localities and health facilities located within those areas.
- Classify departments with a composite criticality index.

## Hypothesis

Departments along the Paraná and Salado rivers concentrate the highest criticality, since they combine high physical exposure (proximity to water, low elevation) with more population and health infrastructure settled on low ground.

## Study area

The entire province of Santa Fe, Argentina — 19 departments. Two units of analysis: locality (point), for detailed exposure, and department, as the synthesis unit for the criticality index.

## Data sources

All data from Argentina's National Geographic Institute (IGN):

| Layer | Type | Source |
|---|---|---|
| Provincial and departmental boundaries | Vector (polygons) | IGN SIG layers |
| Localities (BAHRA settlements database) | Vector (points) | IGN |
| Hydrographic network and water bodies | Vector (lines/polygons) | IGN |
| National road network | Vector (lines) | IGN |
| Health facilities | Vector (points) | IGN |
| Digital Elevation Model (MDE-Ar, 30 m) | Raster | IGN |

CRS: WGS 84 (EPSG:4326).

## Methodology (QGIS)

1. **Layer preparation** — clipped national layers to the provincial boundary; filtered the 19 Santa Fe departments by attribute selection (department code starting with 82).
2. **Flood zone delimitation** — 2 km buffer around the hydrographic network and water bodies, merged into a single flood-prone zone.
3. **Exposure analysis** — selection by location to identify localities and health facilities intersecting the flood zone; point-in-polygon counts per department.
4. **Elevation correction** — the distance-only approach overestimated exposure (some high-elevation departments were classified as critical). The MDE-Ar raster tiles covering the province were merged, clipped to the 19 departments, and sampled with the raster calculator over the flood zone. Elevation statistics there (mean 41.6 m, SD 23.3 m) were used to set a low-terrain threshold of ~65 m (mean + 1 SD), covering ~84% of the flood-prone surface, and reclassify the DEM into a binary low/high terrain mask.
5. **Criticality index** — localities and health facilities were re-sampled against the terrain mask, keeping only those on low ground, and recounted per department. The index combines two variables per department: exposed localities and exposed health facilities. Each is normalized 0–1 with min-max scaling, and the criticality index is the average of the two normalized values.

## Results

After elevation correction, exposure dropped from 61 to 52 localities and from 198 to 177 health facilities provincewide. Three departments were reclassified from critical to non-critical because their entire exposure sat on high ground: Belgrano (0.16 → 0), Caseros (0.25 → 0, terrain ~92 m), and General López (0.07 → 0).

| Department | Localities exp. | Health facilities exp. | Criticality |
|---|---|---|---|
| Rosario | 5 | 67 | 0.81 |
| General Obligado | 8 | 19 | 0.64 |
| La Capital | 4 | 36 | 0.52 |
| San Jerónimo | 7 | 7 | 0.49 |
| Constitución | 6 | 9 | 0.44 |
| San Lorenzo | 5 | 12 | 0.40 |
| Iriondo | 5 | 5 | 0.35 |
| San Javier | 4 | 8 | 0.31 |
| Garay | 4 | 7 | 0.30 |
| Las Colonias | 2 | 3 | 0.15 |
| San Justo | 2 | 2 | 0.14 |
| Vera | 0 | 1 | 0.01 |
| San Cristóbal | 0 | 1 | 0.01 |
| **Total** | **52** | **177** | — |

The most critical departments — Rosario, General Obligado, La Capital, San Jerónimo, Constitución, and San Lorenzo — are all along the Paraná or the main hydrographic network on low terrain, consistent with the initial hypothesis.

## Limitations & future work

Historical flood elevation records and finer hydrological models (e.g. height above nearest drainage) would refine the flood delimitation, and census-tract population data would allow estimating the number of people actually affected, rather than just counting exposed localities.

## Repository contents

- `poster/` — final cartographic poster (4 maps + charts).
- `docs/memoria_metodologica.pdf` — full methodological report, in Spanish, including bibliography (18 pages).

## Tools

QGIS (geoprocessing, raster analysis, map composition) · IGN open data
