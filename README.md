ECG Signal Filtering & Analysis

A Digital Signal Processing (DSP) project that applies FIR and IIR digital filters to real ECG signals from the MIT-BIH Arrhythmia Database, with automated heart rate and HRV analysis.

──────────────────────────────────────────────

Technologies Used

• Python 3
• NumPy
• SciPy
• Matplotlib
• WFDB (PhysioNet Data Reader)

──────────────────────────────────────────────

What I Built

1. Signal Loading

• Loaded real ECG signals from the MIT-BIH Arrhythmia Database (Record 100, sampled at 360 Hz).
• The signal contains baseline wander (low-frequency noise) and EMG muscle artifacts (high-frequency noise).

2. FFT Analysis

• Applied Fast Fourier Transform (FFT) to analyze the frequency content of the raw ECG signal.
• Identified noise frequency bands to guide filter design.

3. Filter Design (Cascaded Bandpass: 0.5–40 Hz)

FIR Filter
• Type: Bandpass
• Order: 101
• Design Method: Hamming Window

IIR Filter
• Type: Bandpass
• Order: 4
• Design Method: Butterworth

• Both filters were implemented using zero-phase filtering (filtfilt / sosfiltfilt) to eliminate phase distortion.
• Zero-phase filtering preserves ECG waveform morphology and ensures accurate R-peak detection.

4. Filter Analysis

• Compared magnitude and phase responses of FIR and IIR filters.
• Performed group delay analysis:
  - FIR → Constant delay
  - IIR → Variable delay

5. ECG Signal Analysis After Filtering

• Detected R-peaks using scipy.signal.find_peaks().
• Calculated Heart Rate (HR) from RR intervals.
• Computed HRV metrics:
  - SDNN
  - RMSSD
• Implemented simple arrhythmia detection:
  - Beats with RR intervals deviating by more than 25% from the mean RR interval were flagged as abnormal.

──────────────────────────────────────────────

Results

Metric                          FIR Filter      IIR Filter
----------------------------------------------------------
SNR                             11.89 dB        1.03 dB
Average Heart Rate              74.02 BPM       74.02 BPM
Abnormal Beats                  0 (0.00%)       0 (0.00%)
Phase Response                  Linear ✓        Non-linear ⚠

Key Findings

• The FIR filter achieved significantly higher SNR and maintained a linear phase response, making it ideal for clinical ECG analysis.
• The IIR filter is computationally efficient (Order 4 vs. Order 101) and suitable for rhythm-monitoring applications.
• Zero-phase filtering corrected IIR phase distortion, resulting in identical clinical timing accuracy for both filters.

──────────────────────────────────────────────

How to Run

Install Dependencies

pip install numpy scipy matplotlib wfdb

Run the Script

python ecg_filtering.py

The script automatically downloads MIT-BIH Record 100 from PhysioNet using the WFDB library.

──────────────────────────────────────────────

Project Structure

ecg-signal-filtering/
│
├── ecg_filtering.py      # Main Script
├── README.md
└── outputs/              # Generated Plots

──────────────────────────────────────────────

References

• MIT-BIH Arrhythmia Database (PhysioNet)
• Oppenheim & Schafer – Digital Signal Processing
• Proakis – Digital Signal Processing

──────────────────────────────────────────────

DSP Project
Faculty of Engineering, Suez Canal University | Supervised by Dr. Doaa Gamal
