# SmartPark AI — AI Engineering, Topic 1 Lab

> This repository contains the Topic 1 lab deliverables: group Part A (ChatGPT observation and evidence), group Part B (ChatGPT conceptual system map), and individual Part C (SmartPark AI project scope).

## Part A — Group Observation

The group observation sheet records the prescribed ChatGPT tests and findings.

- [Part A observation sheet](docs/week1_observation_sheet.md)
- [Part A evidence index and screenshots](docs/evidence/README.md)

## Part B — Group System Map

The conceptual map represents high-level components and possible flows for ChatGPT. It distinguishes observed behaviour from assumed internal components.

- [System map — PNG](docs/week1_system_map.png) (submission image)
- [Editable Draw.io source](docs/week1_system_map.drawio)

## Part C — Individual Project Scope: SmartPark AI

### Project overview

SmartPark AI is a proposed computer-vision prototype that detects parking-bay occupancy, tracks vehicles, and provides basic parking guidance for drivers and parking operators. The prototype will use a miniature parking layout rather than a live shopping-centre deployment.

### Problem statement

Drivers in busy parking facilities may spend time searching for available parking spaces, while parking operators need an efficient way to monitor occupancy. Computer vision can analyse camera images to identify occupied and vacant bays and track vehicles through a parking layout. This project will investigate these capabilities in a controlled miniature prototype.

### Evidence and related work

- Lin, T., Rivano, H. and Le Mouël, F. (2017), *A Survey of Smart Parking Solutions*, IEEE Transactions on Intelligent Transportation Systems. https://doi.org/10.1109/TITS.2017.2685143
- de Almeida, P. R. L. et al. (2015), *PKLot—A Robust Dataset for Parking Lot Classification*. https://doi.org/10.1016/j.eswa.2015.02.009
- CNRPark / CNRPark+EXT parking occupancy datasets: https://cnrpark.it/
- Ultralytics YOLO object-detection framework: https://docs.ultralytics.com/

These works and datasets support the study of camera-based parking occupancy or object detection; integrating occupancy detection, tracking and guidance remains part of the proposed engineering work.

### Project objectives

- **O1:** To design and develop a computer-vision prototype that detects occupied and vacant parking bays in a miniature parking layout.
- **O2:** To implement vehicle tracking and simple guidance towards an available parking bay.
- **O3:** To evaluate the prototype using detection accuracy and tracking/guidance performance.

### Data sources

- [PKLot parking-lot database](https://web.inf.ufpr.br/vri/databases/parking-lot-database/)
- [CNRPark / CNRPark+EXT dataset](https://cnrpark.it/)
- Project-collected images and video from the miniature parking prototype, with manually labelled parking-bay occupancy and vehicle positions.

Check each dataset's licence and terms before use, and record any preprocessing or labels added by the project.

### Responsible AI risks

- **Incorrect occupancy detection:** a vacant bay may be reported as occupied, or an occupied bay as vacant, leading to unreliable guidance.
- **Tracking errors:** a vehicle may be associated with the wrong position or parking bay.
- **Privacy:** camera footage may capture unnecessary visual information. Minimise collection and use controlled miniature scenes where possible.

### Proposed evaluation

Prepare a labelled set of images or video frames from the miniature parking layout. Evaluate parking-bay occupancy detection, vehicle-tracking consistency, guidance correctness and response time. Report test conditions and limitations; results from a miniature layout should not be presented as proof of performance in a real car park.

## Weekly Progress

| Week | Focus | Deliverable | Status |
|------|-------|-------------|--------|
| W1 | System map + project scope | Part A evidence, Part B map, Part C scope, README | ✅ |

## Repository Structure

```text
README.md
docs/
├── week1_observation_sheet.md   # Part A
├── evidence/                    # Part A supporting screenshots
├── week1_system_map.png         # Part B submission image
├── week1_system_map.drawio      # Part B editable source
└── week1_project_scope.md       # Part C — individual SmartPark AI scope
```

**Status:** This repository records the Week 1 scope and lab evidence. SmartPark AI is a proposed prototype, not yet a functioning AI application or a live car-park deployment.
