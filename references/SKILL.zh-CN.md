# 灾后重建遥感监测 | Post-Disaster Reconstruction Monitoring

用多期高分辨率影像的建筑强度代理监测重建进度：对比灾前/损毁期/重建期三期，逐像元分为 non_building / unchanged / destroyed / under_construction / rebuilt 五类（互斥）。毁坏像元恢复进度 = clip((R-D)/(B-D),0,1)，随重建期强度单调增，重建程度越高 rebuilt 像元越多。

## 依赖

```bash
pip install numpy rasterio scipy
```

## 使用方法

### 基本用法（合成数据，离线）

```bash
python geoskill-post-disaster-reconstruction.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### 更多示例

```bash
python geoskill-post-disaster-reconstruction.py --bbox 116 39 117 40 --synthetic --output-dir ./out
python geoskill-post-disaster-reconstruction.py --input series.tif --output-dir ./out
python geoskill-post-disaster-reconstruction.py --bbox 116 39 117 40 --recovery 0.9 --synthetic --output-dir ./out
python geoskill-post-disaster-reconstruction.py --bbox 117 39 118 40 --synthetic --quiet --output-dir ./out
```

## 输出

| 文件 | 格式 | 说明 |
|---|---|---|
| `progress_class.tif` | GeoTIFF | 重建进度分类（整型 0-4） |
| `recovery_progress.tif` | GeoTIFF | 毁坏像元恢复进度 [0,1] |
| `reconstruction_params.json` | JSON | 阈值与类别标签 |

每次运行还会产出 `output-manifest.json`（运行清单，含输入/产物/QA 摘要）。

## 数据源 / Source

真实模式读取多波段 GeoTIFF（band1=灾前、band2=损毁期、band3=重建期建筑强度）；合成模式离线生成三期场景（恢复程度可调）。

## 隐私声明 / Privacy

- 默认离线运行，`--synthetic` 模式完全无网络。
- 所有处理在本地完成，不上传任何用户数据。

## License

MIT
