# Week 2 Architecture Decision Record (ADR) and Traceability Matrix — SmartPark AI

**Project:** SmartPark AI — Computer Vision-Based Parking Occupancy Detection and Vehicle Tracking  
**Version:** 1.0  
**Status:** Initial conceptual architecture decisions for the miniature parking prototype.

This document records why the main architecture components are included. Each decision is linked to a requirement or constraint and states the trade-off accepted. The decisions describe capabilities, not specific implementation technologies.

## 1. Architecture Decision Record (ADR)

### ADR-01 — Use camera-based visual input and a generic vision AI capability

| Decision | Requirement / Constraint | Alternatives Considered | Selected Option | Trade-off Accepted |
|---|---|---|---|---|
| Include camera-based vision input | FR1 requires occupied/vacant bay detection; FR2 requires vehicle detection and tracking across frames; NFR2 and NFR3 require measurable occupancy and tracking results. The project is a computer-vision prototype using a miniature parking layout. | Manual occupancy entry; separate sensors for each bay; camera input analysed by a generic vision AI capability. | **Camera input + generic Vision AI capability**, using images/video frames from the miniature scene. No specific model or camera technology is selected at this stage. | Camera-based analysis can support both occupancy detection and vehicle tracking using the same visual input, but it is sensitive to lighting, occlusion, camera position and image quality. These conditions must be included in testing. |

### ADR-02 — Maintain parking-layout and current parking-state information before selecting a bay

| Decision | Requirement / Constraint | Alternatives Considered | Selected Option | Trade-off Accepted |
|---|---|---|---|---|
| Include a defined parking layout and current parking-state capability | FR3 requires bay status to change when a vehicle enters or leaves; FR4–FR5 require vacant-bay selection and guidance; RAI1 requires occupied or uncertain bays not to be presented as vacant. | Send raw vision detections directly to the user; ask an operator to choose a bay manually; maintain bay identifiers, boundaries, current occupancy/unknown status and temporary vehicle tracks for availability selection. | **Defined parking layout + current parking state + availability/guidance capability.** Select only bays whose latest status is vacant; report when no available bay is identified. | Maintaining state and mapping vehicle positions to bay boundaries adds application logic and creates a need to manage stale or uncertain status. In return, the system can give bay-specific guidance and avoid recommending occupied or unknown bays. |

### ADR-03 — Add an uncertainty control before using vehicle tracks for guidance

| Decision | Requirement / Constraint | Alternatives Considered | Selected Option | Trade-off Accepted |
|---|---|---|---|---|
| Include a track uncertainty / responsible output control | RAI2 requires the system not to provide position-dependent guidance from an uncertain or lost vehicle track. FR5 limits the system to basic guidance rather than vehicle control. | Always use the latest detected position; silently reuse the last known position; mark a lost or mismatched track as unconfirmed and withhold position-dependent guidance until it is re-established. | **Explicit track uncertainty handling and output check.** Unconfirmed tracks are not used for position-dependent guidance. | Some guidance may be delayed or temporarily unavailable when a track is uncertain. This is accepted to reduce misleading guidance caused by an incorrect vehicle position. |

### ADR-04 — Apply data minimisation to camera footage

| Decision | Requirement / Constraint | Alternatives Considered | Selected Option | Trade-off Accepted |
|---|---|---|---|---|
| Include a privacy boundary for captured and retained footage | RAI3 requires footage to be limited to the miniature parking scene needed for evaluation and incidental personal information not to be retained. | Retain all camera footage; retain only relevant miniature-scene footage and labels; avoid retaining footage after processing unless it is required for documented evaluation. | **Privacy boundary and minimum necessary data retention.** Use a controlled miniature scene and exclude or remove incidental personal information before retaining project data. | Less footage is available for later debugging or re-analysis, but unnecessary personal information is less likely to be stored. |

### ADR-05 — Keep evaluation separate from the runtime user flow

| Decision | Requirement / Constraint | Alternatives Considered | Selected Option | Trade-off Accepted |
|---|---|---|---|---|
| Include a separate evaluation process and evidence collection | O3 requires evaluation of detection and tracking/guidance performance. NFR1–NFR4 and RAI1–RAI3 specify measurable quality and responsible-behaviour targets. | Rely on informal visual demonstrations; evaluate using labelled frames, annotated clips, repeatable scenarios, timestamps and documented result logs. | **A separate evaluation process** using held-out labelled data, controlled scenarios and recorded measurements. It is not an additional user-facing runtime feature. | Manual labelling and repeatable tests require time and effort, but they make results measurable, reproducible and traceable to the stated requirements. |

## 2. Initial Requirements Traceability Matrix

The evidence column describes evidence to be collected later. It does not imply that the prototype has already passed these tests.

| Objective | Requirement | Evaluation Criterion | Architecture Element | Later Evidence |
|---|---|---|---|---|
| **O1** — Detect occupied and vacant parking bays | **FR1, NFR2, RAI1** | Occupancy correctness: macro-F1 **≥ 0.90** and **0 false-vacant events** in at least 100 labelled occupied-bay observations. | Camera input; Vision AI capability; defined parking layout; responsible occupancy output check. | Held-out labelled frames, ground-truth labels, confusion matrix / macro-F1 calculation, and false-vacant audit results. |
| **O2** — Track vehicles and provide basic parking guidance | **FR2, NFR3, RAI2** | Tracking consistency: correct track-association rate **≥ 90%** and **0 guidance events** based on incorrect, mismatched or unconfirmed tracks. | Vision AI capability; temporary vehicle-track state; track uncertainty control; responsible output check. | Annotated video clips with normal movement, occlusion and re-entry; track-association calculations; guidance-event review. |
| **O2** — Select an available bay and guide the user | **FR3, FR4, FR5** | Availability and guidance correctness: at least **19 of 20 scenarios (≥ 95%)** correct, with **0 occupied-bay recommendations**. | Defined parking layout; current parking state; bay-status update; availability selection and guidance; user response. | Twenty controlled layout scenarios, expected bay status, selected bay ID, displayed location/direction and comparison against ground truth. |
| **O3** — Evaluate processing performance | **NFR1** | 95th-percentile frame-to-result latency **≤ 2.0 seconds**. | End-to-end runtime flow; evaluation timestamps and measurement process. | Timestamp log for at least 100 test frames and calculated 95th-percentile latency. |
| **O3** — Evaluate prototype operational reliability | **NFR4** | Successful test-run rate **≥ 95%** (at least 19 of 20 runs). | Complete runtime flow; input/status handling; repeatable evaluation process. | Records of at least 20 test sequences, including crashes, input loss, missing outputs and successful completions. |
| **O3** — Check privacy controls during evaluation | **RAI3** | **0 retained privacy-violating frames/files** in a review sample of at least 100 captured or stored items. | Privacy boundary; data-minimisation and retention control; privacy review process. | Sampled-frame/file audit record showing that incidental identifiable people or unrelated personal information were not retained. |

## 3. Final Architecture Review Checklist

- [x] Each major runtime component is justified by one or more functional or Responsible AI requirements.
- [x] Evaluation activities are shown separately from the user-facing runtime flow.
- [x] The architecture remains at capability level and does not commit to a specific model, database, camera, framework or cloud platform.
- [x] The selected decisions state alternatives and accepted trade-offs.
- [x] The traceability matrix links objectives, requirements, evaluation criteria, architecture elements and later evidence.
- [x] The architecture remains limited to the miniature parking prototype; it does not include real-world vehicle control or public-car-park deployment.

**Architecture boundary:** These are initial conceptual decisions. Specific technologies and implementation details may be selected later, once the project constraints and prototype setup are confirmed.
