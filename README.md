# SmartPark AI

> SmartPark AI is a computer-vision prototype that detects parking-bay occupancy, tracks vehicles, and provides basic parking guidance for drivers and parking operators.

**Status:** Week 1 project framing and planning. This repository is an engineering record; it is not yet a functioning AI application.

## Problem Statement

Drivers in busy parking facilities may spend time searching for available parking spaces, while parking operators need an efficient way to monitor occupancy. AI engineering and computer vision can analyse camera images to identify occupied and vacant bays and track vehicles through a parking layout. This project will demonstrate these capabilities using a miniature parking prototype rather than a live car park.

## Evidence of the Problem

- Research on smart parking solutions reviews existing smart parking functions, technologies and challenges, providing context for systems that help drivers find suitable parking spaces. Source: Lin, T., Rivano, H. and Le Mouël, F. (2017), *A Survey of Smart Parking Solutions*, IEEE Transactions on Intelligent Transportation Systems. https://doi.org/10.1109/TITS.2017.2685143
- The PKLot study presents an annotated parking-lot image dataset for classifying spaces as vacant or occupied, demonstrating the feasibility of image-based occupancy experiments. Source: de Almeida, P. R. L. et al. (2015), *PKLot—A Robust Dataset for Parking Lot Classification*. https://doi.org/10.1016/j.eswa.2015.02.009

## Initial Related Work / Existing Approaches

1. **PKLot parking occupancy dataset and classification study** — Source: [paper](https://doi.org/10.1016/j.eswa.2015.02.009) and [official dataset page](https://web.inf.ufpr.br/vri/databases/parking-lot-database/). **Key limitation:** it provides labelled parking imagery for occupancy research but does not itself deliver an integrated vehicle-tracking and parking-guidance application.
2. **CNRPark / CNRPark+EXT datasets** — Source: https://cnrpark.it/. **Key limitation:** the datasets focus on visual parking-space occupancy; application-specific vehicle tracking and destination-based guidance require additional development.
3. **Ultralytics YOLO object-detection framework** — Source: https://docs.ultralytics.com/. **Key limitation:** object detection can locate objects in images, but parking-bay occupancy logic, persistent tracking and user guidance need additional system components.

## Project Objectives

- **O1:** To design and develop a computer-vision prototype that detects occupied and vacant parking bays in a miniature parking layout.
- **O2:** To implement vehicle tracking and simple guidance towards an available parking bay.
- **O3:** To evaluate the prototype using detection accuracy and tracking/guidance performance.

## Target Users

Drivers or visitors who need to find an available parking space, and parking operators who need to monitor parking-bay occupancy.

## Data Sources

- **PKLot parking-lot database** — https://web.inf.ufpr.br/vri/databases/parking-lot-database/
- **CNRPark / CNRPark+EXT dataset** — https://cnrpark.it/
- **Project-collected footage:** images and video captured from the miniature parking prototype, with manually labelled parking-bay occupancy and vehicle positions.

Check each dataset's licence and terms before use, and record any preprocessing or labels added by the project.

## Responsible AI Risks

- **Incorrect occupancy detection:** A vacant bay may be reported as occupied, or an occupied bay as vacant, leading to unreliable guidance.
- **Tracking errors:** A vehicle may be associated with the wrong position or parking bay.
- **Privacy:** Camera footage can capture unnecessary visual information. Minimise collection and use controlled miniature scenes where possible.

## Proposed Evaluation

Prepare a labelled set of images or video frames from the miniature parking layout. Evaluate parking-bay occupancy detection, vehicle-tracking consistency, guidance correctness and response time. Report the test conditions and limitations; results from a miniature layout should not be presented as proof of performance in a real car park.

## Weekly Progress

| Week | Focus | Deliverable | Status |
|------|-------|-------------|--------|
| W1 | System map + project scope | README + docs | ✅ |

## Repository Structure

- `README.md` — project overview, objectives, evidence, sources and weekly progress
- `docs/week1_project_scope.md` — individual Part C project scope
- `docs/week1_observation_sheet.md` — inherited group Part A material; review it against the assignment and actual evidence
- `docs/week1_system_map.png` — inherited Part B system map; verify whether it belongs in the individual submission
- Review inherited group files and clearly distinguish required Part A/B evidence from this individual SmartPark AI project.

## Scope Note

SmartPark AI is currently a proposed prototype. Parking occupancy, vehicle tracking and guidance are intended features, not claimed completed capabilities. The project uses a miniature parking layout and is not a live shopping-centre deployment.
