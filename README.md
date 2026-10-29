# SmartPark AI — AI Engineering, Topic 1 Lab

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
| W1 | System map + project scope | Part A evidence, Part B map, Part C scope, README | Core files present; cross-group reflection omitted per lecturer guidance |


### Part B Reflection

The **model-to-output validation interaction** has the greatest concentration of related failure points in this map: a hallucinated claim (F3), a missed safety issue (F4), and a user mistaking advice for a completed external action (F6). Engineering effort should therefore concentrate on checking generated responses, applying safety checks, and clearly communicating whether the system has actually completed an action. This connects to Topic 1 quality dimensions: **accuracy and reliability** (reduce unsupported answers), **safety** (reduce harmful outputs), and **transparency** (make the system’s capabilities and action status clear). These are plausible failure scenarios shown in a conceptual map, not incidents proven to have occurred.

**Part B files:** [System map PNG](docs/week1_system_map.png) · [Editable Draw.io source](docs/week1_system_map.drawio) · [Map explanation and reflection notes](docs/week1_system_map_notes.md)

## Part C — Individual Project Scope

- [Full Part C scope](docs/week1_project_scope.md)

The scope defines the proposed system, objectives, users, sources and risks. SmartPark AI is a proposed prototype, not yet a functioning AI application.

## Repository Structure

```text
README.md
docs/
├── week1_observation_sheet.md   # Part A
├── evidence/                    # Part A screenshots and index
├── week1_system_map.png         # Part B submission image
├── week1_system_map.drawio      # Part B editable source
├── week1_system_map_notes.md    # Part B explanation and reflection
└── week1_project_scope.md       # Part C — individual SmartPark AI scope
```
