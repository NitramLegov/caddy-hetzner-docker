# Caddy with Hetzner DNS plugin

This repository builds a custom Caddy 2 Docker image that includes the Hetzner DNS provider plugin:

- `github.com/caddy-dns/hetzner/v2`

It uses `xcaddy` inside the official Caddy builder image to compile a custom binary with the plugin enabled.

## Build the image

```bash
docker build -t caddy-hetzner .
```

## Run with a Caddyfile

Create a `Caddyfile` from the example:

```bash
cp Caddyfile.example Caddyfile
```

Then set your Hetzner token:

```bash
export HETZNER_API_TOKEN="your-token-here"
```

Run the container:

```bash
docker run --rm -it \
  -p 80:80 \
  -p 443:443 \
  -p 443:443/udp \
  -e HETZNER_API_TOKEN="${HETZNER_API_TOKEN}" \
  -v "$PWD/Caddyfile:/etc/caddy/Caddyfile:ro" \
  caddy-hetzner
```

## Docker Compose

This repo includes a `docker-compose.yml` example:

```bash
cp .env.example .env
# edit .env with your token

docker compose up --build -d
```

## .env example

```env
HETZNER_API_TOKEN=your-token-here
```

## Notes

- The Dockerfile builds a Caddy binary with the Hetzner DNS plugin included.
- You can adjust the Caddyfile to suit your domain and certificates.
- The plugin is useful for ACME DNS-01 validation with Hetzner.
