# Setup Guide for Ollama, DeepSeeker, and OpenWebUI

This guide provides instructions for setting up **Ollama**, **DeepSeeker**, and **OpenWebUI** on Linux, macOS, and Windows systems.

---

## Prerequisites
- **Hardware**:
  - A machine with sufficient CPU/GPU resources for AI models.
  - Minimum recommended: 8 GB RAM and 4 CPU cores.
- **Software**:
  - Docker.
  - Git.

---

## 1. Install Ollama
### Linux
1. Download the Ollama binary from the [official website](https://ollama.com/download).
2. Extract the tarball:
   ```bash
   tar -xzf ollama-linux.tar.gz
   sudo mv ollama /usr/local/bin/
   ```
3. Verify installation:
   ```bash
   ollama version
   ```

### macOS
1. Download Ollama for macOS from [Ollama Downloads](https://ollama.com/download).
2. Open the `.dmg` file and drag Ollama to the Applications folder.
3. Verify installation:
   ```bash
   ollama version
   ```

### Windows
1. Download the Ollama installer for Windows from [Ollama Downloads](https://ollama.com/download).
2. Run the installer and follow the on-screen instructions.
3. Verify installation in Command Prompt or PowerShell:
   ```powershell
   ollama version
   ```

---

## 2. Install DeepSeeker
### Steps for All Platforms
1. **Pull the Desired Model**:
   - Use the command to pull the required model (e.g., DeepSeeker Base):
     ```bash
     ollama run deepseek-coder-v2:16b
     ```

2. **Verify Installation**:
   - Check installed models:
     ```bash
     ollama list
     ```

3. **Run a Test**:
   - Test the model:
     ```bash
     ollama run deepseek-coder-v2:16b
     ```

---

## 3. Install OpenWebUI
### Linux
1. Clone the OpenWebUI repository:
   ```bash
   git clone https://github.com/OpenWebUI/openwebui.git
   cd openwebui
   ```
2. Run the following command to start the docker container:
   ```terminal
   docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
   ```

### macOS
1. Clone the repository and navigate to the directory:
   ```bash
   git clone https://github.com/OpenWebUI/openwebui.git
   cd openwebui
   ```
2. Run the following command to start the docker container:
   ```terminal
   docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
   ```

### Windows
1. Clone the repository:
   ```powershell
   git clone https://github.com/OpenWebUI/openwebui.git
   cd openwebui
   ```
2. Run the following command to start the docker container:
   ```powershell
   docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
   ```

---

## 4. Run All Together
### Using Docker (Optional)
If you prefer containerized setup, create a Docker Compose file to manage all three tools:

1. Create a `docker-compose.yml`:
   ```yaml
   version: '3.8'
   services:
     ollama:
       image: ollama/ollama:latest
       ports:
         - "11434:11434"
     deepseeker:
       image: deepseek/deepseeker:base
       ports:
         - "11435:11435"
     openwebui:
       build: ./openwebui
       ports:
         - "8000:8000"
   ```

2. Start the services:
   ```bash
   docker-compose up -d
   ```

3. Access the tools:
   - Ollama: `http://localhost:11434`
   - DeepSeeker: `http://localhost:11435`
   - OpenWebUI: `http://localhost:8000`

---

## Troubleshooting
- **Permission Issues**:
  - Use `sudo` if you encounter permission issues on Linux.
- **Python Dependencies**:
  - Ensure you are using a Python virtual environment to avoid conflicts.
- **Docker Not Found**:
  - Install Docker from [docker.com](https://www.docker.com/).

---

# How to stop the docker container and deepseek

```bash
docker ps #to obtain the container id
docker stop <docker-id>
ollama list #to obtain deepseek service name
ollama stop deepseek-coder-v2:16b
```

You’re all set! Let me know if you encounter any issues during the setup.
