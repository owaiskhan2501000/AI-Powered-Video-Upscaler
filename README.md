# 🎥 AI-Powered Video Upscaler 

![Jupyter Notebook](https://img.shields.io/badge/Made_with-Jupyter-F37626.svg?style=for-the-badge&logo=Jupyter)
![Python](https://img.shields.io/badge/Python-3.x-blue.svg?style=for-the-badge&logo=python)
![Real-ESRGAN](https://img.shields.io/badge/AI_Model-Real--ESRGAN-brightgreen)
![FFmpeg](https://img.shields.io/badge/Tools-FFmpeg-black)

A customized, fully automated Google Colab pipeline for upscaling videos up to 8K resolution using **Real-ESRGAN**. This notebook provides a robust workflow that handles audio-visual synchronization issues by using Constant Frame Rate (CFR) conversion and seamlessly pads upscaled videos to perfectly match target resolutions.

## ✨ Features

- **High-Quality Upscaling:** Support for upscaling up to FHD (1080p), 2K, 4K, and 8K (7680x4320) or specific multipliers (2x, 3x, 4x).
- **Multiple AI Models:** Choose from powerful pre-trained models like `RealESRGAN_x4plus`, `RealESRGAN_x4plus_anime_6B`, and `realesr-animevideov3` based on your content type (e.g., standard vs anime).
- **Audio/Video Sync Protection:** Converts the input video to a Constant Frame Rate (CFR) prior to upscaling, preventing the desync issues typical in AI video processing.
- **Automated Dependency Fixes:** Automatically patches the known `torchvision` import bug (`rgb_to_grayscale`) inside the `basicsr` library, meaning zero manual intervention is needed for dependencies.
- **VRAM Optimization:** Uses configurable tile sizes (`--tile 512`) to prevent Out-Of-Memory (OOM) errors on Google Colab GPUs (T4/L4/A100).
- **Google Drive Integration:** Optionally mount Google Drive to directly load input files and save massive 8K output files seamlessly.
- **Exact Resolution Padding:** After Real-ESRGAN runs, FFmpeg automatically pads the video to exactly match your requested target resolution.

## 🚀 Getting Started

### 1. Open in Google Colab
You can run this pipeline directly in your browser without any local GPU requirements:
1. Upload the `AI_Powered_Video_Upscaler.ipynb` notebook to your Google Colab account, or clone this repo directly into Colab.
2. Go to **Runtime > Change runtime type** and ensure **Hardware accelerator** is set to **GPU** (T4 or better).

### 2. Pipeline Execution Steps

The notebook is divided into three simple execution cells:

*   **Cell 1: Environment Setup:** 
    Clones the [Real-ESRGAN](https://github.com/xinntao/Real-ESRGAN) repository, installs required dependencies (`torch`, `basicsr`, `ffmpeg`, etc.), and builds the environment.
*   **Cell 2: Library Patching:** 
    Automatically patches the `degradations.py` file within the `basicsr` installation to fix an outdated `torchvision` import error. 
*   **Cell 3: Video Upscaling:** 
    The core upscaling engine. Here you can configure your video settings.

### 3. Configuration Options (Cell 3)

| Option | Description |
| :--- | :--- |
| `video_path` | The path to your input video (e.g., `/content/input.mp4`). |
| `output_dir` | Where the final video should be saved. |
| `resolution` | Desired output quality (e.g., FHD, 2K, 4K, 8K, or 2x/3x/4x multipliers). |
| `model` | Select the Real-ESRGAN model weight. Use `...anime_6B` for animations. |
| `tile_size` | Chunk size for processing. Default is `512`. Lower this (e.g., `256`) if you hit GPU memory limits. |
| `mount_drive` | Set to `True` to automatically save the upscaled output to your Google Drive (`MyDrive/Upscaled_Videos_REAL_ESRGAN`). |
| `target_fps` | Set the output target framerate (e.g., `24`, `30`, `60`). |

## 🛠️ How It Works (Under the Hood)

1. **Information Extraction:** FFprobe reads the input video streams and identifies the original dimensions and duration.
2. **CFR Conversion:** The video is standardized via FFmpeg to a Constant Frame Rate (e.g., 24fps) using a very fast libx264 preset to guarantee stability.
3. **AI Upscaling:** The `inference_realesrgan_video.py` script runs on the prepared CFR video, multiplying the frame resolution based on the `outscale` parameters.
4. **Rescaling & Padding:** FFmpeg takes the raw Real-ESRGAN output and adds precise Lanczos scaling and black padding to accurately meet standard broadcast dimensions without aspect-ratio distortion.
5. **Cleanup:** Temporary working directories (`realesrgan_work_*`) are cleared out to save Colab disk space.

## 🤝 Acknowledgments

* [Real-ESRGAN](https://github.com/xinntao/Real-ESRGAN) by xinntao for the core image/video upscaling models.
* [FFmpeg](https://ffmpeg.org/) for the powerful media framework handling the heavy lifting of padding and CFR conversion.

## 📄 License

This project utilizes Real-ESRGAN. Please refer to their repository for specific licensing regarding the upscaling models.
