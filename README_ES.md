<div align="center">

<img src="docs/banner.png" width="100%" alt="Flood Detection from Images using Deep Learning"/>

**Clasificador binario Inundación / Sin inundación — transfer learning con MobileNet en TensorFlow / Keras**

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://python.org)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.16%2B-orange)](https://tensorflow.org)
[![Keras](https://img.shields.io/badge/Keras-3-red)](https://keras.io)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

**🌐 Idiomas: [English](README.md) | [हिन्दी](README_HI.md) | Español**

</div>

---

## ⚡ Inicio rápido — sin instalación

> ### [▶ Abrir Flood_Detection_Colab.ipynb en Google Colab](https://colab.research.google.com/github/SyedRafiAhmedGIS/Flood-Detection-from-Images-using-Deep-Learning/blob/main/Flood_Detection_Colab.ipynb)
>
> El notebook de Colab es **totalmente automático**: se instala solo, descarga el modelo
> pre-entrenado, te pide subir **imágenes satelitales / de dron / a nivel de calle** (una a
> una, varias a la vez o un `.zip` completo), ejecuta la detección, muestra una galería
> anotada + panel completo de estadísticas, y descarga todo el análisis (CSV, gráficos,
> copias etiquetadas, modelo) en un único `.zip`. Incluye **re-entrenamiento** y
> **fine-tuning satelital** opcionales.


## 🧰 Tres notebooks, tres flujos de trabajo

| Notebook | Úsalo cuando quieras... |
|---|---|
| **`Flood_Detection_Colab.ipynb`** | Todo en un solo lugar: detectar + entrenar + fine-tuning satelital |
| **`01_Quick_Detection_Colab.ipynb`** | Solo detectar: subir imágenes → veredictos → estadísticas → exportar zip |
| **`02_Train_Your_Model_Colab.ipynb`** | Entrenar tu propio modelo + exportar TFLite + fine-tuning satelital |
| **`FloodDetection.ipynb`** | Ejecutar el pipeline clásico localmente (Jupyter) |

## 📂 Estructura del repositorio

```
Flood-Detection-from-Images-using-Deep-Learning/
├── FloodDetection.ipynb              # pipeline de entrenamiento + evaluación (Jupyter local)
├── Flood_Detection_Colab.ipynb       # Colab todo-en-uno (detectar + entrenar + satélite)
├── 01_Quick_Detection_Colab.ipynb    # Colab: solo detección (camino rápido)
├── 02_Train_Your_Model_Colab.ipynb   # Colab: solo entrenamiento (+ TFLite + satélite)
├── docs/
│   ├── banner.png                    # banner ilustrado generado con IA
│   ├── demo.gif                      # demo animado del flujo de trabajo
│   └── IMAGE_SOURCES.md              # por qué cada imagen es segura en derechos de autor
├── evaluate/                         # 6 imágenes de ejemplo generadas con IA
├── requirements.txt                  # dependencias de Python
├── LICENSE                           # MIT
└── README.md                         # (+ README_HI.md, README_ES.md)
```

## 🧠 Modelo

| | |
|---|---|
| **Modelo base** | MobileNet (pesos de ImageNet, `include_top=False`) |
| **Entrada** | 224 × 224 × 3, preprocesamiento de MobileNet (escala [-1, 1]) |
| **Cabeza** | GlobalAveragePooling2D → Dropout(0.2) → Dense(2, softmax) |
| **Pérdida / Optimizador** | Categorical cross-entropy / Adam (lr = 1e-4) |
| **Fine-tuning** | Últimas 23 capas entrenables, resto congelado |
| **Precisión en test** | ~97-98% (modelo re-entrenado: **97.2%** accuracy, 0.97 macro F1) |

### ✅ Qué se corrigió respecto al notebook original

- Cabeza **softmax + categorical cross-entropy** (el original usaba `sigmoid` con
  `categorical_crossentropy`, así que los puntajes no eran probabilidades válidas)
- Eliminado el **bug de sombreado de la variable `f1_score`** (re-ejecutar la celda fallaba)
- **APIs modernas**: `image_dataset_from_directory` en lugar del obsoleto
  `ImageDataGenerator`; `Adam(learning_rate=...)` en lugar del eliminado `lr`; modelos
  guardados en formato `.keras`
- **Reproducibilidad**: semillas globales; **EarlyStopping** con restauración de mejores pesos
- **Se reporta el recall** (un inningdo omitido es un falso negativo costoso)
- Comentarios engañosos corregidos, celdas de debug eliminadas, salidas de notebook limpiadas
- Los pesos de 2020 se cargan reconstruyendo la arquitectura + transferencia por nombre —
  verificado en Keras 3 moderno

## 🚀 Configuración local

```bash
git clone https://github.com/SyedRafiAhmedGIS/Flood-Detection-from-Images-using-Deep-Learning.git
cd Flood-Detection-from-Images-using-Deep-Learning
pip install -r requirements.txt
jupyter notebook FloodDetection.ipynb
```

El notebook **descarga el dataset automáticamente** (~52 MB desde Google Drive) en la
primera ejecución:

- 📦 Dataset: https://drive.google.com/file/d/1oT07-wVuYj_hByn4FqUtIj63Yg_fe7dP/view
- 🤖 Modelo pre-entrenado: https://drive.google.com/file/d/1mF8NmMClUbKXYJoDdW3OYHhwiSknI5hk/view

Estructura: `data/{train,valid,test}/{flooding,normal}` — 321 train / 81 valid / 72 test.

## 🛰️ Imágenes satelitales

El modelo pre-entrenado aprendió mayormente de fotos **a nivel de calle** — trata los
veredictos satelitales/de dron como estimaciones. Para uso satelital en producción,
haz fine-tuning:

- La **Sección 9** del notebook (o Sección 5 de `02_Train_Your_Model_Colab.ipynb`) acepta
  cualquier zip de dos carpetas — p. ej. [FloodNet](https://github.com/BinaLab/FloodNet)
  (UAV/dron) o [Sen1Floods11](https://github.com/cloudtostreet/Sen1Floods11) (Sentinel-1/2)
- Fine-tuning a tasa de aprendizaje baja para preservar el conocimiento a nivel de calle

## 🎨 Sobre las imágenes de este repositorio (seguras en derechos de autor)

**Todas las imágenes almacenadas en este repositorio son ilustraciones generadas con IA**
(banner, fotogramas del GIF de demo y los ejemplos de `evaluate/`) — sin fotografías, sin
stock de terceros, nada sujeto a reclamaciones de copyright. Las imágenes reales del dataset
permanecen externas (Google Drive) y están correctamente acreditadas. Consulta
[`docs/IMAGE_SOURCES.md`](docs/IMAGE_SOURCES.md) para la lista completa.

## 📊 Evaluación (modelo re-entrenado, conjunto de prueba)

| Clase | Precision | Recall | F1 |
|---|---|---|---|
| Flooding | 1.000 | 0.953 | 0.976 |
| No Flooding | 0.936 | 1.000 | 0.967 |
| **global** | **0.968** | **0.977** | **0.971** |

## 🙏 Créditos

- Código inspirado y adaptado de [deeplizard](https://deeplizard.com/)
- Imágenes de "Flooding" (dataset externo) cortesía de los autores de [*Detecting floodwater
  on roadways from image data with handcrafted features and deep transfer
  learning*](https://arxiv.org/pdf/1909.00125.pdf)
- Modelo base: [MobileNet](https://arxiv.org/abs/1704.04861)

## ⚠️ Descargo de responsabilidad

Demo de investigación, **no** es un sistema de seguridad para vidas. Confirma siempre las
condiciones de inundación por canales oficiales.

## 📄 Licencia

[MIT](LICENSE)
