# Week 2 Requirements — Section 0: Project Framing & Application Scope Checkpoint

**Project:** SmartPark AI — Computer Vision-Based Parking Occupancy Detection and Vehicle Tracking  
**Basis:** Week 1 project scope and repository README  
**Status:** Initial scope checkpoint; requirements may be refined after further analysis.

| Section | SmartPark AI — Initial Requirements |
|---|---|
| **Problem Evidence** | **Evidence 1 — Smart parking need:** Lin, Rivano and Le Mouël (2017) survey smart parking solutions, their functions and challenges, providing research context for systems that help drivers find suitable parking. This supports the general need for better parking information, but it does not measure search time at a specific car park. **Source:** *A Survey of Smart Parking Solutions*. https://doi.org/10.1109/TITS.2017.2685143 <br><br> **Evidence 2 — Camera-based occupancy feasibility:** de Almeida et al. (2015) introduce the PKLot dataset and investigate classifying parking spaces as vacant or occupied from images. This supports the feasibility of image-based occupancy detection. **Source:** *PKLot—A Robust Dataset for Parking Lot Classification*. https://doi.org/10.1016/j.eswa.2015.02.009 |
| **Existing Approaches** | **1. PKLot parking occupancy dataset and classification study:** Provides labelled parking-lot images for researching vacant/occupied-space classification. **Limitation:** It does not itself provide an integrated vehicle-tracking and parking-guidance application. **Source:** https://web.inf.ufpr.br/vri/databases/parking-lot-database/ <br><br> **2. CNRPark / CNRPark+EXT:** Provides camera-based parking-space images and occupancy labels for visual occupancy-classification research. **Limitation:** Vehicle tracking through a layout and user guidance require additional application-specific components. **Source:** https://cnrpark.it/ <br><br> **3. Ultralytics YOLO object-detection framework:** Provides object-detection capabilities that can locate vehicles in images. **Limitation:** Detection alone does not implement parking-bay occupancy logic, persistent vehicle tracking or guidance. **Source:** https://docs.ultralytics.com/ |
| **Project / Engineering Gap** | The existing datasets and object-detection approach address parts of the problem, but the proposed project needs to integrate **parking-bay occupancy detection, vehicle tracking and simple guidance** into one workflow. The integration will be investigated in a controlled miniature parking environment rather than a live car park. |
| **Objective O1** | To design and develop a computer-vision prototype that detects occupied and vacant parking bays in a miniature parking layout. |
| **Objective O2** | To implement vehicle tracking and simple guidance towards an available parking bay. |
| **Objective O3 (if relevant)** | To evaluate the prototype using detection accuracy and tracking/guidance performance. |
| **Intended Users** | **Primary users:** Drivers or visitors who need to identify an available parking space. **Secondary users:** Parking operators who need to monitor parking-bay occupancy. |
| **Supported Tasks** | 1. Analyse parking-lot images or video frames from the miniature prototype. 2. Identify defined parking bays as occupied or vacant. 3. Detect and track vehicles within the observed miniature layout. 4. Identify an available bay based on the detected occupancy. 5. Provide simple guidance towards an available parking bay. |
| **Information / Knowledge Domain** | The system is limited to parking-lot and vehicle information: miniature parking-layout/bay definitions, occupied or vacant bay status, visual vehicle detections and vehicle positions across frames, and the availability information needed for basic parking guidance. |
| **Operating Context** | A controlled miniature parking layout used for prototype development and testing. Camera images or video are analysed to observe parking bays and vehicle movement. The Week 1 scope does not define a live shopping-centre deployment. |
| **Important Constraints** | - The prototype is evaluated in a **miniature parking environment**, so results cannot be assumed to transfer directly to a real car park. <br>- Public datasets (PKLot and CNRPark/CNRPark+EXT) may support initial computer-vision experiments, while project-collected miniature footage is needed to test the actual prototype. <br>- Project-collected occupancy and vehicle-position labels must be prepared for evaluation. <br>- Dataset access terms and licences must be checked before use. <br>- Detection accuracy, tracking consistency, guidance correctness and response time are the stated evaluation concerns. <br>- The Week 1 documents do not yet specify final hardware, processing platform, budget or numerical performance targets; these need to be decided in later requirements work. |
| **Explicit Out-of-Scope Tasks** | - Deploying the system in a real shopping centre or public car park. <br>- Guaranteeing live parking availability in real-world facilities. <br>- Claiming production-level reliability based only on miniature-prototype results. <br>- Controlling or steering real vehicles; the proposed system provides guidance only. |
| **Evaluation Focus** | 1. **Occupancy detection:** compare predicted occupied/vacant bay status against manually labelled examples. 2. **Vehicle tracking:** assess whether vehicles are consistently associated with their positions across frames. 3. **Guidance correctness:** check whether the system directs the user towards a bay labelled as available. 4. **Response time:** record the time needed to produce the occupancy/tracking/guidance output. Report the test setup, dataset or footage used, and limitations of the miniature environment. |

## Scope Notes

- This checkpoint translates the Week 1 project scope into an initial application boundary; it is not a claim that the system has already been implemented.
- Public datasets are supporting data for experiments. They do not guarantee that a model trained or tested on real-car images will perform equally well on miniature toy-car footage.
- The exact camera arrangement, processing hardware, budget, bay-layout representation and numerical acceptance thresholds remain to be specified in later requirements work.

---

# Requirements Specification — SmartPark AI

**Section:** 1 — Functional Requirements  
**Version:** 1.0

| # | Requirement — The system SHALL | Scope Boundary — SHALL NOT (where relevant) |
|---|---|---|
| **FR1** | The system shall identify each predefined parking bay in the miniature layout and report its current occupancy status as **occupied** or **vacant** from the analysed camera image or video frame. The result must be checkable against a manually labelled test image or frame. | The system SHALL NOT report occupancy for areas or parking bays outside the defined miniature parking layout. |
| **FR2** | The system shall detect vehicles within the monitored miniature layout and maintain a separate temporary track for each detected vehicle across consecutive video frames, updating the vehicle's reported position as it moves. This can be tested by moving one or more toy vehicles through the camera view and checking whether their tracks and positions are updated. | The system SHALL NOT identify a real driver, determine a person's identity, or claim to recognise a vehicle's real-world identity. |
| **FR3** | The system shall update the occupancy status of a defined parking bay when a tracked vehicle is detected entering or leaving that bay. This can be tested by recording the bay status before vehicle entry, after entry and after the vehicle leaves. | The system SHALL NOT mark a bay as occupied solely because a vehicle passes nearby without entering the marked bay. |
| **FR4** | When parking availability is requested, the system shall identify and return at least one parking bay whose latest recorded occupancy status is **vacant**. When no bay is recorded as vacant, it shall report that no available bay is identified. | The system SHALL NOT recommend a bay whose latest recorded status is **occupied**. |
| **FR5** | After an available bay is identified, the system shall provide basic guidance by showing the selected bay's identifier and location on the miniature parking layout, or by indicating a simple direction towards that bay. The guidance can be checked against the defined layout and the selected bay. | The system SHALL NOT control or steer a vehicle, make autonomous driving decisions, or claim to provide real-world navigation for a public car park. |

## Initial Functional Requirement Test Scenarios

| Requirement | Example test scenario | Expected observable result |
|---|---|---|
| **FR1** | Submit a labelled frame showing one occupied bay and one vacant bay. | The system reports the correct occupied/vacant status for each defined bay. |
| **FR2** | Move a toy vehicle through the miniature layout across consecutive video frames. | The system detects the vehicle and updates its temporary track and position while it remains visible. |
| **FR3** | Move a toy vehicle into a marked bay and then move it out. | The bay changes to occupied after entry and to vacant after the vehicle leaves. |
| **FR4** | Provide an occupancy state containing vacant bays, then test a layout in which all bays are occupied. | The system returns at least one vacant bay in the first case and reports that no available bay is identified in the second case. |
| **FR5** | Request guidance to a bay returned as vacant. | The system displays the correct bay identifier and location or a simple direction matching the miniature layout. |

**Requirements note:** These are initial, testable functional requirements derived from the Week 1 scope. They describe what SmartPark AI must do, not the model, libraries, camera hardware or software architecture used to implement it. The initial non-functional requirements and measurable prototype targets are specified in Section 2. Their suitability should be reviewed after the test hardware and camera configuration are fixed.


---

# Non-Functional Requirements — SmartPark AI

**Section:** 2 — Non-Functional Requirements  
**Version:** 1.0

The following are initial acceptance targets for the miniature parking prototype. Measurements must be collected under a documented test setup. The thresholds are proposed targets, not claims that the prototype has already achieved them.

| # | Quality Attribute | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| **NFR1** | **Performance** | The system shall produce an updated parking-occupancy and vehicle-position result within a reasonable delay after receiving an input frame. | Record the time when each test frame is received and when its corresponding occupancy and tracking result becomes available. Repeat across at least 100 test frames using the same documented camera and processing setup. | 95th-percentile frame-to-result latency (seconds). | **≤ 2.0 seconds** for at least 95% of measured frames. |
| **NFR2** | **Correctness — Occupancy Detection** | The system shall correctly classify defined parking bays as occupied or vacant on held-out test images from the miniature layout. | Compare predictions against manually labelled ground-truth images. Use at least 100 held-out frames containing both occupied and vacant bays, and calculate the macro-F1 score across the two classes. | Macro-F1 score for occupied and vacant classes. | **≥ 0.90** macro-F1 on the held-out test set. |
| **NFR3** | **Tracking Consistency** | The system shall maintain the correct temporary track association for a visible vehicle as it moves through the miniature layout. | Manually annotate vehicle identities and visible positions in at least five recorded test clips. Compare the system's track assignments with the annotations. | Correct track-association rate: correctly associated visible vehicle frames divided by all annotated visible vehicle frames. | **≥ 90%** correct track association across the evaluated clips. |
| **NFR4** | **Operational Reliability** | The system shall complete repeated prototype test sequences without crashing, losing its input stream, or failing to provide an output. | Run at least 20 repeatable test sequences covering vehicle movement, bay entry/exit and occupancy/guidance output. Record each completed run and any crash, input loss or missing result. | Successful test-run rate: completed runs without a listed failure divided by total runs. | **≥ 95%** successful runs (at least 19 of 20 runs). |

## NFR Measurement Notes

- Record the camera, processing device, input resolution, lighting conditions and test-data split used for each evaluation. These details are not fixed in the Week 1 scope and must not be assumed.
- Use test images and clips that are separate from any data used to tune or develop the system, so the evaluation is not based only on examples the system has already seen.
- Report the actual measured values, sample sizes and test conditions. Do not describe a proposed threshold as an achieved result.
- If the final hardware or test setup changes materially, repeat the measurements and review whether the proposed thresholds remain appropriate.

---

# Responsible AI Requirements — SmartPark AI

**Section:** 3 — Responsible AI Requirements  
**Version:** 1.0

The requirements below convert the Responsible AI risks identified in the Week 1 project scope into observable controls. The thresholds are proposed acceptance criteria for the miniature prototype, not measured results.

| # | Responsible AI Risk | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| **RAI1** | **Incorrect occupancy detection — misleading availability information** | The system shall not present or recommend a parking bay as **vacant** when the bay is labelled occupied in the test ground truth. If the occupancy status cannot be determined, the system shall show it as **unknown** and exclude it from the available-bay recommendations. | Test at least 100 labelled occupied-bay observations, including cases with different vehicle positions and lighting. Compare the displayed status and recommendations with the manually labelled ground truth. | Number of false-vacant status or recommendation events among labelled occupied-bay observations. | **0 false-vacant events** in the test sample. Uncertain bays must be shown as unknown, not vacant. |
| **RAI2** | **Vehicle-tracking errors — incorrect position may lead to confusing guidance** | The system shall identify an uncertain or lost vehicle track as **unconfirmed** and shall not use that unconfirmed track to provide position-dependent vehicle guidance until the track is re-established. | Run at least 20 controlled video clips that include vehicle movement, temporary occlusion and vehicles leaving the camera view. Compare guidance events with manually checked vehicle tracks and positions. | Number of guidance events issued using an incorrect, mismatched or unconfirmed vehicle track. | **0 guidance events** based on an incorrect, mismatched or unconfirmed track in the test clips. |
| **RAI3** | **Privacy — camera footage may capture unnecessary visual information** | The system shall limit retained project footage to the miniature parking scene needed for occupancy and vehicle-position evaluation. It shall not retain identifiable images of people or unrelated personal information captured incidentally in the camera view. | Review at least 100 sampled captured or stored frames/files, including test cases where a person or unrelated personal information is incidentally visible outside the miniature parking area. Check whether such content is retained in the project dataset or outputs. | Number of retained frames/files containing identifiable people or unrelated personal information. | **0 retained privacy-violating frames/files** in the reviewed sample. Incidental content must be excluded or removed before project data is retained. |

## Responsible AI Evaluation Notes

- Keep the test labels, test scenarios and review results so each RAI requirement can be checked later.
- A bay with an uncertain occupancy result must not be treated as vacant merely to provide a recommendation.
- When a vehicle track is lost or ambiguous, the system should communicate uncertainty rather than present an unverified position as reliable.
- Use controlled miniature scenes and minimise retained footage. Do not collect personal information that is not needed for the project.
- These thresholds are initial targets for the prototype. Record actual results and any limitations; do not report proposed thresholds as achieved performance.
