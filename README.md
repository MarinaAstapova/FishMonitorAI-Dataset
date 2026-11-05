# FishMonitorAI Dataset

Annotated video dataset for **fish detection** and **multi-object tracking (MOT)** of Lena sturgeon (*Acipenser baerii*) in an experimental recirculating aquaculture system (RAS).

This repository hosts the dataset documentation and example annotation visualizations. The annotated frames, the original LabelMe/TrackMe annotations and the prepared machine-readable exports are distributed via Google Drive.

## Downloads

| Resource | Contents | Link |
|----------|----------|------|
| `dataset_folder_001` | 900 annotated JPEG frames and 900 LabelMe/TrackMe JSON files (original annotations) | [Google Drive: dataset folder](https://drive.google.com/drive/folders/1-hQVhhnoUH2YhOK55ztxxeBviKuHe6XP) |
| `FMA_prepared` (zip) | The same 900 frames converted to COCO, MOTChallenge and YOLO formats with a fixed train/val/test split | [Google Drive: FMA_prepared](https://drive.google.com/drive/folders/1xVTUanbgdxUw5-JIJtuWgYcit7XYpHPT) |
| `FMA_prepared/README.md` | Detailed description of every folder and every JSON/CSV file in `FMA_prepared` | [Google Drive: FMA_prepared README](https://drive.google.com/file/d/1j8cxf9YGmmMn4Fq-_Uf4eSc_GewqjRof/view?usp=drivesdk) |

## Dataset at a glance

| Item | Value |
|------|-------|
| Species / setting | Lena sturgeon (*Acipenser baerii*) in an experimental RAS tank |
| Camera | Fixed overhead view of the full tank |
| Annotated frames | 900 (3 sequences × 300 frames) |
| Image size | 1410 × 1040 px, JPEG |
| Effective annotation rate | 10 fps (source video 30 fps, every 3rd frame) |
| Bounding boxes | 16,542 in `FMA_prepared` (16,543 in the original annotations; one exact duplicate removed, see below) |
| Classes | 1 (`fish`) |
| Identities | Closed set of 19 persistent track IDs (matching the 19 fish in the tank) |
| Scenario currently annotated | Calm state |

Track IDs are individual-level. They are maintained through occlusions and when a fish leaves and re-enters the camera view.

## Original annotations (`dataset_folder_001`)

Each frame `*.jpg` has a LabelMe/TrackMe JSON file with the same base name. Every fish is an axis-aligned rectangle labelled `fish` with a persistent `track_id`.

## Prepared exports (`FMA_prepared`)

`FMA_prepared` contains the same frames in three interchangeable layouts:

```
FMA_prepared/
├── README.md               # full description of all folders and files
├── prepare_config.json     # parameters used to build the export (stride, seed, split mode)
├── split_manifest.csv      # one row per sequence: source folder, split, frame range, box and track counts
├── split_summary.csv       # aggregated frame/box/track counts per split
├── fixes.csv               # corrections applied during conversion (one duplicate box removed)
├── coco/                   # COCO JSON: instances_train.json, instances_val.json, instances_test.json
├── mot/                    # MOTChallenge layout: FishMOT-{train,val,test}/<seq>/{img1,gt/gt.txt,seqinfo.ini}, seqmaps/
├── yolo/                   # YOLO layout: images/, labels/, data.yaml
└── format_check/           # sample visualizations to check the conversions
```

### Fixed sequence-level split (seed 0)

| Split | Sequence | Frames | Boxes | Tracks | Original frame indices (step 3) |
|-------|----------|--------|-------|--------|---------------------------------|
| train | S002 | 300 | 5,490 | 19 | 1800–2697 |
| val   | S003 | 300 | 5,637 | 19 | 2475–3372 |
| test  | S001 | 300 | 5,415 | 19 | 0–897 |
| **Total** | | **900** | **16,542** | **19** | |

The three sequences come from different source recordings (`folder_003`, `folder_008` and `folder_001`). The split can also be reproduced from the original frame numbers in the file names of `dataset_folder_001`.

One exact duplicate box (sequence S002, original frame 2229, track 6) was removed during conversion. It is logged in `fixes.csv`.

Formats in brief:
- **COCO**: `bbox` is `[x, y, width, height]` in pixels, with the identity in the `track_id` field (1–19). Category id `1` = `fish`.
- **MOTChallenge**: `gt.txt` rows are `frame, id, x, y, w, h, conf, class, visibility`. Frames are renumbered 1–300 per sequence, and `frame_map.csv` maps them back to the original indices.
- **YOLO**: one `.txt` per image with `class cx cy w h` normalized to [0, 1]. Class `0` = `fish`. YOLO labels carry no track IDs.

**Before use**, edit `path` in `yolo/data.yaml` to point to your local `yolo/` folder. Also note that `instances_test_unknown.json`, `data_test_unknown.yaml` and `FishMOT-test-unknown.txt` are auxiliary duplicates of the test split. The full list of notes and known issues is in `FMA_prepared/README.md`.

## Annotation policy (summary)

- One class, `fish`, annotated with an axis-aligned bounding box.
- The box covers the **visible** part of the body. A box is drawn when more than about 50% of the body is visible.
- IDs are individual-level and stable within each continuous sequence.
- Visibility and occlusion attributes are not stored as separate fields. In MOT `gt.txt` the visibility column is always `1`.
- Annotation was done manually, frame by frame, with multi-stage expert QC, including review by an aquaculture specialist.

## Repository layout

```
.
├── README.md
└── Image Annotation/    # example visualizations of annotations
```

## Citation

If you use this dataset, please cite the accompanying paper (FishMonitorAI). The bibliographic entry will be added when the article is published.

## License and contact

Please respect the terms stated in the paper and in the Google Drive folders. For questions about access or annotation format, open an issue in this repository or contact the dataset maintainers listed in the paper.

## Links

- Annotated data (`dataset_folder_001`): https://drive.google.com/drive/folders/1-hQVhhnoUH2YhOK55ztxxeBviKuHe6XP
- Prepared exports (`FMA_prepared`): https://drive.google.com/drive/folders/1xVTUanbgdxUw5-JIJtuWgYcit7XYpHPT
- `FMA_prepared` README: https://drive.google.com/file/d/1j8cxf9YGmmMn4Fq-_Uf4eSc_GewqjRof/view?usp=drivesdk
- This repository: https://github.com/MarinaAstapova/FishMonitorAI-Dataset
