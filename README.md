# Azzbo77-AI

Welcome to my first attempt at building an AI Agent! This is a work-in-progress project, and I’ll be developing it iteratively as I go. The goal is to create a functional AI agent using modern tools and frameworks.

## Current Setup

- Ollama: Used as the core AI model framework.

- Open WebUI: Provides a user-friendly interface to interact with the AI.

- SearXNG: A local, privacy-focused metasearch engine for web search capabilities.

- Docker: Everything runs inside a Docker container for easy setup and portability.

## Project Status

This project is in its early stages. Right now, I’m focused on getting the basic components working together. Future updates will include more features, refinements, and documentation as I progress.

## Getting Started

### Prerequisites

- Docker installed on your machine ([Instructions for Ubuntu](docker-install-ubuntu.md) | [Official Docker Installation Guide](https://docs.docker.com/get-docker/))
  
- NVIDIA Container Toolkit installed if using an NVIDIA GPU for Ollama ([Instructions for Ubuntu](docker-install-ubuntu.md) | [Official NVIDIA Docs](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)). Install with:
  
  ```bash
  sudo apt-get install -y nvidia-container-toolkit
  sudo nvidia-ctk runtime configure --runtime=docker
  sudo systemctl restart docker
  ```
  
- Basic familiarity with command-line tools
  
### Installation

Follow these steps to set up the project locally:

1. **Clone the repository**

   ```bash
   git clone https://github.com/Azzbo77/Azzbo77-AI.git
   cd Azzbo77-AI
   ```

3. **Run the Docker container**

   ```bash
   docker-compose up --build
   ```

4. **Pull a model for Ollama**

   Ollama starts without a pre-loaded model. In a new terminal, pull a model (e.g., llama2) by running:
   ```bash
   docker exec -it ollama ollama pull llama2
   ```
   Alternatively, you can select and pull a model through the Open WebUI interface after it loads.

5. Open your browser and navigate to:

 - http://localhost:3000 to access the Open WebUI.
 - http://localhost:8080 to access SearXNG for local web searches.

## Next Steps

- Configure Ollama with a specific model

- Add custom functionality to the AI agent

- Integrate SearXNG search results with Open WebUI

- Improve the UI experience

- Document usage and features

## Contributing
Since this is my first attempt, I’m open to suggestions! Feel free to open an issue or submit a pull request if you have ideas or improvements.

## License

This project is licensed under the [MIT License](LICENSE).


