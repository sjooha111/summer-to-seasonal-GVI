# Seasonal GVI Estimation Without Seasonal Imagery: A Zero-Shot MLLM Framework for Urban Greenery Assessment
ACM SIGSPATIAL 2026

---

**GVI (Green View Index)** is the share of vegetation pixels in a street-level image, representing greenery visibility from a pedestrian viewpoint (0 = no vegetation, 1 = fully vegetated).

![Anyang Seasonal GVI](assets/anyang_seasonal_gvi.gif)

*Seasonal GVI map of Anyang, South Korea — spring / summer / fall / winter*

- **Application:** Anyang City, South Korea — a mid-sized city where street-view imagery is captured only once a year, exclusively in summer
- **Input imagery:** 38,640 street-view images collected across 9,660 road points at 30 m intervals in **August 2023**
- **Summer (observed):** GVI directly computed from August 2023 street-view imagery via semantic segmentation
- **Spring / Fall / Winter (predicted):** GVI estimated by the proposed zero-shot MLLM framework (Gemini 2.5 Flash, Structured CoT, GVI+IMG+LOC+VEG) using summer imagery alone as input

Interactive map: [ArcGIS Experience Builder](https://experience.arcgis.com/experience/1075add9d1384f1d861f949a38bc0e9c)

---

## Overview

Estimates spring, fall, and winter GVI from summer street-level images using Gemini 2.5 Flash (zero-shot).

1. Collect Google Street View summer images — `01_gsv_collection.ipynb`
2. Compute summer GVI via Mask2Former segmentation — `02_segmentation_gvi.ipynb`
3. Estimate seasonal GVI using Gemini Batch API — `03_inference_batch.ipynb`

---

## Repository Structure

```
├── notebooks/
│   ├── 01_gsv_collection.ipynb     OSM sampling → metadata → image download
│   ├── 02_segmentation_gvi.ipynb   Mask2Former segmentation + summer GVI
│   └── 03_inference_batch.ipynb    Gemini Batch API inference + point aggregation
├── prompts/
│   ├── system_prompt_direct.txt
│   ├── system_prompt_base_cot.txt
│   ├── system_prompt_structured_cot.txt
│   └── user_prompt_example.md
├── data/sample/
│   └── metadata_sample.csv
└── assets/
    └── anyang_seasonal_gvi.gif
```

---

## Requirements

Python 3.10+

```bash
pip install streetview geopandas shapely requests tqdm \
            transformers torch torchvision Pillow \
            "google-genai>=1.34.0" pandas
```

- Google Maps API key required for `01_gsv_collection.ipynb`
- Gemini API key required for `03_inference_batch.ipynb`

---

## Usage

**Step 1.** `notebooks/01_gsv_collection.ipynb` — set `SHP_PATH` and `GOOGLE_MAPS_API_KEY` in the CONFIG cell.

**Step 2.** `notebooks/02_segmentation_gvi.ipynb` — set `IMAGE_DIR` in the CONFIG cell.

**Step 3.** `notebooks/03_inference_batch.ipynb` — set `GEMINI_API_KEY` and `INPUT_CSV` in the CONFIG cell.

A 9-row sample is included at `data/sample/metadata_sample.csv`.  
GSV images are not included (Google Terms of Service).

---

## Data

`data/sample/metadata_sample.csv` — 9 rows from London, Sydney, and Toronto.

| Column | Description |
|--------|-------------|
| `point_id` | Panorama ID |
| `lat`, `lon` | Coordinates |
| `city`, `country` | Location |
| `GVI` | Summer GVI (0–1) |
| `class_4_tree` … `class_72_palm` | Vegetation class pixel ratios |
| `filepath` | Local image path |

---

## Prompts

| File | Strategy | Selected |
|------|----------|:--------:|
| `system_prompt_direct.txt` | Direct | |
| `system_prompt_base_cot.txt` | Base CoT | |
| `system_prompt_structured_cot.txt` | Structured CoT | ✓ |

Set `PROMPT_STRATEGY` in the `03_inference_batch.ipynb` CONFIG cell.
