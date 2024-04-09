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

### Docker Installation and Configuration

### 9. Install Docker
Follow the official Docker installation instructions for Ubuntu:
[Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

### 10. Add User to Docker Group
Ensure your user can execute Docker commands without sudo.
```bash
sudo groupadd docker
sudo usermod -aG docker $USER
```
- Optional: Install Docker Compose.
```bash
sudo apt-get install docker-compose
```

### 11. Install NVIDIA Container Toolkit
Follow the installation guide to enable NVIDIA GPU support in Docker containers:
[Installing NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)

### 12. Configure Docker Daemon
Configure Docker to use the NVIDIA runtime by default.
- Edit `daemon.json`:
```bash
sudo nano /etc/docker/daemon.json
```
- Ensure the file includes the following:
```json
{
    "runtimes": {
        "nvidia": {
            "path": "nvidia-container-runtime",
            "args": []
        }
    },
    "default-runtime": "nvidia",
    "storage-driver": "overlay2"
}
```

### Final Steps: Deploying Your Docker Container

### 13. Prepare Worker Model Repository
Create a directory for your Neurochain worker model repository.
```bash
mkdir ~/ncn
sudo chown -R 1000:1000 ~/ncn
```

### 14. Run Your Docker Container
Execute the Docker command to start your worker container.
```bash
docker run --privileged --env SIGNATURE="Your_neurochain_ai_worker_key" -v /home/<your_user_name>/ncn:/worker-home/models -d nc-uat-registry.neurochain.io/ncllmworker:v0.3.3.linux
```
Replace `<your_user_name>` with your actual username and `"Your_neurochain_ai_worker_key"` with your actual worker key.

### 15. Verify Worker Connection
You can check if your worker has successfully connected to the network.

- List running Docker containers:
```bash
docker ps
```

- Look for your worker container's name. Assuming the container name is `intelligent_mirzakhani`, you can follow its logs:
```bash
docker logs -f intelligent_mirzakhani
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
