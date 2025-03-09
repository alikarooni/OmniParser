# OmniParser: Screen Parsing Tool for Pure Vision-Based GUI Agent (Forked)

<p align="center">
  <img src="imgs/logo.png" alt="OmniParser Logo" width="200">
</p>

This is a forked repository of Microsoft’s OmniParser, a powerful screen parsing tool for creating pure vision-based GUI agents. My fork includes additional setup guidance and a video tutorial to help you get started with chatting with Windows to automate tasks using OmniParser and large language models (LLMs) like OpenAI, DeepSeek, or Qwen.

## News
- [2025/2] Microsoft released OmniParser V2 with updated checkpoints ([huggingface.co/microsoft/OmniParser-v2.0](https://huggingface.co/microsoft/OmniParser-v2.0)).
- [2025/2] OmniTool introduced, enabling control of a Windows 11 VM with OmniParser ([Watch Video](https://1drv.ms/v/c/650b027c18d5a573/EehZ7RzY69ZHn-MeQHrnnR4BCj3by-cLLpUVlxMjF4O65Q?e=8LxMgX)).
- [2025/1] V2 achieves new state-of-the-art results (39.5%) on the Screen Spot Pro benchmark ([github.com/likaixin2000/ScreenSpot-Pro-GUI-Grounding](https://github.com/likaixin2000/ScreenSpot-Pro-GUI-Grounding/tree/main)).
- [2024/11] OmniParser V1.5 released with finer icon detection and interactability prediction.
- [2024/10] OmniParser was the #1 trending model on Hugging Face and achieved top performance on Windows Agent Arena.

## Video Tutorial: Setting Up OmniParser
Check out my step-by-step video tutorial on how to set up and use Microsoft OmniParser to chat with Windows and automate tasks:

[How to Set Up Microsoft OmniParser: Chat with Windows to Automate Tasks](https://youtu.be/a8Ab_92uKs4)

This tutorial covers installation, configuration, and running OmniParser with a Windows 11 VM in Docker.

---

## Quick Setup Guide

### 1- Clone the Repository
```bash
git clone https://github.com/microsoft/OmniParser
cd OmniParser
```

### 2-Create a Python Environment
```bash
python -m venv "omni"
.\omni\Scripts\Activate.ps1
```

### 3-Install Requirements
```bash
pip install -r requirements.txt
```

### 4-Download OmniParser V2 Weights from Hugging Face
Install Hugging Face CLI:
```bash
pip install "huggingface_hub[cli]" 
```
Download the weights:
```bash
$files = @(
    "icon_detect/train_args.yaml",
    "icon_detect/model.pt",
    "icon_detect/model.yaml",
    "icon_caption/config.json",
    "icon_caption/generation_config.json",
    "icon_caption/model.safetensors")

foreach ($f in $files) {
    huggingface-cli download microsoft/OmniParser-v2.0 $f --local-dir weights
}
```
Rename the icon_caption folder to icon_caption_florence:

```bash
mv weights/icon_caption weights/icon_caption_florence
```

### 5-Download Windows 11 ISO:
[Fill out the registration form](https://info.microsoft.com/ww-landing-windows-11-enterprise.html) and Download the Windows 11 Enterprise Evaluation (English, United States) ISO (~6GB).

Rename the file to custom.iso, and copy it to.

### 6-Install Docker Desktop
If not installed, download [Docker Desktop for Windows](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe)

### 7-Build and Run the Docker Image
Navigate to OmniParser/omnitool/omnibox/scripts and run:

```bash
./manage_vm.sh create`
```
This builds the Docker image (~26GB) and installs Windows 11 in the container (~1 hour). 
Follow the installation via [the web interface](http://localhost:8006/vnc.html?view_only=1&autoconnect=1&resize=scale)

### 8-Start OmniParser and Components
Run OmniParser server:
```bash
cd OmniParser/omnitool/omniparserserver
python -m omniparserserver
```

Start omnibox to control the Windows VM:
```bash
cd OmniParser/omnitool/omnibox/scripts
./manage_vm.sh start
```

Launch web UI:
```bash
cd OmniParser/omnitool/gradio
python app.py --windows_host_url localhost:8006 --omniparser_server_url localhost:8000
```

Note: Ensure you have an API key for your preferred LLM (e.g., OpenAI, DeepSeek, Qwen). For example, an OpenAI key format might look like: sk-proj-[YourKey]. Replace with your actual key in the configuration.

---

Resources: 
Official OmniParser Repository: https://github.com/microsoft/OmniParser.

OmniParser V2 Models: https://huggingface.co/microsoft/OmniParser-v2.0.

License:
The OmniParser model checkpoints on Hugging Face have specific licenses: icon_detect is under AGPL (inherited from YOLO), while icon_caption_blip2 and icon_caption_florence are under MIT. See the LICENSE file in each model folder for details.

Citation
If you use OmniParser, please cite the original work:
```
@misc{lu2024omniparserpurevisionbased,
      title={OmniParser for Pure Vision Based GUI Agent}, 
      author={Yadong Lu and Jianwei Yang and Yelong Shen and Ahmed Awadallah},
      year={2024},
      eprint={2408.00203},
      archivePrefix={arXiv},
      primaryClass={cs.CV},
      url={https://arxiv.org/abs/2408.00203}, 
}
```