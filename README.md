<div align="center">

<img src="docs/banner.png" width="100%" alt="Flood Detection from Images using Deep Learning"/>

**Binary Flooding vs. No-Flooding image classifier — MobileNet transfer learning in TensorFlow / Keras**

`flood-detection` · `deep-learning` · `mobilenet` · `tensorflow` · `remote-sensing` · `gis` · `disaster-management`

<img src="docs/demo.gif" width="80%" alt="Demo: upload images, detect floods, view stats, export results"/>

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://python.org)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.16%2B-orange)](https://tensorflow.org)
[![Keras](https://img.shields.io/badge/Keras-3-red)](https://keras.io)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)
[![Colab](https://img.shields.io/badge/Run-Google%20Colab-F9AB00?logo=googlecolab)](https://colab.research.google.com/github/SyedRafiAhmedGIS/Flood-Detection-from-Images-using-Deep-Learning/blob/main/Flood_Detection_Colab.ipynb)

**🌐 Read this in: [English](README.md) | [हिन्दी](README_HI.md) | [Español](README_ES.md)**

</div>

---

## ⚡ Quick start — no installation needed

> ### [▶ Open Flood_Detection_Colab.ipynb in Google Colab](https://colab.research.google.com/github/SyedRafiAhmedGIS/Flood-Detection-from-Images-using-Deep-Learning/blob/main/Flood_Detection_Colab.ipynb)
>
> The all-in-one Colab notebook is **fully automatic**: it installs everything, downloads the
> pre-trained model, asks you to upload **satellite / drone / street-level images** (single
> images, many at once, or a whole `.zip`), runs detection, shows an annotated gallery + full
> statistics dashboard, and downloads all analysis (CSV, graphs, labeled copies, model) as a
> single `.zip`. Optional **retraining** and **satellite fine-tuning** included.


## 🧰 Three notebooks for three workflows

| Notebook | Use it when you want to... |
|---|---|
| **`Flood_Detection_Colab.ipynb`** | Do everything in one place: detect + retrain + satellite fine-tune |
| **`01_Quick_Detection_Colab.ipynb`** | Just detect: upload images → verdicts → stats → export zip |
| **`02_Train_Your_Model_Colab.ipynb`** | Train your own model + TFLite export + satellite fine-tune |
| **`FloodDetection.ipynb`** | Run the classic training pipeline locally (Jupyter) |

## 📂 Repository structure

```
Flood-Detection-from-Images-using-Deep-Learning/
├── FloodDetection.ipynb              # training + evaluation pipeline (local Jupyter)
├── Flood_Detection_Colab.ipynb       # all-in-one Colab (detect + train + satellite)
├── 01_Quick_Detection_Colab.ipynb    # Colab: detect only (fast path)
├── 02_Train_Your_Model_Colab.ipynb   # Colab: train only (+ TFLite + satellite)
├── docs/
│   ├── banner.png                    # AI-generated hero banner (illustration)
│   ├── demo.gif                      # animated demo of the workflow
│   ├── GITHUB_SETUP.md               # repo description + topic tags
│   └── IMAGE_SOURCES.md              # why every image here is copyright-safe
├── evaluate/                         # 6 AI-generated sample images for inference
├── requirements.txt                  # Python dependencies
├── LICENSE                           # MIT
└── README.md                         # (+ README_HI.md, README_ES.md translations)
```

## 🧠 Model

| | |
|---|---|
| **Base model** | MobileNet (ImageNet weights, `include_top=False`) |
| **Input** | 224 × 224 × 3, MobileNet preprocessing (scales to [-1, 1]) |
| **Head** | GlobalAveragePooling2D → Dropout(0.2) → Dense(2, softmax) |
| **Loss / Optimizer** | Categorical cross-entropy / Adam (lr = 1e-4) |
| **Fine-tuned** | Top 23 layers trainable, rest frozen |
| **Test accuracy** | ~97-98% (retrained model: **97.2%** accuracy, 0.97 macro F1) |

### ✅ What was fixed vs. the original notebook

- **Softmax + categorical cross-entropy** head (the original used `sigmoid` with
  `categorical_crossentropy`, so scores were not valid probabilities)
- **`f1_score` variable shadowing bug** removed (re-running the evaluation cell used to crash)
- **Modern APIs**: `image_dataset_from_directory` instead of the deprecated
  `ImageDataGenerator`; `Adam(learning_rate=...)` instead of the removed `lr` argument;
  models saved in the `.keras` format
- **Reproducibility**: global random seeds; **EarlyStopping** with best-weights restore
- **Recall is now reported** (a missed flood is a costly false negative)
- Misleading comments corrected, debug cells removed, notebook outputs cleared
- Legacy 2020 pre-trained weights load via architecture rebuild + by-name transfer —
  verified to reproduce the original model's predictions on modern Keras 3

## 🚀 Local setup

```bash
git clone https://github.com/SyedRafiAhmedGIS/Flood-Detection-from-Images-using-Deep-Learning.git
cd Flood-Detection-from-Images-using-Deep-Learning
pip install -r requirements.txt
jupyter notebook FloodDetection.ipynb
```

The notebook **auto-downloads the dataset** (~52 MB from Google Drive) on first run:

- 📦 Dataset: https://drive.google.com/file/d/1oT07-wVuYj_hByn4FqUtIj63Yg_fe7dP/view
- 🤖 Pre-trained model: https://drive.google.com/file/d/1mF8NmMClUbKXYJoDdW3OYHhwiSknI5hk/view

Dataset layout: `data/{train,valid,test}/{flooding,normal}` — 321 train / 81 valid / 72 test.

## 🛰️ Satellite images

The pre-trained model was trained mostly on **street-level** photos — treat satellite/drone
verdicts as estimates. For production satellite use, fine-tune:

- Notebook **Section 9** (or `02_Train_Your_Model_Colab.ipynb` Section 5) accepts any
  two-folder dataset zip — e.g. [FloodNet](https://github.com/BinaLab/FloodNet) (UAV/drone)
  or [Sen1Floods11](https://github.com/cloudtostreet/Sen1Floods11) (Sentinel-1/2)
- It fine-tunes at a low learning rate so street-level knowledge is preserved

## 🎨 About the images in this repo (copyright-safe)

**Every image stored in this repository is an AI-generated illustration** (banner, demo GIF
frames and the `evaluate/` samples) — no photographs, no third-party stock, nothing to claim
copyright over. Real dataset images stay external (Google Drive) and are properly credited.
See [`docs/IMAGE_SOURCES.md`](docs/IMAGE_SOURCES.md) for the full list.

## 📊 Evaluation (retrained model, held-out test set)

| Class | Precision | Recall | F1 |
|---|---|---|---|
| Flooding | 1.000 | 0.953 | 0.976 |
| No Flooding | 0.936 | 1.000 | 0.967 |
| **overall** | **0.968** | **0.977** | **0.971** |

## 🙏 Credits

- Code inspired by and adapted from [deeplizard](https://deeplizard.com/)
- "Flooding" images (external dataset) courtesy of the authors of [*Detecting floodwater on
  roadways from image data with handcrafted features and deep transfer
  learning*](https://arxiv.org/pdf/1909.00125.pdf)
- Base model: [MobileNet](https://arxiv.org/abs/1704.04861)

## ⚠️ Disclaimer

Research demo, **not** a life-safety system. Always confirm flood conditions through official
channels.

## 📄 License

[MIT](LICENSE)
