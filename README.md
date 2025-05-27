# EEG Signal Compression with Temporal-Modeling Autoencoders  
> High-fidelity, segment-wise compression of multi-channel EEG using LSTM, GRU & RNN encoders.

[![Python](https://img.shields.io/badge/python-3.8%2B-blue)](https://www.python.org/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

## Project Motivation
Continuous EEG monitoring produces gigabytes of data in a matter of hours. Efficient compression **must** (1) cut storage and transmission cost and (2) preserve diagnostically relevant features (spikes, rhythms). This project explores **segment-wise autoencoders** with temporal modeling blocks (LSTM, GRU, vanilla RNN) and compares their reconstruction quality at multiple compression ratios.

## Methodology
1. **Pre-processing**  
   * Butterworth \(1–40 Hz\) + notch \(50 Hz\) filtering  
   * Min-max normalization to (-10, 10)  
   * Fixed-length window segmentation (L = 16 or 32)

2. **Model Variants**  
   | Notebook | Encoder-Decoder Core | Latent Dim | DRF Tested |
   |----------|---------------------|------------|------------|
   | `EEG_Dense+LSTM_3.ipynb` | Dense → **LSTM** → Dense | 128 | 2, 4, 8, 16 |
   | `EEG_Dense+GRU_2.ipynb`  | Dense → **GRU**  → Dense | 128 | 2, 4, 8, 16 |
   | `EEG_Dense+RNN_2.ipynb`  | Dense → **RNN**  → Dense | 128 | 2, 4, 8 |

3. **Metrics**  
   * **SSIM** – structural similarity between original & reconstructed windows  
   * **Signal-loss %** – relative MSE w.r.t. original amplitudes

## Repository Structure
```text
├── EEG_Dense+LSTM_3.ipynb
├── EEG_Dense+GRU_2.ipynb
├── EEG_Dense+RNN_2.ipynb
├── README.md
└── requirements.txt
```

## Quick Start
1. Clone repo & install dependencies
```bash
git clone https://github.com/RuchiShah2233/major-project.git
cd eeg-compression
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
```
2. Download CHB-MIT EEG (or your own) into data/raw - 
[https://physionet.org/content/chbmit/1.0.0/](https://physionet.org/content/chbmit/1.0.0/)
4. Launch notebooks
```bash
jupyter lab notebooks/EEG_Dense+GRU_2.ipynb
```
> **Tip:** Each notebook is self-contained – change DATA_DIR at the top to point to your dataset.

## Results

| Model | DRF 2 (50% CR) | DRF 4 (75% CR) | DRF 8 (87.5% CR) |
|-------|----------------|----------------|------------------|
| **LSTM** | SSIM = 0.99, loss = 0.6 % | SSIM ≈ 0.87, loss ≈ 13 % | SSIM ≈ 0.80, loss ≈ 20 % |
| **GRU**  | SSIM = 0.998, loss = 0.2 % | SSIM = 0.946, loss = 5.4 % | SSIM ≈ 0.83, loss ≈ 17 % |
| **RNN**  | SSIM ≈ 0.97, loss ≈ 3 %    | SSIM ≈ 0.34, loss ≈ 66 % | — |

**Note:** DRF = Distortion Rate Factor; CR = Compression Ratio

## Datasets

- **[CHB-MIT Scalp EEG](https://physionet.org/content/chbmit/1.0.0/)** — Pediatric epilepsy dataset from PhysioNet  
- **[SJTU SEED EEG](https://bcmi.sjtu.edu.cn/home/seed/)** *(optional)* — Emotion-elicited EEG dataset
