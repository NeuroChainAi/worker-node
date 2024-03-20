# Windows Setup Instructions for AI Mining

## Step 1: Update NVIDIA Drivers

Ensure your system is running the latest NVIDIA drivers. If not, download and install the latest drivers from the NVIDIA official website:

[NVIDIA Drivers Download](https://www.nvidia.com/download/index.aspx)

## Step 2: System Reboot

After updating your drivers, reboot your computer to ensure all updates are applied correctly.

## Step 3: Install CUDA for Windows

Download and install CUDA for Windows by selecting your target OS (Windows) and architecture (x86_64) from the following link:

[CUDA Downloads for Windows](https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64)

## Step 4: Another System Reboot

Reboot your computer again to ensure the CUDA installation is fully configured.

## Step 5: Verify NVIDIA Driver Installation

Open Terminal and enter the command `nvidia-smi` to check if it returns information about your GPU. If you encounter an error message indicating that the command is not recognized, this suggests that the drivers were not installed successfully. In this case, restart the process from Step 1.

## Step 6: Download and Install Git with Git LFS

Download and install Git for Windows, which includes support for Git Large File Storage (LFS), from the following link:

[Git for Windows](https://gitforwindows.org/)

## Step 7: Prepare Your Workspace

Download the worker image and extract all files to your preferred location. Ensure you have between 100 to 240 GB of free space available for AI models.

## Step 8: Initialize the Worker

Open a PowerShell terminal and navigate to your worker files directory using the `cd` command. Set your worker signature from the Neurochain.ai website and start the server with the following commands:

```powershell
cd <path_of_your_worker_files>
$env:SIGNATURE="Your_worker_signature_from_neurochain.ai_website"
./server.exe



You can save this content as a markdown file (e.g., `setup-instructions.md`) and use it as needed. Remember to replace placeholders like `<path_of_your_worker_files>` and `"Your_worker_signature_from_neurochain.ai_website"` with actual values specific to your setup.

