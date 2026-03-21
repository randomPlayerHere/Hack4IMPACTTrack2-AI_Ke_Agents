# Net Ninja — AI-Powered Network Intrusion Detection

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-0.100%2B-009688?style=flat-square&logo=fastapi&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.13%2B-FF6F00?style=flat-square&logo=tensorflow&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=flat-square)

Net Ninja is a full-stack **Network Intrusion Detection System (NIDS)** that uses a deep convolutional neural network (DCNN) to classify network traffic flows as either **BENIGN** or **ATTACK** in real time. You upload a CSV of network traffic logs, and it gives you back a risk assessment, attack rate, confidence scores, and a detailed breakdown — all through a dark-mode web dashboard.

It was built using the [CICIDS-2017](https://www.unb.ca/cic/datasets/ids-2017.html) dataset and trained on over 2 million labelled network flows.

---

##  Features

- **Upload & Analyze** — Drop a CSV of network flows and get results in seconds
- **DCNN Model** — A 1D convolutional neural network with batch normalization and dropout, quantized to TFLite for fast inference
- **Risk Assessment** — The backend computes a risk level (MINIMAL → CRITICAL) based on the percentage of malicious flows detected
- **Visual Dashboard** — Attack vs. Benign bar chart, confidence score histogram, and a detailed per-flow results table
- **Sample Dataset** — Hit "Try Sample Dataset" to run the model instantly without needing your own data
- **CSV Export** — Download your results as a clean CSV for further analysis
- **REST API** — Fully documented FastAPI backend with a `/health` endpoint and `/predict` for integrations

---

##  Project Structure

```
Hack4IMPACTTrack2-AI_Ke_Agents/
├── app.py                  # FastAPI backend — all API routes + inference logic
├── frontend/
│   ├── upload.html         # Upload page (served at GET /)
│   └── results.html        # Results dashboard (served at GET /results)
├── models/
│   ├── cicids_scaler.pkl       # Required scaler + expected feature schema
│   ├── nids_dcnn_model.tflite  # Quantized TFLite model (used by default)
│   └── nids_dcnn_model.h5      # Full Keras model (fallback)
├── data/
│   ├── raw/                # Raw CICIDS-2017 CSV files (not tracked in git)
│   └── processed/          # Preprocessed numpy arrays (not tracked in git)
├── src/
│   └── proprocessing.ipynb # Data exploration & preprocessing notebook
├── test.csv                # Sample network traffic file for demo purposes
├── requirements.txt        # Pip dependencies
├── pyproject.toml          # Project metadata + uv/pip-tools dependencies
└── .env.example            # Template for environment variables
```

---

##  Getting Started

### Prerequisites

- Python **3.10 or higher**
- A working internet connection (for CDN-loaded frontend assets like TailwindCSS)

### 1. Clone the repo

```bash
git clone https://github.com/randomPlayerHere/Hack4IMPACTTrack2-AI_Ke_Agents.git
cd Hack4IMPACTTrack2-AI_Ke_Agents
```

### 2. Set up a virtual environment

**Option A — using `pip` (standard)**

```bash
python -m venv .venv
source .venv/bin/activate       # On Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

**Option B — using `conda`**

```bash
conda create -n nids python=3.10
conda activate nids
pip install -r requirements.txt
```

**Option C — using `uv` (fastest)**

```bash
pip install uv
uv sync
```

### 3. Add your models

The trained model files are too large for Git. Place them in the `models/` directory:

```
models/
├── nids_dcnn_model.tflite   ← Required (or the .h5 fallback)
├── nids_dcnn_model.h5       ← Optional fallback if no .tflite
└── cicids_scaler.pkl        ← Required
```

> If you don't have the model files, contact the project maintainers or retrain using the preprocessing notebook in `src/`.

### 4. (Optional) Set up environment variables

```bash
cp .env.example .env
# Edit .env if you want to change the default host/port
```

### 5. Run the server

```bash
python app.py
```

The app will be available at **[http://localhost:8008](http://localhost:8008)**.

---

##  Using the App

1. Open your browser and go to `http://localhost:8008`
2. **Upload a CSV** of network traffic logs, or click **"Try Sample Dataset"** to use the included demo file
3. Wait a few seconds while the model runs inference
4. You'll be redirected to the **Results Dashboard** showing:
   - Risk level (MINIMAL / LOW / MEDIUM / HIGH / CRITICAL)
   - Total flows analyzed, attack rate, and average model confidence
   - Attack vs. Benign bar chart and confidence histogram
   - A per-flow details table (first 30 rows)
5. Hit **"Download Results as CSV"** to export the full prediction output

---

##  API Reference

All endpoints are served by the FastAPI backend. You can access the auto-generated docs at `http://localhost:8008/docs`.

| Method | Endpoint | Description |
|--------|----------|-------------|
| `HEAD` | `/` | Lightweight page check |
| `GET` | `/` | Serves the upload page |
| `GET` | `/results` | Serves the results dashboard |
| `GET` | `/health` | Returns model status and feature count |
| `GET` | `/sample-dataset` | Returns `test.csv` if present, otherwise generates a synthetic sample CSV |
| `POST` | `/predict` | Accepts a `.csv` file, returns JSON predictions |

### `/predict` — Example

```bash
curl -X POST http://localhost:8008/predict \
  -F "file=@your_traffic.csv"
```

**Response:**

```json
{
  "total_flows": 500,
  "n_benign": 430,
  "n_attack": 70,
  "attack_pct": 14.0,
  "avg_confidence": 97.3,
  "risk_level": "MEDIUM",
  "risk_message": "Moderate attack activity detected.",
  "results": [ ... ]
}
```

> **Note:** The API accepts CSVs with up to **10,000 rows** per request. The input schema must match the CICIDS-2017 feature set (78 features after dropping metadata columns).

### `/predict` response behavior (important limits)

- `results`: preview rows are capped at **30** entries (UI table)
- `csv_download`: downloadable rows are capped at **500** entries
- Risk is derived from attack percentage:
  - `> 75` → `CRITICAL`
  - `> 50` → `HIGH`
  - `> 25` → `MEDIUM`
  - `> 5`  → `LOW`
  - else   → `MINIMAL`

---

##  Model Details

| Property | Value |
|----------|-------|
| Architecture | 1D Deep Convolutional Neural Network (DCNN) |
| Dataset | CICIDS-2017 (Canadian Institute for Cybersecurity) |
| Classes | BENIGN, ATTACK |
| Input | 78 normalized network flow features |
| Inference Format | TFLite (default) with Keras H5 fallback |
| Preprocessing | Runtime cleaning (`Inf/-Inf -> NaN -> 0`) + fitted `cicids_scaler.pkl` transform |

The model treats each network flow as a 1D "signal" and learns spatial patterns across feature dimensions using stacked Conv1D layers, followed by dense layers for classification.

---

##  Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | [FastAPI](https://fastapi.tiangolo.com/) + [Uvicorn](https://www.uvicorn.org/) |
| ML Inference | [TensorFlow Lite](https://www.tensorflow.org/lite) / [Keras](https://keras.io/) |
| Data Processing | [Pandas](https://pandas.pydata.org/) + [NumPy](https://numpy.org/) + [scikit-learn](https://scikit-learn.org/) |
| Frontend | Vanilla HTML/CSS/JS + [TailwindCSS](https://tailwindcss.com/) (CDN) |
| Fonts & Icons | Google Fonts (Manrope, Inter) + Material Symbols |

---

##  Known Limitations

- **Max 10,000 rows per request** — larger files will be rejected. Split them up if needed.
- **CICIDS-2017 schema only** — the model was trained on a specific set of 78 features. CSVs with a different column schema will fail at the feature validation step.
- **TFLite inference is row-by-row** — for very large batches, the Keras H5 model will be faster if you have it available.
- **Preview/download caps** — API response table preview is limited to 30 rows, and CSV export payload is limited to 500 rows per request.
- **No authentication** — this is a demo/hackathon tool. Don't expose it publicly without adding auth.

---

##  Contributing

Contributions are welcome! If you want to improve the model, add new features, or fix bugs:

1. Fork the repo
2. Create a new branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add your feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

---

##  License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

---

> Built for Hack4IMPACT Track 2 · AI & Agents
