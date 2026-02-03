# eIQ GenAI Flow 2.0 on NXP i.MX95

**Version:** 1.0
**Release Date:** Feb 2026 
**Copyright:** © 2026 Advantech Corporation. All rights reserved.

## Overview

The `eIQ GenAI Flow 2.0 on NXP i.MX95` container image provides a comprehensive environment for building and deploying AI applications on NXP i.MX 95 hardware. This container features full hardware acceleration support, optimized AI frameworks, and industrial-grade reliability. With this container, developers can quickly prototype and deploy AI use cases such as computer vision without the burden of solving time-consuming dependency issues or manually setting up complex toolchains. All required runtimes, libraries, and drivers are pre-configured, ensuring seamless integration with NXP AI acceleration stack.

## Key Features
- **eIQ® GenAI Flow 2.0:** The eIQ® GenAI Flow integrates multiple AI technologies to create a seamless HMI experience.

- **Edge AI Capabilities:** Optimized support for LLM and ASR leveraging NXP NPU acceleration.

- **Hardware Acceleration:** Direct passthrough access to NPU hardware ensures high-performance and low-latency inference with minimal power consumption.

- **Preconfigured Environment:** Eliminates time-consuming setup by bundling drivers, toolchains, and AI libraries, so developers can focus directly on building applications.

- **Rapid Prototyping & Deployment:** Ideal for quickly testing AI models, validating PoCs, and deployment without rebuilding from scratch.


## Hardware Specifications

| Component       | Specification      |
|-----------------|--------------------|
| Target Hardware | [Advantech AOM-5521](https://www.advantech.com/en/https://www.advantech.com/zh-tw/products/77b59009-31a9-4751-bee1-45827a844421/aom-5521/mod_75b36e99-ac3f-4801-8b2b-1706ade1025d) |
| SoC             | [NXP i.MX95](https://www.nxp.com/products/I.MX95?tab=Documentation_Tab)   |
| GPU             | Arm® Mali™ G310         |
| NPU             | eIQ neutron N3-1034S    |
| Memory          | 8 GB LPDDR5             |

## Operating System

| Environment        | Operating System                                    |
|--------------------|-----------------------------------------------------|
| **Device Host**    | Yocto 5.2 Walnascar                                 |
| **Container**      | Ubuntu:24.04                                        |
| **Base Container** | [NXP-iMX95-Neutron-Passthrough Container](https://github.com/Advantech-EdgeSync-Containers/Neutron-NPU-Passthrough-on-NXP-i.mx95)            |


## Software Components

| Component         | Version   | Description                                               |
|-------------------|-----------|-----------------------------------------------------------|
| eIQ GenAI Flow    | 2.0       |                                                           |
| Python            | 3.13      | Python runtime for building applications                  |
| ONNX Runtime      | 1.22.0 from NXP BSP v6.12.20  | Inference runtime |

## Supported AI Capabilities

### ASR Models

| Model                         | Format    | Note                                  |
|-------------------------------|-----------|---------------------------------------|
| moonshine-tiny                | .onnx     | Provide By NXP eIQ GenAI Flow         |                                      
| moonshine-base                | .onnx     | Provide By NXP eIQ GenAI Flow         |
| whisper-small.en              | .onnx     | Provide By NXP eIQ GenAI Flow         |

### LLM Models

| Model                         | Format    | Note                                  |
|-------------------------------|-----------|---------------------------------------|
| danube-500M-q4                | .onnx     | Provide By NXP eIQ GenAI Flow         |                                      
| danube-500M-q8                | .onnx     | Provide By NXP eIQ GenAI Flow         |

### TTS Models
| Model                         | Format    | Note                                  |
|-------------------------------|-----------|---------------------------------------|
| vits-english                  | .onnx     | Provide By NXP eIQ GenAI Flow         | 

* The models provide by NXP eIQ GenAI Flow is encrypted .

## Hardware Acceleration Support

| Accelerator | Support Level | Compatible Libraries |
|-------------|---------------|----------------------|
| NPU         |  INT8 (primary), INT4  | NXP eIQ GenAI Flow |

### Precision Support

| Precision  | Support Level | Notes |
|------------|---------------|-------|
| INT8       | NPU,CPU       |Primary mode for NPU acceleration, best performance-per-watt |

## Repository Structure
```
eIQ-GenAI-Flow2.0-Container-on-NXP-i.mx95/
├── README.md                               # Overview and quick start steps
├── run.sh                                  # Script to launch the container
└── docker-compose.yml                      # Configuration file of docker compose
```

## Quick Start Guide

### Prerequisites
* Please ensure docker & docker compose are available and accessible on device host OS
* Since default eMMC boot provides only 16 GB storage which is in-sufficient to run/build the container image, it is required to boot the Host OS using a 32 GB (minimum) SD card.

### Clone the Repository (on your development machine)

> **Note for Windows Users:**  
> If you are using **Linux**, no changes are needed — LF line endings are used by default.  
> If you are on **Windows**, please follow the steps in [Windows Git Line Ending Setup](./windows-git-setup.md) before cloning to ensure scripts and configuration files work correctly on Device.

```bash
git clone https://github.com/Advantech-EdgeSync-Containers/eIQ-GenAI-Flow2.0-Container-on-NXP-i.mx95.git
cd eIQ-GenAI-Flow2.0-Container-on-NXP-i.mx95
```

### Transfer the `eIQ-GenAI-Flow2.0-Container-on-NXP-i.mx95` folder to AOM-5521 device

If you cloned the repo on a **separate development machine**, use `scp` to transfer only the relevant folder:

```bash
# From your development machine (Ubuntu or Windows PowerShell if SCP is installed)
scp -r ./eIQ-GenAI-Flow2.0-Container-on-NXP-i.mx95\ <username>@<aom5521-ip>:/home/<username>/
```

Replace:

* `<username>` – Login username on the AOM-5521 board (e.g., `root`)
* `<aom5521-ip>` – IP address of the AOM-5521 board (e.g., `192.168.1.42`)

This will copy the folder to `/home/<username>/eIQ-GenAI-Flow2.0-Container-on-NXP-i.mx95`.

Then SSH into the NXP device:

```bash
ssh <username>@<aom5521-ip>  (add '-X' tag for X11 display)
cd ~/eIQ-GenAI-Flow2.0-Container-on-NXP-i.mx95
```

### Installation

```bash
# Make the build script executable
chmod +x run.sh

# Launch the container
./run.sh
```

### Launch eIQ GenAI Flow
Launch in CPU mode
```
python3 eiq_genai_flow.py
```
Enable Neutron(NPU) acceleration
```
python3 eiq_genai_flow.py -n
```
Ref: https://github.com/nxp-appcodehub/dm-eiq-genai-flow-demonstrator

### Benchmark
Benchmark in CPU mode
```
python3 eiq_genai_flow.py -b
```
Benchmark in NPU mode
```
python3 eiq_genai_flow.py -b -n
```

### eIQ GenAI Flow 2.0 API Demo Launcher
After the container launched, you may do a quick test to the eIQ GenAI Flow 2.0 API.
Modify the file `advantech-eiq-api-demo.py` to build your own AI Agent.

Test ASR
```
python3 advantech-eiq-api-demo.py --asr
```

Test LLM
```
python3 advantech-eiq-api-demo.py --llm
```

Test TTS
```
python3 advantech-eiq-api-demo.py --tts
```



## Best Practices

### Precision Selection
- **Prefer INT8 for NPU acceleration:** The i.MX95 NPU is optimized for quantized INT8 models. Always convert to INT8 using post-training quantization or quantization-aware training for maximum performance and efficiency.


### Deployment Practices

- **Python API is provided for further developing** By importing eIQ GenAI flow python libraries, you can create your own AI application by creating a python file under “dm-eiq-genai-flow-demonstrator”.

Example usage
#### ASR API
```
from asr.streaming.speech_to_text import SpeechToText
from shared_utils.utils import get_default_playback_device

asr = SpeechToText(
    model_name = 'whisper-small.en', #  'whisper-small.en', 'moonshine-base', etc.
    language = 'English',  # 'only for multilingual models: English, French, Chinese, etc.'
    task = 'transcribe',  # 'only for multilingual models: transcribe' or 'translate'
    source='mic',  # 'mic' or 'file',
    audio_card_name = get_default_playback_device(),  # specify the playback device
    stream_print=False
)

''' transcribe speech from a file (requires source='file') '''
text_from_file = asr.file_to_text(audio_file='asr/tests/data/sample_en.wav')
print(text_from_file) # final text after end of transcription

''' transcribe speech from the microphone (requires source='mic') '''
while not input("\nPress Enter key..."): # keyboard
    text_from_mic = ''
    for text_from_mic in asr.mic_to_text():
        pass
    print(text_from_mic)  # final text after end of transcription

```

#### LLM API
```
from llm.modeling_llm import make_LLM
from llm.config.user_config import Config as user_config

llm = make_LLM(name="danube-500M-q8",  # LLM model
               user_params=user_config # user-defined configuration
               )

while True:
    question = input("\nType your question here: ")
    for i, token in enumerate(llm(question, user_config.prompt)):
        print(token, end="")

```
#### TTS API

```
import os
from tts.inference import TTSPlayer
from tts.config import MultiSpeakerTTS16kHzConfig, MultiSpeakerTTS16kHzQuantConfig

# you can choose between the normal or quantized model (the latter is faster)
# config = MultiSpeakerTTS16kHzConfig(
config = MultiSpeakerTTS16kHzQuantConfig(
  speaker_id=1,  # between 1 and 900
  speed=0.52  # the greater, the faster
)
# it will generate speech and play it
tts = TTSPlayer(
  config=config,
  # playback_device="plughw:CARD=wm8962audio"  # optionally, configure the playback device
)
# whole text
tts("Hello world!", eos=True)
tts.join()  # wait until generation & audio playback are finished
# token by token
for token in ["This", " is", " a", " sentence", ".", " Another", " one", "."]:
  tts(token)
tts(eos=True)
tts.join()  # wait until generation & audio playback are finished

os._exit(0) # exit the program and avoid waiting for the timeout to end
```

## Known Limitations
- **Minimum storage** Storage required for running Docker containers is 32 GB

- **NPU passthrough** Neutron EP error sometimes, reboot the development board may solve this problem.
```
Neutron: NEUTRON_IOCTL_BUFFER_CREATE failed!: Cannot allocate memory
```

Copyright © 2026 Advantech Corporation. All rights reserved.