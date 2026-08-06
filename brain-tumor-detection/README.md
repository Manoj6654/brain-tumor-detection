<img src="https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white" alt="Python"> <img src="https://img.shields.io/badge/TensorFlow-2.15-FF6F00?logo=tensorflow&logoColor=white" alt="TensorFlow"> <img src="https://img.shields.io/badge/Flask-3.0-000000?logo=flask&logoColor=white" alt="Flask"> <img src="https://img.shields.io/badge/SQLite-003B57?logo=sqlite&logoColor=white" alt="SQLite"> <img src="https://img.shields.io/badge/License-MIT-green" alt="License">

# 🧠 NeuroScan AI — Brain Tumor Detection

> **AI-powered MRI brain tumor detection system** using deep learning (VGG16 transfer learning) with real-time visualization, stage analysis, and PDF report generation.

---

## ✨ Features

| Feature | Description |
|---|---|
| 🔍 **4-Class Detection** | No Tumor · Pituitary · Meningioma · Glioma |
| 🎯 **Confidence Scoring** | Per-class probability with visual confidence bar |
| 🌡️ **Stage Analysis** | Early / Intermediate / Advanced stage classification |
| 🗺️ **2D Heatmap Overlay** | Colour-coded tumor region overlay on original MRI |
| 📦 **3D Visualization** | Interactive Plotly surface & point-cloud models |
| 📋 **Precautions Generator** | Personalized medical precautions by tumor type & stage |
| 📄 **PDF Report** | Downloadable diagnostic report with all findings |
| 🔐 **Auth System** | Register / Login / Logout with bcrypt password hashing |
| 🌓 **Dark / Light Mode** | Full theme toggle persisted via localStorage |
| 📱 **Responsive UI** | Mobile-friendly glassmorphism design |

---

## 🖼️ Screenshots

> Dashboard · Upload MRI · Results with Heatmap · 3D Visualization · PDF Report

---

## 🏗️ Tech Stack

| Layer | Technology |
|---|---|
| Language | Python 3.11 |
| AI / ML | TensorFlow 2.15, Keras, VGG16 |
| Web Framework | Flask 3.0 |
| Image Processing | OpenCV, Pillow |
| Visualization | Plotly |
| Database | SQLite (via sqlite3) |
| Security | Flask-Bcrypt |
| Frontend | Vanilla HTML/CSS/JS, Font Awesome, Google Fonts |

---

## 📁 Project Structure

```
brain-tumor-detection/
├── app.py                  # Main Flask application
├── train_model.py          # Model training script (VGG16 transfer learning)
├── gradcam.py              # Grad-CAM visualization utility
├── run.py                  # App runner
├── check_packages.py       # Dependency checker
├── requirements.txt        # Python dependencies
├── model_integration.md    # Model integration guide
├── models/                 # Trained model files (.h5) — not tracked by git
│   └── brain_tumor_cnn_model.h5
├── templates/              # Jinja2 HTML templates
│   ├── home.html           # Main dashboard
│   ├── login.html          # Login page
│   ├── register.html       # Registration page
│   ├── about.html          # About page
│   ├── forgot_password.html
│   └── reset_password.html
├── uploads/                # Temporary uploaded MRI images (not tracked)
├── users.db                # SQLite database (not tracked)
└── tests/
    ├── __init__.py
    └── test_app.py         # Pytest test suite
```

---

## ⚡ Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/deepthi-tr05/brain-tumor-detection.git
cd brain-tumor-detection
```

### 2. Create a Virtual Environment

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# macOS/Linux
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Train the Model (First-time setup)

You need a trained `.h5` model file in the `models/` directory.

```bash
# Place your brain tumor MRI dataset in data/ with this structure:
# data/Training/no_tumor/
# data/Training/pituitary_tumor/
# data/Training/meningioma_tumor/
# data/Training/glioma_tumor/

python train_model.py
```

The trained model will be saved as `models/brain_tumor_cnn_model.h5`.

> 📦 **Pre-trained model**: Download from the [Releases](https://github.com/deepthi-tr05/brain-tumor-detection/releases) page and place in the `models/` folder.

### 5. Run the Application

```bash
python app.py
```

Open your browser at: **http://127.0.0.1:5000**

---

## 🧪 Running Tests

```bash
pip install pytest
python -m pytest tests/ -v
```

Expected output: all 18 tests passing ✅

---

## 🔌 API Reference

| Method | Endpoint | Auth Required | Description |
|---|---|---|---|
| GET | `/` | ✅ | Main dashboard |
| GET/POST | `/login` | ❌ | User login |
| GET/POST | `/register` | ❌ | User registration |
| GET | `/logout` | ✅ | Clear session |
| GET/POST | `/forgot-password` | ❌ | Password reset request |
| GET/POST | `/reset-password` | ❌ | Password reset with token |
| POST | `/predict` | ✅ | Upload MRI → get prediction |
| GET | `/about` | ❌ | About page |

### POST `/predict`

**Request** — `multipart/form-data`:
```
image: <MRI scan file (.jpg, .jpeg, .png, .bmp, .tiff)>
```

**Response** — JSON:
```json
{
  "prediction": "Glioma Tumor",
  "confidence": 92.4,
  "visualization": {
    "success": true,
    "no_tumor": false,
    "original": "<base64 PNG>",
    "overlay": "<base64 PNG>",
    "plotly_surface": "<Plotly JSON>",
    "tumor_info": {
      "location": "Frontal Lobe, Central Region",
      "area_percentage": 18.3,
      "tumor_type": "Glioma Tumor",
      "confidence_score": 92.4,
      "stage": "Intermediate Stage",
      "stage_description": "...",
      "precautions": ["..."]
    }
  }
}
```

---

## 🧬 Model Architecture

- **Base Model**: VGG16 (ImageNet pre-trained, top layers removed)
- **Fine-tuning**: Last 4 convolutional blocks unfrozen
- **Classification Head**: GlobalAveragePooling2D → Dense(512, ReLU) → Dropout(0.5) → Dense(4, Softmax)
- **Input Shape**: `(224, 224, 3)` — BGR channel order (OpenCV convention)
- **Classes**: `['No Tumor', 'Pituitary Tumor', 'Meningioma Tumor', 'Glioma Tumor']`
- **Loss**: Sparse Categorical Cross-Entropy
- **Optimizer**: Adam

---

## ⚠️ Medical Disclaimer

> NeuroScan AI is a **research and educational tool** designed to assist medical professionals. All results are AI-generated and must be reviewed and confirmed by a qualified neurologist, radiologist, or neurosurgeon.  
> **Do NOT use this as a substitute for professional medical advice, diagnosis, or treatment.**

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m "feat: add my feature"`
4. Push to the branch: `git push origin feature/my-feature`
5. Open a Pull Request

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 👩‍💻 Author

**Deepthi** — [@deepthi-tr05](https://github.com/deepthi-tr05)

---

<p align="center">Built with ❤️ using TensorFlow, Flask, and Plotly</p>
