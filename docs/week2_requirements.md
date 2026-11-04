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

**Requirements note:** These are initial, testable functional requirements derived from the Week 1 scope. They describe what SmartPark AI must do, not the model, libraries, camera hardware or software architecture used to implement it. Numerical performance targets and detailed non-functional requirements remain to be defined during later requirements and evaluation work.
