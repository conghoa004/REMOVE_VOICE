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
- **GPU (Optional but Recommended)**: Significantly speeds up the inference process. If you are running this on Kaggle, it is highly recommended to enable the **GPU T4** accelerator.

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
- **Windows**: Open **PowerShell as Administrator** and install FFmpeg using Winget:
  ```powershell
  winget install Gyan.FFmpeg
  ```

---

## Step-by-Step Usage (from the Notebook)

You can run the steps interactively using [remove-voice.ipynb](remove-voice.ipynb).

Before running the notebook, place or upload your source audio file at `input/input_music.mp3`. The notebook no longer downloads audio from Google Drive, and the conversion step expects this exact path.

Run the notebook cells in order:

1. **Install the BS-Roformer inference library:**
   ```bash
   pip install -q bs-roformer-infer
   ```

2. **List available pre-trained models:**
   ```bash
   bs-roformer-download --list-models
   ```

3. **Download the Vocals Resurrection model:**
   ```bash
   bs-roformer-download --model roformer-model-bs-roformer-vocals-resurrection-by-unwa --output-dir models
   ```

4. **Install FFmpeg:**
   ```bash
   apt-get -qq install ffmpeg
   ```

5. **Convert the input audio to WAV:**
   Convert `input/input_music.mp3` to stereo WAV at a 44100Hz sample rate:
   ```bash
   ffmpeg -i input/input_music.mp3 -ar 44100 -ac 2 input/input_music.wav
   ```

6. **Run source separation inference:**
   ```bash
   bs-roformer-infer --config_path /kaggle/working/models/roformer-model-bs-roformer-vocals-resurrection-by-unwa/config_bs_roformer_vocals_resurrection_unwa.yaml --model_path /kaggle/working/models/roformer-model-bs-roformer-vocals-resurrection-by-unwa/bs_roformer_vocals_resurrection_unwa.ckpt --input_folder /kaggle/working/input --store_dir /kaggle/working/output
   ```

Outputs will be saved in the `output/` directory.

## Multi-Instrument Separation

Use [separate-instruments.ipynb](separate-instruments.ipynb) to separate a track into multiple stems. It uses the recommended BS-RoFormer-SW 6-stem model by default (vocals, drums, bass, guitar, piano, and other). For more source categories, the notebook also documents the optional Mega 53-stem model, which needs about 16 GB of VRAM and may produce weaker individual stems.

By default, place `input_music.mp3` or `input_music.wav` in `input/`; WAV files are used directly, while other formats are converted to 44.1 kHz stereo. The notebook saves separated WAV files under `output/instruments/`.

### Directory Structure & Execution Results

After running the pipeline, your workspace folders will contain the following files:

```
REMOVE_VOICE/
├── input/
│   ├── input_music.mp3                 # Source audio provided before running the notebook
│   └── input_music.wav                 # Preprocessed WAV audio (~36.6 MB)
└── output/
    ├── input_music_instrumental.wav    # Isolated backing track (~75.0 MB)
    └── input_music_vocals.wav          # Isolated vocal stem (~75.0 MB)
```

- **`input_music.mp3`**: The source audio file (original mix), provided before running the notebook.

  [![Listen to Original Mix](https://img.shields.io/badge/Listen_to-Original_Mix-green?style=for-the-badge&logo=googleplaymusic)](./input/input_music.mp3)

- **`input_music_vocals.wav`**: Contains the extracted vocal track (singer's voice).

  [![Listen to Vocals](https://img.shields.io/badge/Listen_to-Vocals-blue?style=for-the-badge&logo=soundcharts)](./output/input_music_vocals.wav)

- **`input_music_instrumental.wav`**: Contains the backing instrumental track (music only).

  [![Listen to Instrumental](https://img.shields.io/badge/Listen_to-Instrumental-orange?style=for-the-badge&logo=itunes)](./output/input_music_instrumental.wav)

## Project Structure

- [remove-voice.ipynb](remove-voice.ipynb): Jupyter Notebook containing step-by-step pipeline.
- [separate-instruments.ipynb](separate-instruments.ipynb): Notebook for multi-instrument stem separation.
- [README.md](README.md): Project documentation and guidelines.

## License

This project is open-source. Please check the license of [BS-Roformer](https://github.com/lucidrains/bs-roformer) and the respective models before commercial use.
