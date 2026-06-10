# ECG Signal Filtering & Analysis

A Digital Signal Processing (DSP) project that applies FIR and IIR digital filters to real ECG signals from the MIT-BIH Arrhythmia Database, with automated heart rate and HRV analysis.

---

## Technologies Used
- Python 3
- NumPy
- SciPy
- Matplotlib
- WFDB (PhysioNet data reader)

---

## What I Built

### 1. Signal Loading
- Loaded real ECG signal from the **MIT-BIH Arrhythmia Database** (Record 100, sampled at 360 Hz)
- Signal contains baseline wander (low-frequency noise) and EMG muscle artifacts (high-frequency noise)

### 2. FFT Analysis
- Applied Fast Fourier Transform to analyze frequency content of the raw noisy signal
- Identified noise frequency bands to guide filter design

### 3. Filter Design (Cascaded Bandpass: 0.5 – 40 Hz)
| Filter | Type | Order | Method |
|--------|------|-------|--------|
| FIR | Bandpass | 101 | Hamming window |
| IIR | Bandpass | 4 | Butterworth |

- Both filters implemented using **zero-phase filtering** (`filtfilt` / `sosfiltfilt`) to eliminate phase distortion
- Zero-phase filtering ensures accurate R-peak detection and preserves ECG waveform morphology

### 4. Filter Analysis
- Magnitude and phase response comparison (FIR vs IIR)
- Group delay analysis: FIR → constant delay, IIR → variable delay

### 5. ECG Signal Analysis After Filtering
- **R-peak detection** using `scipy.signal.find_peaks`
- **Heart Rate (HR)** calculation from RR intervals
- **HRV metrics**: SDNN and RMSSD
- **Arrhythmia detection**: beats with RR interval deviating >25% from mean flagged as abnormal

---

## Results

| Metric | FIR Filter | IIR Filter |
|--------|-----------|-----------|
| SNR | **11.89 dB** | 1.03 dB |
| Average Heart Rate | 74.02 BPM | 74.02 BPM |
| Abnormal Beats | 0 (0.00%) | 0 (0.00%) |
| Phase Response | Linear ✅ | Non-linear ⚠️ |

- FIR filter achieved significantly higher SNR and linear phase response — ideal for clinical ECG analysis
- IIR filter is computationally efficient (order 4 vs 101) and suitable for rhythm-only analysis
- Zero-phase filtering corrected IIR phase distortion, resulting in identical clinical timing accuracy for both filters

---

## How to Run

### Install dependencies
```bash
pip install numpy scipy matplotlib wfdb
```

### Run the script
```bash
python ecg_filtering.py
```

> The script automatically downloads MIT-BIH Record 100 from PhysioNet via the `wfdb` library.

---

## Project Structure
```
ecg-signal-filtering/
│
├── ecg_filtering.py       # Main script
├── README.md
└── outputs/               # Generated plots (auto-created)
```

---

## References
- MIT-BIH Arrhythmia Database — PhysioNet
- Oppenheim & Schafer, *Digital Signal Processing*
- Proakis, *Digital Signal Processing*

---

*DSP Project — Faculty of Engineering, Suez Canal University | Supervised by Dr. Doaa Gamal*
