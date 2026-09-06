# OccluAttnBench_2026

A comparative evaluation of attention-based person-detection methods under real-world occlusion and real-time conditions.

## Research Context

E-commerce platforms can record structured information about user interactions and purchasing activities. In physical retail environments, however, customer activities are commonly recorded only as CCTV footage, which is more difficult to convert into structured data.

Computer vision can help extract information such as people's locations, appearance times, quantities, and spatial distributions from video. When combined with later tracking and analysis stages, this information could support customer-traffic assessment, identification of frequently visited areas, occupancy monitoring, and analysis of activity over time.

Accurate person detection is the foundation of this pipeline. A person must first be located reliably in each frame before observations can be associated over time or used in subsequent tracking, re-identification, and activity-analysis tasks.

## Research Problem and Scope

Person detection in retail and warehouse environments is affected by:

- high crowd density;
- partial occlusion by other people or surrounding objects;
- changes in body posture and movement;
- variations in object scale;
- different camera viewpoints;
- changing illumination and background conditions.

When a person is partially hidden, the detector receives incomplete visual information. This can lead to missed detections, duplicate predictions, or inaccurate bounding-box localisation. These errors may subsequently reduce the reliability of information derived from CCTV footage.

This study is limited to person detection at the individual-frame level. Tracking, re-identification, multi-camera association, identity recognition, and behaviour analysis are outside the current research scope.

## Research Aim

The aim of this research is to develop and evaluate YOLO26s variants for improving person detection in occlusion-prone CCTV environments while maintaining a computational cost suitable for practical deployment.

The study:

- establishes YOLO26s as the baseline;
- develops three literature-based YOLO26s variants;
- trains all four models under the same experimental conditions;
- compares detection accuracy, bounding-box localisation, model complexity, and processing speed;
- qualitatively examines challenging detection cases.

The resulting detections are treated as foundational observations that could support the collection of spatial, temporal, quantitative, and distributional information in future research.

## Proposed Methods

Three architectures are compared with the original YOLO26s baseline:

### YOLO26s CA

Coordinate Attention is added near the end of the backbone to preserve positional information along the horizontal and vertical directions. This may help the model locate informative visible features when parts of a person are occluded.

### YOLO26s RCAC3k2

The final C3k2 backbone block is replaced with RCAC3k2, which combines residual feature preservation with Coordinate Attention. This configuration examines whether coordinate-aware attention can be integrated more efficiently within the feature-extraction process.

### YOLO26s CBAM-AKConv

CBAM applies channel and spatial attention to select informative features. AKConv then uses learned sampling positions to adapt feature extraction to incomplete, irregular, or deformed visible regions.

## Dataset and Preparation

The dataset was collected from security cameras operated by one company in retail and warehouse environments.

It contains approximately 12,000 images showing realistic variations in:

- viewpoint and distance;
- scale and posture;
- movement and illumination;
- occlusion caused by people, shelves, counters, boxes, and equipment.

Dataset preparation consisted of:

```text
Raw video collection
        ↓
Uniform frame sampling
        ↓
SSIM-based temporal-duplication filtering
        ↓
Image review and annotation
        ↓
YOLO dataset organisation
```

The retained frames were annotated as a single `person` class. X-AnyLabeling was used during the annotation workflow, and the labels were converted into the Ultralytics YOLO horizontal bounding-box format.

The common evaluation set used for all four models contains:

- 1,226 images;
- 2,665 person instances.

## Evaluation

All models used the same dataset partition, input size, training settings, and evaluation set.

Performance was compared using:

- precision;
- recall;
- F1-score;
- mAP50;
- mAP50-95;
- parameter count;
- GFLOPs;
- reported processing time;
- qualitative examination of challenging cases.

## Research Results

| Model | Precision | Recall | F1 | mAP50 | mAP50-95 | Parameters | GFLOPs | Reported time |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| YOLO26s Base | 0.940 | 0.942 | 0.9410 | 0.967 | 0.764 | 9.466 M | 20.8 | 4.2 ms |
| YOLO26s CA | **0.968** | 0.937 | 0.9522 | 0.973 | **0.815** | 14.052 M | 43.3 | 3.1 ms |
| YOLO26s RCAC3k2 | 0.967 | 0.945 | **0.9559** | **0.976** | 0.814 | **9.211 M** | **20.6** | 3.0 ms |
| YOLO26s CBAM-AKConv | 0.966 | **0.946** | **0.9559** | 0.975 | 0.810 | 11.358 M | 30.3 | **2.5 ms** |

All three modified models improved F1-score, mAP50, and mAP50-95 over the baseline.

- **YOLO26s CA** achieved the highest precision and mAP50-95, but introduced the greatest computational cost.
- **YOLO26s RCAC3k2** achieved the highest mAP50, tied for the highest F1-score, and slightly reduced the parameter count and GFLOPs compared with the baseline. It provided the strongest overall balance between detection performance and model complexity.
- **YOLO26s CBAM-AKConv** achieved the highest recall and lowest reported processing time.
- No model was superior across every evaluation criterion.

Qualitative observations suggested that RCAC3k2 performed well in selected background-confusion and crowded cases, CA suppressed duplicate detections more effectively, and CBAM-AKConv appeared more stable under substantial pose deformation.

## Research Limitations

The research aim was substantially met for overall person detection but only partially met for severe occlusion.

The main limitations are:

- the dataset does not include explicit occlusion categories or severity levels;
- visible-body ratios were not annotated;
- pose and shape deformation were not systematically labelled;
- severe occlusion was examined through selected qualitative cases rather than a separate quantitative benchmark;
- each model used one deterministic training run;
- repeated random seeds and statistical significance testing were not performed;
- the dataset was collected from one company's retail and warehouse environments;
- complete tracking and re-identification pipelines were not evaluated.

The results therefore demonstrate promising mitigation of occlusion-related detection problems rather than a conclusive solution to severe occlusion.

## Repository Guide

| Location | Contents |
|---|---|
| [`dataset/raw data/`](dataset/raw%20data/) | Data-collection documentation, SmartPSS automation, frame-extraction tools, and raw-image examples. |
| [`dataset/processed data/`](dataset/processed%20data/) | Annotation workflow, YOLO label format, dataset structure, and labeling illustrations. |
| [`models/`](models/) | Configurations and architectural explanations for the four evaluated YOLO26s models. |
| [`scr/`](scr/) | Custom neural-network blocks and parser integration used by the modified architectures. |
| [`results/`](results/) | Training outputs, quantitative results, evaluation plots, predictions, and model-specific summaries. |
| [`docs/`](docs/) | Project reports and supporting research documentation. |
| [`docs/paper/`](docs/paper/) | Reference papers and summaries covering occlusion, attention mechanisms, and object detection. |
| [`ultralytics_adjusted/`](ultralytics_adjusted/) | Research-specific adaptation of Ultralytics supporting the custom model components. |

Detailed architecture descriptions are available in the [`models` documentation](models/README.md).

Detailed experiment outputs are available in the [`results` documentation](results/README.md).

## Data Availability

The complete dataset is not publicly available because it contains private company CCTV data.

A limited number of raw and labelled samples are included to illustrate the research context and annotation results. Identifying information about the company, sites, and cameras is not disclosed.
