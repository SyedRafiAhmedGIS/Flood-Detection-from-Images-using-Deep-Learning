<div align="center">

<img src="docs/banner.png" width="100%" alt="Flood Detection from Images using Deep Learning"/>

**बाढ़ बनाम कोई बाढ़ नहीं — इमेज क्लासिफ़ायर, TensorFlow / Keras में MobileNet ट्रांसफर लर्निंग**

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://python.org)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.16%2B-orange)](https://tensorflow.org)
[![Keras](https://img.shields.io/badge/Keras-3-red)](https://keras.io)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

**🌐 भाषा: [English](README.md) | हिन्दी | [Español](README_ES.md)**

</div>

---

## ⚡ त्वरित शुरुआत — कोई इंस्टॉलेशन नहीं चाहिए

> ### [▶ Google Colab में Flood_Detection_Colab.ipynb खोलें](https://colab.research.google.com/github/SyedRafiAhmedGIS/Flood-Detection-from-Images-using-Deep-Learning/blob/main/Flood_Detection_Colab.ipynb)
>
> यह Colab नोटबुक **पूरी तरह ऑटोमैटिक** है: खुद सब इंस्टॉल करता है, प्री-ट्रेंड मॉडल डाउनलोड करता है,
> आपसे **सैटेलाइट / ड्रोन / सड़क-स्तर की इमेज** अपलोड करवाता है (एक-एक करके, कई साथ में, या पूरी `.zip`),
> डिटेक्शन चलाता है, एनोटेटेड गैलरी + पूरा स्टैटिस्टिक्स डैशबोर्ड दिखाता है, और सारा विश्लेषण
> (CSV, ग्राफ़, लेबल की हुई कॉपियाँ, मॉडल) एक ही `.zip` में डाउनलोड करा देता है।
> **री-ट्रेनिंग** और **सैटेलाइट फ़ाइन-ट्यूनिंग** के वैकल्पिक सेक्शन भी शामिल हैं।


## 🧰 तीन नोटबुक, तीन काम

| नोटबुक | कब इस्तेमाल करें |
|---|---|
| **`Flood_Detection_Colab.ipynb`** | सब कुछ एक जगह: डिटेक्शन + ट्रेनिंग + सैटेलाइट फ़ाइन-ट्यूनिंग |
| **`01_Quick_Detection_Colab.ipynb`** | सिर्फ़ डिटेक्शन: इमेज अपलोड → निर्णय → आँकड़े → zip एक्सपोर्ट |
| **`02_Train_Your_Model_Colab.ipynb`** | अपना मॉडल ट्रेन करना + TFLite एक्सपोर्ट + सैटेलाइट फ़ाइन-ट्यूनिंग |
| **`FloodDetection.ipynb`** | क्लासिक ट्रेनिंग पाइपलाइन स्थानीय रूप से (Jupyter) चलाने के लिए |

## 📂 रिपॉज़िटरी संरचना

```
Flood-Detection-from-Images-using-Deep-Learning/
├── FloodDetection.ipynb              # ट्रेनिंग + मूल्यांकन पाइपलाइन (लोकल Jupyter)
├── Flood_Detection_Colab.ipynb       # ऑल-इन-वन Colab (डिटेक्ट + ट्रेन + सैटेलाइट)
├── 01_Quick_Detection_Colab.ipynb    # Colab: केवल डिटेक्शन (तेज़ रास्ता)
├── 02_Train_Your_Model_Colab.ipynb   # Colab: केवल ट्रेनिंग (+ TFLite + सैटेलाइट)
├── docs/
│   ├── banner.png                    # AI-जनरेटेड बैनर (इलस्ट्रेशन)
│   ├── demo.gif                      # वर्कफ़्लो का एनिमेटेड डेमो
│   └── IMAGE_SOURCES.md              # हर इमेज कॉपीराइट-सुरक्षित क्यों है
├── evaluate/                         # इन्फ़रेंस के लिए 6 AI-जनरेटेड नमूना इमेज
├── requirements.txt                  # Python डिपेंडेंसी
├── LICENSE                           # MIT
└── README.md                         # (+ README_HI.md, README_ES.md अनुवाद)
```

## 🧠 मॉडल

| | |
|---|---|
| **बेस मॉडल** | MobileNet (ImageNet वेट, `include_top=False`) |
| **इनपुट** | 224 × 224 × 3, MobileNet प्रीप्रोसेसिंग ([-1, 1] स्केल) |
| **हेड** | GlobalAveragePooling2D → Dropout(0.2) → Dense(2, softmax) |
| **लॉस / ऑप्टिमाइज़र** | Categorical cross-entropy / Adam (lr = 1e-4) |
| **फ़ाइन-ट्यून** | अंतिम 23 लेयर ट्रेनेबल, बाकी फ़्रोज़न |
| **टेस्ट सटीकता** | ~97-98% (री-ट्रेंड मॉडल: **97.2%** accuracy, 0.97 macro F1) |

### ✅ मूल नोटबुक से क्या ठीक किया गया

- **Softmax + categorical cross-entropy** हेड (पहले `sigmoid` के साथ `categorical_crossentropy`
  था, जिससे स्कोर सही प्रोबेबिलिटी नहीं थे)
- **`f1_score` वेरिएबल शैडोइंग बग** हटाया (मूल्यांकन सेल दोबारा चलाने पर क्रैश होता था)
- **आधुनिक API**: deprecated `ImageDataGenerator` की जगह `image_dataset_from_directory`;
  हटाए गए `lr` की जगह `Adam(learning_rate=...)`; मॉडल अब `.keras` फ़ॉर्मेट में सेव होते हैं
- **पुनरुत्पादनीयता**: ग्लोबल रैंडम सीड; **EarlyStopping** (बेस्ट वेट्स रिस्टोर)
- **Recall अब रिपोर्ट होता है** (बाढ़ छूट जाना महँगा false negative है)
- भ्रामक कमेंट ठीक किए, डिबग सेल हटाए, नोटबुक आउटपुट साफ़ किए
- 2020 के पुराने प्री-ट्रेंड वेट आर्किटेक्चर रीबिल्ड + by-name ट्रांसफ़र से लोड होते हैं —
  आधुनिक Keras 3 पर सत्यापित

## 🚀 लोकल सेटअप

```bash
git clone https://github.com/SyedRafiAhmedGIS/Flood-Detection-from-Images-using-Deep-Learning.git
cd Flood-Detection-from-Images-using-Deep-Learning
pip install -r requirements.txt
jupyter notebook FloodDetection.ipynb
```

नोटबुक पहली बार चलाने पर **डेटासेट खुद डाउनलोड** हो जाता है (~52 MB, Google Drive):

- 📦 डेटासेट: https://drive.google.com/file/d/1oT07-wVuYj_hByn4FqUtIj63Yg_fe7dP/view
- 🤖 प्री-ट्रेंड मॉडल: https://drive.google.com/file/d/1mF8NmMClUbKXYJoDdW3OYHhwiSknI5hk/view

डेटासेट संरचना: `data/{train,valid,test}/{flooding,normal}` — 321 ट्रेन / 81 वैलिड / 72 टेस्ट।

## 🛰️ सैटेलाइट इमेज

प्री-ट्रेंड मॉडल ज़्यादातर **सड़क-स्तर** की तस्वीरों पर ट्रेंड है — सैटेलाइट/ड्रोन निर्णयों को
अनुमान मानें। उत्पादन-स्तर के सैटेलाइट उपयोग के लिए फ़ाइन-ट्यून करें:

- नोटबुक **सेक्शन 9** (या `02_Train_Your_Model_Colab.ipynb` सेक्शन 5) कोई भी दो-फ़ोल्डर
  डेटासेट zip लेता है — जैसे [FloodNet](https://github.com/BinaLab/FloodNet) (UAV/ड्रोन)
  या [Sen1Floods11](https://github.com/cloudtostreet/Sen1Floods11) (Sentinel-1/2)
- कम लर्निंग रेट पर फ़ाइन-ट्यून होता है ताकि सड़क-स्तर का ज्ञान बना रहे

## 🎨 इस रिपो में इमेज के बारे में (कॉपीराइट-सुरक्षित)

**इस रिपॉज़िटरी में मौजूद हर इमेज AI-जनरेटेड इलस्ट्रेशन है** (बैनर, डेमो GIF फ़्रेम और
`evaluate/` के नमूने) — कोई फ़ोटोग्राफ़ नहीं, कोई थर्ड-पार्टी स्टॉक नहीं, कॉपीराइट दावे का कोई
खतरा नहीं। असली डेटासेट इमेज बाहरी (Google Drive) हैं और उनका उचित श्रेय दिया गया है।
पूरी सूची के लिए [`docs/IMAGE_SOURCES.md`](docs/IMAGE_SOURCES.md) देखें।

## 📊 मूल्यांकन (री-ट्रेंड मॉडल, हेल्ड-आउट टेस्ट सेट)

| क्लास | Precision | Recall | F1 |
|---|---|---|---|
| Flooding | 1.000 | 0.953 | 0.976 |
| No Flooding | 0.936 | 1.000 | 0.967 |
| **कुल मिलाकर** | **0.968** | **0.977** | **0.971** |

## 🙏 श्रेय

- कोड [deeplizard](https://deeplizard.com/) से प्रेरित / अनुकूलित
- "Flooding" इमेज (बाहरी डेटासेट): [*Detecting floodwater on roadways from image data with
  handcrafted features and deep transfer learning*](https://arxiv.org/pdf/1909.00125.pdf)
  के लेखकों की सौजन्यता से
- बेस मॉडल: [MobileNet](https://arxiv.org/abs/1704.04861)

## ⚠️ अस्वीकरण

यह रिसर्च डेमो है, **जीवन-सुरक्षा प्रणाली नहीं**। बाढ़ की स्थिति हमेशा आधिकारिक स्रोतों से
पुष्टि करें।

## 📄 लाइसेंस

[MIT](LICENSE)
