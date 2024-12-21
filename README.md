# Privateer

## Services

### **traefik** ([traefik][12])
- Routes all traffic through port 443 (https) to minimize open ports
- Auto-renews https certificates before they expire
    (require a cloudflare DNS provider)

### **dyndns** ([cloudflare-dynamic-dns][2]):
- Updates cloudflare DNS records with the host's public IP address

### sablier ([sablier][14]):
- Automatically shuts down user facing services when they are not in use

### **vpn** ([gluetun][3])
- Protects sensitive connections with a VPN

### **torrent** ([qbittorrent][4])
- Downloads and seeds requested content

### **proxy** ([flaresolverr][5])
- Bypasses cloudflare captchas when reaching torrent sites

### **resolver** ([prowlarr][6])
- Automatically configures torrent sites for requesters (sonarr + radarr)

### **movies** ([radarr][7])
- Automatically fetches requested movies

### **tv** ([sonarr][8])
- Automatically fetches requested TV Shows

### **oversee** ([overseerr][9])
- Manages users and limit how many request/month they can make
- Also: necessary for discord bot integration

### **bot** ([doplarr][10])
- Discord bot that allows users to request media

### **jellyfin** ([jellyfin][11])[^1]
- Streams media to users


## What you need to make this work:

1. A domain name
2. A cloudflare account (free but the domain DNS must be managed by cloudflare)
3. An active PIA VPN subscription [^2]
4. A machine with docker and docker-compose installed
   (and sufficient resources to store and serve media)
5. A discord server in which you are an admin
   (optional but lets you request media and get status update in discord)

## How to set it up:

1. Clone this repository
2. copy the `template.env` file to `.env` and fill in the necessary values
3. Run `docker-compose up -d` to start the services

## Tips & Tricks:

[lazydocker][13]: Is a great tool to monitor the status of your docker containers. If anything
goes wrong. Looking at a containers logs is a great way to figure out what went wrong.

The `template.env` file contains a `MISC_DIR` variable. This is for you to stream any media
that isn't a movie or TV show (ex: family videos).

Once the services are online, you will need to configure each service through their web UI. It's
simpler to do so on your LAN network calling the ports:

<div align="center">

| Service     | Port                                       |
|-------------|--------------------------------------------|
| Traefik     | `:8080`                                    |
| Prowlarr    | `:9696`                                    |
| Radarr      | `:7878`                                    |
| Sonarr      | `:8989`                                    |
| Overseerr   | `:5055`                                    |
| qbittorrent | `:4242`[^3]  |

</div>

[1]: https://wiki.servarr.com
[2]: https://github.com/mxmlndml/cloudflare-dynamic-dns
[3]: https://hub.docker.com/r/qmcgaw/gluetun
[4]: https://hub.docker.com/r/linuxserver/qbittorrent
[5]: https://hub.docker.com/r/flaresolverr/flaresolverr
[6]: https://hub.docker.com/r/linuxserver/prowlarr
[7]: https://hub.docker.com/r/linuxserver/radarr
[8]: https://hub.docker.com/r/linuxserver/sonarr
[9]: https://hub.docker.com/r/linuxserver/overseerr
[10]: https://hub.docker.com/r/linuxserver/doplarr
[11]: https://hub.docker.com/r/linuxserver/jellyfin
[12]: https://doc.traefik.io/traefik/
[13]: https://github.com/jesseduffield/lazydocker
[14]: https://github.com/sablierapp/sablier

[^1]: Note that even though I use plex on a daily bases, I did not include it in this
configuration as my instance is running bare-metal to avoid nvidia-docker
shenanigans. In this usecase, jellyfin is used as a fallback which works out of
the box even without plex being configured.

[^2]: Other VPNs supported by gluetun can be configured with minimal changes. PIA required a
qbittorrent to be configured a bit differently as it provides a port-forwarding service with
the port being randomly assigned and stored in a json file. Look at the qbit-pia folder to
see the changes that needed to be made.

[^3]: `:8080` is the default port but it is already in use by Traefik
