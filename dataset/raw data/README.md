# Raw Data

## Overview

This directory contains source documents, sample data, and utilities used during the data acquisition and extraction stages of the OccluAttnBench_2026 project.

## Directory Structure

```text
raw data/
├── DOC090226-09022026233644.pdf
├── data_collection/
└── data_extraction/
```

### `DOC090226-09022026233644.pdf`

A 29-page scanned bilingual Vietnamese-English agreement governing remote access and personal data processing for research and development.

The agreement defines:

- the permitted research purpose and processing scope;
- remote-access and data-storage requirements;
- responsibilities of the data controller and authorised recipient;
- security, confidentiality, retention, and deletion requirements;
- anonymisation and re-identification testing requirements;
- publication and reuse restrictions.

### `data_collection/`

Contains the SmartPSS Playback automation workflow used to export authorised video recordings from an Excel-based task list.

See [`data_collection/README.md`](data_collection/README.md) for configuration, operating requirements, and usage instructions.

### `data_extraction/`

Contains the command-line utility used to extract frames from authorised video files with FFmpeg.

The extraction utility supports:

- processing a single video or a directory of videos;
- configurable frame-extraction intervals;
- recursive input-directory scanning;
- resumable processing through completion records;
- multiple common video formats.

## Data Workflow

```text
Authorised SmartPSS recordings
        ↓
Video collection and export
        ↓
Frame extraction
        ↓
Cleaning, anonymisation, and annotation
        ↓
Processed dataset
```

## Scope and Limitations

The entire workflow in this directory was designed specifically for the company's existing infrastructure, software environment, and operational setup.

It is environment-specific and is not intended to be directly applied as a general-purpose or production-ready workflow in other organisations or industrial deployments. Any reuse or adaptation may require substantial changes to the software configuration, hardware environment, data-access process, and operational procedures.

For further information or reference, please contact:

`leviethuyhaong172@gmail.com`

## Data Availability

The original dataset will not be made publicly available due to the company's data-protection policies.

Access to the complete dataset is restricted and subject to the company's internal authorisation, privacy, and data-governance requirements.

## Sample Data

A limited number of extracted image samples are provided to help readers understand the type of data used in the project and visualise the project context.

These samples are provided for demonstration and documentation purposes only and do not represent the complete dataset.
