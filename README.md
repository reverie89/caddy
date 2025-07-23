# caddy

This is an automated build for [reverie89/caddy](https://hub.docker.com/r/reverie89/caddy/).

Checks daily for updates to the [caddy Docker image](https://hub.docker.com/_/caddy).

## Overview for Caddy v2
Based on [official image](https://hub.docker.com/_/caddy), then added: [caddy-dns/cloudflare](https://github.com/caddy-dns/cloudflare) module

## Usage example
docker-compose.yaml
```sh
services:
 caddy:
   image: reverie89/caddy
   container_name: caddy
   restart: always
   environment:
     - CLOUDFLARE_API_TOKEN=xxx
   ports:
     - "80:80/tcp"
     - "443:443/tcp"
   volumes:
     - "/etc/localtime:/etc/localtime:ro"
     - "./Caddyfile:/etc/caddy/Caddyfile"
     - "./config:/config"
     - "./data:/data"
     - "/var/www:/var/www"
```

### How to use Caddyfile
[Official docs](https://caddyserver.com/docs/quick-starts/caddyfile)
```sh
subdomain.example.com {
 tls {
   dns cloudflare {env.CLOUDFLARE_API_TOKEN}
 }
 reverse_proxy /* endpoint:80
}

example.com {
 tls {
   dns cloudflare {env.CLOUDFLARE_API_TOKEN}
 }
 root * /var/www/example.com
}
```

**Take note**
- Caddy v2 requires Cloudflare API **token**. You will need to [create a token](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/) if you don't have one.
- Remember to volume mount `/config` and `/data`
- Graceful Caddyfile reload: `docker exec -w /etc/caddy {container_name} caddy reload`
- Format and overwrite Caddyfile: `docker exec -w /etc/caddy {container_name} caddy fmt --overwrite`