<p align="right">English | <a href="./README_CN.md">简体中文</a></p>

<div align="center">

# AutoDriDM: An Explainable Benchmark for Decision-Making of Vision-Language Models in Autonomous Driving

**Zecong Tang<sup>1,\*</sup>, Zixu Wang<sup>1,\*</sup>, Yifei Wang<sup>1,\*</sup>, Weitong Lian<sup>1,\*</sup>, Tianjian Gao<sup>1</sup>, Haoran Li<sup>1</sup>, Tengju Ru<sup>1</sup>, Lingyi Meng<sup>1</sup>, Zhejun Cui<sup>1</sup>, Yichen Zhu<sup>1</sup>, Qi Kang<sup>1</sup>, Kaixuan Wang<sup>2</sup>, Yu Zhang<sup>1,†</sup>**

<sup>1</sup>Zhejiang University, Hangzhou, China &nbsp;&nbsp; <sup>2</sup>The University of Hong Kong, Hong Kong, China  
<sup>*</sup>Equal contribution &nbsp;&nbsp; <sup>†</sup>Corresponding author

[![Paper](https://img.shields.io/badge/arXiv-2601.14702-b31b1b.svg)](https://arxiv.org/abs/2601.14702)
[![Dataset](https://img.shields.io/badge/HuggingFace-Dataset-yellow.svg)](https://huggingface.co/datasets/ColamentosZJU/AutoDriDM)

<img src="docs/figs/pipeline.png" width="95%"/>

</div>

Autonomous driving is a highly challenging domain that requires reliable perception and safe decision-making in complex scenarios. Recent vision-language models (VLMs) demonstrate reasoning and generalization abilities, opening new possibilities for autonomous driving; however, existing benchmarks and metrics overemphasize perceptual competence and fail to adequately assess decision-making processes. In this work, we present **AutoDriDM**, a decision-centric, progressive benchmark with 6,650 questions across three dimensions—Object, Scene, and Decision. We evaluate mainstream VLMs to delineate the perception-to-decision capability boundary in autonomous driving, and our correlation analysis reveals weak alignment between perception and decision-making performance. We further conduct explainability analyses of models’ reasoning processes, identifying key failure modes such as logical reasoning errors, and introduce an analyzer model to automate large-scale annotation. AutoDriDM bridges the gap between perception-centered and decision-centered evaluation, providing guidance toward safer and more reliable VLMs for real-world autonomous driving.

---

## ✨ Overview

**AutoDriDM** is a **decision-centric**, progressive benchmark for evaluating the capability boundary of VLMs from **perception → scene understanding → decision-making** in autonomous driving.

### Key Facts

- **Protocol:** 3 progressive levels — **Object → Scene → Decision**
- **Tasks:** 6 tasks (two per level)
- **Scale:** **6,650** QA items built from **1,295** front-facing images
- **Risk-aware evaluation:** each item has a 5-level risk label `danger_score ∈ {1,2,3,4,5}`  
  - **High-risk** can be defined as `average danger_score ≥ 4.0`
- **Explainability:** supports reasoning-trace analysis with categorized failure modes and an analyzer model

---

## 🧩 Benchmark Structure

AutoDriDM follows a **progressive evaluation** protocol:

- **Object Level:** perception of key objects and their states  
- **Scene Level:** global scene understanding and critical context factors  
- **Decision Level:** driving action selection and risk awareness  

---

## 📦 Dataset

> **Note:** We do **not** provide the raw images in this repository. Please download the images from the three original datasets: **nuScenes**, **KITTI**, and **BDD100K**.

The dataset is hosted on Hugging Face:

- https://huggingface.co/datasets/ColamentosZJU/AutoDriDM

It contains **six JSON files**, corresponding to six tasks:

### Object Level (single-choice)

- **Object-1 (`Object-1.json`)**: Identify the **most important object** influencing the driving decision.
- **Object-2 (`Object-2.json`)**: Determine the **state** of a designated key object (e.g., traffic light state).

### Scene Level (multiple-choice)

- **Scene-1 (`Scene-1.json`)**: Recognize **weather / illumination** (e.g., daytime, nighttime, rain, snow, heavy fog).
- **Scene-2 (`Scene-2.json`)**: Identify **special scene factors** that potentially affect driving decisions (e.g., accident scene, construction zone).

### Decision Level (single-choice)

- **Decision-1 (`Decision-1.json`)**: Select the **optimal driving action** for the ego vehicle.
- **Decision-2 (`Decision-2.json`)**: Evaluate the **risk level** of a specified (potentially suboptimal) action.

---

## 🧾 Data Format

Each file is a JSON array. Each element is an object with the following fields:

- `image_name` (string): image identifier/path
- `taskX_q` (string): question text for task X
- `taskX_o` (string): options as a single string (e.g., `"A....; B....; C...."`)
- `taskX_a` (string): answer letters  
  - single-choice: one letter (e.g., `"C"`)  
  - multiple-choice: comma-separated letters (e.g., `"A,C"`)
- `danger_score` (int or string): risk label on a 5-level scale (1=minimal, 5=severe)

### Example (JSON)

```json
{
  "image_name": "images/xxxx.jpg",
  "task1_q": "...",
  "task1_o": "A....; B....; C....",
  "task1_a": "C",
  "danger_score": "2"
}
```

---

## 📊 Results & Analysis (Figures)

### Word Cloud of QA Content

<p align="center">
  <img src="docs/figs/word_cloud.png" alt="Word Cloud" width="30%"/><br/>
  <em>Word cloud built from AutoDriDM QA text.</em>
</p>

### Radar: Overall Performance

<p align="center">
  <img src="docs/figs/Lidar_overall.jpg" alt="Radar Overall" width="100%"/><br/>
  <em>Overall performance radar across tasks/models.</em>
</p>

### Radar: High-Danger Scenarios

<p align="center">
  <img src="docs/figs/Lidar_high_danger.jpg" alt="Radar High Danger" width="100%"/><br/>
  <em>Performance radar on high-risk (high danger) scenarios.</em>
</p>

### Error Category Statistics

<p align="center">
  <img src="docs/figs/error_category.jpg" alt="Error Category" width="100%"/><br/>
  <em>Distribution of explainability error categories.</em>
</p>

### Similar-Scene Robustness Examples

<p align="center">
  <img src="docs/figs/similarity.png" alt="Similarity" width="100%"/><br/>
  <em>Examples of visually similar scene pairs for robustness evaluation.</em>
</p>

### Explainability Case Study

<p align="center">
  <img src="docs/figs/Explanation.png" alt="Explanation" width="100%"/><br/>
  <em>Representative explainability analysis examples.</em>
</p>

---

## 🚀 Quickstart

### 1) Download Annotations

Get the six JSON files from:

- https://huggingface.co/datasets/ColamentosZJU/AutoDriDM

### 2) Load in Python

```python
import json

with open("Object-1.json", "r", encoding="utf-8") as f:
    data = json.load(f)

print(len(data), list(data[0].keys()))
```

---

## 📌 Citation

```bibtex
@article{tang2026autodridm,
  title={AutoDriDM: An Explainable Benchmark for Decision-Making of Vision-Language Models in Autonomous Driving},
  author={Tang, Zecong and Wang, Zixu and Wang, Yifei and Lian, Weitong and Gao, Tianjian and Li, Haoran and Ru, Tengju and Meng, Lingyi and Cui, Zhejun and Zhu, Yichen and Kang, Qi and Wang, Kaixuan and Zhang, Yu},
  journal={arXiv preprint arXiv:2601.14702},
  year={2026}
}
```

---

## ⚖️ License

This project is released under the **Apache License 2.0**.  
Some components or third-party implementations included in this repository may be distributed under different licenses.  

---

## 🙏 Acknowledgments

We thank the open-source community and dataset providers (nuScenes, KITTI, BDD100K) that make this research possible.
