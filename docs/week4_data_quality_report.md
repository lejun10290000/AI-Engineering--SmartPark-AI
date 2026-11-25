# SmartPark AI Week 4 Data Quality, Bias and Responsible Data Report

**Quality gate: GO for offline prototype development and replay.** This review uses the project owner's FYP miniature-parking evidence. The detector pilot supplies car-image annotations for development. Indexed camera recordings supply bay and track observations for replay and debugging. Independent accuracy, tracking and safety evaluation is later work.

## 1. Data Quality Scorecard

Ratings use a 1–5 scale, where 5 means the dimension is fully evidenced for the stated development scope. N.V. means Not Verified.

| Dimension | Rating | Evidence | Limit or action |
|---|---:|---|---|
| Completeness | 5 | 85 detector images and 200 replay frames from five sessions. All 24 bay IDs appear in every replay frame. All 20 windows preserve ten consecutive indexed frames. | Keep complete sessions for future tests and add capture conditions. |
| Consistency | 5 | All 285 images passed decoding and SHA-256 checks. Box schema, bay coverage, timestamps, tracks and alignment abstention passed. | Revalidate when the camera view, index or application schema changes. |
| Accuracy | N.V. | The annotation records satisfy the structural contract and the pilot's boxes were visually reviewed. Source-frame inspection confirmed real parking and obstruction scenarios. | Independently label bay states and whole-clip track IDs to measure physical accuracy. |
| Timeliness | 4 | Sessions are dated 19–21 November 2026 and the audited manifest has a stable SHA-256. Replay preserves elapsed time and source order. | Verify the configuration and refresh footage when the board or camera changes. |
| Uniqueness | 4 | Zero exact file or pixel duplicates across 285 records. The historical detector splits share zero source IDs. | Adjacent frames and derived crops can still share evidence. Group by recording session. |

## 2. Validation and Data-Defect Findings

Pandera ran seven checks. The denominators are the number of records to which each rule applies.

| Rule | Your condition | Why this rule? / Failure detected | Failures | Decision |
|---|---|---|---:|---|
| Unique identity and source | Unique record and relative image path, recognised type and recorded source. | Detects duplicate or untraceable index entries. | 0/285 | Quarantine failures. |
| Image integrity | SHA-256 matches and the RGB JPEG verifies and decodes. | Detects missing, altered, corrupt or incompatible images. | 0/285 | Quarantine failures. |
| Detector labels | Class 0. Label count matches, and all boxes have finite coordinates and are inside the image. Approved negatives can have zero boxes. | Detects wrong classes, detached counts and malformed coordinates. The 0.000001 boundary tolerance allows for decimal rounding. | 0/85 | Quarantine failures. |
| Replay bay coverage | All 24 distinct bay IDs have a reason and an occupied, vacant or unknown state per frame. | Detects missing or invented states that would undermine replay. | 0/200 | Quarantine failures. |
| Replay time linkage | Bay and track elapsed times match the indexed frame within 0.1 ms. | The tolerance covers the export precision and exposes wrong frame-to-log joins. | 0/200 | Quarantine failures. |
| Track record integrity | Each track ID occurs at most once in a frame and has finite coordinates. | Detects ambiguous identities or unusable positions in replay. | 0/200 | Quarantine failures. |
| Unaligned frames abstain | Every bay is unknown when alignment is invalid. | Detects known occupancy claims from an unusable view. | 0/200 | Quarantine failures. |

No missing files, unreadable JPEGs, invalid indexed values or exact duplicates were found. No stale records were confirmed for this selection. The failure CSV has a header and zero entries. The validation summary records the rules and counts.

| Reviewed case | Evidence | Classification and action |
|---|---|---|
| R01 frame 224 and R04 frame 177 | Sampled previews show alignment loss, and the logs mark all bays unknown. | Valid uncertainty cases. Retain for replay and abstention checks. |
| R03 frame 81 | The aligned scene shows cars in bays and lanes. The prototype can keep a bay unknown while dwell is pending. | Valid timing-dependent observation. Retain the source frame and time. |
| R05 frame 297 | A hand moves a toy car and partly obscures the board. | Valid obstruction case. Retain for qualitative tracking review. |
| Nine detector negatives | The manifest identifies these as negatives. They have zero boxes and passed validation. | Valid exception. Keep empty areas and board states in the pilot set. |

## 3. Duplicate and Anomaly Findings

Exact byte and decoded-pixel hashes yielded **0 duplicate records**. Detector records carry the original split, source alias and base-frame ID. Replay records preserve the session alias, camera size, original frame index, elapsed time and ten-frame window. Nearby video frames and crops from the same source can look similar. Future dataset splits should group whole recordings.

The **23 unaligned frames**, **1,972 unknown bay observations** and **9 detector negatives** were retained with their context. The telemetry was checked for alignment and linkage. Its physical accuracy still requires an independent reference.

## 4. Representation, Coverage and Bias Assessment

| Needed area | Expected evidence | Observed evidence and limitation |
|---|---|---|
| FR1 and RAI1 occupancy and abstention | All bays, both definite states, unknown states and alignment loss. | 4,800 observations across 24 bays. 1,220 occupied, 1,608 vacant, 1,972 unknown. All 23 unaligned frames abstained. These are prototype outputs. |
| FR2 and RAI2 tracking | Ordered observations from multiple recordings and hard cases. | Five recordings across three dates. Twenty complete ten-frame windows. 2,977 track observations in 170 replay frames. Obstruction, alignment loss and shared-bay reports are represented. |
| Detector development | Actual toy-car images and negative examples. | 85 pilot images, 573 reviewed car boxes and 9 negatives. The 80/2/3 historical split is useful for provenance. The entire pilot is development evidence for this review. |

| Risk | Propagation | Mitigation and later test |
|---|---|---|
| Representation | One board and overhead setup produce related frames and crops. Models or retrieved examples could overrepresent familiar lighting, vehicles and camera angles, reducing reliability on a changed setup. | Collect varied sessions and evaluate by held-out session and capture condition. Check any later evidence index for coverage of session and setup. |
| Measurement | Boxes originated from model proposals. Bay and track logs use alignment, dwell and position proxies. Supervised learning or retrieved summaries could reinforce underlying misses and wrong vacancy claims. | Preserve label origin. Independently check labels during tag loss, occlusion and near bay boundaries. |
| Historical | Earlier and later recordings may describe different settings. Pooling them could make earlier results appear to describe the current prototype. | Retain provenance and compare within known camera or tracker versions. |

System-level fairness, model performance and any later retrieval or generation accuracy remain unevaluated.

## 5. Privacy, Consent, Rights/Access and Data Governance

| Question | Evidence and status | Action and owner |
|---|---|---|
| Source provenance | Project owner directed use of their local FYP recordings. The review sampled indexed archives and preserved session, elapsed time and pilot source aliases. | Project owner retains the original archives and private manifest. |
| Internal use and access | Project owner approved use for this lab and publication of the lab deliverables. The FYP documents specify local footage storage. | Use an approved runtime. GitHub carries the notebook, aggregate evidence and pseudonymous cleaned metadata. The private input bundle holds the images and source mapping. |
| Personal information and consent | The subject is the miniature board and toy vehicle tags. Sampled frames may show hands or hair. The private bundle contains no audio. | Keep frames private. Verify permissions and inspect any footage intended for broader sharing. |
| Version and retention | The review manifest hash, source IDs and capture dates identify the reviewed snapshot. Public redistribution rights were not assessed for the private recordings. | Project owner controls access, refreshes the version when collection changes and reviews retention. |

Reviewed manifest SHA-256: 11b270f4d4fb7c97cb81463505881cab037f497c514ad2a2f0e2a3af95d83612.

## 6. Mitigation Plan and Residual Risk

| Issue | Evidence | Action | Owner and timing | Residual risk |
|---|---|---|---|---|
| Model-related measurement bias | Visually reviewed box proposals and prototype-generated telemetry. | Independently label new test sessions including ID switches and uncertain states. | Project owner before formal evaluation. | Development footage can miss unusual failures. |
| Related frames and limited coverage | Same board, adjacent frames and overlapping crops. | Split by whole capture session and add varied lighting, obstacles and placements. | Project owner before selecting held-out examples. | Miniature results have limited transfer to public car parks. |
| Uncertainty handling | 23 unaligned frames and 1,972 logged unknown observations. | Preserve unknown states and test whether application guidance abstains. | Project owner during integration and test. | Unknown-heavy scenes can reduce guidance availability. |
| Privacy and retention | Operators can enter the camera frame. | Keep footage in approved storage and inspect new captures before sharing. | Project owner during collection. | Future captures may have incidental identifiers. |

## 7. Impact on Topic 2 Requirements and Evaluation Criteria

- **FR1, NFR2 and RAI1.** Detector images and replay support occupancy and safe-guidance development. The **macro-F1 ≥ 0.90** and **zero false-vacant events** targets still require independent labels, including at least 100 occupied-bay observations.
- **FR2, NFR3 and RAI2.** Consecutive windows and vehicle logs support debugging. The **≥ 90% correct track association** and **zero guidance based on an incorrect or unconfirmed identity** targets require the independently labelled clips from the Week 2 test plan.
- **FR3–FR5 and RAI3.** Replay can expose state transitions and bay conflicts. Availability, guidance and latency thresholds require scenario ground truth, measurements and a privacy review.

## 8. Topic 4 Quality Gate Decision

**GO for offline SmartPark prototype development and replay.** All **285** candidates passed their applicable file, provenance and metadata rules. The data covers the 24 bays, has intact consecutive windows, preserves both definite and unknown states, and includes reviewed detector-pilot annotations. The cleaned JSONL and pickle matched at **285 records** on reload. These are the reviewed development inputs for the next stage. Use newly held-out, independently labelled sessions for the formal Week 2 performance and safety tests.

See the [Week 3 lineage manifest](../data/week3_manifest.json), [clean JSONL](../data/prepared_chunks_clean.jsonl), [clean pickle](../data/chunks_clean.pkl), [validation summary](week4_validation_summary.csv) and [coverage plot](week4_coverage_by_source.png).
