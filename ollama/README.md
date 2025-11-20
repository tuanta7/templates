# Ollama Setup on Mac

This guide details the steps required to set up **Ollama** (to run the Gemma model), configure a **reverse proxy using
Nginx**, and expose the service securely via a **Cloudflare Tunnel**.

## 1. Ollama and Gemma Model Setup

This [link](https://ai.google.dev/gemma/docs/integrations/ollama) provides the necessary command for installing Ollama and pulling the Gemma model. Once installed, you can verify the setup by running the Gemma model:

```sh
ollama run gemma:2b
```

## 2. Nginx Reverse Proxy Setup

Nginx will be used to run a reverse proxy for your Ollama service. This is often required for domain mapping, SSL
termination, and better security/management.

```sh
# install Nginx (using Homebrew on macOS)
brew install nginx

# start the Nginx service
brew services start nginx

# check service status
brew services
```

### Configuration

Use `nano` (or your preferred editor) to modify the main configuration file. The path below is typical for Homebrew
installations.

> [!TIP]
> To show hidden folders on a Mac, open Finder and press `Command + Shift + .` (period) to toggle their visibility.

```sh
nano /opt/homebrew/etc/nginx/nginx.conf
```

Inside the `http` block, you will typically define a `server` block to proxy requests to the default Ollama port (
`11434`).

```nginx
server {
    listen 80; # Or a custom port, e.g., 8080
    server_name your.local.domain.name; # Replace with your domain or IP

    location /o {
        rewrite ^/o/(.*)$ /$1 break;
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

A Cloudflare Tunnel securely connects your locally running service (via Nginx) to the Cloudflare network without
exposing an open incoming port on your firewall.

- Install the `cloudflared` daemon and authenticate it with your Cloudflare account.
- Create policies and tunnels using the Cloudflare [dashboard](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/get-started/create-remote-tunnel/).

> [!NOTE]
> Detailed steps for Cloudflare Tunnel setup are extensive and depend on your Cloudflare dashboard configuration. Please
> refer to the official Cloudflare Zero Trust documentation.

Run the tunnel service

```sh
# remove all existing tunnels
sudo cloudflared service uninstall

# create a new tunnel
sudo cloudflared service install eyJhI..xyz
```
