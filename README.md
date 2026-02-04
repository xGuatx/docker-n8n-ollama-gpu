# Docker n8n + Ollama + GPU + Cloudflare Tunnel

n8n workflow automation platform with Ollama LLM on the same Docker network with NVIDIA GPU support and Cloudflare tunnel for secure external access.

## Components

- **n8n**: Workflow automation platform (port 5678)
- **Ollama**: Local LLM with NVIDIA GPU acceleration
- **Cloudflare Tunnel**: Secure external access without exposing ports

## Prerequisites

- Docker and Docker Compose
- NVIDIA GPU with Docker GPU support (nvidia-docker2)
- Cloudflare account with tunnel configured
- Ollama Docker image built as `ollama-docker:v2`

## Setup

1. **Copy environment template:**
   ```bash
   cp .env.example .env
   ```

2. **Configure Cloudflare Tunnel:**
   - Go to https://dash.cloudflare.com/
   - Create a new tunnel
   - Copy your tunnel token
   - Add it to `.env` file:
     ```
     TUNNEL_TOKEN=your_actual_token_here
     ```

3. **Start services:**
   ```bash
   docker compose up -d
   ```

## Access

- **n8n Web UI**: http://localhost:5678 (or via Cloudflare tunnel)
- **Ollama**: Available on the `n8n` Docker network for workflow integration

## Security Notes

- Never commit `.env` file with real credentials
- The Cloudflare tunnel token provides external access - keep it secure
- GPU access is limited to 1 NVIDIA GPU for Ollama container

## Network Architecture

All services run on the same `n8n` bridge network, allowing n8n workflows to communicate with Ollama directly using the container name as hostname.

## Troubleshooting

**GPU not detected:**
```bash
docker run --rm --gpus all nvidia/cuda:11.0-base nvidia-smi
```

**n8n not accessible:**
Check that port 5678 is not already in use:
```bash
sudo lsof -i :5678
```

**Cloudflare tunnel issues:**
Verify your tunnel token is valid in the Cloudflare dashboard.
