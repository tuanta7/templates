# Ollama Setup on Mac

This guide details the steps required to set up **Ollama** (to run the Gemma model), configure a **reverse proxy using Nginx**, and expose the service securely via a **Cloudflare Tunnel**.

## 1. Ollama and Gemma Model Setup

This [link](https://ai.google.dev/gemma/docs/integrations/ollama) provides the necessary command for installing Ollama and pulling the Gemma model.

Once installed, you can verify the setup by running the Gemma model:

```sh
ollama run gemma:2b
```

## 2. Nginx Reverse Proxy Setup

Nginx will be used to run a reverse proxy for your Ollama service. This is often required for domain mapping, SSL termination, and better security/management.

```sh
# Install Nginx (using Homebrew on macOS)
brew install nginx

# Start the Nginx service
brew services start nginx

# Check service status
brew services
```

### Configuration

Use `nano` (or your preferred editor) to modify the main configuration file. The path below is typical for Homebrew installations.

> [!TIP]
> To show hidden folders on a Mac, open Finder and press `Command + Shift + .` (period) to toggle their visibility.

```sh
nano /opt/homebrew/etc/nginx/nginx.conf
```

Inside the `http` block, you will typically define a `server` block to proxy requests to the default Ollama port (`11434`).

```nginx
# Add this block inside the 'http' section
server {
    listen 80; # Or a custom port, e.g., 8080
    server_name your.local.domain.name; # Replace with your domain or IP

    location / {
        proxy_pass http://localhost:11434; # Ollama's default port
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

After saving the changes, test the configuration syntax and reload Nginx.

```sh
nginx -t
brew services reload nginx
```

## 3. Cloudflare Tunnel Setup

A Cloudflare Tunnel securely connects your locally running service (via Nginx) to the Cloudflare network without exposing an open incoming port on your firewall.

- **Install and Authenticate**: Install the `cloudflared` daemon and authenticate it with your Cloudflare account.
- **Create and Configure the Tunnel**: Use the `cloudflared tunnel create` command and configure the **Ingress** rules to route traffic from your desired domain to the local Nginx port (e.g., port 80 or 8080).

> [!NOTE]
> Detailed steps for Cloudflare Tunnel setup are extensive and depend on your Cloudflare dashboard configuration. Please refer to the official Cloudflare Zero Trust documentation.

Run the tunnel service

```sh
sudo cloudflared service install eyJhI..xyz
```
