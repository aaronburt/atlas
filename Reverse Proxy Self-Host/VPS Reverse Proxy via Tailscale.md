
### Goal 

Your goal is to share local services like a [Homepage](https://gethomepage.dev) but you don't want to use Cloudflare Tunnels or expose your personal network IP address to the internet. You might be also be unable to do so as your network is inside a [CGNAT](https://en.wikipedia.org/wiki/Carrier-grade_NAT)

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

#### Setting up tailscale

To set things up, first, install Tailscale on your local machine. If you're on Linux, run `curl -fsSL https://tailscale.com/install.sh | sh` command followed by `tailscale up` command; if you're on Windows, use the application.

Next, SSH into your VPS and install Tailscale. Confirm that the Tailscale client connects to the Coordination server successfully. You can check device status in the Coordination service's admin panel [here](https://login.tailscale.com/admin/machines).

Make sure the listed machines show as 'Connected.' Now, confirm communication between the VPS and the local machine. Open a terminal window on either system and run:

```
tailscale status
```

This command should display all machines allowed by your ACL policies. Next, run `tailscale ping` followed by the machine name (e.g., `tailscale ping desktop`) to check connectivity. If it says 'via Derp,' don't worry; it might take a few hours to establish a direct connection. This relayed connection is still secure, utilizing Wireguard end-to-end, albeit a bit slower.

### Setting up Docker 

Installing Docker on Debian-based machines is straightforward. Simply run the following one-liner provided by Docker:

```bash
curl https://get.docker.com | sh
```

This command will install Docker, Docker-cli, and Docker Compose, essential components for running Nginx Proxy Manager.

Check Docker is running with `docker run hello-world`, this will pull, run and output to the user a docker script that just says hello to verify docker is working as intended

#### Optional but recommended

For enhanced security, I highly recommend establishing a 'Rootless' user and including this user in the 'Docker' group. This allows the user to execute Docker commands without granting containers excessive permissions.

```bash
sudo useradd -m nginx
sudo usermod -aG docker nginx
```

### Setting up Nginx Proxy Manager

* Begin by establishing a directory named "Nginx."
* Within this directory, generate a file named "docker-compose.yml" and input the provided contents (e.g., using nano: `nano docker-compose.yml`).

```yml
version: '3.8'
services:
  app:
    container_name: 'NginxProxyManager'
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```

Next boot up the container with `docker compose up -d`

Upon successful execution, the output should indicate 'created' followed by an ID. Verify the running container status using `docker ps`.

#### Signing into NPM

It's very important to login to the admin panel of the NPM as you need to change the username and password from their insecure defaults. 

```
Email:    admin@example.com
Password: changeme
```

After updating the account credentials, you're good to go with the User Interface to initiate address proxying. Tailscale addresses, like your local machine's, can be reached using either the Tailscale IP address (e.g., 100.121.107.58) or the Tailnet DNS name if you've enabled 'Magic DNS' on the admin panel (e.g., local-machine.random-word.ts.net). Dealing with SSL certs creation, management, and DNS service setup involves too much variation for me to cover, so I'll leave that in your capable hands.

### ACL Policies 

Access Control List are very easier to make mistakes with, You are attempting to narrowly allow as much access to machines as deemed necessary without breaking things, it can sometimes be an extremely fine line. 

By default, Tailscale associates each machine with your email address, visible in the admin panel, indicating ownership. To make ACL policies effective, we must disconnect your email from the machine.

This is achieved by introducing a 'tag' that removes your direct ownership, rendering the machine without network permissions unless explicitly specified.

Let's start by creating a group to collectively own the 'tag':

```json
	"groups": {
		"group:admin": ["example@gmail.com"],
	},
```

Next, define the existence of the tag for application to the machine:

```json
	"tagOwners": {
		"tag:external":  ["group:admin"],
	},
```

Finally, specify where the tags are allowed to connect, including the option to designate other tags as destinations:

```json
"acls": [
	{
		"action": "accept",
		"src":    ["tag:external"],
		"dst":    ["100.111.13.102:53", "local:*"],
	},
]
```

In this example, the tag 'external' has access to view port 53 on the 102 address and any port on machines tagged as 'local'. To apply these tags, navigate to the Tailscale admin panel, click the three dots next to 'Last Seen' for each machine, and select 'Edit ACL Tags...'.

Be cautious, as selecting any ACL will require a full reauthentication of the machine if removal is attempted later on.