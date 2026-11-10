# Week 3 Pipeline Decisions and Traceability — SmartPark AI

**Project:** SmartPark AI — Computer Vision-Based Parking Occupancy Detection and Vehicle Tracking  
**Version:** 1.0  
**Date:** 10 November 2026  
**Selected pathway:** C — Computer Vision / Images  
**Execution status:** Part C Cells 7, 8C and 9 completed successfully in Google Colab.

> **Dataset boundary:** The executed source is the lecturer-provided synthetic practice subset containing six simple shape images. It is not real parking-scene data and does not demonstrate SmartPark AI model performance.

## 1. Requirement traceability and selected source

| Topic 2 requirement | Data needed by the requirement | Selected source and relevance | Limitation |
|---|---|---|---|
| **FR1 — Parking-bay occupancy detection** | Camera images showing vehicles and defined parking bays, with occupied/vacant labels and bay identity | S02: `topic3_dummy_datasets/03_images/` is used as a temporary image-ingestion and preprocessing practice source | Contains circles/squares and colour labels, not vehicles, bays or occupancy labels. It cannot train or validate FR1. |
| **FR2 — Vehicle tracking** | Ordered frames, vehicle/marker identity, timestamps or frame numbers, and object locations | No tracking source is available in the selected dummy image subset | Six independent still images contain no temporal order or persistent vehicle identity. |
| **RAI1 — Safe parking guidance** | Reliable occupied/vacant/unknown states and confidence/uncertainty evidence | FR1-related parking imagery and annotations will be required for the real prototype | Dummy labels do not represent parking states or uncertainty. Do not use these images to validate safe guidance. |

The primary requirement justifying the selected image modality is **FR1**. RAI1 is recorded as a downstream safety concern, not as a requirement demonstrated by the dummy dataset. FR2 remains an identified future data need and is not implemented by this image-only pathway.

## 2. Selected source and preparation decision

| Decision item | Selected configuration |
|---|---|
| Source path in Colab | `/content/week3_project/data/raw/topic3_dummy_datasets/03_images/` |
| Repository source path | `data/raw/topic3_dummy_datasets/03_images/` |
| Source content | Six PNG still images |
| Processing library | Pillow (PIL) |
| Colour handling | Convert to RGB |
| Spatial transformation | Preserve aspect ratio, then pad to the target canvas |
| Target canvas | 224 × 224 pixels |
| Padding | White, RGB [255, 255, 255] |
| Part C output in Colab | `/content/week3_project/data/pipeline_v1/images/` |
| Intended repository output | `data/pipeline_v1/images/` |
| Pipeline manifest | `pipeline_manifest.json` in the selected output folder |

### Part B comparison reused in Part C

Part B compared direct resize (Approach A) with aspect-ratio-preserving resize and padding (Approach B), using the same input image. Approach B was selected because it produces a consistent 224 × 224 canvas without stretching the original shape. The accepted limitation is that padding introduces white pixels and changes the proportion of the canvas occupied by the object. For real object detection or segmentation, annotations such as bounding boxes and masks must receive the corresponding geometric transformation.

Part C reuses this selected decision rather than introducing a different transformation.

## 3. V1 pipeline stages and stage contracts

| Stage | Implemented operation | Input contract | Output contract / evidence | Failure response |
|---|---|---|---|---|
| **Source** | Explicitly select the approved image folder | Exact folder path under `data/raw/` | Selected source folder is identified | Do not guess another folder. |
| **Ingest** | Resolve folder and list supported PNG/JPG/JPEG/WEBP files | Source folder exists and contains supported files | Six input image paths identified | Missing folder raises `FileNotFoundError`; empty input raises a validation error. Stop and correct the source. |
| **Inspect** | Open and verify each image; read dimensions and mode | Every selected image is readable | Inspection summary includes image count and representative properties | An unreadable/corrupt image raises an error or is flagged; do not silently treat it as valid. |
| **Prepare / transform** | Convert to RGB; preserve aspect ratio; pad to 224 × 224 with white | Readable source image and the Part B configuration | Six prepared PNG images in the pipeline output folder | Stop/report transformation failures; retain the raw source unchanged. |
| **Validate output** | Check that prepared outputs exist and are non-empty; calculate output fingerprints | Prepared files produced by the selected pathway | Six outputs; output checksums and byte sizes recorded | Stop if no output is produced, a file is missing, or an output is empty. |
| **Record lineage** | Store source and output paths, SHA-256 hashes, configuration, processing description and source-to-output mapping | Source and prepared outputs exist | `pipeline_manifest.json` saved in the images output folder | Do not claim traceability if a source, output, checksum or mapping is missing. |

### Two plausible failure points

1. **Missing or incorrectly specified source folder:** the explicit source resolver raises `FileNotFoundError`. The pipeline stops; correct the path and rerun rather than selecting a different folder automatically.
2. **Unreadable or corrupt image:** image verification/opening fails. Flag the affected file and stop/report the failure. Do not silently export it or count it as a valid prepared image.

## 4. ETL / ELT decision

**Decision: ETL (Extract → Transform → Load/store).**

The pipeline identifies the six source images, inspects them, applies the selected RGB and aspect-ratio-preserving preparation, and stores the resulting PNG files in the prepared output directory. The transformation is performed before the prepared artefacts are used by a later model or application component. This is therefore an ETL-style preparation workflow. The original source files remain unchanged in the raw directory.

## 5. Batch / streaming decision

**Decision: Batch processing.**

The selected source is a finite folder of six independent still images. Processing all six images as one repeatable batch is appropriate and produces a stable source-to-output mapping. Streaming is not justified because the selected practice data contains no live camera feed, ordered video frames or real-time processing requirement. The decision does not prevent a later SmartPark implementation from using a separate streaming pipeline for live overhead-camera footage.

## 6. Execution evidence — Part C Step C1

| Cell | Purpose | Observed result |
|---|---|---|
| **Cell 7** | Define shared V1 paths, source resolver, SHA-256 and pipeline runner | `Cell 7 ready.` |
| **Cell 8C** | Execute selected computer-vision image pathway | `SUCCESS: images | Inputs: 6 | Outputs: 6`; manifest saved |
| **Cell 9** | Rerun registered pipeline and compare source/output fingerprints and mappings | `PASS: images - source fingerprints, prepared outputs and mappings match`; all selected pipelines passed repeatability smoke checks |

**Observed Colab output directory:**

```text
/content/week3_project/data/pipeline_v1/images/
```

**Observed pipeline manifest:**

```text
/content/week3_project/data/pipeline_v1/images/pipeline_manifest.json
```

**Intended repository locations after Step C4:**

```text
data/pipeline_v1/images/
├── prepared_001.png
├── prepared_002.png
├── prepared_003.png
├── prepared_004.png
├── prepared_005.png
├── prepared_006.png
└── pipeline_manifest.json
```

The execution screenshots evidence the console messages and successful run. The actual notebook, prepared image files and manifest must also be copied from Colab before the repository can be considered to contain all runtime artefacts.

## 7. Provenance, access and quality caveats

- The data is synthetic and supplied for the Topic 3 teaching exercise. The source archive does not state a verified version or individual creator in the inventory.
- The course materials' redistribution conditions must be checked before committing the original ZIP, raw images or labels to a public repository. Do not assume that course access implies permission for public redistribution.
- The pipeline manifest records SHA-256 checksums and mappings. A checksum can detect a change to file bytes; it does not establish source authorisation, licence, authenticity, representativeness or quality.
- The manifest fields `source_version` and `access_rights` are recorded as unverified until confirmed from the course materials.
- The pipeline performs image preparation only. It does not train a model, infer parking occupancy, detect vehicles, recognise number plates, track vehicles or evaluate safe parking guidance.
- Six synthetic shape images are not representative of real camera footage. Actual miniature-parking images/video and task-specific annotations are required for project evaluation.

## 8. Traceability summary

| Requirement / decision | Evidence in this lab | Next evidence needed |
|---|---|---|
| FR1 — image input for occupancy capability | Pathway C selected; six images ingested and prepared | Labelled miniature-parking images with bay IDs and occupancy states |
| FR2 — tracking across frames | Requirement and data gap documented | Ordered video frames, timestamps/frame IDs, vehicle/marker IDs and locations |
| RAI1 — safe guidance | Limitation of dummy labels documented | Reliable occupancy labels, uncertainty handling and tests preventing occupied/unknown recommendations |
| Part B preparation decision | Aspect-ratio-preserving resize selected and reused | Preserve annotation geometry when real detection labels are introduced |
| Part C reproducibility | Cell 9 fingerprint and mapping comparison passed | Re-run in a clean runtime/environment for stronger reproducibility evidence |

## 9. Step C2 / C3 completion checklist

- [x] Draw the selected image workflow from source through ingest, inspect, prepare/transform, validate and record lineage.
- [x] Annotate two plausible failure points and their responses.
- [x] Record the selected source, transformation and Part B comparison.
- [x] State ETL and batch-processing decisions with rationale.
- [x] Record stage contracts, output paths and provenance/access caveats.
- [ ] Export and commit the original Colab notebook and permitted runtime artefacts as part of Step C4.
- [ ] Verify the uploaded files and links in the GitHub repository.
