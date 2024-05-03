# Advanced Linux Setup Instructions for NVIDIA Compute Support with Docker

## Prerequisites
- Knowledge of Docker and root privileges on your machine.
- We used Ubuntu Server 22.04 with an NVIDIA RTX 3090 GPU for this manual preparation.  
- Existing NVIDIA and CUDA installation accessible from Docker (if so, skip to Docker command execution).

## Fresh system Preparation Nvidia driver, Cuda and Docker install (Optional)

### 1. Update System
Update your system's package list and upgrade all installed packages to their latest versions.
```bash
sudo apt-get update
sudo apt-get upgrade
```

### 2. Reboot
Reboot your system to apply updates and changes.
```bash
sudo reboot
```

### 3. Install Essential Packages
Install Git and build-essential for compiling software.
```bash
sudo apt-get install git build-essential
```

### 4. Disable Nouveau Drivers
Nouveau drivers can interfere with the proper functioning of NVIDIA's proprietary drivers.
- Open or create the blacklist file for Nouveau:
```bash
sudo nano /etc/modprobe.d/blacklist-nouveau.conf
```
- Insert the following lines:
```plaintext
blacklist nouveau
options nouveau modeset=0
```
- Update initramfs:
```bash
sudo update-initramfs -u
```

### 5. Reboot
Reboot your system to apply the changes.
```bash
sudo reboot
```

### 6. Download and Install NVIDIA Driver and CUDA
- Download the CUDA installer:
```bash
wget https://developer.download.nvidia.com/compute/cuda/12.1.0/local_installers/cuda_12.1.0_530.30.02_linux.run
```
- Run the installer (accept the EULA and choose to install the Driver and CUDA):
```bash
sudo sh cuda_12.1.0_530.30.02_linux.run
```

### 7. Update Environment Variables
Add CUDA to your environment variables.
- Edit `.bashrc`:
```bash
nano ~/.bashrc
```
- Add the following lines at the end:
```bash
export CUDA_HOME=/usr/local/cuda-12.1
export LD_LIBRARY_PATH=${CUDA_HOME}/lib64:$LD_LIBRARY_PATH
export PATH=${CUDA_HOME}/bin:${PATH}
```
- Apply the changes:
```bash
source ~/.bashrc
```

### 8. Reboot and Verify GPU Information
Reboot your system and then verify that your GPU is recognized by running `nvidia-smi`.

## Running model

### 1. Download recent model, extract and open directory in terminal 

execute: 
```bash
export SIGNATURE=<YOUR_SIGNATURE>
```

and run the model:

```bash
./server
```


You should see output similar to this in the terminal, indicating the worker's initialization and configuration:
```
2024-03-20 15:27:54,205 INFO Initialising app...
2024-03-20 15:27:54,917 INFO Received configuration...
2024-03-20 15:27:54,917 INFO Found LLM config: models/vicuna-13B-v1.5-GPTQ of model: vicuna-13B-v1.5-GPTQ
2024-03-20 15:27:54,917 INFO LLM model exist, cancel downloading model: vicuna-13B-v1.5-GPTQ
2024-03-20 15:27:54,917 INFO Found LLM config: models/Llama-2-13B-chat-GPTQ of model: Llama-2-13B-chat-GPTQ
2024-03-20 15:27:54,917 INFO LLM model exist, cancel downloading model: Llama-2-13B-chat-GPTQ
2024-03-20 15:27:54,917 INFO Found LLM config: models/Mistral-7B-OpenOrca-GPTQ of model: Mistral-7B-OpenOrca-GPTQ
2024-03-20 15:27:54,917 INFO LLM model exist, cancel downloading model: Mistral-7B-OpenOrca-GPTQ
2024-03-20 15:27:54,917 INFO Found LLM config: models/Mistral-7B-Instruct-v0.2-GPTQ-Neurochain-custom of model: Mistral-7B-Instruct-v0.2-GPTQ-Neurochain-custom
2024-03-20 15:27:54,917 INFO LLM model exist, cancel downloading model: Mistral-7B-Instruct-v0.2-GPTQ-Neurochain-custom
2024-03-20 15:27:54,917 INFO Found LLM config: models/Mistral-7B-Instruct-v0.2-GPTQ-Neurochain-custom of model: Mistral-7B-Instruct-v0.2-GPTQ-Neurochain-custom-io
2024-03-20 15:27:54,917 INFO LLM model exist, cancel downloading model: Mistral-7B-Instruct-v0.2-GPTQ-Neurochain-custom-io
2024-03-20 15:27:54,917 INFO Found LLM config: models/CodeLlama-34B-Instruct-GPTQ of model: CodeLlama-34B-Instruct-GPTQ
