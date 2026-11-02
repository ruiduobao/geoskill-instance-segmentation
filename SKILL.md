---
name: geoskill-instance-segmentation
description: '阈值分割+连通域标记+实例属性提取，输出实例标注GeoJSON（离线numpy等价实现）'
---

# 实例分割 | Instance Segmentation

Segments each individual object in the imagery (buildings, plots, tree canopies, ponds, etc.) into separate instances and extracts each instance's area, centroid, bounding box, and mean brightness, outputting an instance-annotation GeoJSON and an instance-label raster.

This skill is an **offline numpy-equivalent implementation** of the Mask R-CNN instance segmentation network: without relying on any deep learning framework, it reproduces the instance segmentation pipeline with "threshold / Otsu foreground separation → scipy connected-component labeling (4-/8-connectivity) → per-instance attribute extraction → geocoding". Connectivity differences, instance counting, and attribute computation are all verified by unit tests.

## Dependencies / 依赖

```bash
pip install numpy rasterio scipy scikit-learn geopandas shapely
```

## Usage / 使用方法

### Example 1 (Synthetic data, offline)

```bash
python geoskill-instance-segmentation.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### Example 2: Real imagery + manual threshold + 4-connectivity

```bash
python geoskill-instance-segmentation.py --input scene.tif --threshold 80 --connectivity 4 --min-area 9 --output-dir ./out
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `instances.geojson` | GeoJSON | Instance bbox polygons + area/centroid/brightness attributes |
| `instance_labels.tif` | GeoTIFF | Instance label raster (0 = background, 1..K = instances) |
| `output-manifest.json` | JSON | Run manifest (input/output/QA/exit code) |

## Data Source / 数据源 / Source

Local single-/multi-band GeoTIFF (first band used), or a `--synthetic` scene (dark background + separated bright blobs).

## Privacy / 隐私声明 / Privacy

- Runs offline by default; `--synthetic` mode is fully offline with no network access.
- All processing is performed locally; user data is never uploaded.

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
