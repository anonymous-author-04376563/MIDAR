# MIDAR: Multimodal Inconsistency Dataset for Augmented Reality
This is the official repository of the paper "When Eyes and Ears Disagree: Detecting Multimodal Inconsistencies in Augmented Reality", submitted to IEEE ISMAR 2025. It introduces AR-VIM, a dataset of 624 egocentric videos.

The dataset can be accesssed through this [link](https://drive.google.com/drive/folders/16ARNoCDM4d32V49GFdMH9w4FUnFNDPX1?usp=sharing).

## Overview

MIDAR is a benchmark dataset for studying **multimodal inconsistency in augmented reality (AR)**, where visual AR cues and audio instructions convey semantically contradictory information. The dataset is designed for task-oriented AR scenarios and supports research on multimodal scene understanding, inconsistency detection, and safety-oriented AR perception.

MIDAR contains **624 egocentric AR videos** collected across **6 application domains** and **104 scenes**. The dataset is balanced with a **1:1 ratio** between inconsistent and consistent samples. Each scene includes both positive and negative variants to support controlled benchmarking.

### Key Features

- **624 videos** in total
- **104 scenes**
- **6 AR application domains**
- **3 inconsistency types**
- **Balanced labels**
- **Egocentric AR recordings**
- **JSON/JSONL metadata** for each sample

<figure>
  <img src="./dataset_example_large.png" alt="Description" width="1200" />
  <figcaption align="center"> Example screenshots, audio instructions, user tasks, and video labels from MIDAR across different AR application scenarios: (a) navigation, (b) safety inspection, (c) smart home, (d) smart retail, (e) AR-guided labor, and (f) AR-guided cooking. </figcaption>
</figure>

---

## What Is Multimodal Inconsistency?

In MIDAR, multimodal inconsistency refers to cases where the **visual AR guidance** and the **audio instruction** are semantically contradictory with respect to the user’s task.

We consider three types of inconsistency:

### 1. Reference-level inconsistency
The visual cue and audio instruction refer to different targets.

**Example:**  
Audio says *“Pick up the bucket”*, while the visual arrow points to a box.

### 2. Attribute-level inconsistency
The visual cue and audio instruction refer to the same object category but disagree on a key attribute.

**Example:**  
Audio says *“Pick up the white bottle”*, while the visual cue highlights a yellow bottle.

### 3. Spatial-level inconsistency
The visual cue and audio instruction provide conflicting spatial guidance.

**Example:**  
Audio says *“Turn left”*, while the visual cue indicates forward or right.

---

## Dataset Description

MIDAR is constructed to simulate realistic AR interactions in which users rely on both **visual overlays** and **spoken audio guidance** to complete tasks. The dataset focuses on common AR assistance settings and covers the following representative domains: **Navigation**, **Safety**, **Home**, **Retail**, **Labor**
and **Cooking**.


Each video captures the user’s **first-person AR view** during task execution. The virtual cue and audio instruction are intentionally designed to be either:

- **consistent** with each other, or
- **inconsistent** with each other.

All videos are recorded at:

- **Resolution:** 1920 × 1080
- **Frame rate:** 30 FPS
- **Duration:** 5–18 seconds

---

## Video Collection Pipeline

MIDAR is collected through a controlled AR data acquisition process. For each scene, we first define a user task and then design the corresponding multimodal guidance, including a visual AR cue and an audio instruction. Each scene is constructed to represent one of three inconsistency types: **reference**, **attribute**, or **spatial**. For every inconsistent case, we also create a matched consistent version to maintain a balanced dataset.

During recording, the virtual visual cue is placed in the scene but remains invisible at the beginning. Each video starts with only the real-world environment visible. At a selected time point, the visual cue is activated and the corresponding audio instruction is played simultaneously. This synchronized onset is intentional, as temporally aligned cross-modal signals are more likely to be perceived as related by human users.

All data are collected using **Meta Quest 3**. The AR applications are developed in **Unity 2022.3.61f1** with **ARCore** for spatial tracking. Videos are captured using the headset’s built-in screen recording, ensuring that each sample reflects the user’s actual egocentric AR experience.

---

## Dataset Statistics

### Distribution by Application Domain and Inconsistency Type

| Application | Reference A | Reference N | Attribute A | Attribute N | Spatial A | Spatial N | Total A | Total N |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Navigation | 12 | 12 | 21 | 21 | 15 | 15 | 48 | 48 |
| Safety | 15 | 15 | 18 | 18 | 24 | 24 | 57 | 57 |
| Home | 18 | 18 | 18 | 18 | 15 | 15 | 51 | 51 |
| Retail | 15 | 15 | 15 | 15 | 12 | 12 | 42 | 42 |
| Labor | 18 | 18 | 18 | 18 | 18 | 18 | 54 | 54 |
| Cooking | 24 | 24 | 18 | 18 | 18 | 18 | 60 | 60 |
| **Total** | **102** | **102** | **108** | **108** | **102** | **102** | **312** | **312** |

### Distribution Summary

- The dataset is **perfectly balanced** at the label level.
- Attribute-level samples are slightly more numerous than the other two inconsistency types.
- Cooking contains the largest number of samples.
- Retail contains the fewest samples among the six domains.

---

## Dataset Structure

The dataset is organized as follows:

```text
MIDAR/
├── Navigation/
│   ├── Reference/
│   ├── Attribute/
│   └── Spatial/
├── Safety/
│   ├── Reference/
│   ├── Attribute/
│   └── Spatial/
├── Home/
│   ├── Reference/
│   ├── Attribute/
│   └── Spatial/
├── Retail/
│   ├── Reference/
│   ├── Attribute/
│   └── Spatial/
├── Labor/
│   ├── Reference/
│   ├── Attribute/
│   └── Spatial/
└── Cooking/
    ├── Reference/
    ├── Attribute/
    └── Spatial/
```

## File Naming Rule

Each video file follows the naming format:

`{A|N}-{xxx}-{y}`

where:

- `A` = inconsistent sample
- `N` = consistent sample
- `xxx` = three-digit scene sequence number
- `y` = one-digit video index

## Full Path Pattern

`MIDAR/{domain}/{inconsistency_type}/{A|N}-{xxx}-{y}.mp4`

where:

- `{domain}` ∈ `{Navigation, Safety, Home, Retail, Labor, Cooking}`
- `{inconsistency_type}` ∈ `{Reference, Attribute, Spatial}`

## Examples

- `MIDAR/navigation/reference/A-001-1.mp4`
- `MIDAR/navigation/reference/N-001-2.mp4`
- `MIDAR/home/attribute/A-037-1.mp4`
- `MIDAR/cooking/spatial/N-104-3.mp4`

## Metadata Format

A JSON file is provided to facilitate dataset usage and benchmarking.

### Example Fields

```json
{
  "video_path": "MIDAR/navigation/reference/A-001-1.mp4",
  "duration": 8.4,
  "inconsistency": true,
  "domain": "navigation",
  "type": "reference",
  "visual_cue": "A virtual arrow indicating the wrong target",
  "audio_transcript": "Please pick up the box on the desk.",
  "task": "Pick up the correct object"
}
```

### Field Description

| Field | Description |
|---|---|
| `video_path` | Relative path to the video |
| `duration` | Video duration in seconds |
| `inconsistency` | Boolean version of the label |
| `domain` | Application domain |
| `type` | Inconsistency type |
| `visual_cue` | Description of the AR visual cue |
| `audio_transcript` | Transcribed spoken instruction |
| `task` | User task associated with the sample |

## Result Highlights

MIDAR is designed for benchmarking multimodal inconsistency detection systems in AR.

In our benchmark, multiple pipelines were evaluated, including:

- video-input vision-language models
- frame-sampling vision-language models
- feature-similarity-based baselines

The benchmark shows that multimodal inconsistency detection in AR is **challenging rather than trivial**, especially for spatial inconsistencies.

### Benchmark Results

| Method | Input Type | All Accuracy | All Precision | All Recall | Avg. Latency (s) |
|---|---|---:|---:|---:|---:|
| Gemini-3-Flash | Video-input | 86.38 | 81.62 | 93.91 | 6.76 |
| GPT-5.4 | Frame-sampling | 90.71 | 95.04 | 85.90 | 12.13 |
| Qwen-2.5-Omni-7B | Video-input | 52.56 | 55.56 | 25.64 | 10.46 |
| Qwen-3-8B | Frame-sampling | 67.54 | 81.91 | 45.03 | 23.42 |
| LLaVA-34B | Frame-sampling | 46.31 | 46.30 | 46.15 | 10.30 |
| Mistral-Small-3.1-24B | Frame-sampling | 59.13 | 74.78 | 27.56 | 9.93 |
| CLIP | Feature-similarity | 51.76 | 56.96 | 14.42 | 1.78 |


## Recommended Usage

MIDAR can support research on:

- multimodal inconsistency detection
- AR scene understanding
- egocentric AR video analysis
- multimodal reasoning
- safety and reliability in AR
- cross-modal grounding
- audio-visual contradiction detection



