# Error-Analysis-of-Sepedi-Street-names-pronunciation-in-existing-navigation-system-

This repository contains the computational pipeline for evaluating and improving the pronunciation of Sepedi street names in commercial navigation systems (specifically Apple Maps). 

The project was developed for an Honours research report in Computing and compares the baseline TTS output against native speaker references, followed by the implementation of Rule-Based and Neural Grapheme-to-Phoneme (G2P) adaptation models.

## 📊 Dataset
*   **50 Sepedi street names** sourced from Gauteng municipal records.
*   **250 Human Reference Recordings** (5 native speakers × 50 names).
*   **50 Apple Maps TTS Outputs**.
*   **3,515 Phone-level alignment comparisons**.

## ⚙️ Pipeline Architecture

The pipeline is divided into three main phases:

### Phase 1: Data Processing & Error Analysis
*   `01_capture_audio.py`: Normalizes all audio to 16kHz mono WAV.
*   `02_phones.py`: Generates IPA transcriptions using two ASR models (Allosaurus & wav2vec2).
*   `03_align_classify.py`: Aligns system vs. human phonemes using Levenshtein distance and classifies errors using a custom Sepedi taxonomy.
*   `04_validate.py`: Validates the ASR pipeline using 4 layers (Recogniser agreement, hand-transcription, MFCC/DTW, and Whisper ASR).
*   `05_results.py`: Generates the baseline error taxonomy distribution and street-difficulty charts.

### Phase 2: Model Adaptation
*   `06_train_model.py`: Builds a Rule-Based G2P dictionary by extracting character-to-IPA mappings and injecting explicit Sepedi phonological rules (e.g., `kg` → `kʰ`).
*   `06b_train_neural.py`: Trains a PyTorch LSTM Neural G2P model on the training data.
*   `07_generate_tts.py` & `07b_generate_neural.py`: Uses eSpeak-NG to synthesize audio for the 10 held-out test streets using both models.

### Phase 3: Evaluation
*   `08_evaluate_metrics.py` & `08b_evaluate_neural.py`: Calculates Phoneme Error Rate (PER), Edit Distance, and Pronunciation Similarity, culminating in a 3-way comparison chart.

## 📈 Key Results

| Model | Average PER | Improvement vs Baseline |
| :--- | :--- | :--- |
| **Baseline (Apple Maps)** | 0.891 | - |
| **Rule-Based G2P** | 0.842 | **+4.9%** |
| **Neural LSTM G2P** | 0.894 | -0.3% |

*Note: The Rule-Based model successfully corrected English heuristics (e.g., the `kg` digraph). The Neural model suffered from data scarcity (trained on only 40 words), highlighting the need for larger Sepedi speech corpora in future work.*

## 🚀 Setup & Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/tebogosako/Error-Analysis-of-Sepedi-Street-names-pronunciation-in-existing-navigation-system.git
   cd Error-Analysis-of-Sepedi-Street-names-pronunciation-in-existing-navigation-system
