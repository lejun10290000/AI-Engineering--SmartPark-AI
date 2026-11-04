# Week 2 Architecture — SmartPark AI

**Project:** SmartPark AI — Computer Vision-Based Parking Occupancy Detection and Vehicle Tracking  
**Version:** 1.0  
**Architecture level:** Initial conceptual application architecture for the miniature parking prototype.

## 1. Start with the simplest architecture

The initial user interaction can be represented as:

```text
Driver / Operator → SmartPark Application → Parking AI Capability → Response
```

The driver or visitor requests an available parking bay, or the parking operator views occupancy. The application passes the task to the parking AI capability and presents the occupancy result or parking guidance.

This is a useful starting point, but it is not sufficiently explicit to support all requirements. The system also needs camera images or video, a defined miniature parking layout, vehicle tracking across frames, a way to maintain current bay/track state, vacant-only selection and responsible handling of uncertain results and captured footage.

## 2. Simple Architecture Requirement Check

The simple flow (Driver / Operator → SmartPark Application → Parking AI Capability → Response) is a useful starting point, but by itself it does not describe every behaviour and quality target in Part A or the evaluation criteria in Part B. The following check identifies what each important requirement needs and whether the simple flow is enough.

| Requirement ID | What does the requirement need? | Architecture Response |
|---|---|---|
| **FR1, NFR2, RAI1** — Occupancy detection and correctness | The system must classify each defined bay as occupied or vacant, achieve the proposed macro-F1 target, and avoid presenting an occupied or uncertain bay as vacant. | **Simple flow: not sufficient by itself.** Add **camera input** for image/video frames, a **defined parking-layout/bay map** to identify each bay, and a **vision occupancy capability**. Add a **responsible output check** so occupied or unknown bays cannot be recommended as vacant. Evaluate using held-out labelled frames, macro-F1 (target ≥ 0.90) and false-vacant events (target 0). |
| **FR2, NFR3, RAI2** — Vehicle tracking and uncertainty handling | The system must maintain a temporary track for each visible vehicle across frames, meet the proposed track-association target, and avoid position-dependent guidance when a track is uncertain or unconfirmed. | **Simple flow: not sufficient.** A one-off query-to-response flow does not represent continuity across video frames. Add **sequential video-frame input** and **temporary vehicle-track state** to maintain vehicle positions. Add a **track-confidence/uncertainty control** that marks lost or mismatched tracks as unconfirmed and blocks guidance based on them. Evaluate with annotated clips (tracking target ≥ 90%; zero guidance events from incorrect/unconfirmed tracks). |
| **FR3** — Bay occupancy updates | The system must update a bay's status when a tracked vehicle enters or leaves that defined bay, rather than treating a vehicle passing nearby as occupying it. | **Simple flow: not sufficient.** Add **bay-boundary mapping** and **parking-state update logic** that relates a tracked vehicle's position to a defined bay and records the occupied/vacant transition. |
| **FR4, FR5** — Availability selection and parking guidance | The system must select a bay whose latest status is vacant, report when no vacant bay is identified, and show the selected bay's identifier and location or a simple direction. | **Simple flow: partially sufficient.** It can represent a request and response, but it does not define how a safe bay is selected. Add **availability-selection and guidance capability** using the current parking state and defined layout. Exclude occupied and unknown bays; return a clear no-available-bay response when appropriate. Evaluate with controlled layout scenarios (target ≥ 95% correct decisions/guidance and zero occupied-bay recommendations). |
| **NFR1** — Processing latency | The system must return occupancy and vehicle-position results within the proposed 95th-percentile latency target of ≤ 2.0 seconds. | **Simple flow: sufficient as a high-level path, but not measurable on its own.** Add **measurement points/timestamps** at frame receipt and result availability, plus an evaluation process to calculate 95th-percentile latency over at least 100 frames. This is test instrumentation, not a new user-facing runtime feature. |
| **NFR4** — Operational reliability | The full prototype must complete repeated test sequences without crashing, losing its input stream or failing to provide an output. | **Simple flow: not sufficient to express the operational quality.** Add **input-stream/status monitoring** and clear handling of unavailable input or missing results, along with a **repeatable test process**. Evaluate at least 20 sequences; the proposed target is ≥ 95% successful runs (at least 19 of 20). |
| **RAI3** — Privacy and data minimisation | The system must retain only footage needed for the miniature parking evaluation and must not retain incidental identifiable people or unrelated personal information. | **Simple flow: not sufficient.** Add a **privacy boundary/data-handling control** around captured and retained footage: limit collection to the miniature scene, exclude or remove incidental personal information, and review retained samples. The proposed review threshold is zero privacy-violating retained files in the sample. |

**Decision:** Keep the simple user → application → AI capability → response flow as the overall starting point. Extend it with camera input, a defined bay layout, temporary tracking/parking state, availability and guidance logic, responsible output checks, and privacy handling because the requirements justify them. Use timestamps, monitoring and labelled test material to evaluate quality; do not turn every measurement activity into a separate runtime component.

---

## 3. Requirements-driven architecture


![SmartPark AI requirements-driven application architecture](week2_architecture.svg)

**Editable source:** [week2_architecture.drawio](week2_architecture.drawio)  
**Rendered diagram:** [week2_architecture.svg](week2_architecture.svg)

The extended architecture adds only the capabilities justified by the project requirements:

1. **Camera input:** supplies images or video frames from the miniature parking scene for FR1–FR3.
2. **Privacy boundary:** limits retained footage to what is needed for the prototype and excludes incidental personal information, supporting RAI3.
3. **Vision AI capability:** analyses the camera input for occupied/vacant bays and vehicle movement, supporting FR1 and FR2.
4. **Defined miniature parking layout:** provides bay identifiers, boundaries and positions so detections and guidance can refer to the correct bay, supporting FR1 and FR5.
5. **Current parking state:** represents the latest occupied/vacant/unknown status and temporary vehicle tracks; this supports occupancy updates and tracking continuity under FR2–FR3.
6. **Availability and guidance:** uses the latest parking state and layout to select a recorded vacant bay or report that no available bay is identified, supporting FR4–FR5.
7. **Responsible output checks:** excludes occupied or unknown bays from recommendations and withholds position-dependent guidance when a vehicle track is unconfirmed, supporting RAI1–RAI2.
8. **Response:** returns an occupancy view or the selected bay identifier and location/direction, or reports that no available bay is identified.

The evaluation area in the diagram is shown separately from the runtime flow. It represents the test process used to measure requirements, not another user-facing application feature.

## 4. Requirement traceability

| Requirement / objective | Architecture element | Why it is needed |
|---|---|---|
| FR1 / O1 — identify occupied and vacant bays | Camera input, vision AI capability, defined bay layout | The system needs visual input and bay boundaries to classify each defined parking bay. |
| FR2 / O2 — track vehicles across frames | Vision AI capability and current parking state | A temporary track must be maintained and updated as a vehicle moves through the observed scene. |
| FR3 / O1–O2 — update a bay when a vehicle enters or leaves | Current parking state and layout | Vehicle movement must be associated with the relevant marked bay so its status can be updated. |
| FR4 / O2 — return a vacant bay or report none | Availability and guidance | The system must select only a bay whose latest recorded status is vacant and handle the no-vacancy case. |
| FR5 / O2 — provide basic parking guidance | Defined layout, availability and guidance, response | The chosen bay identifier and location/direction must match the miniature layout. |
| NFR1 / O3 — frame-to-result latency | End-to-end processing flow and evaluation process | The time from frame receipt to result must be measured against the proposed latency threshold. |
| NFR2 / O1, O3 — occupancy classification correctness | Vision output and labelled test set | Predictions must be compared with manually labelled occupied/vacant ground truth. |
| NFR3 / O2, O3 — tracking consistency | Vehicle tracks and annotated test clips | Track associations must be compared with manual vehicle annotations. |
| NFR4 / O3 — operational reliability | Complete runtime flow and repeated test sequences | The full prototype must be tested for crashes, input loss and missing outputs. |
| RAI1 — misleading vacant status | Responsible output checks | Occupied and unknown bays must not be recommended as vacant. |
| RAI2 — incorrect vehicle position | Track state and responsible output checks | Unconfirmed or mismatched tracks must not be used for position-dependent guidance. |
| RAI3 — unnecessary personal information | Privacy boundary | Retained footage must be limited to the miniature scene needed for evaluation. |

## 5. What is deliberately not added yet

The current scope does not justify choosing a particular foundation model, object-detection framework, database, cloud service, camera model, authentication system, payment facility, GPS navigation service or autonomous vehicle-control component. These are implementation or additional product decisions, not necessary parts of the initial requirements-driven architecture. The hardware arrangement and processing platform remain to be decided.

## 6. Architecture boundary

This is a conceptual architecture for a controlled miniature parking prototype. It does not claim production deployment in a shopping centre or public car park. The system provides parking information and simple guidance; it does not identify real drivers, control vehicles or guarantee real-world parking availability.

## 7. Architecture decisions

The design choices, alternatives considered, accepted trade-offs and initial requirements traceability matrix are recorded separately in [week2_architecture_decisions.md](week2_architecture_decisions.md). This decision record explains why the additional architecture components are justified by the project requirements.
