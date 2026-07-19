# Vocal Separation using BS-Roformer

This repository contains a Jupyter Notebook designed for high-quality audio source separation (extracting vocals and instrumentals) using the state-of-the-art **BS-Roformer** model.

## Overview

The separation is powered by the [bs-roformer-infer](https://github.com/lucidrains/bs-roformer) package, utilizing the **Vocals Resurrection** model checkpoint by Unwa, which delivers premium-quality vocal isolation from mixed tracks.

## Features

- **High-Fidelity Vocal Separation**: Isolates vocals and instrumentals with minimal artifacts.
- **Automated Workflow**: From package installation, model downloading, audio preprocessing (converting to WAV via FFmpeg), to running separation inference.
- **Easy Integration**: Ready to run on cloud platforms like Kaggle or Google Colab (with GPU support).

## Prerequisites

- **Python 3.8+**
- **FFmpeg**: Required for audio decoding/encoding and converting files to standard 44.1kHz WAV.
- **GPU (Optional but Recommended)**: Significantly speeds up the inference process.

## Supported Models

You can download different pre-trained models depending on your target audio separation task. Below is the list of available models:

| Model ID (for download command) | Category | Checkpoint File | Description / Author |
| :--- | :--- | :--- | :--- |
| `roformer-model-bs-roformer-sw-by-jarredou` | multi-stem | `BS-Rofo-SW-Fixed.ckpt` | Multi-stem separator by jarredou |
| `roformer-model-bs-roformer-chorus-male-female-by-sucial` | vocals | `model_chorus_bs_roformer_ep_267_sdr_24.1275.ckpt` | Chorus separation by Sucial |
| `roformer-model-bs-roformer-instrumental-resurrection-by-unwa` | instrumental | `bs_roformer_instrumental_resurrection_unwa.ckpt` | Instrumental separation by unwa |
| `roformer-model-bs-roformer-male-female-by-aufr33` | vocals | `bs_roformer_male_female_by_aufr33_sdr_7.2889.ckpt` | Male-Female vocal separation by aufr33 |
| `roformer-model-bs-roformer-vocals-resurrection-by-unwa` | vocals | `bs_roformer_vocals_resurrection_unwa.ckpt` | High-quality vocals separation by unwa |
| `roformer-model-bs-roformer-vocals-revive-v2-by-unwa` | vocals | `bs_roformer_vocals_revive_v2_unwa.ckpt` | Vocals Revive V2 by unwa |
| `roformer-model-bs-roformer-vocals-revive-v3e-by-unwa` | vocals | `bs_roformer_vocals_revive_v3e_unwa.ckpt` | Vocals Revive V3e by unwa |
| `roformer-model-bs-roformer-vocals-revive-by-unwa` | vocals | `bs_roformer_vocals_revive_unwa.ckpt` | Vocals Revive by unwa |
| `roformer-model-bs-roformer-vocals-by-gabox` | vocals | `bs_roformer_vocals_gabox.ckpt` | Vocals separation by Gabox |
| `roformer-model-bs-roformer-de-reverb` | dereverb | `deverb_bs_roformer_8_384dim_10depth.ckpt` | De-reverb model |

To download any of the models above, use the command:
```bash
bs-roformer-download --model <model-id> --output-dir models
```

---

## Getting Started & Installation

### 1. Clone the Repository
```bash
git clone https://github.com/conghoa004/REMOVE_VOICE.git
cd REMOVE_VOICE
```

### 2. Install Dependencies
Install the `bs-roformer-infer` Python package:
```bash
pip install bs-roformer-infer
```

### 3. Install FFmpeg
- **Ubuntu/Debian**:
  ```bash
  sudo apt-get update && sudo apt-get install -y ffmpeg
  ```
- **macOS (via Homebrew)**:
  ```bash
  brew install ffmpeg
  ```
- **Windows**: Download the binaries from the official site or install via Scoop/Choco:
  ```powershell
  choco install ffmpeg
  ```

---

## Step-by-Step Usage (from the Notebook)

You can run the steps interactively using [remove-voice.ipynb](remove-voice.ipynb):

1. **Download the pre-trained BS-Roformer model:**
   ```bash
   bs-roformer-download --model roformer-model-bs-roformer-vocals-resurrection-by-unwa --output-dir models
   ```

2. **Download custom audio file from Google Drive (Optional):**
   You can download a custom audio file from Google Drive using `gdown`:
   ```python
   import gdown
   gdown.download("https://drive.google.com/uc?id=<file-id>", quiet=False)
   ```

3. **Prepare your input audio:**
   Convert your target song (e.g. `.mp3`) to a stereo `.wav` file at a 44100Hz sample rate:
   ```bash
   ffmpeg -i "path/to/your/song.mp3" -ar 44100 -ac 2 "input/song.wav"
   ```

4. **Run inference:**
   Perform vocal separation:
   ```bash
   bs-roformer-infer \
     --config_path models/roformer-model-bs-roformer-vocals-resurrection-by-unwa/config_bs_roformer_vocals_resurrection_unwa.yaml \
     --model_path models/roformer-model-bs-roformer-vocals-resurrection-by-unwa/bs_roformer_vocals_resurrection_unwa.ckpt \
     --input_folder input \
     --store_dir output
   ```

Outputs will be saved in the `output/` directory, containing:
- `song_vocals.wav` (Isolated voice)
- `song_instrumental.wav` (Instrumental backing track)

## Project Structure

- [remove-voice.ipynb](remove-voice.ipynb): Jupyter Notebook containing step-by-step pipeline.
- [README.md](README.md): Project documentation and guidelines.

## License

This project is open-source. Please check the license of [BS-Roformer](https://github.com/lucidrains/bs-roformer) and the respective models before commercial use.
