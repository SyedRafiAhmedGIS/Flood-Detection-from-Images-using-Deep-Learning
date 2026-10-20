# 🎨 Image Sources & Copyright Notes

This document explains the origin of **every image stored in this repository** and why the
repo is safe from third-party copyright claims.

## ✅ Images committed to this repository

| File | What it shows | How it was made |
|---|---|---|
| `docs/banner.png` | Hero banner: flooded road, neural network, drone & satellite | **AI-generated illustration** (GLM image model), title text added programmatically |
| `docs/demo.gif` | Animated demo: upload → detect → dashboard → export | Composed from **AI-generated illustrations** + matplotlib mock + programmatic text overlays |
| `evaluate/1.jpg` | Flooded residential street | **AI-generated illustration** |
| `evaluate/2.jpg` | Aerial/satellite view of a flooded neighborhood | **AI-generated illustration** |
| `evaluate/3.jpeg` | Dry sunny street (no flooding) | **AI-generated illustration** |
| `evaluate/4.jpg` | Aerial/satellite view of a dry neighborhood | **AI-generated illustration** |
| `evaluate/5.jpg` | Flooded house | **AI-generated illustration** |
| `evaluate/6.jpg` | Calm river between fields (normal water level) | **AI-generated illustration** |

All of the above were produced by an AI image-generation model at the request of this
project's maintainers. **No photographs were copied from any website, dataset, stock library
or individual photographer.** The illustrations are provided under the same MIT license as
the rest of the repository.

> ℹ️ The original version of this repository shipped **real photographs** in `evaluate/`
> (street-level flood photos collected for the research paper below, plus images scraped
> from Google image search). Those photos have been **removed** to avoid any copyright
> ambiguity. The `FloodDetection.ipynb` inference section works identically with the
> AI-generated illustrations.

## 🌐 Images NOT stored in this repository

- **The training dataset** (flooding / normal photos) is *not* committed — it downloads at
  runtime from the project's Google Drive link and remains credited to its original authors:
  *"Flooding" images* were collected by the authors of [Detecting floodwater on roadways from
  image data with handcrafted features and deep transfer learning
  (arXiv:1909.00125)](https://arxiv.org/pdf/1909.00125.pdf); *"No Flooding" images* were
  collected via an automated Google image search (label noise expected, as documented by the
  original authors). Use of the dataset for research/education follows the original paper's
  context; verify licensing before any commercial redistribution.
- **The pre-trained model weights** are likewise hosted externally on Google Drive and are
  the product of training on the dataset above.

## 🔒 Practical guidance

1. **Redistributing this repo** (e.g. your GitHub fork) is copyright-safe — it only contains
   AI-generated illustrations + code + documentation.
2. **Redistributing the dataset** requires checking with the original paper's authors.
3. **Third-party satellite datasets** (FloodNet, Sen1Floods11, etc.) used in the satellite
   fine-tuning section come with their own licenses — review them on their project pages
   before use.
