# Week 3 Data Inventory — SmartPark AI

**Pipeline release:** week3-fyp-v1  
**Dataset scope:** Offline prototype development and replay using the FYP miniature car park.

## 1. Requirements and source selection

| Topic 2 requirements | Required evidence | Selected FYP source |
|---|---|---|
| FR1 and RAI1 — Occupancy and safe guidance | Actual toy vehicles, occupied and vacant bays, and examples of uncertain observations | Detector-pilot images and synchronised footage with per-frame bay telemetry |
| FR2 and RAI2 — Tracking and uncertainty | Consecutive source frames, elapsed time, positions, session-scoped track IDs and quality flags | Indexed replay from five FYP overhead-camera sessions |

The Computer Vision pathway uses JPEG imagery and structured metadata. The source view, label origin and temporal order matter for the project. Independent held-out ground truth is required for formal model evaluation.

## 2. Source inventory and access

| ID | Source | Contents and coverage | Label origin | Access |
|---|---|---|---|---|
| D01–D05 | FYP detector-pilot archive | 85 prepared RGB JPEGs from five source groups. Includes negative images, full-board crops and derived tiles. | Model-assisted car bounding boxes that were visually reviewed in the FYP pilot. | Project-owner authorised for the lab. Images remain in the private input bundle. |
| R01–R05 | FYP overhead camera archives and synchronised logs | 200 RGB replay frames across five recordings, dated 19–21 November 2026. Four consecutive ten-frame windows per recording. The legacy logs use session-scoped track IDs. | Prototype observations of bay states, estimated positions and alignment. | Same private image access. The public pipeline uses source aliases and image fingerprints. |

The approved private input is **smartpark_fyp_review_input.zip**. Its **source_inventory.json** stores the mapping from aliases to FYP recording names and original source file hashes. The identity of the exact prepared input is recorded in [the Week 3 lineage manifest](../data/week3_manifest.json). Some replay frames can show the operator's hands or hair. Keep camera images in approved storage and use an approved runtime to process them.

## 3. Inspection and preparation evidence

The notebook loaded the manifest and verified that its 285 image paths were unique. It sampled a detector image and a replay frame, inspected their dimensions and encoding, and compared the original view with an aspect-ratio-preserving preview. The detector source contains normalised boxes. The replay source uses indexed images and logs joined at the source frame and elapsed time.

The pipeline retains each source image's original bytes and geometry. The replay frames are intended for diagnosis and carry prototype overlays. Detector images provide the pilot's development inputs. The pipeline validates the SHA-256, JPEG decoding, RGB mode and label counts, then creates a portable image index with source aliases, dimensions, annotation provenance and role-specific counts.

| Evidence | Path |
|---|---|
| Executed notebook and repeatability output | [week3_pipeline.ipynb](../notebooks/week3_pipeline.ipynb) |
| Prepared references | [image_manifest.csv](../data/processed/image_manifest.csv) |
| Record-level source and export fingerprints | [week3_manifest.json](../data/week3_manifest.json) |
| V1 execution and repeatability evidence | [pipeline_manifest.json](../data/pipeline_v1/images/pipeline_manifest.json) |

Two executions on the same bundle produced the same **285 ordered records** and fingerprints. The Week 4 review uses the identical underlying input manifest.
