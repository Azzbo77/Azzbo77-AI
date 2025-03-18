# Installing Docker on Ubuntu

These instructions are for installing Docker on an Ubuntu system using the official Docker repository.

## Steps

Open your terminal and run the following commands to set up Docker’s apt repository:

1. **Update your package index and install prerequisites**

   ```bash
   sudo apt-get update
   sudo apt-get install ca-certificates curl
   ```

2. **Set up the keyring directory and download Docker’s GPG key**

   ```bash
   sudo install -m 0755 -d /etc/apt/keyrings
   sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
   sudo chmod a+r /etc/apt/keyrings/docker.asc
   ```

3. **Add Docker’s repository to your sources**

   ```bash
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

4. **Install Docker**

   After setting up the repository, update the package index again and install Docker:

   ```bash
   sudo apt-get update
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

5. **Install NVIDIA Container Toolkit (for GPU support)**
   
   If you plan to use an NVIDIA GPU with Docker (e.g., for Ollama), install the NVIDIA Container Toolkit:

   ```bash
   sudo apt-get install -y nvidia-container-toolkit
   sudo nvidia-ctk runtime configure --runtime=docker
   sudo systemctl restart docker
   ```

6. **Verify Installation**
   
   Check that Docker is installed correctly:

   ```bash
   docker --version
   ```

   **If using an NVIDIA GPU, verify GPU access:**

   ```bash
   docker run --rm --gpus all nvidia/cuda:11.0-base nvidia-smi
   ```

##  Notes

- These commands assume you have sudo privileges.

- For more details or troubleshooting, see the [Official Docker Installation Guide](https://docs.docker.com/get-docker/) or [NVIDIA Container Toolkit docs](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html).



## For Other Linux Distributions

- For other Linux distributions, refer to the [Official Docker Installation Guide](https://docs.docker.com/get-docker/).








 
