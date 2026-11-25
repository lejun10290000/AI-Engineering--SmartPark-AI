# Week 3 Pipeline Decisions and Traceability

The V1 pipeline stages the FYP miniature parking data for offline development and replay. Pipeline configuration is pinned in [the executed run manifest](../data/pipeline_v1/images/pipeline_manifest.json).

## Preparation choices

| Decision | Evidence and trade-off |
|---|---|
| Keep detector and replay record roles explicit | Pilot images carry visually reviewed car proposals. Replay frames carry model-generated bay and track telemetry. The roles help prevent reuse of replay overlays as detector inputs. |
| Preserve native image bytes and geometry | Compared the original detector and replay samples with previews that fit inside 640 × 640 pixels. Previews make inspection lightweight. Full geometry preserves visual detail of small roof markers and the existing annotation linkage. |
| Use batch ETL | Load one immutable input bundle, verify image references, normalise the image index and export once per reviewed snapshot. Offline replay uses source order. Live camera ingestion has a separate runtime. |
| Use reproducible aliases and fingerprints | Paths are relative to the private ZIP, and image SHA-256 values and the input manifest fingerprint survive export. The original recordings and source mapping remain in approved storage. |

## Stage contracts

| Stage | Output contract | Failure response |
|---|---|---|
| Source and ingest | Approved private ZIP, nonempty input manifest and unique image paths and IDs. | Stop and report an invalid, missing or ambiguous input. |
| Inspect | Representative images display or decode and have traceable source roles and geometry. | Review the source and correct the camera or file selection. |
| Prepare | Portable references with typed fields, exact image bytes and retained annotation origins. | Stop if an image cannot be joined to its expected source or detector label count. |
| Validate | JPEG verification and decode pass, RGB mode matches and SHA-256 agrees with the input manifest. | Stop and restore the verified private capture or export. |
| Register lineage | Portable index and two manifests. Rerun retains the same 285 ordered rows, mappings and source fingerprints. | Quarantine the hand-off if repeatability fails. |

## Traceability to Topic 4

- **FR1 and RAI1:** Image geometry and annotation provenance support occupancy development. The synchronised bay logs preserve unknown observations.
- **FR2 and RAI2:** The register retains original processed frame numbers, elapsed time, session aliases and window IDs. Topic 4 checks that the replay windows remain consecutive and the logs link to the correct frame.
- **Formal performance evaluation:** Source annotations and telemetry have different origins. Independent labels and fresh held-out recordings are needed to evaluate the Week 2 targets.

The [Topic 4 notebook](../notebooks/week4_quality.ipynb) uses the input manifest pinned by Week 3 and verifies the reviewed export in both JSONL and pickle formats.
