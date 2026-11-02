# Instance Segmentation (geoskill-instance-segmentation)

> Threshold segmentation + connected-component labeling + instance attribute extraction; outputs instance-annotated GeoJSON (offline numpy-equivalent implementation)

---

## 1. Overview

Segments every individual object in an image (buildings, parcels, tree crowns, ponds, etc.) into separate instances and extracts each instance's area, centroid, bounding box, and mean brightness, outputting instance-annotated GeoJSON and an instance label raster. This skill is an **offline numpy-equivalent implementation** of the Mask R-CNN instance segmentation network: it does not depend on deep learning frameworks and reproduces the instance segmentation pipeline via "threshold/Otsu foreground separation → scipy connected-component labeling (4/8-connectivity) → per-instance attribute extraction → geocoding". Connectivity differences, instance counts, and attribute computations are all validated by unit tests.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-instance-segmentation.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `instances.geojson` | GeoJSON | Instance bbox polygons + area/centroid/brightness attributes |
| `instance_labels.tif` | GeoTIFF | Instance label raster (0=background, 1..K=instances) |
| `output-manifest.json` | JSON | Run manifest (input/output/QA/exit code) |

## 6. Technical Principle

(see SKILL.md for details)

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
