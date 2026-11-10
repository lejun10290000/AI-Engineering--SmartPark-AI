# Week 3 Data Inventory — SmartPark AI

**Project:** SmartPark AI — AI-Based Computer Vision for Smart Parking  
**Version:** 1.0  
**Date:** 10 November 2026  
**Practice source:** `Topic_3_Dummy_Datasets.zip` supplied for Topic 3  
**Selected pathway:** C — Computer Vision (`03_images/`)

> **Important:** The selected dataset is a synthetic teaching dataset. It contains six simple shape images, not cars, number plates, parking bays, or real camera footage. It is suitable for practising image ingestion, inspection, label handling, and basic preprocessing. It cannot demonstrate SmartPark AI's vehicle-detection or parking-occupancy performance.

## 1. Topic 2 requirement-to-data mapping (Step A1)

| Requirement ID | Needed information / feature | Candidate source | Why usable / constraint |
|---|---|---|---|
| **FR1 — Parking-bay occupancy detection** | Image pixels showing the parking scene; labels indicating whether a bay is occupied or vacant; ideally bay ID and bay-region coordinates | Practice source: `03_images/shape_01.png` to `shape_06.png` and `03_images/labels.csv`. Later replace with labelled overhead-camera images or video frames of the miniature parking prototype | The images allow us to practise loading and inspecting image data and matching filenames to labels. However, the labels are only `circle` or `square` plus colour. They do **not** represent cars, bays, or occupied/vacant states, so they cannot train or validate FR1. |
| **FR2 — Vehicle tracking (supporting requirement)** | Ordered video frames or image sequences; vehicle/marker identity; frame number or timestamp; object location/bounding box in each frame | No suitable tracking data is included in the dummy pack. Actual source should be overhead-camera video from the miniature parking prototype, with annotated vehicle/marker IDs and frame timestamps | The six shape images are independent still images. They contain no temporal sequence, vehicle identity, or movement annotations, so they cannot support tracking. |
| **RAI1 — Safe parking guidance** | Reliable occupancy state and confidence/uncertainty information so the system can avoid recommending occupied or unknown bays | Same future labelled parking-scene data as FR1, together with bay-state and model-confidence records | The dummy shape labels do not provide occupancy states or confidence values. The practice dataset therefore cannot verify that guidance avoids occupied or uncertain bays. The system should treat an unconfirmed bay as unavailable until its state is verified. |

### Requirement selection for this exercise

- **Primary functional requirement:** FR1 — detect parking-bay occupancy from camera images.
- **Supporting functional requirement:** FR2 — track a vehicle across consecutive frames. This is recorded as a data need, but cannot be practised with the supplied still images.
- **Responsible AI requirement:** RAI1 — do not recommend occupied or unknown bays. This requires reliable occupancy information and uncertainty handling; the dummy data cannot test it.

## 2. Selected dataset details

| Item | Observation |
|---|---|
| Dataset folder | `topic3_dummy_datasets/03_images/` |
| Image files | 6 PNG files (`shape_01.png`–`shape_06.png`) |
| Label file | `labels.csv` |
| Label columns | `filename`, `shape_label`, `colour` |
| Label categories | Shape: `circle`, `square`; colour: `red`, `blue` |
| Image format / mode | PNG, RGB |
| Image dimensions | Variable: width 92–152 px; height 97–132 px |
| Source type | Synthetic practice dataset supplied by the lecturer |
| Real-world coverage | No vehicles, number plates, parking bays, camera viewpoints, timestamps, or tracking IDs |

## 3. Initial data observations and preparation considerations

1. There are six images and six corresponding label rows. The `filename` column is the key used to associate each image with its label.
2. The images have different dimensions. A later model-training pipeline may need a consistent input size, but resizing should be documented and applied consistently.
3. The images are RGB. Confirm image mode and pixel dimensions during ingestion rather than assuming every image has the same shape.
4. Preserve the original `shape_label` and `colour` columns. Do not relabel circles or squares as cars or parking states.
5. Check that every filename in `labels.csv` exists and that every image has a matching label before processing.
6. Keep the original files in `data/raw/`; save resized or otherwise transformed copies in `data/processed/`. Do not overwrite the supplied originals.
7. Six images are far too few and too simple for a meaningful SmartPark model evaluation. Any result from this dataset must be described as a data-preparation exercise only.

## 4. What actual project data will replace the practice data?

For the real SmartPark AI project, the practice images should later be replaced by data captured from the miniature parking prototype:

- **Overhead camera images/video:** toy cars, parking-bay boundaries, and different occupancy conditions.
- **Occupancy annotations:** bay ID and state (`occupied`, `vacant`, or `unknown`) for each labelled frame.
- **Vehicle annotations:** vehicle/marker ID and bounding-box or centre coordinates.
- **Temporal information:** frame number or timestamp so consecutive frames can be used for tracking.
- **Optional gate-camera images:** close-up images for number-plate recognition, with clear labels for the toy-car plate text.

The actual dataset should reflect the planned camera position, lighting, toy-car sizes, parking layout, and movement conditions of the prototype. Only data that is genuinely captured and labelled should be described as project data.

## 5. Source and limitation record

| Field | Record |
|---|---|
| Source name | Topic 3 Dummy Dataset Pack |
| Selected files | `03_images/shape_01.png`–`shape_06.png`; `03_images/labels.csv` |
| Source classification | Synthetic practice dataset |
| Intended use | Practise image loading, inspection, label matching, and basic preprocessing |
| Not suitable for | Training or evaluating vehicle detection, parking occupancy detection, number-plate recognition, or vehicle tracking |
| Replacement plan | Collect and annotate overhead-camera images/video from the SmartPark miniature parking prototype |

## 6. Conclusion

Pathway C (Computer Vision) is the closest match to SmartPark AI because the project's main input is camera imagery. The supplied image dataset is therefore selected for this first coding lab. However, its simple shape and colour labels do not represent the project's real parking domain. It is only a temporary practice source; actual SmartPark data must be collected and labelled before making claims about system accuracy or performance.
