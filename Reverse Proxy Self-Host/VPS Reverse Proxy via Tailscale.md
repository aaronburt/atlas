
### Goal 

Your goal is to share local services like a [Homepage](https://gethomepage.dev) but you don't want to use Cloudflare Tunnels or expose you personal network IP address to the internet. You might be also be unable to do so as your network is inside a [CGNAT](https://en.wikipedia.org/wiki/Carrier-grade_NAT)

### Requirements

You will need the following in order to follow this guide. 

* Debian VPS with root access (Updated with `apt-get update && apt-get upgrade -y `)
	* Recommend Oracle for free 
	* Scaleway for cheap European based
	* Google Cloud for secure
* Tailscale account
* Local machine that will host the application in question

### The idea 

The idea is that the VPS will host a [Reverse Proxy](https://www.cloudflare.com/en-gb/learning/cdn/glossary/reverse-proxy/) called [Nginx Proxy Manager](https://nginxproxymanager.com/) and will talk via an Tailscale Wireguard encrypted tunnel to a local machine on your personal network that will host the application in question. This means important credentials don't need to be saved on the VPS and your public IPV4 address doesn't need to be open to the elements. 

### The Execution 


#### Setting up tailscale

To set things up, first, install Tailscale on your local machine. If you're on Linux, run `curl -fsSL https://tailscale.com/install.sh | sh` command followed by `tailscale up` command; if you're on Windows, use the application.

Next, SSH into your VPS and install Tailscale. Confirm that the Tailscale client connects to the Coordination server successfully. You can check device status in the Coordination service's admin panel [here](https://login.tailscale.com/admin/machines).

Make sure the listed machines show as 'Connected.' Now, confirm communication between the VPS and the local machine. Open a terminal window on either system and run:

```
tailscale status
```

This command should display all machines allowed by your ACL policies. Next, run `tailscale ping` followed by the machine name (e.g., `tailscale ping desktop`) to check connectivity. If it says 'via Derp,' don't worry; it might take a few hours to establish a direct connection. This relayed connection is still secure, utilizing Wireguard end-to-end, albeit a bit slower.

### Setting up Docker 

Docker installation is rather simple on Debian based machines. You only need execute this one-liner provided by Docker themselves. 

```bash
curl https://get.docker.com | sh
```

This will install Docker, Docker-cli and Docker compose all 