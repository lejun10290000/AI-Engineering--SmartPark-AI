# SmartPark AI — AI Engineering Project

> SmartPark AI is a computer-vision project for parking-bay occupancy detection, vehicle tracking and basic parking guidance in a miniature parking layout.

This repository contains the group evidence for Parts A and B, my individual Part C project scope, and the engineering artefacts through Week 4.

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
| W3 | FYP data inventory, preparation and V1 pipeline | Executed Colab notebook, inventory, decisions, diagram and verified data hand-off | 285 records prepared |
| W4 | FYP data quality and responsible data practices | Executed Colab notebook, cleaned exports, validation evidence and quality report | GO for offline development and replay |

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

The Computer Vision pathway prepares actual FYP miniature-parking data. It verified the image fingerprints and retained original JPEG geometry and time alignment for **85 detector-pilot images and 200 replay frames**. Repeating the pipeline reproduced all 285 ordered references.

- [Executed pipeline notebook](notebooks/week3_pipeline.ipynb) · [Open in Colab](https://colab.research.google.com/github/lejun10290000/AI-Engineering--SmartPark-AI/blob/main/notebooks/week3_pipeline.ipynb)
- [Data inventory](docs/week3_data_inventory.md) and [pipeline decisions](docs/week3_pipeline_decisions.md)
- [Pipeline diagram](docs/week3_pipeline_diagram.png) and [editable Draw.io source](docs/week3_pipeline_diagram.drawio)
- [Prepared image index](data/processed/image_manifest.csv), [lineage manifest](data/week3_manifest.json) and [V1 pipeline run manifest](data/pipeline_v1/images/pipeline_manifest.json)

## Week 4 — Data Quality and Responsible Data Practices

The FYP review passed all seven applicable validation rules. The cleaned JSONL and pickle both reloaded with **285 records**. The decision is **GO for offline prototype development and replay**. Formal accuracy and safety evaluation requires independent labels and new held-out recordings.

- [Executed quality notebook](notebooks/week4_quality.ipynb) · [Open in Colab](https://colab.research.google.com/github/lejun10290000/AI-Engineering--SmartPark-AI/blob/main/notebooks/week4_quality.ipynb)
- [Data quality and governance report](docs/week4_data_quality_report.md)
- [Validation rules](docs/week4_validation_summary.csv), [validation failures](docs/week4_validation_failures.csv), [duplicate review](docs/week4_duplicate_review.csv) and [coverage chart](docs/week4_coverage_by_source.png)
- [Cleaned JSONL](data/prepared_chunks_clean.jsonl), [cleaned pickle](data/chunks_clean.pkl) and [review summary](docs/week4_fyp_review_summary.json)

### Rerun the notebooks

Use the approved private **smartpark_fyp_review_input.zip** containing the project images and source mapping. Set **SMARTPARK_FYP_INPUT_ZIP** to its path or choose it in the notebook's Colab prompt. The image references in the prepared and cleaned metadata are relative to that ZIP. Load pickle files only from sources you trust.

For Topic 7 use the reviewed FYP development records and the matching private image archive. Refer to the scope and limitations in the Week 4 report.

## Current Project Status

The FYP image hand-off is ready for offline SmartPark prototype development and replay. The independent evaluation described in the Week 2 criteria is planned separately.
