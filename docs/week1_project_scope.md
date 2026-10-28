# Week 1 Project Scope — SmartPark AI

**Assignment:** AI Engineering, Topic 1 Lab, Part C (individual)  
**Project name:** SmartPark AI — Computer Vision-Based Parking Occupancy Detection and Vehicle Tracking  
**Domain:** Artificial intelligence / computer vision / smart transportation  
**Status:** Preliminary scope; refine as the project develops.

## Problem Statement

Drivers in busy parking facilities may spend time searching for available parking spaces, while parking operators need an efficient way to monitor occupancy. SmartPark AI proposes a computer-vision prototype that detects occupied and vacant parking bays, tracks vehicles, and provides simple parking guidance. The system will be demonstrated using a miniature parking layout rather than a live shopping-centre deployment.

## Evidence of the Problem

**Evidence 1 — Smart parking context:** Lin, Rivano and Le Mouël review smart parking solutions and the technologies, functions and challenges involved in helping drivers find suitable parking. This provides general context for the project; it does not independently measure the experience of a particular car park.  
**Source:** Lin, T., Rivano, H. and Le Mouël, F. (2017), *A Survey of Smart Parking Solutions*, IEEE Transactions on Intelligent Transportation Systems, 18(12), 3229–3253. https://doi.org/10.1109/TITS.2017.2685143

**Evidence 2 — Camera-based occupancy detection:** The PKLot study introduces a labelled parking-lot image dataset and investigates classification of parking spaces as vacant or occupied, supporting the feasibility of image-based occupancy experiments.  
**Source:** de Almeida, P. R. L. et al. (2015), *PKLot—A Robust Dataset for Parking Lot Classification*, Expert Systems with Applications, 42(11), 4937–4949. https://doi.org/10.1016/j.eswa.2015.02.009

## Initial Related Work / Existing Approaches (at least three)

1. **PKLot parking occupancy dataset and classification study**  
   **Source:** Paper: https://doi.org/10.1016/j.eswa.2015.02.009 ; official dataset page: https://web.inf.ufpr.br/vri/databases/parking-lot-database/  
   **Key limitation:** It provides labelled parking imagery for occupancy research but does not itself deliver an integrated vehicle-tracking and parking-guidance application.

2. **CNRPark / CNRPark+EXT parking occupancy datasets**  
   **Source:** https://cnrpark.it/  
   **Key limitation:** The datasets focus on visual parking-space occupancy; application-specific vehicle tracking and destination-based guidance require additional development.

3. **Ultralytics YOLO object-detection framework**  
   **Source:** https://docs.ultralytics.com/  
   **Key limitation:** Object detection can locate objects in images, but parking-bay occupancy logic, persistent tracking and user guidance need additional system components.

## Engineering Gap / Limitation

**Gap / Limitation:** A practical prototype is needed to integrate parking-bay occupancy detection, vehicle tracking and basic guidance in one workflow. This project will investigate that integration in a controlled miniature environment; it will not claim that the prototype is ready for deployment in a real car park.

## Initial Project Objectives

- **O1:** To design and develop a computer-vision prototype that detects occupied and vacant parking bays in a miniature parking layout.
- **O2:** To implement vehicle tracking and simple guidance towards an available parking bay.
- **O3:** To evaluate the prototype using detection accuracy and tracking/guidance performance.

## Target Users

Drivers or visitors who need to find an available parking space, and parking operators who need to monitor parking-bay occupancy.

## Data Sources (at least two)

1. **PKLot parking-lot database** — https://web.inf.ufpr.br/vri/databases/parking-lot-database/
2. **CNRPark / CNRPark+EXT dataset** — https://cnrpark.it/
3. **Project-collected footage:** images and video captured from the miniature parking prototype, with manually labelled parking-bay occupancy and vehicle positions.

Check each dataset's licence and terms before use. Project-collected footage can be used to test the miniature setup, with manually labelled occupancy and vehicle positions.

## Similar to EduAdvisor

Both projects apply AI to a practical user problem and aim to provide useful decision support through a system prototype.

## Different from EduAdvisor

SmartPark AI focuses on computer vision for parking occupancy and vehicle tracking, rather than education-related recommendations or advising. Its intended output is guidance towards an available parking bay.

## Responsible AI Risks (at least two)

1. **Incorrect occupancy detection:** A vacant bay may be reported as occupied, or an occupied bay as vacant, leading to unreliable guidance. Test against labelled examples and communicate that the prototype may make mistakes.
2. **Vehicle-tracking errors:** A vehicle may be associated with the wrong position or parking bay, causing confusing guidance. Evaluate tracking consistency and avoid presenting uncertain locations as confirmed.
3. **Privacy:** Camera footage may capture unnecessary visual information. Use a controlled miniature environment, minimise collection and avoid storing irrelevant imagery.

## Proposed Evaluation

Create a small labelled set of parking images or video frames. Evaluate occupancy-detection accuracy, vehicle-tracking consistency, parking-guidance correctness and response time. Describe the testing setup and limitations, and do not treat miniature-prototype results as proof of performance in a real car park.

## Scope for the Initial Prototype

**In scope:** camera-based parking-bay occupancy detection, vehicle tracking within a miniature layout, and basic guidance to an available bay.  
**Out of scope:** deployment in a real shopping centre, guaranteed real-time availability in public car parks, and claims of production-level reliability.
