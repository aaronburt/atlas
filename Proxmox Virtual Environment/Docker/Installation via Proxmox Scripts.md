The fastest and simplest way to install Docker is via the Custom LXC deployment from Proxmox Scripts.

Proxmox scripts supports two distinct versions of the LXC.

* Debian/Ubuntu - Feature rich
* Alpine - Lightweight

Each version has its own advantages and disadvantages the Debian install is far more feature rich out of the box but due to that its a far larger in install size and can take a bit more time to deploy. Conversely the Alpine install is far lighter, faster to deploy but commonly requires external packages to be installed to match the out of the box experience from Ubuntu/Debian, I only recommend using Alpine where you would consider it a production system and you don't expect to install anything else.

Regardless of which version you choose, I would suggest installing docker compose when prompted.

It may also prompt you for installing either Portainer or Portainer-Agent, this again is entirely up you but isn't required to deploy and run containers. 

For more information see my Portainer guide (TODO)

For Debian Execute the following command in the Proxmox WebUI's Shell 
```bash
bash -c "$(wget -qLO - https://github.com/tteck/Proxmox/raw/main/ct/docker.sh)"
```

For Alpine
```bash
bash -c "$(wget -qO - https://github.com/tteck/Proxmox/raw/main/ct/alpine-docker.sh)"
```
