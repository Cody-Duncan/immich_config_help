# immich_config_help
Sharing my configuration files because I need help figuring them out.

# Goal
Setup `immich` as a photo sharing website. I'd like to share my good photos with friends and people that I meet.

Using
- [Unraid](https://unraid.net/) - OS and Docker platform
- [Traefik](https://doc.traefik.io/traefik/) - Reverse-Proxy to forward requests to the web server and handle SSL certification and generation via Automated Certificate Management Environment (ACME) protocol.
- [Let's Encrypt](https://letsencrypt.org/) - SSL Certificate Authority and ACME protocol provider.
- [Cloudflare](https://www.cloudflare.com/) - DNS provider and handles challenging the SSL certificate.
- [Cloudflare Tunnel](https://www.cloudflare.com/products/tunnel/) - Tunnel to hide my public IP.
- [Immich](https://immich.app/) - Self-Hosted Photo sharing website

## Roadmap
1. ✅ Setup `immich` on `unraid`
    - [Tutorial](https://documentation.immich.app/docs/install/unraid) Complete. Can see Immich from local network 
2. ✅ Setup `traefik` on `unraid`
    - [Tutorial](https://doc.traefik.io/traefik/user-guides/docker-compose/basic-example/) Complete. Can se whoami from local network.
3. ✅ Setup `traefik` with `Let's Encrypt` to handle SSL Certification so the website can be https.
    - [Tutorial](https://doc.traefik.io/traefik/user-guides/docker-compose/acme-dns/) Complete. Can see whoami from local network with successful https certification.
4. ☐ Setup `traefik` as the reverse proxy in front of `immich_proxy` container, as described in [immich's documentation](https://documentation.immich.app/docs/administration/reverse-proxy) referred by [u/altran1502](https://www.reddit.com/r/immich/comments/12on862/comment/jgiy5ob/?utm_source=share&utm_medium=web2x&context=3).
    - I.E. Connect `traefik` to immich
5. ☐ Setup `Cloudflare Tunnel` to hide my ip address.
    - [Christian Lempa's tutorial here](`traefik`) as referred by [u/admecoach's post](https://www.reddit.com/r/Traefik/comments/13fjlsk/recommended_setup_for_traefik_using_cloudflare/)

## Current Status
Immich and Traefik containers are running.

Configuration Files:
- [traefik docker-compose](traefik_docker_compose.yml)
- [immich docker-compose](immich_docker_compose.yml)
- [immich .env](immich.env)

Unraid Docker Dashboard
![unraid_docker_page.png](unraid_docker_page.png)

Traefik seems to be running, and is forwarding requests sent to host `photos.codyduncan.net` towards `immich_proxy`.

Traefik Dashboard
![traefik_dashboard.png](traefik_dashboard.png)

When using IMAGE: `nginx:latest` for `immich_proxy`  
Visiting `photos.codyduncan.net` shows the nginx welcome page.
![navigate_to_address_shows_nginx_welcome.png](navigate_to_address_shows_nginx_welcome.png)

When using IMAGE: `ghcr.io/immich-app/immich_proxy:${IMMICH_VERSION:-release}` for `immich_proxy`  
Visiting `photos.codyduncan.net` shows `bad gateway`.
![navigate_to_address_shows_bad_gateway.png](navigate_to_address_shows_bad_gateway.png)

Visiting `192.168.1.104:2283` shows the immich welcome screen.
![immich_welcome_at_port_2283.png](immich_welcome_at_port_2283.png)

Visiting `192.168.16.9:8080`, which I think is the frontend to immich_proxy, says `The connection has timed out`.
![immich_frontend_address_times_out.png](immich_frontend_address_times_out.png)


## Problems

- `http://photos.codyduncan.net/` show the welcome page. The https version `https://photos.codyduncan.net/` shows `404 page not found`. How do I configure routing for https?
- `immich_proxy` is an nginx instance. How do I configure `traefik` to point at `immich_proxy`, and `immich_proxy` to point at immich, so that `https://photos.codyduncan.net/` shows the immich server? (Is `immich_proxy` already pointing at `immich_web`?)

# Any Help and Guidance is Appreciated!


