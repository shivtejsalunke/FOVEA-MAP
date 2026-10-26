# Semantic Point-Cloud Ontology

The planned nine-class ontology is:

| ID | Class | Examples / intent |
|---:|---|---|
| 0 | Drivable surface | Firm road or ground intended to support motion |
| 1 | Rough but traversable terrain | Uneven ground potentially within vehicle limits |
| 2 | Non-drivable terrain | Surface outside the configured traversability limits |
| 3 | Vegetation | Grass, shrubs, trees and foliage |
| 4 | Static obstacle | Walls, barriers and infrastructure |
| 5 | Pole or thin vertical structure | Poles, narrow posts and similar structures |
| 6 | Vehicle | Road or off-road vehicles |
| 7 | Pedestrian | People in the scene |
| 8 | Unknown | Unclassified or low-confidence points |

A final implementation must freeze class IDs and map them to the chosen dataset/model label space. This nine-class project ontology will not match every public dataset one-to-one; mapping and ignored labels must be documented. Model candidates such as sparse voxel CNNs or cylinder-based networks are not a selection until a concrete model, model licence/weights, preprocessing, class mapping and evaluation path have been committed.
