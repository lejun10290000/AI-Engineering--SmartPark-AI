# SmartPark AI Week 4 Data Quality, Bias and Responsible Data Report

**Scope.** This review covers the six prepared synthetic shape images from SmartPark AI's Topic 3 Computer Vision pathway. Part B provides the candidate image and labels for each source. Part C provides a second prepared path and independent manifest entries for comparison. The audited Git revision is [a674686](https://github.com/lejun10290000/AI-Engineering--SmartPark-AI/commit/a674686f55874905d7b24f4c9f4eee189a62a816).

## 1. Data Quality Scorecard

Ratings use 1 for a blocking failure and 5 for fully evidenced fitness for the stated scope. N.V. means Not Verified. Uniqueness is rated for the six practice records. Application readiness is evaluated separately in Section 8.

| Dimension | Rating | Evidence | Action or limitation |
|---|---|---|---|
| Completeness | 1/5 | All six practice labels and mappings are present. The dataset contains zero labelled parking-bay frames and zero annotated tracking sequences. | Collect miniature parking footage and task annotations. |
| Consistency | 1/5 | Six Part B fingerprints and six Part C sizes differ from their manifests. The six PNG headers declare palette mode P under an RGB contract. | Restore or re-export from the approved originals, then verify images and regenerated manifests together. |
| Accuracy | N.V. | Five images are readable and their shape/colour labels appear plausible on visual inspection. One failed integrity verification. Parking labels and independent ground truth are absent. | Recheck practice labels against the course source. Independently annotate real miniature parking scenes before judging accuracy. |
| Timeliness | N.V. | The pipeline manifest records a preparation date. The source archive version and capture date are unverified. | Obtain a source release date/version. Record capture sessions and layout versions for actual project footage. |
| Uniqueness | 4/5 | Six distinct Part B file hashes and no exact match among five decodable RGB pixel arrays. The six Part B/C pairs were traced to the same six sources. | Repeat the pixel check after recovering the corrupt image. Review near duplicates and scene overlap before splitting project data. |

## 2. Validation and Data-Defect Findings

The notebook applies nine Pandera rules across six source records. Counts are per rule, so a record can appear several times. The [validation failures CSV](week4_validation_failures.csv) records the evidence, classification, action and follow-up for each flag.

| Rule | Observable condition | Failures | Action |
|---|---|---:|---|
| Files present | Both prepared paths refer to existing files. | 0/6 (0%) | Hard fail if missing. |
| Label vocabulary | Shape is circle or square and colour is red or blue, as documented in Week 3. | 0/6 (0%) | Hard fail if absent or outside the vocabulary. |
| Manifest lineage | Source path, original hash and Part B output path agree across the relevant manifests. | 0/6 (0%) | Hard fail if inconsistent. |
| Image integrity | Pillow can verify and fully decode both PNGs. | 1/6 (16.7%) | Quarantine. Both copies of shape_02 fail at the PNG IDAT checksum. |
| Image contract | Both headers match PNG, RGB and the declared 224 × 224 dimensions. | 6/6 (100%) | Quarantine. Both paths declare P mode for every record. The dimensions match. |
| Part B fingerprint | Observed SHA-256 equals the hash in the lineage manifest. | 6/6 (100%) | Quarantine pending verified bytes. |
| Part C file size | Observed size equals the size recorded in the pipeline manifest. | 6/6 (100%) | Quarantine pending verified export. |
| Source version | Traceable source version or date recorded. | 6/6 (100%) | Review. Source version is unverified for all six. |
| Access rights record | Lineage record explicitly marks rights verified. | 6/6 (100%) | Review and verify permitted use with the course provider. |

The checks produced **31 flags**, including **19 hard failures** and **12 governance review flags**. There were no missing prepared files, empty class labels or missing source mappings in the hand-off. One source pair has measurable corruption. Staleness is uncertain because the source date is unknown. No noise was confirmed by visual inspection of the five decodable images. Padding and differences in foreground size were observed, and the Topic 3 pipeline explicitly documents padding. Visual inspection gives limited evidence about label plausibility and cannot establish a trusted processing history.

| Triaged example | Evidence and interpretation | Decision |
|---|---|---|
| shape_01.png | Red circle with white padding is visually plausible. File appears at both prepared paths with identical bytes, but the Part B SHA-256 differs and observed Part C size is 1,895 bytes against 5,490 recorded. The cause of the discrepancy is unresolved. | Quarantine and compare with an authorised Topic 3 export. |
| shape_02.png | Both prepared copies fail verification with a bad checksum in the IDAT chunk. The image data cannot be verified. | Quarantine as a confirmed defect and regenerate from the authorised original. |
| shape_04.png | Red square appears consistent with the annotation. P mode conflicts with the RGB encoding recorded for the source. The palette could preserve visible colour, but the lineage also fails. | Quarantine while checking when the encoding changed and whether the file can be traced to the source. |
| shape_06.png | Blue square with a substantial white margin. Padding is a valid documented transformation for this practice image. The preparation fingerprint mismatch remains unresolved. | Record the visual exception and quarantine the file pending verified lineage. |

## 3. Duplicate and Anomaly Findings

The six canonical Part B records have distinct file fingerprints. The five decodable images have distinct RGB pixel fingerprints. Each Part C copy is byte-identical to its corresponding Part B image. The [duplicate review CSV](week4_duplicate_review.csv) preserves both paths and each observed SHA-256. Each cross-stage pair represents one source record. There were zero exact duplicate groups across the six sources under the checks run, and zero records removed by deduplication.

Near duplicates were outside this exact-hash check. Pixel-level equivalence involving the corrupt image remains N.V. There was no conflicting label found between exact duplicates of independently sourced images. The cause of the repository-versus-manifest anomalies needs independent confirmation. Replacing the manifest hashes with the repository hashes would record the current bytes without explaining their lineage.

## 4. Representation, Coverage, and Bias Assessment

All six candidates come from the course's synthetic shape subset. The recorded labels include three circles and three squares, and three red and three blue images. The shape/colour counts are red circles 2, blue circles 1, red squares 1 and blue squares 2. These are practice annotations. The [coverage plot](week4_coverage_by_source.png) shows candidate counts and the number admitted by the gate.

| Required area | Expected evidence | Actual evidence and conclusion |
|---|---|---|
| Bay occupancy and safe guidance | Labelled occupied and vacant bays, uncertain cases, bay IDs and boundaries across lighting conditions. | Zero parking scenes or occupancy labels. This is a coverage gap for FR1 and RAI1. |
| Tracking and uncertainty handling | Ordered clips with timestamps/frame IDs, persistent vehicle identities and locations, plus occlusion and close passes. | Zero ordered sequences or vehicle IDs. This is a coverage gap for FR2 and RAI2. |
| Repeatability after changes | Capture dates, layout and camera versions, and coverage of current and superseded setups. | Classroom archive version is unverified. There are no miniature footage sessions to compare. |

| Risk | Propagation path | Mitigation and later test |
|---|---|---|
| Potential representation bias | Synthetic still shapes and one source → corpus has no bay, vehicle, lighting, movement or occlusion examples → a later vision stage has no evidence for those situations and a retrieval index would have no relevant records → incorrect or unavailable guidance for affected parking scenes. | Collect representative miniature scenarios. Test held-out occupancy and track association under uneven lighting and occlusion. If retrieval is required later, check that queries for these scenarios return relevant evidence. |
| Potential measurement bias | Shape/colour labels and fixed-size padded images → geometry and colours are recorded without occupancy, motion or annotation coordinates → later inference or retrieval could reward background or size cues → vehicles with different colours, sizes or lighting may be missed. | Use manually verified occupancy and identity labels. Apply geometric transforms to annotations and retain transform parameters. Measure false-vacant events and association errors for small and partially occluded vehicles. |
| Historical bias N.V. | The version of the practice set is unknown and it has no temporal parking evidence. Spatial or temporal historical bias in project footage cannot yet be assessed. | Compare captured sessions across time and equipment/layout revisions after recording them. Test held-out footage after a material change. |

SmartPark plans a vision and guidance workflow. Any course retrieval or generation demonstration requires separate authorised evidence. If built later, test whether relevant, current evidence is retrieved and whether generated guidance abstains when evidence is missing or uncertain. These data findings do not measure system-level fairness.

## 5. Privacy, Consent, Rights/Access, and Data Governance

| Question | Evidence and status | Required action and proposed owner |
|---|---|---|
| Is source provenance traceable? | The Week 3 inventory identifies the course as provider. The original source paths and hashes are recorded. Archive date and version are N.V., and the prepared bytes cannot be reconciled to the manifests. | Project owner confirms the archive release and candidate lineage with the course provider. |
| Are rights for use and sharing known? | The inventory records course access and no verified separate licence. Publication and reuse rights are to be verified. | Project owner requests the applicable terms from the lecturer and restricts use accordingly. The Week 4 evidence files carry only aggregate results, labels, paths and fingerprints. |
| Are personal or sensitive data present? | The five viewable images show simple shapes and no apparent personal data. The damaged image was unavailable for visual review. The practice source is described as synthetic. | Project owner rechecks any restored file. Frame future footage to exclude identifiable people and real number plates. |
| Is consent or another collection basis needed? | The synthetic shape exercise does not involve known collection from individuals. For future footage, the appropriate basis depends on the planned scene and any identifiable data captured. | Capture lead confirms site permission and applicable consent/disclosure requirements before collecting footage that could identify a person. |
| Who maintains updates, retention and removal? | The source has recorded pipeline versions. Ownership, retention dates and removal procedures for project footage are pending. | Project owner assigns a data steward before collection and documents version change, access, retention and deletion procedures. |

## 6. Mitigation Plan and Residual Risk

| Issue | Evidence | Action | Proposed owner and timing | Residual risk |
|---|---|---|---|---|
| Reproducible hand-off | 6/6 fingerprint, size and mode discrepancies and one corrupt source pair. | Obtain the approved originals, regenerate both outputs, match hashes, verify CRC/decoding and rerun this notebook. | Project owner before clearing the practice gate. | Future exports can diverge again if the build and verification are performed separately. |
| Domain coverage and measurement | 0 parking frames, 0 occupancy annotations and 0 tracking clips. | Record a miniature layout with known bays, capture normal and difficult scenarios, review the annotations independently and split by capture session. | Data collection and annotation leads before model development and held-out testing. | Controlled miniature results may not transfer to public car parks. |
| Access, versions and consent | 6/6 source rights and versions are unverified. | Confirm teaching-use terms, catalogue approved versions, document future collection basis and minimise personal data. | Project owner with the course provider and capture lead before reuse or collection. | Some material may remain restricted or require removal. |
| Near duplicates and leakage | Exact hashes cover identical bytes or decoded pixels. | Compare adjacent frames or visually similar scenes and keep recording sessions together when splitting data. | Evaluation lead before building hold-out data. | Subtle repeated scenes can still inflate results. |

## 7. Impact on Topic 2 Requirements and Evaluation Criteria

- **FR1, NFR2 and RAI1.** There are no labelled bays for the proposed macro-F1 target of **≥ 0.90** or the required **zero false-vacant events** across at least 100 labelled occupied-bay observations. The source also cannot test withholding recommendations for occupied or uncertain bays.
- **FR2, NFR3 and RAI2.** The target of **≥ 90% correct track association** and zero guidance events from incorrect, mismatched or unconfirmed tracks needs at least 20 annotated clips with relevant motion and occlusion. The six still images provide no tracking evidence.
- **FR3–FR5 and RAI3.** Availability and guidance scenarios need labelled layout transitions and ground-truth guidance outcomes. The privacy requirement needs a documented future capture and retention review. None of the project performance targets were measured by this quality lab.

## 8. Topic 4 Quality Gate Decision

**HOLD.** The file-quality gate quarantined all six candidates due to integrity and preparation-contract failures. Source version and rights remain unverified. Project scope additionally requires labelled miniature parking data. Both [cleaned JSONL](../data/prepared_chunks_clean.jsonl) and [cleaned pickle](../data/chunks_clean.pkl) were reloaded and matched at **0 eligible records**.

To lift the practice-data hold, confirm the archive version and permissions, restore traceable prepared files and rerun the notebook. To lift the project-data hold, acquire compliant, representative parking footage, verify occupancy and tracking annotations, and approve the reviewed dataset against the Topic 2 requirements. Keep Topic 7 downstream ingestion on hold until the applicable gate is cleared. Once cleared, select the reviewed outputs and retain the source revision and validation evidence.
