# FishMonitorAI Dataset
Annotated video dataset for **fish detection** and **multi-object tracking (MOT)** of Lena sturgeon (*Acipenser baerii*) in an experimental recirculating aquaculture system (RAS).
This repository hosts documentation and example annotation visualizations. The full set of annotated frames and LabelMe/TrackMe JSON files is published on Google Drive (see below).
## Annotated images and labels
**`dataset_folder_001`** — annotated images (JPEG) and LabelMe/TrackMe JSON annotations:
➡️ [Google Drive — dataset_folder_001](https://drive.google.com/drive/folders/1-hQVhhnoUH2YhOK55ztxxeBviKuHe6XP)
Contents of that folder:
- **900** frames at **1410 × 1040** px (calm-state subset)
- **900** LabelMe/TrackMe JSON files with bounding boxes and persistent `track_id`
- Closed set of **19** individual IDs (matching the 19 fish in the tank)
- Track IDs are maintained after occlusions and when a fish exits and re-enters the camera view
## Dataset at a glance
| Item | Value |
|------|--------|
| Species / setting | Lena sturgeon (*Acipenser baerii*) in an experimental RAS tank |
| Camera | Fixed overhead view of the full tank |
| Annotated frames | 900 (3 sequences × 300 frames) |
| Bounding boxes | 16,543 |
| Track IDs per sequence | 19 (closed population) |
| Image size | 1410 × 1040 |
| Effective annotation rate | 10 fps (source video 30 fps, step 3) |
| Annotation format (public) | LabelMe / TrackMe JSON (`track_id` field) |
| Scenario currently annotated | Calm state |
Machine-readable exports (COCO JSON, MOTChallenge, YOLO) and official train/val/test splits may be added in a later release.
## Annotation policy (summary)
- One class: `fish` (axis-aligned bounding box)
- Box covers the **visible** part of the body; a box is drawn if more than ~50% of the body is visible
- IDs are **individual-level** and stable within each continuous sequence
- Visibility / occlusion attributes are not stored as separate fields in the current release
- Manual frame-by-frame annotation with multi-stage expert QC (including aquaculture specialist review)
## Repository layout
