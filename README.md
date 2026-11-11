# SmartPark AI — AI Engineering Project

> SmartPark AI is a proposed computer-vision prototype that detects parking-bay occupancy, tracks vehicles and provides basic parking guidance for drivers and parking operators.

This repository contains the group evidence for Parts A and B, together with my individual Part C project scope and the supporting Week 2 and Week 3 engineering artefacts.

## Problem Statement

Drivers in busy parking facilities may spend time searching for available parking spaces, while parking operators need an efficient way to monitor occupancy. Computer vision can analyse camera images to identify occupied and vacant bays and track vehicles through a parking layout. This project will investigate these capabilities in a controlled miniature parking prototype, not a live shopping-centre deployment.

## Evidence of the Problem

- Lin, Rivano and Le Mouël review smart-parking functions, technologies and open challenges that aim to help drivers find suitable spaces. Source: *A Survey of Smart Parking Solutions* (2017). https://doi.org/10.1109/TITS.2017.2685143
- The PKLot study presents a labelled parking-lot image dataset for vacant/occupied-space classification and discusses the challenge of generalising across parking lots. Source: de Almeida et al. (2015), *PKLot—A Robust Dataset for Parking Lot Classification*. https://doi.org/10.1016/j.eswa.2015.02.009

## Initial Related Work / Existing Approaches

- **PKLot parking occupancy dataset and classification study** — [paper](https://doi.org/10.1016/j.eswa.2015.02.009), [dataset information](https://web.inf.ufpr.br/vri/databases/parking-lot-database/). **Limitation:** it supports occupancy-classification research but does not itself provide an integrated vehicle-tracking and parking-guidance application.
- **CNRPark / CNRPark+EXT** — [official dataset documentation](https://cnrpark.it/). **Limitation:** it focuses on visual occupancy classification; application-specific tracking and destination-based guidance require additional components.
- **Ultralytics YOLO object-detection framework** — [documentation](https://docs.ultralytics.com/). **Limitation:** detecting objects does not by itself implement parking-bay occupancy logic, persistent vehicle tracking or user guidance.

## Engineering Gap / Limitation

The project will investigate how to integrate parking-bay occupancy detection, vehicle tracking and basic guidance in one workflow. The prototype will be evaluated in a miniature environment, and its results will not be presented as proof of readiness for a real car park.

## Project Objectives

- **O1:** To design and develop a computer-vision prototype that detects occupied and vacant parking bays in a miniature parking layout.
- **O2:** To implement vehicle tracking and simple guidance towards an available parking bay.
- **O3:** To evaluate the prototype using detection accuracy and tracking/guidance performance.

## Target Users

Drivers or visitors who need to find an available parking space, and parking operators who need to monitor parking-bay occupancy.

## Data Sources

- **PKLot parking-lot database** — https://web.inf.ufpr.br/vri/databases/parking-lot-database/
- **CNRPark / CNRPark+EXT dataset** — https://cnrpark.it/
- **Project-collected footage:** images and video from the miniature parking prototype, with manually labelled bay occupancy and vehicle positions.

**Public technical documents used to inform the design:**
- Lin, Rivano and Le Mouël (2017), *A Survey of Smart Parking Solutions* — public PDF: https://perso.citi.insa-lyon.fr/flemouel/Research/Publications/Journals/2017/TITS2017.pdf
- de Almeida et al. (2015), *PKLot—A Robust Dataset for Parking Lot Classification* — public PDF: https://www.inf.ufpr.br/lesoliveira/download/ESWA2015.pdf
- Amato et al. (2017), *Deep Learning for Decentralized Parking Lot Occupancy Detection* — public preprint PDF: https://openportal.isti.cnr.it/data/2017/366883/2017_366883.preprint.pdf

The image datasets are intended for computer-vision experiments. The papers are background documents for reviewing the engineering context and evaluation approaches; they are not claimed to be training data for the vision model. Check dataset licences and access terms before use.

## Responsible AI Risks

- **Incorrect occupancy detection:** a vacant bay may be reported as occupied, or an occupied bay as vacant, leading to unreliable guidance.
- **Tracking errors:** a vehicle may be associated with the wrong position or parking bay.
- **Privacy:** camera footage may capture unnecessary visual information. Minimise collection and use controlled miniature scenes where possible.

## Proposed Evaluation

Prepare a labelled set of parking images or video frames. Evaluate occupancy-detection accuracy, vehicle-tracking consistency, guidance correctness and response time. Report the testing conditions and limitations.

## Weekly Progress

| Week | Focus | Deliverable | Status |
|------|-------|-------------|--------|
| W1 | Project framing and system map | Part A observation evidence, Part B system map and individual Part C project scope | Complete |
| W2 | Requirements, evaluation and V1 architecture | Requirements specification, evaluation criteria, architecture diagram, ADR and traceability | Complete |
| W3 | Data inventory, preparation and V1 pipeline | Executed notebook, inventory, decisions, diagram, Part B and Part C manifests, and prepared image outputs | Complete |

## Week 1 — Project Framing and System Map

- [Part A observation sheet](docs/week1_observation_sheet.md)
- [Part A evidence folder](docs/evidence/)
- [Part B system map (PNG)](docs/week1_system_map.png)
- [Part B editable system map (Draw.io)](docs/week1_system_map.drawio)
- [Part B system map notes and reflection](docs/week1_system_map_notes.md)
- [Part C individual project scope](docs/week1_project_scope.md)

## Week 2 — Requirements, Evaluation and Architecture

- [Project framing, application scope and requirements specification (Sections 0–4)](docs/week2_requirements.md)
- [Evaluation criteria and reflection](docs/week2_evaluation_criteria.md)
- [V1 architecture, architecture decisions, trade-offs and requirements traceability](docs/week2_architecture_decisions.md)
- [V1 architecture diagram (PNG)](docs/week2_architecture_v1.png)
- [Editable V1 architecture source (Draw.io)](docs/week2_architecture.drawio)

The Week 2 documents connect the project objectives to functional, non-functional and Responsible AI requirements; define measurable evaluation criteria; and explain the conceptual architecture, supporting components, architecture decisions, accepted trade-offs and traceability to later evidence.

## Week 3 — Data Preparation and V1 Pipeline

- [Data inventory and Part B/C execution record](docs/week3_data_inventory.md)
- [V1 image preparation pipeline diagram (PNG)](docs/week3_pipeline_diagram.png)
- [Editable V1 pipeline diagram (SVG)](docs/week3_pipeline_diagram.svg)
- [Pipeline decisions, stage contracts and traceability](docs/week3_pipeline_decisions.md)
- [Executed Week 3 pipeline notebook](notebooks/week3_pipeline.ipynb)

**Selected pathway:** C — Computer Vision / Images. The six lecturer-supplied synthetic shape images were inspected and prepared in Colab. The selected configuration converts images to RGB and uses aspect-ratio-preserving fit with white padding to produce 224 × 224 outputs. The notebook records the preparation comparison, validation, lineage registration, V1 pipeline execution and repeatability smoke check. Six Part B prepared images and six Part C pipeline outputs were produced; the repeatability check passed.

**Manifest and output locations:**
- Part B lineage manifest: `data/week3_manifest.json`
- Part B prepared image manifest: `data/processed/image_manifest.csv`
- Part B prepared images: `data/processed/images/`
- Part C pipeline manifest: `data/pipeline_v1/images/pipeline_manifest.json`
- Part C pipeline outputs: `data/pipeline_v1/images/`

**Scope and data limitation:** The six supplied images are synthetic shape images used to demonstrate and validate the data-preparation workflow. They are not real parking-lot images and do not demonstrate SmartPark occupancy detection, vehicle tracking, number-plate recognition or model performance. The repeatability result is a smoke check in the supplied Colab runtime, not a claim of cross-environment reproducibility. The original course ZIP and raw source images were not committed because redistribution rights were not verified.

## Repository Structure

```text
README.md
notebooks/
└── week3_pipeline.ipynb
data/
├── week3_manifest.json
├── processed/
│   ├── image_manifest.csv
│   └── images/
│       ├── shape_01_17a1e2828014.png
│       ├── shape_02_dfa0925ff27b.png
│       ├── shape_03_bb10030d1f0b.png
│       ├── shape_04_2514fccc54d1.png
│       ├── shape_05_94d4f1150747.png
│       └── shape_06_5908f2d0dabc.png
└── pipeline_v1/
    └── images/
        ├── pipeline_manifest.json
        ├── prepared_001.png
        ├── prepared_002.png
        ├── prepared_003.png
        ├── prepared_004.png
        ├── prepared_005.png
        └── prepared_006.png
docs/
├── evidence/
├── week1_observation_sheet.md
├── week1_system_map.png
├── week1_system_map.drawio
├── week1_system_map_notes.md
├── week1_project_scope.md
├── week2_requirements.md
├── week2_evaluation_criteria.md
├── week2_architecture_decisions.md
├── week2_architecture_v1.png
├── week2_architecture.drawio
├── week3_data_inventory.md
├── week3_pipeline_diagram.png
├── week3_pipeline_diagram.svg
└── week3_pipeline_decisions.md
```

## Current Project Status

The repository documents the project scope and the engineering work completed through Week 3. The Week 3 notebook demonstrates an image-preparation pipeline using synthetic practice data. The SmartPark miniature parking prototype, real parking-scene dataset collection, model training and project-level performance evaluation remain part of the planned project development; they are not claimed as completed by the Week 3 exercise.
