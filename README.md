# SmartPark AI — AI Engineering Project

> SmartPark AI is a proposed computer-vision prototype that detects parking-bay occupancy, tracks vehicles and provides basic parking guidance for drivers and parking operators.

This repository keeps the required group evidence for Part A and Part B alongside my individual Part C project scope.

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
| W3 | Data inventory, preparation and V1 pipeline | Data inventory, decisions, pipeline diagram and runnable Colab notebook committed; exported data manifests and prepared image outputs still need individual repository upload | In progress |


## Week 2 — Requirements, Evaluation and Architecture

- [Project framing, application scope and requirements specification (Sections 0–4)](docs/week2_requirements.md)
- [Evaluation criteria and reflection](docs/week2_evaluation_criteria.md)
- [V1 architecture, architecture decisions, trade-offs and requirements traceability](docs/week2_architecture_decisions.md)
- [V1 architecture diagram (PNG)](docs/week2_architecture_v1.png)
- [Editable V1 architecture source (Draw.io)](docs/week2_architecture.drawio)

The Week 2 documents connect the project objectives to functional, non-functional and Responsible AI requirements; define measurable evaluation criteria; and explain the conceptual architecture, supporting components, architecture decisions, accepted trade-offs and traceability to later evidence.

## Week 3 — Data Preparation and V1 Pipeline

- [Data inventory and Part B/C execution record](docs/week3_data_inventory.md)
- [Executed V1 image pipeline workflow (PNG)](docs/week3_pipeline_diagram.png)
- [Editable V1 pipeline workflow (SVG)](docs/week3_pipeline_diagram.svg)
- [Pipeline decisions, stage contracts and traceability](docs/week3_pipeline_decisions.md)

**Selected pathway:** C — Computer Vision / Images. The six lecturer-supplied synthetic shape images were processed in Colab. Cells 7, 8C and 9 completed; six prepared images were produced and the repeatability smoke check passed. These are data-preparation results only, not SmartPark model-performance results.

**Step C4 artefact transfer:** The runnable Colab notebook (`notebooks/week3_pipeline.ipynb`) is committed. The pipeline diagram (`docs/week3_pipeline_diagram.png`) is also present in the repository. The exported Part B manifest (`data/week3_manifest.json`), Part B image manifest and six prepared images (`data/processed/`), and Part C six prepared images plus `pipeline_manifest.json` (`data/pipeline_v1/images/`) remain in the supplied `week3_C4_artefacts.zip` export and have not yet been added as individual repository files. The original course ZIP/raw source images should not be committed until redistribution rights are verified.

## Part C — Individual Project Scope

- [Full Part C scope](docs/week1_project_scope.md)

The scope defines the proposed system, objectives, users, sources and risks. SmartPark AI is a proposed prototype, not yet a functioning AI application.

## Repository Structure

```text
README.md
notebooks/
└── week3_pipeline.ipynb
docs/
├── week1_observation_sheet.md   # Part A
├── evidence/                    # Part A screenshots and index
├── week1_system_map.png         # Part B submission image
├── week1_system_map.drawio      # Part B editable source
├── week1_system_map_notes.md    # Part B explanation and reflection
├── week1_project_scope.md       # Part C — individual SmartPark AI scope
├── week2_requirements.md        # Week 2 — Sections 0–4 requirements
├── week2_evaluation_criteria.md # Week 2 — evaluation criteria
├── week2_architecture_decisions.md # Week 2 — architecture, ADR and traceability
├── week2_architecture_v1.png       # Week 2 — architecture diagram (PNG)
└── week2_architecture.drawio       # Week 2 — editable architecture source
```
