# Azzbo77-AI

Welcome to my first attempt at building an AI Agent! This is a work-in-progress project, and I’ll be developing it iteratively as I go. The goal is to create a functional AI agent using modern tools and frameworks.

## Current Setup

- **Ollama:** Core AI model framework, running in a container with GPU support (NVIDIA) and persistent storage for models.
- **Open WebUI:** User-friendly interface at https://<your-ip>:3000, connected to Ollama and enhanced with SearXNG for web search.
- **SearXNG:** Privacy-focused metasearch engine at https://<your-ip>:8080, integrated with Open WebUI for Retrieval-Augmented Generation (RAG).
- **Nginx**: Reverse proxy providing SSL termination for both Open WebUI and SearXNG.
- **Docker:** All services run in Docker containers, orchestrated with Docker Compose for easy setup and portability.

## Project Status

The basic components are fully operational! Open WebUI chats with Ollama models, SearXNG provides local search, and Nginx secures access with SSL. Future updates will add features, refine functionality, and expand documentation.

## Getting Started

### Prerequisites

- Docker installed on your machine ([Instructions for Ubuntu](docker-install-ubuntu.md) | [Official Docker Installation Guide](https://docs.docker.com/get-docker/)).
  
  - Windows: Use WSL 2 with Docker Desktop. Install WSL 2 (wsl --install), an Ubuntu distro, and enable WSL 2 in [Docker Desktop WSL 2 setup](https://docs.docker.com/desktop/wsl/).
  
- NVIDIA Container Toolkit installed if using an NVIDIA GPU for Ollama ([Instructions for Ubuntu](docker-install-ubuntu.md) | [Official NVIDIA Docs](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)).
  
- Basic familiarity with command-line tools.
  
### Installation

Follow these steps to set up the project locally:

1. **Clone the repository**

   ```bash
   git clone https://github.com/Azzbo77/Azzbo77-AI.git
   cd Azzbo77-AI
   ```

2. **Generate Self-Signed Certificates**  
Replace 192.1xx.x.xxx with your machine’s IP or use localhost: (These files are ignored by `.gitignore` for security)

   ```bash
   mkdir certs
   openssl req -x509 -newkey rsa:4096 -keyout certs/key.pem -out certs/cert.pem -days 365 -nodes -subj "/CN=192.1xx.x.xxx" -addext "subjectAltName = DNS:localhost, IP:192.1xx.x.xxx"
   ```

3. **Configure Environment Variables**  
Copy the example file:

   ```bash
   cp .env.example .env
   ```
   
   - Edit .env to set NGINX_SERVER_NAME (e.g., 192.1xx.x.xxx) and other variables if needed.
   - .env is optional but recommended for external access (ignored by .gitignore).

4. **Verify Nginx Config**  
The nginx.conf uses localhost as server_name. Update to your IP (e.g., 192.1xx.x.xxx) if accessing remotely:
  
   ```bash
   server_name 192.1xx.x.xxx;
   ```
   
5. **Run the Docker container**

   ```bash
   docker compose up -d
   ```
    
   - Starts all services in detached mode.
   - Use --env-file .env if you customized .env.

6. **Pull a model for Ollama**  
   Ollama starts without a model. Pull one (e.g., phi-3):
   
   ```bash
   docker exec -it ollama ollama pull phi-3
   ```

   - phi-3 (~2.3GB, 5-10 minutes): Lightweight and fast.
   - llama2 (~7GB, 10-15 minutes): More capable but heavier.
   - The model is cached in the ollama-data volume after pulling.

7. Open your browser and navigate to:

   - Open WebUI: https://localhost (or your IP/domain).
   - SearXNG: https://localhost/searxng (or your IP/domain with /searxng path).

## Configuration
 
### The docker-compose.yml file defines:

 - **Reverse-proxy (Nginx)**:
   - Port: 443 (HTTPS, configurable via NGINX_PORT in .env).
   - SSL: Self-signed certs from ./certs.

 - **Ai-interface (Open WebUI)**:
   - Connects to model-runtime:11434 and search-engine:8080 for RAG (5 results, 10 concurrent requests).
   - No direct port exposed; accessed via Nginx.
 
 - **Model-runtime (Ollama)**:
   - Persistent storage: `ollama-data` volume.
   - GPU support: Reserves 1 NVIDIA GPU. (if available)
 
 - **Search-engine (SearXNG)**:
   - Config: ./searxng volume.

**Edit docker-compose.yml or .env to customize.**

## Next Steps

- [ ] Preload a default Ollama model. (e.g., phi-3). (Struggling with this due to pull times)
- [ ] Add custom AI agent features. (e.g., specific tasks)
- [x] Integrate SearXNG search results with Open WebUI (basic integration done via environment variables)
- [ ] Improve the UI experience.
- [ ] Expand documentation with screenshots and examples.

## Contributing

This is my first go, so I’d love feedback! Open an issue or submit a PR with ideas or fixes.

## Troubleshooting

- **Ollama fails to start with GPU**: Ensure NVIDIA drivers and the Container Toolkit are installed.
- **Port Conflicts**: Check 443 is free. (sudo netstat -tuln | grep 443)
- **No model loaded**: Pull a model as shown in the Installation steps.
- **"Thinking" State**: Ensure nginx.conf has location /ws/ or /ws/socket.io/.
- **Certificate Errors**: Ensure nginx.conf has location /ws/ or /ws/socket.io/.
- **Cert Errors**: Match CN and SAN to your IP/hostname.

## Notes
 
- Replace 192.1xx.x.xxx with your IP or hostname.
- .env enhances flexibility; defaults are in docker-compose.yml.

## License

This project is licensed under the [MIT License](LICENSE).

## Dependencies and Licenses

This project uses the following open-source tools:
- **Ollama**: License TBD (check [Ollama’s repository](https://github.com/ollama/ollama) for details).
- **Open WebUI**: Licensed under the MIT License ([source](https://github.com/open-webui/open-webui)).
- **SearXNG**: Licensed under the GNU AGPLv3 ([source](https://github.com/searxng/searxng)).
- **Nginx**: Licensed under the 2-clause BSD License ([source](https://nginx.org/LICENSE)).
- **Docker**: Licensed under the Apache License 2.0 ([source](https://github.com/docker/docker-ce)).

These tools are pulled as Docker images and run as separate services. Their respective licenses apply to their distributions.

