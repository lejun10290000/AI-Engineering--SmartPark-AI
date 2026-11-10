# Week 3 Data Inventory — SmartPark AI

**Project:** SmartPark AI — AI-Based Computer Vision for Smart Parking  
**Version:** 1.2  
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

1. The practice subset contains six images and six corresponding label rows. The `filename` column is the key used to associate each image with its label.
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

## 7. Step A3 — Data Type and Initial Inspection Evidence

### 7.1 Selected source and data type

| Field | Inspection record |
|---|---|
| Selected source path | `data/raw/topic3_dummy_datasets/03_images/` |
| Selected data pathway | C — Computer Vision |
| Main data type | Unstructured image data |
| Supporting metadata | Structured CSV label data (`labels.csv`) |
| Image file type | PNG |
| Image representation | Pixel data; RGB image mode |
| Metadata fields | `filename`, `shape_label`, `colour` |

The project requires image data because SmartPark AI uses camera imagery as the input to its parking-scene vision capability. The CSV file is supporting structured metadata that associates each image filename with its shape and colour labels. The processing route is therefore image inspection with CSV metadata validation, rather than treating the selected source as a document, audio, or time-series dataset.

### 7.2 Inspection evidence

The supplied archive was inspected. Its `03_images/labels.csv` contains six records and the six corresponding PNG files were opened to inspect their dimensions, format, and mode.

| File | CSV shape label | CSV colour | Format | Mode | Dimensions (width × height) |
|---|---|---|---|---|---|
| `shape_01.png` | circle | red | PNG | RGB | 92 × 97 |
| `shape_02.png` | square | blue | PNG | RGB | 104 × 104 |
| `shape_03.png` | circle | blue | PNG | RGB | 116 × 111 |
| `shape_04.png` | square | red | PNG | RGB | 128 × 118 |
| `shape_05.png` | circle | red | PNG | RGB | 140 × 125 |
| `shape_06.png` | square | blue | PNG | RGB | 152 × 132 |

**Inspection summary:**

- Number of PNG images: 6.
- Number of CSV label records: 6.
- CSV columns: `filename`, `shape_label`, `colour`.
- Shape categories: `circle` and `square`.
- Colour categories: `red` and `blue`.
- All six CSV filenames have matching image files in the supplied archive.
- Two representative samples to display in Colab: `shape_01.png` and `shape_02.png`.

These findings describe inspection of the supplied archive. Run the notebook inspection cell in the Colab workspace as well and retain its output/screenshots as execution evidence.

### 7.3 Initial findings and limitations

- The images are unstructured visual inputs, while `labels.csv` is structured metadata.
- The filenames provide the link between image files and CSV records; this mapping was checked against the supplied archive.
- Image dimensions vary, so a consistent resizing step may be required in Part B. Preserve the originals and document any transformation.
- The label vocabulary (`circle` / `square` and colour values) is not a SmartPark target label set.
- The still images do not provide frame order, timestamps, or persistent vehicle identifiers, so they cannot support vehicle tracking.
- The subset does not contain bay boundaries or `occupied` / `vacant` / `unknown` labels, so it cannot validate occupancy detection or safe parking recommendations.
- File existence alone is not proof of successful ingestion. The CSV reading, image opening, representative image display, and filename consistency check provide the required initial inspection evidence.

### 7.4 Step A3 conclusion

Pathway C is the most relevant practice pathway for SmartPark AI because the proposed system processes camera images. The inspected archive confirms that the six PNG images can be opened and matched with the CSV metadata. The dummy subset supports practice with image and metadata inspection only. Actual parking-scene data from the miniature prototype will be required to train, test, and evaluate vehicle detection, occupancy detection, and tracking.

## 8. Step A4 — Data Inventory Checkpoint

The inventory below records the source identity, intended purpose, provenance, access conditions, and inspection evidence. The two entries distinguish the complete teaching pack from the specific image subset selected for this project.

| Source ID | Filename / URL | Type and purpose | Version / date / owner | Access / rights | Inspection evidence |
|---|---|---|---|---|---|
| **S01** | `Topic_3_Dummy_Datasets.zip` (Topic 3 course-provided archive) | Synthetic teaching dataset pack; contains six optional pathways. Used as the source archive for the selected Computer Vision practice data. | Version/date not stated in the archive. Supplied for Topic 3. Owner/provider: course lecturer or course materials; individual name not specified. | Obtained through the course-provided materials. The archive README identifies the contents as synthetically generated for teaching. No separate licence file or explicit redistribution licence was found in the archive; follow course-portal terms and do not assume public redistribution rights. | Archive contents and `topic3_dummy_datasets/README.md` inspected. README confirms that examples are synthetic and must not be used to claim domain accuracy or model performance. |
| **S02** | `topic3_dummy_datasets/03_images/shape_01.png`–`shape_06.png`; `topic3_dummy_datasets/03_images/labels.csv` | Unstructured PNG images with structured CSV labels. Selected Pathway C practice subset for image loading, visual inspection, label matching, and basic preprocessing. | Inherits S01 provenance. Individual file version/date and creator are not stated in the archive. | Same course-provided teaching-use conditions as S01. No separate image-subset licence was found. Do not publish or redistribute the files separately unless permitted by the course terms. | Six PNG images opened; all are RGB. Dimensions range from 92 × 97 to 152 × 132 pixels. CSV contains six matching filename records with `filename`, `shape_label`, and `colour` columns. Two representative samples: `shape_01.png` (red circle) and `shape_02.png` (blue square). |

### 8.1 Requirement justification

- **FR1 — Parking-bay occupancy detection:** S02 is selected as a temporary image-processing practice source because FR1 requires visual input. Its shape labels are not occupancy labels, so it cannot train or validate parking-bay occupancy detection.
- **FR2 — Vehicle tracking:** S02 does not contain sequential frames, timestamps, or vehicle IDs. A separate overhead-camera video/image-sequence dataset will be required for this requirement.
- **RAI1 — Safe parking guidance:** S02 does not contain occupied/vacant/unknown bay states or confidence information. Actual labelled parking-scene data and uncertainty handling will be required to assess this requirement.

### 8.2 Raw and prepared data storage

| Storage location | Contents / status |
|---|---|
| `data/raw/Topic_3_Dummy_Datasets.zip` | Original supplied ZIP archive, if retained after extraction. |
| `data/raw/topic3_dummy_datasets/03_images/` | Extracted original PNG images and `labels.csv`; retain unchanged as raw input. |
| `data/processed/` | Reserved for outputs created by Part B, such as consistently resized images and any prepared manifest. No prepared output is claimed at this checkpoint unless it has actually been generated. |

**Storage rule:** Keep source files unchanged in the raw area. Write transformed files to the processed area and record the transformation, output filename, and source relationship. Do not count the ZIP archive and its extracted copies as two independent datasets.

### 8.3 Step A4 checkpoint conclusion

S01 identifies the provenance and access limitations of the lecturer-provided practice archive. S02 identifies the exact image subset selected for SmartPark AI and records its data type, purpose, and inspection evidence. The selection is justified by FR1's need for image input, but the synthetic shapes are only a temporary practice dataset. Actual miniature-parking camera data will be needed for project-level evaluation. Raw source files and future prepared outputs are kept distinct so that every transformation remains traceable.

## 9. Part B — Step B1: Selected Data Preparation Pathway

### 9.1 Pathway decision

| Field | Decision |
|---|---|
| Selected pathway | **C — Computer Vision (images)** |
| Source folder | `data/raw/topic3_dummy_datasets/03_images/` |
| Input files | Six PNG images and `labels.csv` |
| Loading / image library | Pillow (PIL); pandas for the CSV label metadata |
| Required operations | Read and verify images; inspect dimensions and modes; convert to RGB; resize to a consistent illustrative size; retain source identity; export prepared images and a manifest |
| Prepared artefacts | `data/processed/images/` and `data/processed/image_manifest.csv` |
| Configuration used in this lab | 224 × 224 pixels; RGB; aspect-ratio-preserving resize with white padding |

### 9.2 Justification

Computer Vision is selected because SmartPark AI's primary input is camera imagery. The project's parking-bay occupancy detection requirement (FR1) needs visual information from an overhead camera. The image pathway is therefore the closest match to the intended data modality.

The other pathways (PDF/TXT, CSV/Excel as a standalone data pathway, JSON/API, audio, and time-series) are not selected for this preparation exercise because they are not the primary input modality of the chosen practice source. The CSV file in `03_images/` is used only as supporting label metadata for the images; it is not a reason to run the separate tabular pathway.

### 9.3 Scope and limitations

The six supplied images are synthetic geometric shapes, not vehicles or parking scenes. Their labels describe shape and colour, not vehicle identity or occupied/vacant parking-bay states. This pathway practises image ingestion and preparation only; it does not train or evaluate SmartPark AI. Actual overhead-camera images/video and suitable annotations must replace the dummy images for project-level development and evaluation.

## 10. Part B — Step B3: Prepared Output Validation and Decision Comparison

### 10.1 Basic output validation

The Colab B3 validation cell was run after the selected Pathway C preparation. The observed output was:

| Validation item | Observed result | Status |
|---|---:|---|
| Prepared manifest | `data/processed/image_manifest.csv` | Pass |
| Manifest file size | 1,850 bytes | Pass — non-empty |
| Prepared image records | 6 | Pass |
| Images present | 6 of 6 | Pass |
| Images readable | 6 of 6 | Pass |
| Output dimensions | 224 × 224 pixels for all six images | Pass |
| Output colour mode | RGB for all six images | Pass |
| Basic validation failures | 0 | Pass |

The validation output showed that each prepared file existed, was readable, and had the expected dimensions and colour mode. This is a basic output contract check; it is not a comprehensive assessment of image quality or model performance.

### 10.2 Preparation configuration comparison

The same representative source image, `shape_01.png` (original dimensions 92 × 97 pixels), was used to compare two resizing configurations.

| Comparison | Approach A — Direct resize | Approach B — Aspect-ratio-preserving resize |
|---|---|---|
| Operation | Resize directly to 224 × 224 | Fit image within 224 × 224 and pad remaining space with white |
| Output dimensions | 224 × 224 | 224 × 224 |
| Aspect ratio | May change the original proportions | Preserves the original proportions |
| Visual observation | The image is stretched to fill the square output | The circular shape retains its proportions, with padding where required |
| Decision | Not selected | **Selected** |

The actual exported prepared image was visually inspected and matched Approach B.

### 10.3 Selected approach, benefit, and accepted limitation

**Selected approach:** Approach B — aspect-ratio-preserving resize with white padding.

**Benefit:** The operation produces a consistent 224 × 224 input size while preserving the original image proportions. This avoids unnecessary geometric distortion of the synthetic shapes and is a sensible practice configuration for this dataset.

**Accepted limitation:** Padding adds white background pixels and can reduce the proportion of the output image occupied by the object. The 224 × 224 size is an illustrative lab configuration, not a universal requirement for SmartPark AI. For real object detection or segmentation, any resizing or padding must also be reflected consistently in bounding-box or mask annotations.

### 10.4 B3 conclusion

All six prepared images passed the observed basic validation checks: they existed, were readable, used RGB mode, and measured 224 × 224 pixels. The manifest was present and non-empty at 1,850 bytes. Approach B was selected because it preserves image proportions while standardising the input dimensions. These results demonstrate successful preparation of the synthetic practice images only; they do not demonstrate vehicle detection, parking occupancy detection, or tracking performance.

## 11. Part C — Step C1: Integrated V1 Image Pipeline Execution

### 11.1 Executed pathway and configuration

The Part C Colab notebook executed the common pipeline utilities (Cell 7), the selected image pathway (Cell 8C), and repeatability/provenance checks (Cell 9). Only Pathway C was run because the selected source is an image folder.

| Field | Executed record |
|---|---|
| Selected pathway | C — Computer Vision / Images |
| Source folder in Colab | `/content/week3_project/data/raw/topic3_dummy_datasets/03_images` |
| Source files | 6 PNG shape images |
| Cell 7 | Completed — common V1 pipeline utilities ready |
| Cell 8C | Completed — 6 inputs and 6 prepared outputs |
| Cell 9 | Passed — source fingerprints, prepared-output fingerprints and source-to-output mappings matched on rerun |
| Colour conversion | RGB |
| Geometric transformation | Aspect-ratio-preserving fit/pad |
| Target dimensions | 224 × 224 pixels |
| Padding colour | White ([255, 255, 255]) |
| Colab output folder | `/content/week3_project/data/pipeline_v1/images/` |
| Colab lineage file | `/content/week3_project/data/pipeline_v1/images/pipeline_manifest.json` |

### 11.2 Prepared output and verification evidence

The observed Cell 8C console output reported:

- `SUCCESS: images | Inputs: 6 | Outputs: 6`
- Manifest written to `/content/week3_project/data/pipeline_v1/images/pipeline_manifest.json`.
- `Cell 8C completed successfully.`

The observed Cell 9 output reported:

- `PASS: images - source fingerprints, prepared outputs and mappings match`.
- `All selected pipelines passed repeatability smoke checks.`

These are execution and repeatability checks for the six synthetic images. They do not establish image-label correctness, SmartPark occupancy-detection accuracy, vehicle tracking performance, or full cross-environment reproducibility.

### 11.3 Repository storage mapping for Step C4

When the Colab artefacts are copied into the repository, retain this structure:

```text
notebooks/
└── week3_pipeline.ipynb

data/
├── week3_manifest.json
├── processed/
│   ├── images/
│   └── image_manifest.csv
└── pipeline_v1/
    └── images/
        ├── prepared_001.png
        ├── prepared_002.png
        ├── prepared_003.png
        ├── prepared_004.png
        ├── prepared_005.png
        ├── prepared_006.png
        └── pipeline_manifest.json

docs/
├── week3_data_inventory.md
├── week3_pipeline_diagram.png
└── week3_pipeline_decisions.md
```

The Colab paths under `/content/week3_project/` are temporary runtime paths. The repository paths above are the intended committed locations. Copy only files permitted for submission; do not publish the course ZIP or source images if course access terms do not allow redistribution.

### 11.4 Provenance and scope caveats

- The source remains the lecturer-supplied synthetic shape-image subset (S01/S02), not project-collected parking footage.
- The Part C pipeline manifest records source and prepared-output SHA-256 fingerprints and source-to-output mappings. Hashes support integrity checks; they do not prove permission, ownership, authenticity, or data quality.
- The manifest's source version and access-rights fields remain unverified unless confirmed from the course materials.
- The six images are independent still images. This pipeline is batch image preparation, not video streaming or vehicle tracking.
- Preserve the original raw images unchanged. Store Part B and Part C prepared artefacts separately.


## 12. Step C4 — Repository Artefact Transfer Record

The supplied Colab notebook has been committed to `notebooks/week3_pipeline.ipynb`; the existing pipeline diagram is present at `docs/week3_pipeline_diagram.png`. The C4 export bundle contains the remaining manifests and prepared images, but those files have not yet been added individually to the repository. The notebook is retained as supplied; execution results are documented in Sections 10–11 and in `docs/week3_pipeline_decisions.md`.

| Artefact | Repository path | Transfer record |
|---|---|---|
| Runnable notebook | `notebooks/week3_pipeline.ipynb` | Committed; 17 cells retained |
| Part B lineage manifest | `data/week3_manifest.json` | Committed |
| Part B image manifest | `data/processed/image_manifest.csv` | Committed; 1,850 bytes |
| Part B prepared images | `data/processed/images/` | Six PNGs committed |
| Part C prepared images | `data/pipeline_v1/images/` | Six PNGs committed |
| Part C pipeline manifest | `data/pipeline_v1/images/pipeline_manifest.json` | Committed |
| Pipeline diagram | `docs/week3_pipeline_diagram.png` | Existing repository diagram retained |
| Pipeline decisions | `docs/week3_pipeline_decisions.md` | Updated with C2/C3 decisions and execution evidence |

**Transfer boundary:** The original course ZIP and raw source images were not added to the public repository because the course redistribution rights were not verified. The prepared practice outputs and manifests are included as the exported artefacts. Confirm that the course permits public submission of these prepared outputs before using the public repository as the final submission location.

**Repository validation status:** The notebook and pipeline diagram are present in GitHub. The C4 ZIP contains the Part B/Part C manifests and prepared image outputs, but they remain to be committed as individual files. The supplied Colab evidence reports Cell 7 ready, Cell 8C successful with 6 inputs and 6 outputs, and Cell 9 passing the repeatability smoke check. This confirms the recorded run and matching fingerprints/mappings; it is not a full cross-environment reproducibility test or a data-quality/model-performance evaluation.
