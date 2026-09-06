# OccluAttnBench_2026

A comparative evaluation of attention-based person-detection methods under real-world occlusion and real-time conditions.

## Project Overview

OccluAttnBench_2026 investigates person detection in CCTV images captured in retail and warehouse environments.

Person detection is the first step in converting CCTV footage into structured information such as the number, location, appearance time, and spatial distribution of people. Reliable detections could subsequently support occupancy monitoring, customer-traffic analysis, spatial heatmaps, tracking, and re-identification.

This project focuses only on person detection at the individual-frame level. Tracking, identity recognition, multi-camera association, and behaviour analysis are outside the current scope.

## Research Objective

The project uses YOLO26s as the baseline and evaluates three modified architectures:

- **YOLO26s CA**, using Coordinate Attention;
- **YOLO26s RCAC3k2**, combining residual feature learning with Coordinate Attention;
- **YOLO26s CBAM-AKConv**, combining channel-spatial attention with adaptive convolution.

All four models were trained and evaluated under the same experimental conditions to compare:

- detection accuracy;
- bounding-box localisation;
- precision and recall;
- model complexity;
- processing speed.

The private dataset contains approximately 12,000 CCTV images. The common evaluation set contains 1,226 images and 2,665 person instances.

All three modified models improved the main detection metrics over the baseline. RCAC3k2 provided the strongest overall balance between detection performance and model complexity, while CA achieved the highest precision and CBAM-AKConv achieved the highest recall and lowest reported processing time.

Detailed measurements are available in the [`results` directory](results/README.md).

## Project Workflow

```text
CCTV recordings
        ↓
Frame extraction
        ↓
Annotation with X-AnyLabeling
        ↓
YOLO dataset preparation
        ↓
Model training and evaluation
        ↓
Performance comparison
```

## Repository Guide

| Location | Contents |
|---|---|
| [`dataset/raw data/`](dataset/raw%20data/) | Data-collection documentation, SmartPSS automation, frame-extraction tools, and example raw images. |
| [`dataset/processed data/`](dataset/processed%20data/) | Annotation workflow, YOLO label format, dataset structure, and examples illustrating the labeling results. |
| [`models/`](models/) | YAML configurations for the baseline and three modified YOLO26s architectures. |
| [`scr/`](scr/) | Custom neural-network blocks and parser integration used by the modified models. |
| [`results/`](results/) | Training outputs, evaluation metrics, plots, confusion matrices, prediction examples, and model-specific summaries. |
| [`docs/`](docs/) | Project reports and supporting research documentation. |
| [`docs/paper/`](docs/paper/) | Reference papers and summaries covering occlusion handling, attention mechanisms, and object detection. |
| [`ultralytics_adjusted/`](ultralytics_adjusted/) | Research-specific adaptation of Ultralytics used to support the custom model components. |
| [`requirements.txt`](requirements.txt) | Python dependencies required by the project. |

For detailed model architecture explanations, see the [`models` documentation](models/README.md).

For individual experiment results, open the relevant model directory:

- [YOLO26s Base](results/yolo26s_base/)
- [YOLO26s CA](results/yolo26s_ca/)
- [YOLO26s RCAC3k2](results/yolo26s_rcac3k2/)
- [YOLO26s CBAM-AKConv](results/yolo26s_cbam_akconv/)

## Data Availability

The complete dataset is not publicly available due to the company's data-protection policy.

A limited number of raw and labelled samples are included to illustrate the project context and annotation results.

## Scope and Limitations

The data-collection and processing workflow was designed for the company's existing infrastructure and should not be treated as a general-purpose industrial solution.

The dataset does not contain separate quantitative labels for occlusion type or severity. Consequently, severe occlusion is examined qualitatively, while the reported quantitative metrics represent overall person-detection performance.
