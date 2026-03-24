<div align="center">

  <h1>🛸 Inter-IIT Tech Meet — ISRO Challenge</h1>
  <h3>Lunar Elemental Abundance Estimation from Chandrayaan-2 XRF Spectra</h3>

  <p>
    A machine learning pipeline to predict the weight percentage of lunar surface elements — Magnesium (Mg) and Silicon (Si) — from X-Ray Fluorescence (XRF) spectral data captured by ISRO's CLASS instrument aboard Chandrayaan-2, and generate spatial elemental abundance maps of the Moon.
  </p>

  ![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)
  ![Jupyter](https://img.shields.io/badge/Jupyter-Notebooks-orange?logo=jupyter)
  ![ISRO](https://img.shields.io/badge/Data-Chandrayaan--2%20CLASS-darkblue)
  ![Inter-IIT](https://img.shields.io/badge/Competition-Inter--IIT%20Tech%20Meet-red)

</div>

---

## 📖 Overview

This project was developed for the **Inter-IIT Tech Meet**, where the problem statement was issued by **ISRO (Indian Space Research Organisation)**. The task was to process XRF spectral data from the **CLASS (Chandrayaan-2 Large Area Soft X-ray Spectrometer)** instrument and build ML models capable of estimating the elemental composition of the lunar surface.

X-Ray Fluorescence spectroscopy works by measuring the characteristic X-rays emitted by elements on the Moon's surface when illuminated by solar X-rays. The intensity and shape of peaks in the spectrum are directly related to the concentration of each element. The challenge is extracting accurate weight percentages from noisy, real space instrument data.

---

## 🎯 What the Project Does

- Ingests raw XRF spectral data from Chandrayaan-2 in **FITS format** (standard astronomical data format)
- Applies **addnorm corrections** and **spectra fitting** to calibrate the raw instrument readings
- Trains ML models to predict **true weight percentages** of Mg and Si from fitted spectral features
- Generates **spatial elemental abundance maps** of the lunar surface by applying predictions across orbital scan data

---

## 🗂️ Repository Structure

```
Inter-IIT---ISRO/
│
├── Xrf_train_V1.ipynb                        # Main XRF model training pipeline
├── Xrf_train_V1 (1).ipynb                    # Iterated version of training notebook
├── addnorm_calculation_nd_spectrafitting.ipynb  # Spectral calibration & Gaussian fitting
├── elemental maps.ipynb                      # Spatial elemental map generation
├── final.fits.ipynb                          # FITS file processing & exploration
├── pred.ipynb                               # Prediction & inference notebook
│
├── true_wt_mg_model_final_v4.pkl            # Trained model — Mg weight % prediction
├── true_wt_si_model_final_v4.pkl            # Trained model — Si weight % prediction
│
├── ISRO.data_collection_v4.json             # Structured data collection metadata
└── data/                                    # Raw and processed spectral data
```

---

## 🔬 Pipeline Breakdown

### Step 1 — FITS File Processing (`final.fits.ipynb`)

- Reads raw Chandrayaan-2 CLASS instrument data stored in **FITS (Flexible Image Transport System)** format — the standard format for astronomical data
- Extracts spectral counts, energy channels, timestamps, and orbital metadata
- Converts the instrument-level data into a workable DataFrame for downstream processing

---

### Step 2 — Addnorm Calculation & Spectra Fitting (`addnorm_calculation_nd_spectrafitting.ipynb`)

This is the most scientifically critical step.

**Addnorm Calculation:**
- XRF spectra need to be normalised against the incoming solar X-ray flux — because the intensity of fluorescence depends on how bright the Sun is at the time of measurement
- Addnorm (additional normalisation) factors are computed to correct for this variation, making spectra from different orbital passes comparable

**Spectra Fitting:**
- Each XRF spectrum contains characteristic peaks for elements like Mg, Si, Al, Ca, Fe
- **Gaussian functions** are fitted to each elemental peak to extract: peak position, peak amplitude, and full-width at half-maximum (FWHM)
- These fitted parameters become the features fed into the ML model — because the raw spectral counts are noisy, but the fitted peak properties are much more stable and informative

---

### Step 3 — Model Training (`Xrf_train_V1.ipynb`)

- Uses the fitted spectral features (peak amplitudes, areas, ratios) as inputs
- Trains separate regression models for each element:
  - **`true_wt_mg_model_final_v4.pkl`** — predicts Magnesium weight %
  - **`true_wt_si_model_final_v4.pkl`** — predicts Silicon weight %
- Ground truth labels come from known lunar sample compositions (Apollo samples and lunar meteorite databases) matched to orbital positions
- Models are serialised with pickle for deployment in inference notebooks

---

### Step 4 — Prediction (`pred.ipynb`)

- Loads the trained model `.pkl` files
- Applies them to new/unseen XRF spectra
- Outputs predicted weight percentages for Mg and Si at each orbital scan location

---

### Step 5 — Elemental Maps (`elemental maps.ipynb`)

- Takes predictions across all orbital scan positions
- Maps each prediction to its corresponding lunar latitude/longitude using orbital metadata from the FITS files
- Generates **2D spatial abundance maps** — colour-coded visualisations showing how Mg and Si concentrations vary across the lunar surface
- These maps are directly comparable to reference geochemical maps from previous lunar missions

---

## 🌕 Why This Problem Is Hard

- XRF spectra from orbit are **extremely noisy** — the CLASS instrument captures weak fluorescence signals mixed with solar background radiation
- Solar X-ray flux varies constantly, so spectra taken at different times are not directly comparable without careful normalisation
- The Moon has no atmosphere, so the instrument sees a mix of surface geology and scattered radiation
- Ground truth labels are sparse — only regions overflown during calibration passes with known composition can be used for training
- The model must generalise from limited labelled data to the entire lunar surface

---

## 📦 Key Libraries & Tools

| Library | Purpose |
|---|---|
| `astropy` | FITS file reading and astronomical data handling |
| `scipy` | Gaussian curve fitting for spectral peak extraction |
| `scikit-learn` | Regression model training and evaluation |
| `numpy` / `pandas` | Numerical computation and data handling |
| `matplotlib` | Spectral plots and elemental map visualisation |

---

## 🚀 Running the Pipeline

### 1. Clone the Repository

```bash
git clone https://github.com/BayesianDecoder/Inter-IIT---ISRO.git
cd Inter-IIT---ISRO
```

### 2. Install Dependencies

```bash
pip install astropy scipy scikit-learn numpy pandas matplotlib
```

### 3. Run Notebooks in Order

```
1. final.fits.ipynb                          → Process raw FITS data
2. addnorm_calculation_nd_spectrafitting.ipynb → Calibrate & fit spectra
3. Xrf_train_V1.ipynb                        → Train Mg and Si models
4. pred.ipynb                               → Run predictions
5. elemental maps.ipynb                      → Generate surface maps
```

---

## 🏆 Context

This project was submitted as part of the **Inter-IIT Tech Meet**, one of India's most prestigious inter-college technical competitions, where IITs compete on real industry and government problem statements. The ISRO problem statement required teams to work with actual satellite mission data from Chandrayaan-2 and produce scientifically meaningful outputs — making this a research-grade ML application, not just a standard data science exercise.

---

<div align="center">
  Made for Inter-IIT Tech Meet · ISRO Problem Statement · <a href="https://github.com/BayesianDecoder">BayesianDecoder</a>
</div>
