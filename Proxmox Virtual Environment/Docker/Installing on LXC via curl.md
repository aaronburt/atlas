

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