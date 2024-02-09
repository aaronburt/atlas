
Docker is an open-source project for automating and deploying applications. At some point you are going to run into an application that can either be installed via Docker or is built as a docker first application.

Docker is a runtime environment that needs to be installed on any of the machines that its deployed on. Its similar be required to install Java Runtime environment before being able to run java applications. 

### Installation via Proxmox Scripts

The fastest and simplest way to install Docker is via the Custom LXC deployment from Proxmox Scripts.


Proxmox scripts supports two versions of the LXC.

* Debian/Ubuntu 
* Alpine

The Debian install is far more feature rich out of the box but due to that its a far larger install size and can take a bit more time to deploy. Regardless of version you choose, I would suggest installing Docker compose V2 when prompted.

It may also prompt you for installing either Portainer or Portainer-Agent, this again is entirely up you and you current docker management suite. For more information see my Portainer guide (TODO)

For Debian Execute the following command in the Proxmox WebUI's Shell 
```bash
bash -c "$(wget -qLO - https://github.com/tteck/Proxmox/raw/main/ct/docker.sh)"
```

For Alpine
```bash
bash -c "$(wget -qO - https://github.com/tteck/Proxmox/raw/main/ct/alpine-docker.sh)"
```


### Installing on LXC via curl 

First, create an unprivileged Linux container. 

Update and upgrade the container to ensure all patches are installed
```bash
apt-get update && apt-get upgrade -y
```

Install curl as it doesn't come with most installations of Debian/Ubuntu by default for LXC's
```bash
apt install curl -y
```

Install docker and docker compose via the one-liner
```bash
curl https://get.docker.com | sh
```

You don't need to do rootless installs as with LXC's you are by default a 'root' user. 

### Installing on VM

Once setup its the exact same as installing via LXC's except right at the end you may want to setup rootless mode, in which a non-root user can run docker to avoid giving containers overly broad permissions from root.

You do this by adding the user in question to the `docker` group. 

To add the current user to the docker group you can run this command
```bash
sudo usermod -aG docker $USER 
```

If you would like to add another user to the `docker` group then swap out the $USER with the Linux username of them, this would be the login name and the name of the /home/ directory they own. 